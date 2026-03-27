~={red}(1point)=~ for Alpha Rarefaction Plot
Run Core Metrics ~={red}(1 point; .25pts per line)=~
Make alpha diversity plots ~={red}(3points)=~
~={red}10 points=~ for the questions 

~={red}15 points total=~
------------------------------------------------------------------

Due: 

**For complete credit for this assignment, you must answer all questions and include all commands in your obsidian upload.**

------------------------------------------------------------------
**Learning Objectives**
1. Practice recording commands and editing code to match your analysis.
2. Perform alpha rarefaction and determine an appropriate sequencing depth.
3. Run core metrics, generate plots for alpha and beta diversity
--------------------------------------------------

**Cow Site Data Workflow**, part 3

Load qiime2 in a terminal session after you go into the **cow** folder

```
ainteractive --ntasks=6 --time=02:00:00

# Insert the two commands to activate qiime2

module purge
module load qiime2/2024.10_amplicon
```

### Alpha Rarefaction Plot ~={red}(1 point)=~
- Chose the input sequencings depths (min and max) for generating the alpha rarefaction plot: 

```
#go to the cow directory
# plot alpha diversity of each sample to look at sequencing depth
qiime diversity alpha-rarefaction \
--i-table dada2/cow_table_dada2_filtered300.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization alpha_rarefaction_curves_16S.qzv \
--p-min-depth 10 \
--p-max-depth 33000
```
- look at table.qzv and find minimum reads and highest reads, and then round to the nearest 1000.
- 

![[Recording 20260327111705.m4a]]

### Run Core Metrics ~={red}(1 point)=~

```
qiime diversity core-metrics-phylogenetic \
--i-table dada2/cow_table_dada2_filtered300.qza \
--i-phylogeny tree/tree_gg2.qza \
--m-metadata-file metadata/cow_metadata.txt \
--p-sampling-depth 1500 \ #Switch to 50000
--output-dir core_metrics_results #make new core_metric result
```
<span style="color:rgb(238, 170, 170)">Alpha rarefaction plot- do on observed features- want to know number of ASVs in each sample so we can see if were missing any rare stuff. Sample metadata option should be something that selects every single sample individually. <br>- if u rarefaction level off is too low, will miss some of the ones that are increasing in alpha diversity. WAnt to follow traces, look globally and make sure all of samples are plateauing for sequences depth. Want to find a value where samples are leveling off but not loosing alot of samples<br>- If ur rarefaction is too high, will loose anything that doesnt have a sequencing depth at that high value.     
- SUPER SUBJECTIVE
- <br>- They accepted anything between 4000-6000</span>
<mark style="background: #FFF3A3A6;">- RERARIFY THIS to 4000-5000</mark>
- dont want alot of samples below 10,000
- When looking at table.qzv can lookat sampling depth and all of the sames you loose will show up in red
![[Recording 20260327112733.m4a]]

![[Recording 20260327113042.m4a]]



### Visualize alpha diversity plots
- generate a plot to visualize the observed features ~={red}(1 point)=~
```
qiime diversity alpha-group-significance \
--i-alpha-diversity core_metrics_results/observed_features_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results/observed_features_statistics.qzv
```



- generate a plot to visualize faith's PD ~={red}(2 points)=~
```
## insert the entire code chunk for generating this visualization 

qiime diversity alpha-group-significance \
--i-alpha-diversity core_metrics_results/faith_pd_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results/faiths_pd_statistics.qzv
```

![[Recording 20260327113123.m4a]]



## Homework questions ~={red}(10 points)=~

1. what is the name of the file you needed to use to figure out what min and max depths to use to generate the alpha rarefaction plot? (Hint: which file contains the sequencing depths for each sample)
-  cow_table_dada2_filtered300.qza

![[Recording 20260327113204.m4a]]

1. what did you choose for the rarefaction depth (the input for core metrics -p-sampling-depth flag)? why?
 - I chose 10,000 because theres where I started to see a drop off of sequences
 - <span style="color:rgb(238, 170, 170)">Should be 4-6000</span>

![[Recording 20260327113211.m4a]]

1. Which cow body location had more observed features? Which has the lowest?
- Based on the alpha diversity boxplots, it appears that fecal samples had the most observed features, while nasal had the lowest.
- look at observed features plot. 

![[Recording 20260327113251.m4a]]

1. What is the main difference between Faiths PD and Shannons alpha diversity metrics?  
- Faiths pd uses phylogenetic information and richness to determine alpha diversity, while shannons diversity uses richness and evenness, but does not include phylogeny. 

![[Recording 20260327113305.m4a]]

1. Which diversity metrics produced by the core-metrics pipeline require phylogenetic information?
- Faiths PD

![[Recording 20260327113513.m4a]]

1. Which two body sites have the highest Faiths PD alpha diversity?  Are the groups significantly different?
- Skin and fecal had the highest Faiths PD. These groups are statistically significant with a P-value of 0.000182.

![[Recording 20260327113329.m4a]]

![[Recording 20260327113406.m4a]]


1. Does it seem like there are any groupings in the beta diversity? What are the groupings? 
- It appears there are groupings by body site,  with nasal and oral and udder and skin. Fecal is also grouped tightly.

![[Recording 20260327113526.m4a]]

![[Recording 20260327113533.m4a]]


1. Why do you think these samples are grouping together? 
- The nasal and oral cavities are similiar and anatomically close together. The udder and skin are likely grouped because they are made of the same cellular matrix and the outer layer of the udder is skin. 

![[Recording 20260327113544.m4a]]

1. What test can you run to determine if the groups are significantly different?
	- We can test for beta group significance (Permanova)

![[Recording 20260327113610.m4a]]

1. What command would you use to run that test?
<span style="color:rgb(238, 170, 170)">should be beta group significance</span>
```
#insert command for running the test you suggest from question 7

qiime diversity alpha-correlation \
--i-alpha-diversity core-metrics-results/faith_pd_vector.qza \
--m-metadata-file metadata/metadata.txt \ #metadata file w/ numeric columns
--o-visualization core-metrics-results/faith_pd_correlation_statistics.qzv

```

### Questions
-use table vs table with mitochlondria/chloroplast filtered table.

![[Recording 20260327113817.m4a]]
- should probably use one thats filtered as best practice.