# Clark / Campino Group SOP 
## Kraken2 classification (remote server)
### Document version: 1.0
##### Description: A guide for taxonomically classifying and visualising FASTA and FASTQ files using kraken2 server.

We can now streamline the use of kraken2 classification using the [kraken-2 server tool](https://github.com/epi2me-labs/kraken2-server), made by the ONT team, by hosting a server which keeps the ~200 GB database in memory, removing the need to load in to RAM each run, thus speeding up workflows significantly.

Below is a guide to:
1) Install the client.
2) Run the classifier from a remote server.
3) Visualise the kraken2 output using RCF.
### 1) Install the client.

Install the client using conda or mamba.

    mamba create -n kraken2 -c conda-forge -c epi2melabs kraken2-server
    conda create -n kraken2 -c conda-forge -c epi2melabs kraken2-server

### 2) Start the server.

Launching a screen with a persistent server

    screen -S kraken2_server
    conda activate kraken2
    kraken2_server --thread-pool 10 --max-requests 2 --host-ip <host-ip>:8080 --db <path to db>




### 3) Run the classifier from a remote server.

Run the client on your set of reads.
Activate the conda env
    
    conda activate kraken2

Run kraken2 client
    
    kraken2_client --host-ip plum-g1 --sequence <sequence file> --report <report output destination + filename > kraken_output.kraken
    
If you want to loop across multiple files:
    
    ls *.fastq.gz | while read -r line ; do kraken2_client --host-ip plum-g1 --sequence $line --report $line.report > $line.kraken ; done
    
### 3) Visualise the kraken2 output using RCF.

    conda deactivate
    pip install recentrifuge

Before running Recentrifuge, you need to download a database of taxa from NCBI. Put this in a memorable place so that you don’t have to keep downloading it:
    
    retaxdump

Now run recentrifuge

    rcf -n <path to 'taxdump' folder> -k kraken_output.kraken

On multiple outputs:

    ls *.kraken | while read -r line ; do rcf -n <path to retaxdump> -k $line ; done
    
If successful, you'll find a HTML document with a visualisation for classification. 
     

