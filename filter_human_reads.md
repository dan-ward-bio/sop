# Clark / Campino Group SOP 
## Filtering Human reads for a fastq file without Kraken2 on a machine with limited resources
### Document version: 1.0
##### Description: A guide for removing human reads from a fastq file

Below is a guide to:
1) Map reads to the human genome, with a precompiled Minimap2 index.
2) Use seqtk to remove reads from the original fastq

### Before starting: Group fastq files

On the local machine, concatenate  the fastq files from the MinKNOW output directory and recompress them
    
    mkdir combined
    cd <MINKNOW DIRECTORY>
    for i in `ls` ; do cd ${i} ; zcat *.fastq.gz | gzip > ../combined/${i}.fastq.gz ;  cd ../ ; done

## Method 1: Exclusion through mapping

### Dependencies:
    conda install -c bioconda seqtk
    conda install -c bioconda minimap2


### 1) Build the Minimap2 index.

Run Minimap2 on a machine with > 50 GB RAM, then transfer the MMI to the local machine.

    minimap2 -x map-ont -d hs38-ont.mmi /mnt/hs38.fa


### 2) Mapping human reads.

Move back to the combined folder, run minimap2 in a loop over each barcoded file
    
    for i in `ls *fastq.gz | sed 's/.fastq.gz//g'` ; do minimap2 -ax map-ont -a ~/refeq/GRCh38_latest_genomic.fna.mmi ${i}.fastq.gz | samtools view -f 4 | samtools fastq - | gzip > ${i}_human_filtered.fastq.gz ; done


## Method 2: Mini Kraken2 human database

Below is a guide to:
1) Download the custom kraken2 database
2) Run kraken2 to classify reads on a custom database
3) Exclude reads using krakentools 

### Dependencies:
    conda install -c bioconda krakentools
    conda install -c bioconda kraken2


### 1) Download the kraken database and unzip it
    
    dan@s8:/mnt/storage8/dan/kraken/human_kraken2_db.tar.gz
    tar -xf human_kraken2_db.tar.gz
    
### 2) Grouping fastq files.

Run kraken2 on the local machine. Do not use the remote kraken server.
    
    for i in `ls *.fastq.gz | sed 's/.fastq.gz//g'` ; do kraken2 --use-names --threads 20 --db ~/past/to/database --report ${i}.kraken.report ${i}.fastq.gz > ${i}.kraken ; done

### 3) Exclude reads using krakentools
     for i in `ls *.fastq.gz | sed 's/.fastq.gz//g'` ; do extract_kraken_reads.py -s ${i}.fastq.gz -k ${i}.kraken --report ${i}.kraken.report --taxid 9606 --exclude --include-parents --fastq-output -o ${i}.filtered.fastq ; gzip ${i}.filtered.fastq  ; done
    
    
    