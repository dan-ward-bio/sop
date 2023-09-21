# Clark / Campino Group SOP 
## Filtering Human reads for a fastq file without Kraken2 on a machine with limited resources
### Document version: 1.0
##### Description: A guide for removing human reads from a fastq file

Below is a guide to:
1) Map reads to the human genome, with a precompiled Minimap2 index.
2) Use seqtk to remove reads from the original fastq

### Dependencies:
    conda install -c bioconda seqtk
    conda install -c bioconda minimap2


### 1) Build the Minimap2 index.

Run Minimap2 on a machine with > 50 GB RAM, then transfer the MMI to the local machine.

    minimap2 -x map-ont -d hs38-ont.mmi /mnt/hs38.fa

    
### 2) Grouping fastq files.

On the local machine, concatenate  the fastq files from the MinKNOW output directory and recompress them
    
    mkdir combined
    cd <MINKNOW DIRECTORY>
    for i in `ls` ; do cd ${i} ; zcat *.fastq.gz | gzip > ../combined/${i}.fastq.gz ;  cd ../ ; done

### 3) Mapping human reads.

Move back to the combined folder, run minimap2 in a loop over each barcoded file
    
    for i in `ls *fastq.gz | sed 's/.fastq.gz//g'` ; do minimap2 -ax map-ont -a ~/refeq/GRCh38_latest_genomic.fna.mmi ${i}.fastq.gz | samtools view -f 4 | samtools fastq - | gzip > ${i}_human_filtered.fastq.gz ; done


### Example: Kraken2 analysis on unfiltered and filtered

Check out these kronagrams demonstrating the removal of human reads from the original fastq:

#### [Unfiltered](https://link-url-here.org)
#### [Human-filtered](https://link-url-here.org)