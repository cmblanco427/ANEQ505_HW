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


## Canceling jobs
```r
#after loading slurm module
scancel <jobid> #cancels this job
scancel --user=$USER #cancels all jobs
scancel --stae=PENDING --user=$USER #cancels pending jobs
scancel --name=<job_name> #cancel job by name

#Can also go to jobs in OnDemand and see whats running/cancel from there
```

