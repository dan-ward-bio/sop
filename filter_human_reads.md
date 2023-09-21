# Clark / Campino Group SOP 
## Filtering Human reads for a fastq file without Kraken2 on a machine with limited resources
### Document version: 1.0
##### Description: A guide for removing human reads from a fastq file

Below is a guide to:
1) Map reads to the human genome, with a precompiled Minimap2 index.
2) Use seqtk to remove reads from the original fastq

### Dependencies:
    conda install -c bioconda seqtk


### 1) Build the Minimap2 index.

Run Minimap2 on a machine with > 50 GB RAM, then transfer the MMI to the local machine.

    minimap2 -x map-ont -d hs38-ont.mmi /mnt/hs38.fa

    
### 2) Mapping human reads.

On the local machine, group together the fastq files from the MinKNOW output directory
    
    mkdir combined
    cd <MINKNOW DIRECTORY>
    for i in `ls` ; do cd ${i} ; zcat *.fastq.gz | gzip > ../combined/${i}.fastq.gz ;  cd ../ ; done

Move back to the combined folder, run minimap2 in a loop over each barcoded file
    
    for i in `ls *fastq.gz | sed 's/.fastq.gz//g'` ; do minimap2 -ax map-ont -a ../../Database/Human/GRCh38_latest_genomic.fna.mmi ${i}.fastq.gz | cut -f1 | grep -v @ > human_reads_${i}.txt  ; done
    
Now use seqtk to remove the reads from the original fastq file:
    
    ls *.fastq | while read -r line ; do kraken2_client --host-ip plum-g1 --sequence $line --report $line.report > $line.kraken ; done
    

     

