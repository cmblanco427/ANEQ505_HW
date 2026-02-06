Class Node 
<span style="color:rgb(255, 255, 0)"><span style="color:rgb(255, 255, 0)"><span style="color:rgb(255, 255, 0)"><span style="color:rgb(255, 255, 0)">sinteractive</span> <span style="color:rgb(255, 255, 0)">--reservation=aneq505 --time=01:00:00 --partition=amilan --nodes=1 --ntasks=2 --qos=normal</span></span></span></span>
# 1/23/26

1. Navigate to [ondemand-rmacc.rc.colorado.edu](http://ondemand-rmacc.rc.colorado.edu/ "(opens in a new window)")
2. Login through Colorado State University
3. Practice some codes
	1.<span style="color:rgb(255, 255, 0)"> <b>ls</b></span> = list files in current working directory. Can provide options for this
		1. <span style="color:rgb(255, 255, 0)"><b>ls-a</b></span>: saying to list _all_ of the files in your current directory, including any hidden files that start with a period (called "dot files")
		2.<span style="color:rgb(255, 255, 0)"> <b>-l</b></span>: list the directory contents in a long listing format
		2. <span style="color:rgb(255, 255, 0)"><span style="color:rgb(255, 255, 0)"><span style="color:rgb(255, 255, 0)"><span style="color:rgb(255, 255, 0)"><span style="color:rgb(255, 255, 0)"><b><span style="color:rgb(112, 48, 160)">-G</b></span></span></span></span></span></span>:  not print group names in a long listing
		3. Explore more options to supply to the ls command [here](https://man7.org/linux/man-pages/man1/ls.1.html "(opens in a new window)")
4. <span style="color:rgb(255, 255, 0)">pwd </span>= print working directory. 
5. 1. You should see an output that looks like /home/USER@colostate.edu. Remember the three different types of spaces (home, projects, scratch). We landed in the "home" space!
6. We almost never want to be in "home". Lets navigate to scratch.
7. We can do this in one command (change "USER" to your username)
`cd /scratch/alpine/USER@colostate.edu`, 
8. <span style="color:rgb(255, 255, 0)">cd </span>=change directory.
9. <span style="color:rgb(255, 255, 0)">cd ../ </span>= Move out of directory by one level

![[Pasted image 20260131161136.png]]








1/30/26