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

- Homework1
    - 1. Launch interactive session, load Qiime2 w/in cow directory
    - 2. Import raw reads -> output as Qiime2 readable format   (qza)
    - 3. Demultiplex by submitting a job
        - 3.a make demux.sh file in slurm directory to demultiplex sequences quicker. Add following shebang followed by batch commands to the file
        - 3.b Demultiplex
        - 3.C Visualize demultiplexed read quality
        - 3.D Full code that goes into slurm /sh script
    - 4. Run script from slurm directory as a job
    - 5. Denoise
        - 5a Denoise Script
        - 5b Visualize Denoised Tables
            - Denoising Stats
            - Denoised Table (Feature table)
            - Sequences Table
    - DADA2 Info
- Homework 2: Practice Qiime2 Workflow
    - Remove long (300+ base pair) amplicons from the representative sequences file and the feature table
    - Classify taxonomy using GreenGenes2
    - Filtered Taxa Bar Plot Questions red(10 points)
    - Phylogenetic tree red(1 point)
        - Once this job finishes, copy and paste what the slurm email says here red(1 point):
            - for example:
```
# Homework_1
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
--type EMPPairedEndSequences \ #paired sequences
--input-path raw_reads \
--output-path cow_reads.qza
```
## 3. Demultiplex by submitting a job
### 3.a make demux.sh file in slurm directory to demultiplex sequences quicker. Add following shebang followed by batch commands to the file
-  lets u send data to alpine to work -> job. Takes a long time, so best to run as a job and not in the terminal
- DONT need to put into command line
- 
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
--m-barcodes-column barcode \ #matches name of column
--p-rev-comp-mapping-barcodes \ #metadata column
--p-rev-comp-barcodes \ #barcode sequence reads will be reverse complemented prior to demultiplexing???? During seq can seq barcodes, wnat to get reverse complement of barcode. If we go R->L in F +R in primers, if we seq thru direction, need reverse complement so we can remove it later. To get rid of sequencing artifacts so no primers in seq files. 
--i-seqs ../cow_reads.qza \ #Artifact: paired end sequences to be demultiplexed from previously generated qza file??? F+ R reads
--o-per-sample-sequences demux_cow.qza \ #output resulting demultiplexed sequences
--o-error-correction-details cow_demux_error.qza #generates details about barcode error corrections
```

### 3.C Visualize demultiplexed read quality
```r
qiime demux summarize \
--i-data demux_cow.qza \
--o-visualization demux_cow.qzv #Shows Q scores so you know where to trim and/or truncate. Also shows read length
```

### 3.D Full code that goes into slurm /sh script
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

#What needs to go here in order to “turn on” qiime2? 
module purge  
module load qiime2/2024.10_amplicon

#change the following line if your file path looks different
cd /scratch/alpine/$USER/cow/demux

#Below is the command you will run to demultiplex the samples.

qiime demux emp-paired \
--m-barcodes-file ../metadata/cow_barcodes.txt \
--m-barcodes-column barcode \--p-rev-comp-mapping-barcodes \
--p-rev-comp-barcodes \
--i-seqs ../cow_reads.qza \
--o-per-sample-sequences demux_cow.qza \
--o-error-correction-details cow_demux_error.qza

#visualize the read quality
qiime demux summarize \
--i-data demux_cow.qza \
--o-visualization demux_cow.qzv
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
### 5a Denoise Script
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

![[Recording 20260220113310.m4a]]

### 5b Visualize Denoised Tables
#### Denoising Stats
```r
##like quality control:
	#Visualize denoising stats
qiime metadata tabulate \
--m-input-file cow_dada2_stats.qza \ #input qza stats file that has stats from dada2 (ie. input reads, filtered reads, denoised reads, merged reads, non-chimeric reads)
--o-visualization YOUR_OUTPUT_FILENAME_HERE.qzv #converts metadata like file into visualizable .qzv file Shows reads retained vs lost per sample (% of input non-chimeric reads). (EC files are controls, ignore em)
```

- **Read Processing Performance & read count removal stats for each sample**
- **Contains**: Reads input, Reads filtered, Reads denoised, Reads merged, Reads non-chimeric, Percent retained
- **Answers**: % reads lost per sample, how many read survive filtering, how efficient was merging, retention of chimera removal?
#### Denoised Table (Feature table)
```r	
	#Summarize feature table
qiime feature-table summarize \ #generates summary of ASV table (includes total seqs per sample, total ASVs, frequency distibution, sampling depth suggestions)
--i-table cow_table_dada2.qza \ #feature table from dada2
--m-sample-metadata-file ../metadata/cow_metadata.txt \ #adds sample metadata to visualization (helps sort by tx group, check seq depth by category, see patterns)
--o-visualization YOUR_OUTPUT_FILENAME_HERE.qzv  #output file w/ ASVs
#gives mean/median and total reads per sample (frequency= number of reads, mean freq= avg # of reads per sample)
```
- Table for which each sample includes relative abundance of features
- **Contains**: Total/ mean/median, Min/max reads per sample, Total number of ASVs
- **Answers**: mean reads per sample, total number of reads, how many ASVs detected
#### Sequences Table
```r	
	#View the actual sequence
