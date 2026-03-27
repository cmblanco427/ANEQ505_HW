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
## Quiz 5
