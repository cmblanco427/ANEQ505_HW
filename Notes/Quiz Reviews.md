
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

- Quiz 1
- Quiz 2
- Quiz 3
- Quiz 4
```

## Quiz 1

![[Quiz 1 review audio.m4a]]
- 12 bp primer attaached to barcode is for decomplexing
- Diff betwen multiplex and denoising. Demultiplexing is sorting seq read back to sample and assgn sample name. denoising is where we are cleaning up seq reads for errors and creating feature table that gives usa  count for sample and radseqs????
-  Consider 16s rRNA amplicon...
	1. No, becuase the mitochondria is now part of host genome, not part of a host community. its an endosymbiont thats evolved to be part of host genome
- Main reason well work in scratch directory?
	- Our scratch directory has 10tb of storage, but projects is alot less. Mostly for storage
- If unsure which directory youre in?
	- pwd= if unsure where you are
	- ls= helpful if you have an idea of what should be there in terms of files
- What type of phylo method were using?
	- Insertion tree. trees hard to build, so well match our reads to the main tree and insert things. Either exact matches or insert to most closely related one so we retain that robust phylogenetic tree. IF try to build denovo tree from short reads of 16s wont get good tree

## Quiz 2

![[Recording 20260227111257.m4a]]

1. Amplicon insertion tree
2. 3 file types
	1. feature table, representative seqs, stats
- samples most likely to have high alpha diversity
	- extreme temps and UV prolly kill alot of life, abx kills stuff, clean rooms kill everything
- High faiths phylogenetic
	- Bacteria and archea- 2 different domains of life, so as far apart as u can get, so high phylogenetic diversity in that community. If u have 10 types of bacteria but sample two and all in same order, wont cover as much branch length in TOL as sample 1
- A and B diversity
		- have both
- Dataset w/ following files from sequencing facility
	- forward, reverse, and barcodes
	- these are straight off sequencer, contains all forward, all reverse so you can demultiplex. need to import reads without a manifest, then demultiplex reads using barcode file. 
	- ONLY need manifest if youre given demultiplexed data by sequencing facilities. This is common, but not what were doing in this class. 
- Identical trimming/truncating parameters needed
	- HAve to have same truncation but not trimming. 
	- when u merge sequence data, the sequences have to be the same length. have to overlap 100% because if they dont, will get completely different ASVs for one run vs the other. 
	- Ex. in 1 run, get ASV1 (150bp), in run 2 its 148bp, these could match exactly but would be called different ASVs, so MUST be same length so you can get appropriate ASVs.
	- Matters less if you trim (3'), ALWAYS matters if you truncate (5'). Truncate changes length
	- For one proj where u have sequences for differnet runs, the trunk length needs to be the same, but all of your data in diff projects DOESNT need to match unless youre merging them.
	- If youre just using forward reads need to trim and truncate the same. 
## Quiz 3
- you overestimate alpha diversity because it thinks theres more ASVs than there are
- Birds bats and caterpillars dont really have a resident gut microbiome. Birds n bats have to be super light, so cant carry around the pound of extra microbes so dont show much of a phylogenetic signal, mostly transient microbes in their poop
- Age= one of biggest effects on human gut composition (ie. beta and alpha diversity)
- Non-parametric test- we do special differential abundance to determine if taxa is enriched or depleted in a group. Cant do if abundance has increased or if something else decrearsed that led to the increase, so need to look at ratios. So we dont do just a statistical test on relative abundance. DONT DO THIS. Use things like ancom bc or lefC, mazlan, so these use info about how ratios of taxas are changing. 
- Non-phylogenetic beta diversity metric: Bray Curtis
- Longitudinal data set with repeated measures, linear mixed effects models can be useful statistical approaches
- colostrum feeding Q - 
- PCoA - exploratory/qualitative. not a statistical approach
- TAxa bar plots of neg extraction controls- report taxa found on controls and use statistical contamination removal methods. 
- 
![[Recording 20260327110814.m4a]]

## Quiz 4
1. 4 main func of gut microbiome - digestion, immunity, gut health, neurochem signaling.
2.  Vagus nerve= primary neural connection between gut and brain. gut bacteria interact with it vai bio active molecules
3. Scientists can move past correlation and test causation by using gnotobiotic mice and including particular microbiomes to assess its effect on host phenotype - TRUE
4. A genetically mod strain of e.coli is best described as an example of - SYNTHETIC BIOLOGY
5. A synthetic microbial comm designed from a metabolic model to inc phos + micronutrient availability in an otherwise sterile sys is an ex of - Bott up microbiome engineering
6. what are 2 methods u can use to export a feature table from wiime2 for analysis in R
	1. unzip table qza in terminal, download it as a tsv from tableqzv as ur visualizeing files in qiimeview
7. What do these results tell us about shannon diveristy
	1. - results of this test suggest that ADD does not significantly affect shannon diversity over entire study. 
8. In the model lmer(shannon)entropy...etc) whats primary purpose of 1|host_subject_id
	1. to account for repeated measures and non ind among samples from same donor
9. Which scenario would benefit from LMA testing
	1. mice are sampled befor a tx, immediately after, and 24hrs later.
	2. sample mice samples in linear study, so repeated samples from same study.
10. If u want to ID interesting taxa correlated withan outcome, what is an analysis approach u could use
	1. ANCOMBC2, differential abundance testing
	2. 
11. Which would give u a higher alpha diveristy (richness metric) for an amplicon seq data set like 16s rRNA.
	1. processing your data for ASVs - basically keeping it at 100% similarity. ASVs are the most high resolution approach for IDentifying ASVs. 
![[Recording 20260410114213.m4a]]