qiime feature-table tabulate-seqs \ #Creates table w/ ASV ID, DNA seq, length
--i-data cow_seqs_dada2.qza \ #representative seqs file (has unique denoised ASVs)
--o-visualization YOUR_OUTPUT_FILENAME_HERE.qzv #output, gives sequence length distribution in "sequence length histogram", max length of sequences
```
- **Contains**: actual ASV DNA sequences, feature IDs linked to each sequence
- **Answers**: How long are final ASVs, max sequence length, what do sequences look like?



truncate at 250bp because bp#251 has a middle of the box quality score of 13, well below the recommendation of 30
Briefly **describe** the key information from each denoising output file:
1. Representative Sequences
	1. This table has the ASV feature ID and gives you their corresponding sequence
2. Denoising Stats
	1. This table helps us with quality control to  determine if any samples have a large portion of removed reads. Generated data includes number of reads per sample, number and percent of reads that passed filtering, number of reads kept after denoising, number of reads merged if using paired end data, number and percent of non-chimeric reads. 
3. Denoised Table
	1. The feature table contains ASVs and when visualized will show an overview of summary statistics. It includes a table summary with the number of samples, number of ASVs, and total feature frequency. It also includes the frequency per sample that shows the frequency of features observed within a specific sample and frequency per feature that shows statistics on how often an ASV observed. 

**Answer the following questions:**  

1. **Where does the median Q-score begin to dip below Q30 for the forward reads and the reverse reads?
	1. For forward reads, none showed a median Q score below 30. For the reverse reads, read 251 (the last one) showed a median Q score of 13, the rest were at or above 30. If have a low QS in the middle of reads, dont want to cut them out cuz then cant merge things back together. If theyre super low call the sequencing company to see if something happened

2. **What is the mean reads per sample?
	1. 11,115.7 (From cow_table_dada2_)
	
3. **How long are the reads?
	1. 251 base pairs (from: cow_seqs_dada2) If max length is alot longer than the min length, need to check the file to make sure non-target DNA wasnt targeted. Check in BLAST (ailgn sequences in the amplicon to a database). Can add specific strings into the SBATCH file to be excluded --p exclude, can also add --p include to include things in and below a certain class level.
	
4. **What is the maximum length of all your sequences?
	1. The maximum length was 427 nucleotides
	
5. **Which sample (not including extraction controls starting with EC) lost the highest % of reads?
	1. Sample 2019.3.14.cow.oral.20 has an 8.76% of input non-chimeric, indicating it lost 91.24% of associated reads. ( could be that it was low biomass, didnt extract well, )
	
6. **Why did you chose to trim or truncate where you did?
	1. I chose not to trim any reads as all had a median Q score >30. However I truncated the reverse reads at read 250 because read 251 had a median Q score of 13.

## DADA2 Info
- Filters low quality reads
- trims primers and adapters
- corrects sequencing errors
- removes chimeras
- can merge paired reads
- Produces ASVs (NOT OTUs)


---------------------------------------
----------------------------------
-----------------------------------

# __
# Homework #2: Practice Qiime2 Workflow**

Due: March 5th 2026 at midnight

**For complete credit for this assignment, you must answer all questions and include all commands in your obsidian upload.**

------------------------------------------------------------------
**Learning Objectives**
1. Practice independently running a partial Qiime2 workflow on new data using both the command line and slurm to submit jobs. 
2. Practice recording commands and editing code to match your analysis.
3. Perform taxonomic assignments using greengenes2, filtering out unwanted taxonomy and large amplicons, run another job script for generating a SEPP tree.
--------------------------------------------------

**Cow Site Data Workflow**, part 2

Load qiime2 in a terminal session after you go into the taxonomy folder 

```
# Insert the two commands to activate qiime2

module purge
module load qiime2/2024.10_amplicon

```

### Remove long (300+ base pair) amplicons from the representative sequences file and the feature table
```r
# filter out any large amplicons from the seqs and table (because they are contaminates)

cd /scratch/alpine/$USER/cow/dada2

#Filter feature table
qiime feature-table filter-seqs \
--i-data cow_seqs_dada2.qza \
--m-metadata-file cow_seqs_dada2.qza \
#filter out seq less than 300 bp to filter out 18s
--p-where 'length(sequence) < 300' \
--o-filtered-data cow_seqs_dada2_filtered300.qza

qiime feature-table tabulate-seqs \
--i-data cow_seqs_dada2_filtered300.qza \
--o-visualization cow_seqs_dada2_filtered300.qzv

qiime feature-table filter-features \
--i-table cow_table_dada2.qza \
--m-metadata-file cow_seqs_dada2_filtered300.qza \
--o-filtered-table cow_table_dada2_filtered300.qza

