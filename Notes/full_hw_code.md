# HW1
## 1. Launch interactive session, load Qiime2 w/in cow directory

```r
#launch an interactive session: 
ainteractive --ntasks=6 --time=02:00:00

#activate qiime.
module purge  
module load qiime2/2024.10_amplicon
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
qiime demux emp-paired \
--m-barcodes-file ../metadata/cow_barcodes.txt \ #metadata
--m-barcodes-column barcode \--p-rev-comp-mapping-barcodes \ #metadata column
--p-rev-comp-barcodes \ #barcode sequence reads will be reverse complemented prior to demultiplexing????
--i-seqs ../cow_reads.qza \ #Artifact: paired end sequences to be demultiplexed from previously generated qza file???
--o-per-sample-sequences demux_cow.qza \ #output resulting demultiplexed sequences
--o-error-correction-details cow_demux_error.qza #generates details about barcode error corrections
```

## 4. Run script from slurm directory as a job
script runs from script file in slurm directory (generated in first step of demultiplexing by submitting a job step)
```r
dos2unix demux.sh #PC users; demux.sh is the file in slurm folder with our script
sbatch demux.sh
```

## 5. Denoise