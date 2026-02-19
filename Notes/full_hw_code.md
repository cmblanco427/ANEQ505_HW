```insta-toc
---
title:
  name: Table of Contents
  level: 1
  center: false
exclude: ""
style:
  listType: dash
omit: []
levels:
  min: 1
  max: 6
---

# Table of Contents

- HW1
    - 1. Launch interactive session, load Qiime2 w/in cow directory
    - 2. Import raw reads -> output as Qiime2 readable format   (qza)
    - 3. Demultiplex by submitting a job
        - 3.a make demux.sh file in slurm directory to demultiplex sequences quicker. Add following shebang followed by batch commands to the file
        - 3.b Demultiplex
        - 3.C Visualize demultiplexed read quality
    - 4. Run script from slurm directory as a job
    - 5. Denoise
        - 5a Denoise
        - 5b Visualize Denoised Tables
            - Denoising Stats
            - Denoised Table
            - Sequences Table
            - DADA2 Info
```
# HW1
## 1. Launch interactive session, load Qiime2 w/in cow directory

```r
#launch an interactive session: 
ainteractive --ntasks=6 --time=02:00:00

#activate qiime.
module purge  
module load qiime2/2024.10_amplicon #what exactly is happening here? Modifying shell to use QIIME2 v 2024.10?
```

## 2. Import raw reads -> output as Qiime2 readable format   (qza)
```r
qiime tools import \
--type EMPPairedEndSequences \
--input-path raw_reads \
--output-path cow_reads.qza
```
## 3. Demultiplex by submitting a job
### 3.a make demux.sh file in slurm directory to demultiplex sequences quicker. Add following shebang followed by batch commands to the file
	- lets u send data to alpine to work -> job. Takes a long time, so best to run as a job and not in the terminal
	- DONT need to put into command line
```r
#!/bin/bash
#SBATCH --job-name=demux
#SBATCH --nodes=1
#SBATCH --ntasks=12
#SBATCH --partition=amilan
#SBATCH --time=02:00:00
#SBATCH --mail-type=ALL
#SBATCH --output=slurm-%j.out
#SBATCH --qos=normal
#SBATCH --mail-user=c832916267@colostate.edu
```
### 3.b Demultiplex
```r
#Confirm in correct directory
pwd
#If not change to demux folder (DOES FOLDER HAVE TO BE CALLED THIS???)
cd /scratch/alpine/$USER/cow/demux

#Demultiplexing code
qiime demux emp-paired \ #so qiime knows its paired reads
--m-barcodes-file ../metadata/cow_barcodes.txt \ #metadata
--m-barcodes-column barcode \--p-rev-comp-mapping-barcodes \ #metadata column
--p-rev-comp-barcodes \ #barcode sequence reads will be reverse complemented prior to demultiplexing???? During seq can seq barcodes, wnat to get reverse complement of barcode. If we go R->L in F +R in primers, if we seq thru direction, need reverse complement so we can remove it later. To get rid of sequencing artifacts so no primers in seq files. 
--i-seqs ../cow_reads.qza \ #Artifact: paired end sequences to be demultiplexed from previously generated qza file??? F+ R reads
--o-per-sample-sequences demux_cow.qza \ #output resulting demultiplexed sequences
--o-error-correction-details cow_demux_error.qza #generates details about barcode error corrections
```

### 3.C Visualize demultiplexed read quality
```r
qiime demux summarize \
--i-data demux_cow.qza \
--o-visualization demux_cow.qzv #Shows Q scores so you know where to trim and/or truncate. 
```