#visualize, double check all amplicons are gone
qiime feature-table summarize \
--i-table cow_table_dada2_filtered300.qza \
--m-sample-metadata-file ../metadata/cow_metadata.txt \
--o-visualization cow_table_dada2_filtered300.qzv
```


### Classify taxonomy using GreenGenes2

First get the Greengenes2 database:

```
cd /scratch/alpine/$USER/cow/taxonomy
```

```
wget --no-check-certificate https://ftp.microbio.me/greengenes_release/2024.09/2024.09.backbone.v4.nb.qza
```

Classify taxonomy using GreenGenes2 classify the ASVs (takes about 5 mins). ~={red}(1point)=~
```
qiime feature-classifier classify-sklearn \--i-reads ../dada2/cow_seqs_dada2_filtered300.qza \--i-classifier NAME OF CLASSIFIER HERE.qza \--o-classification taxonomy_gg2_filtered.qza
```

Visualize the taxonomy of your ASVs: (~={red}1point)=~
```
qiime metadata tabulate \--m-input-file NAME OF TAXONOMY FILE.qza \--o-visualization taxonomy_gg2_filtered.qzv
```

- Filter mitochondria and chloroplast out to generate a filtered feature table, keep only ASVs with a class or lower taxonomy. fill in the blank (--p-exclude) to exclude these DNA. Fill in the blank to include only class level or below classifications. ~={red}(1point)=~
```
qiime taxa filter-table \--i-table ../dada2/<YourDenoisedTable.qza> \--i-taxonomy taxonomy_gg2.qza \--p-exclude WHAT TO EXCLUDE HERE \--p-include WHAT TO INCLUDE HERE \--o-filtered-table ../dada2/table_nomitochloro_gg2_filtered300.qza
```

- Visualize the taxa bar plot
```
qiime taxa barplot \--i-table ../dada2/table_nomitochloro_gg2_filtered300.qza \--i-taxonomy taxonomy_gg2_filtered.qza \--m-metadata-file ../metadata/cow_metadata.txt \--o-visualization ../taxaplots/taxa_barplot_nomitochloro_gg2_filtered300.qzv
```

## Filtered Taxa Bar Plot Questions ~={red}(10 points)=~

**Question 1**: Attach a picture of your taxa bar plot, organized by cow sampling location (body_site) at the level 7 taxonomic level. What general trends do you notice? 

**_Question 2**: What are the top 2 most abundant bacterial **classes** in the fecal samples? 

**_Question 3**: What highly abundant ASV is shared between both the udder and skin samples?

**_Question 4**: Which samples (still sorted by body_site) have higher alpha diversity in terms of observed features?

**Question 5**: do all samples contain archaea as well?

**Question 6**: why do we filter out sp004296775?

**Question 7**: what is the difference between these two flags? 
--p-exclude mitochondria,chloroplast,sp004296775 \
--p-include c__ \

**Question 8**: do the positive controls look the same as each other? Yes or No?

**Question 9**: Do the negative/extraction controls (Samples labeled as EC), look like the positive controls? Yes or no? 

**Question 10**: do the negative/extraction controls (Samples labeled as EC), look like the real samples? Yes or no?

## Phylogenetic tree ~={red}(1 point)=~
Create a job script to run the phylogenetic tree building. Remember you must start a new terminal session, navigate to your slurm directory, and then submit the job. You do NOT need to start any other interactive sessions.This job will take about an hour. 

Go to OnDemand and create a new text file for your job script
```
nano <YourJobName.sh>
```

```
#!/bin/bash
#SBATCH --job-name=tree
#SBATCH --nodes=1
#SBATCH --ntasks=8
#SBATCH --partition=amilan
#SBATCH --time=04:00:00
#SBATCH --mail-type=ALL
#SBATCH --mail-user=YOUR_EMAIL_HERE@colostate.edu
#SBATCH --output=slurm-%j.out
#SBATCH --qos=normal

#Activate qiime
#Insert the two commands you need to load qiime2


#Get reference
wget --no-check-certificate -P ../tree https://ftp.microbio.me/greengenes_release/2022.10/2022.10.backbone.sepp-reference.qza


#Command
qiime fragment-insertion sepp \--i-representative-sequences ../dada2/Your_FILTERED_RepresentativeSequencesFile.qza \--i-reference-database ../tree/2022.10.backbone.sepp-reference.qza \--o-tree ../tree/tree_gg2.qza \--o-placements ../tree/tree_placements_gg2.qza
```

- submit the job from the terminal
```
#submit the job
dos2unix YourJobName.sh
sbatch YourJobName.sh
```
We will use this file in the next homework!

### Once this job finishes, copy and paste what the slurm email says here ~={red}(1 point)=~: 

#### for example: 
Job ID: 24289371  
Cluster: alpine  
User/Group: [lindsval@colostate.edu](mailto:lindsval@colostate.edu)/[lindsvalpgrp@colostate.edu](mailto:lindsvalpgrp@colostate.edu)  
State: TIMEOUT (exit code 0)  
Nodes: 1  
Cores per node: 8  
CPU Utilized: 03:57:54  
CPU Efficiency: 12.37% of 1-08:03:52 core-walltime  
Job Wall-clock time: 04:00:29  
Memory Utilized: 6.55 GB  
Memory Efficiency: 21.83% of 30.00 GB (3.75 GB/core)