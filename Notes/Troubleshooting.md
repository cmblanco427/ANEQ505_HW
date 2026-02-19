```insta-toc
```
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

- Monitor real time resource usage
- Check CPU Usage of SLURM Job
- Check SLURM output file
- Canceling jobs
```
## Monitor real time resource usage
```r
top

#MUST BE ON COMPUTE NODE FOR THIS

q #quits when youre done
Shift + P # sort provessor or memory usage
```
Output
#####



## Check CPU Usage of SLURM Job
```r
top -u $USER

#or

htop
```

## Check SLURM output file

```r
cd /scratch/alpine/$USER/cow/slurm
ls -lh

#Look at your file
tail -50 slurm-24093944.out

```

## Canceling jobs
```r
#after loading slurm module
scancel <jobid> #cancels this job
scancel --user=$USER #cancels all jobs
scancel --stae=PENDING --user=$USER #cancels pending jobs
scancel --name=<job_name> #cancel job by name

#Can also go to jobs in OnDemand and see whats running/cancel from there
```