## 4. Run script from slurm directory as a job
script runs from script file in slurm directory (generated in first step of demultiplexing by submitting a job step) So must be in slurm directory. Submits job to be scheduled in slurm, instead of running in terminal. 
```r
cd /scratch/alpine/$USER/cow/slurm #make sure ur in slurm folder
dos2unix demux.sh #PC users; converts file to unix format
#demux.sh is the file in slurm folder with our script
sbatch demux.sh #tells slurm (which CURC uses) to run everything in this script (demux.sh) on a compute node using the requested resources. Slurm will put the job in a que and run it when the resources are available (itll pop output into slurm-<jobid>.out file). #sends everything to slurm scheduler to put into que
```
- outputs batch job id that can be used to kill job or check its status. Can also use on demand portal to check
	- ex output:  Submitted batch job 23996208
## 5. Denoise
- denoise samples based on what should be <span style="color:rgb(0, 112, 192)">trimmed </span>(front of reads) or <span style="color:rgb(0, 112, 192)">truncated</span> (ends of reads). Use the demux_cow.qzv file. This can be done in the terminal or as a job. 
- This is a dada2 analog step
- **Can run in terminal**
### 5a Denoise
```r
cd /scratch/alpine/$USER/cow/dada2

qiime dada2 denoise-paired \ #runs dada2 algorithm for paired end reads
--i-demultiplexed-seqs ../demux/cow_demux.qza \ #tells dada2 to use this demultiplexed dataset as input (we made this dataset as output from qiime demux emp-paired step), code links to demux folder where the demux_cow file is.Contains forward/reverse reads and is sorted by sample.
--p-trim-left-f NUMBER \ #these 2 lines do left trims off forward 5' end. The number we put here removes the first #b from every read. Ex: p-trim-left-f 7 removes first 7 bases from every forward read. If we dont need to trim put 0.
--p-trim-left-r NUMBER \
--p-trunc-len-f NUMBER \ #these 2 lines cut every read at the input length and the rest are discarded from 3' end. Ex.p-trunc-len-f 240 keeps reads until 240 then drops the rest
--p-trunc-len-r NUMBER \
--o-representative-sequences cow_seqs_dada2.qza \ #output file w/ final ASVs after denoising. clean and ready to analyze
--o-denoising-stats cow_dada2_stats.qza \ #denoising stats. Show how many reads filtered, denoised, merged, non-chimeric, etc.
--o-table cow_table_dada2.qza #generates feature table w/ rows=ASVs, columns=samples, values=counts. 

#i=input (qza), p= parameter (instructions), o=output (qza or qzv), m=metadata
```

### 5b Visualize Denoised Tables
#### Denoising Stats
```r
##like quality control:
	#Visualize denoising stats
qiime metadata tabulate \
--m-input-file cow_dada2_stats.qza \ #input qza stats file that has stats from dada2 (ie. input reads, filtered reads, denoised reads, merged reads, non-chimeric reads)
--o-visualization YOUR_OUTPUT_FILENAME_HERE.qzv #converts metadata like file into visualizable .qzv file 
```
#### Denoised Table
```r	
	#Summarize feature table
qiime feature-table summarize \ #generates summary of ASV table (includes total seqs per sample, total ASVs, freq distibution, sampling depth suggestions)
--i-table cow_table_dada2.qza \ #feature table from dada2
--m-sample-metadata-file ../metadata/cow_metadata.txt \ #adds sample metadata to visualization (helps sort by tx group, check seq depth by category, see patterns)
--o-visualization YOUR_OUTPUT_FILENAME_HERE.qzv  #output file w/ ASVs
#gives mean/median and total reads per sample
```
#### Sequences Table
```r	
	#View the actual sequence
qiime feature-table tabulate-seqs \ #Creates table w/ ASV ID, DNA seq, length
--i-data cow_seqs_dada2.qza \ #representative seqs file (has unique denoised ASVs)
--o-visualization YOUR_OUTPUT_FILENAME_HERE.qzv #output, gives sequence length distribution in "sequence length histogram"
```
truncate at 250bp because bp#251 has a middle of the box quality score of 13, well below the recommendation of 30

#### DADA2 Info
- Filters low quality reads
- trims primers and adapters
- corrects sequencing errors
- removes chimeras
- can merge paired reads
- Produces ASVs (NOT OTUs)




