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

- 1.23.26- Intro to command line, linux, Alpine, and Obsidian
    - More About Nodes
    - Logging into Alpine using OnDemand
    - File Transfers
    - Linux Navigation
        - More Terminology!
            - 1. Git & Version Control
            - 2. Obsidian & Note Management
        - Install and use Obsidian
        - Normal Workflow
- 1.30.26 - Intro to QIIME2 & Decomposition Tutorial
- QIIME2
    - QIIME2 Resources
    - Core Concepts
    - Q2view
    - Semantic Types
    - Plugins
    - How commands/methods work in QIIME2:
    - Additional Educational Resources
        - Human Decomposition Tutorial Dataset
        - Let's Begin!
    - Metadata
- 2.6.26 - Demultiplexing
    - Importing Data
    - Demultiplexing
        - Phred Quality Scores
        - Let's get started!
        - Create directories
            - Create a directory for the manifest and import the manifest files
            - Copy the demultiplex reads into your analysis directory:
            - Import data into QIIME2:
        - Check Data Quality
        - Binned Quality Scores
            - Explanation of Interactive Quality Plots
        - Denoising sequences to ASVs using DADA2
- 2.13.26 - Denoising & Intro to HW
    - Denoising sequences to ASVs using DADA2
    - Important notes about denoising:
    - DADA 2
        - 1. DADA2 STATS FILE:
        - 2. DADA2 TABLE FILE (AKA Denoised Table, Feature Table):
        - 3. DADA2 SEQS FILE:
        - Running jobs on Alpine
        - Summary
            - Cow Dataset for Homework:
-  Week 5 Tutorial: Taxonomy, Taxa Barplots, Filtering, Phylogenetic Tree
    - Taxonomic Classification
    - Taxa Barplots
    - Phylogenetic Trees
    - Summary
- Week 6: Alpha Rarefaction, Core Metrics, Alpha Diversity Plots
    - 1. Long Amplicons
    - Remove amplicons larger than 300bp
        - 1. Filter sequences by length
        - 2. Visualize filtered sequences
        - 3. Filter the feature table & remove corresponding ASVs
        - 4. Summarize filtered feature table
        - 2. Using taxonomic classifiers
        - 3. How to know if a taxonomic classifier is incompatible with your version of qiime2?
        - 4. Back to the decomp tutorial!
            - 1. Remove unwanted taxa from feature table
            - 2. Generate Taxa barplot
    - Alpha Rarefaction, Core Metrics, Alpha Diversity Plots
        - Alpha Rarefaction
            - Discussing table.qzv
            - 1. Remove controls
            - 2. Create Alpha rarefaction directory
            - 3. Run Alpha Rarefaction
        - Alpha Diversity Review
        - Running the Diversity Pipeline (Core-Metrics) to Generate Alpha and Beta Diversity
        - Run Core-Metrics
            - Calculate standard alpha and beta diversity using phylogenetic tree
        - Core-metrics-phylogenetic Pipeline
            - Automatically computes:
            - Outputs generated
    - Alpha Diversity Files
        - Alpha group significance[Links to an external site.](https://docs.qiime2.org/jupyterbooks/cancer-microbiome-intervention-tutorial/030-tutorial-downstream/060-alpha-diversity.htmlalpha-group-significance "Permalink to this headline (opens in a new window)")
            - Statistical testing on alpha diversity metrics
            - 1. Alpha group significance (categorical comparisons)
            - 2. Faiths Pd- Phylogenetic comparisons
            - 3. Alpha Correlation (Continuous vars)
        - Conceptual Differences
        - What You Are Testing Biologically
            - Observed Features:
            - Shannon:
            - Faith PD:
            - Correlation:
            - Note about VS Code and Obsidian
- Week 7 Tutorial: Beta Diversity, Longitudinal Analysis, Exporting Qiime2 Data
    - Beta diversity metrics, PCoA plots, and significance testing
    - Distance Matrices:
    - Beta Diversity Files
        - Unweighted UniFrac:
        - Weighted Unifrac:
        - Jaccard:
        - Bray Curtis:
    - Testing for Significance
    - Beta diversity stats (part 1)
        - Running a PERMANOVA via the beta group significance test command in qiime2
    - Longitudinal Analysis with Qiime2
        - When is it time to move your longitudinal analysis outside of Qiime2?
    - Exporting Qiime2 data
        - Homework 3
```
```insta-toc
```

___
# **1.23.26- Intro to command line, linux, Alpine, and Obsidian**
___

**Introduction to Alpine**

**What is Alpine?** 
- The University of Colorado's High Performance Cluster (HPC) - they named it Alpine. 
- Alpine is a supercomputer that is located at CU Boulder and CSU has access to using.
- Here are the docs on Alpine: [https://curc.readthedocs.io/en/latest/clusters/alpine/index.htmlLinks to an external site.](https://curc.readthedocs.io/en/latest/clusters/alpine/index.html "(opens in a new window)")
- switch-> login node -> users submitting jobs
- They also have a help desk: [rc-help@colorado.edu](mailto:rc-help@colorado.edu "(opens in a new window)") 
- They host tutorial and workshops regularly: https://curc.readthedocs.io/en/latest/getting_started/current-sem-trainings.html# 

**Basic System Architecture of Alpine**

When you are trying to visualize what a supercomputer looks like, you can picture something like this:

- **compute nodes (top)** are the worker bees, where computing gets done
- the **switch** connects all the compute nodes together so they can communicate
- the **login node** is the "front door" to the supercomputer 
- the internet connects us to the supercomputer 

![[Pasted image 20260214171847.png|400]]

### **More About Nodes** 
Alpine has different kinds of nodes. For the purpose of this class, y**ou can think of a node as different computer modes** where you are given **different types of compute resources and are meant for running different tasks.** You can access these nodes in several ways. Alpine has 4 different types:

- **1. Login node - THE FRONT DOOR** 
    - This is where you "land" when you login
    - Do not run commands while in login
    - **Primarily used for script editing and job submissions only** , just quick stuff
- **2. Compile node**
    - Translates human-readable source code that we give into computer-executable machine code
    - **Primarily used to install software**
    - Use `acompile` to start a compile session  
-  **3. Compute node**
    - We do not navigate into these
    - **This is where jobs run after submission**
	    - in the background
    - There are different kinds of compute nodes
    - Here is a list of the compute node resources available on Alpine ([link to more detailed info hereLinks to an external site.](https://curc.readthedocs.io/en/latest/clusters/alpine/alpine-hardware.html "(opens in a new window)") )
    - Use `sbatch` to submit jobs  
- **4. Interactive node**
    - **We can navigate here to run jobs and commands interactively** 
    - We will use an interactive node to run our Qiime2 commands
    - Use `ainteractive` to start an interactive session
    - More computation resources to run jobs
    - **We will primarily use this node!!**

### **Node Types and Specifications<!-- omit -->**
- **AMD Milan** **compute nodes** (nodes are like individual computers) and each node has a certain amount of cores (or power) with a certain amount of RAM (memory)
    - these are the **worker bees,** where computing/calculations are run. 
    - 64 cores / node
    - ~240 GB ram / node
    - An average laptop has around 4 cores & 8-16 GB ram, so each node is about 16X more powerful than your average laptop
    - **this is what we will primarily use: name = amilan**
- High memory AMD Milan compute nodes 
    - 48 cores / node
    - ~1 TB ram / node (😱)  
    - **name = amem**
- GPU nodes - name = ami100 and aa100

![[Pasted image 20260214171902.png]]
- Partitions ar elike worker bees, 1 node= 3.75, so super powerful
**Directories (fancy word for folders)**

There are also different **storage spaces** within Alpine that are meant for different things. 

- **Home directory**
    - /home/$USER
    - This is where you "land" when you first login
    - Not for direct computation
    - Small quota (2 GB) for storage
    - Backed up  
- **Scratch directory**
    - /scratch/alpine/$USER
    - 10 TB quota - can ask for more if needed
    - High performance storage for computation
    - Files are purged after 90 days of inactivity - **Be careful about this!!! files are deleted and are not recoverable. you have to be diligent about saving your files from Alpine when you are done.** 
    - Folders will persist, so it will fool you into thinking your files are safe.    
    - Where we mostly work. If done w/ proj, need to download all files or put into proj directory, folders remain, all files delete
- **Project directory**
    - /projects/$USER
    - Mid-level quota (250 GB)
    - Large file storage
    - Not high performance storage
    - Backed up - if you choose to use Alpine for long-term storage, this is where you keep your files

Think of these spaces like folders on a computer, where "home" is a folder within "/" (root directory), and "$USER" is a folder within "home", and so on.

 ![[Pasted image 20260214171915.png|200]]
For this class, we will **work in the scratch directory** and back our files up to the projects directory. again this looks like: 

- /scratch/alpine/$USER

Alpine documentation: [https://curc.readthedocs.io/en/latest/Links to an external site.](https://curc.readthedocs.io/en/latest/index.html "(opens in a new window)") (can be really helpful!)

---
## **Logging into Alpine using OnDemand**

We will use the On Demand website to launch a terminal window that will connect us to the Alpine cluster.

1. Navigate to [ondemand-rmacc.rc.colorado.edu. Links to an external site.](http://ondemand-rmacc.rc.colorado.edu/ "(opens in a new window)")You should see this webpage:  
    ![[Pasted image 20260214171946.png]]
      
2. **Change ORCID to Colorado State University and click Log On.**  
3. You will next be prompted to login with your NetID.  
4. You will then be prompted to use Duo to authenticate.   
5. You should see the CU Research Computing OnDemand landing page. Once you log in, save this page as a shortcut or bookmark. we will use this page a lot, so easy access to it will be helpful.   
    
    There are 3 paths under the "Files" tab, these are the same that we talked about above (you have a home, scratch, and projects folder).  Remember, you will almost never use the "home" path, and for this class you won't use the "pl" path either. We will be working in the SCRATCH directory only. 

1. Launch a terminal session by clicking on Clusters at the top and then select >_Alpine. This should open a new window that looks like this:
You have successfully logged into Alpine and opened a terminal session!

---
## **File Transfers**

When doing your microbiome analyses on Alpine, you will need a way to transfer files from your local computer to Alpine and vice versa. While there are several ways of doing this, our preferred way is to use OnDemand.  

**OnDemand**
1. After logging in, click on Files. You will see paths to your Home, Scratch, and Projects directories. Click on one!  
2. This will show you all your folders and files in that directory.   
3. From here you'll be able to:
    - Download files
    - Upload files
    - Rename files
    - Delete files
    - View contents of a file
    - Open a terminal window from that location

**Lets practice!**

1. Using OnDemand, create a new folder in your scratch directory, call it "test"
2. Open the directory, and create a new file, call it "test_file.txt". 

---

## **Linux Navigation**

Now we will go over general commands to use on the command line. These will work on MacOS and Linux systems, but not necessarily PCs. Here is the "anatomy" of what a basic command might look like. It is important to know a few basic commands in linux so you can easily navigate the terminal as well as OnDemand. 

![[Pasted image 20260214172033.png|300]]
- ls= list files
- a= all files
- laG= list all files (G)

- First, you have the **command** itself. `ls` is a command that says to list the files in your current working directory, which you can think of as the "folder" you are currently in. 
- You can supply **options** to the command.
    - For example, `ls -a` is saying to list _all_ of the files in your current directory, including any hidden files that start with a period (called "dot files") 
    - The `-l` option says to list the directory contents in a long listing format
    - The `-G` option says to not print group names in a long listing
    - Explore more options to supply to the ls command [hereLinks to an external site.](https://man7.org/linux/man-pages/man1/ls.1.html "(opens in a new window)")
- You can supply **arguments** to the command.
    - The last line of the graphic above is saying to list all hidden files in a long listing format in the Downloads directory.

These are some common commands that you should learn, as we will use them throughout the semester:

![[Pasted image 20260214172043.png]]

**Now let's explore.**

1. Use OnDemand and open up a terminal session, if it is not still open from the first one we opened.
    1. We have just logged into Alpine, so where are we?
    2. Let's use `pwd`, for "print working directory". The "pwd"  in different font below is our first example of a "code chunk", which are things that you should make sure you run along with me.

`pwd`

1. You should see an output that looks like /home/USER@colostate.edu. Remember the three different types of spaces (home, projects, scratch). We landed in the "home" space!
2. We almost never want to be in "home". Lets navigate to scratch.
3. We can do this in one command (change "USER" to your username)

`cd /scratch/alpine/USER@colostate.edu`
We did this using cd, or "change directory.

example: mine looks like: `cd /scratch/alpine/lindsval@colostate.edu`

1. To see what files and folders are in our scratch directory, use the "list" command. you should see the folder you created earlier. 

`ls`

1. Now let's navigate to the directory we created earlier.

`cd test`

This is a good time to talk about **relative vs absolute paths**.

- An **absolute** path is the path directly from the root, as shown above when we used `cd /scratch/alpine/USER@colostate.edu`
- A **relative** path is a path based on where you are now. For example, if I am in the "`/scratch/alpine/USER@colostate.edu`" directory, the path to get to the "test" directory would be:
    - cd test
    - to move in and out of directories, you can use  `cd ../`to move out of the directory by one level

---
**Keeping track of commands and submitting homework with Obsidian**
Before we start, we should become familiar with tracking our command history. Keeping a record of your commands is important for several reasons:

1. Helps you remember what you did six months from now
2. Ensures your research will be reproducible to others
3. Great reference for future data analysis projects
4. Often a required part of manuscript submission

The easiest way to track commands is to copy and paste them into a text editor file. It's also helpful to include a comment above each command describing what you're doing. For this class we are going to use Obsidian, a program that will act as a text editor but it is connected to github which will allow you to save your homework and projects in one place.  

**Questions**: 
1. does anyone already use obsidian? 
2. who already has a github account? 
3. who has a PC versus a Mac? 

If you have a PC, the instructions are in the word doc called "[Obsidian windows setup.docx](https://colostate.instructure.com/courses/220471/modules/items/7682908 "Obsidian windows setup.docx")" under module 1.[Obsidian windows setup.docx](https://colostate.instructure.com/courses/220471/modules/items/7682908 "Link")
### **More Terminology!**
#### **1. Git & Version Control**
- **Git** – A version control system that tracks changes to files and allows collaboration.
- **Repository (repo)** – A folder where Git stores the history of your project.
- **Commit** – A snapshot of your project at a specific point in time.
- **Branch** – A parallel version of your project; `main` is the default branch.
- **Remote** – A version of your repository hosted online (e.g., on GitHub).
- **Origin** – The default name for a remote repository.
- **Push** – Sending your local commits to a remote repository.
- **Pull** – Fetching changes from the remote repository to your local repository.
- **GitHub** – An online platform for hosting Git repositories.
- **Access Token** – A string of letters/numbers used as a password to authenticate with GitHub
#### **2. Obsidian & Note Management**
- **Obsidian** – A note-taking app that organizes files into a “vault.”
- **Vault** – A folder that Obsidian manages, containing all your notes.
- **Note** – An individual file inside a vault.
- **Community Plugin** – Add-ons for Obsidian that extend functionality.
- **Obsidian Git Plugin** – A plugin that integrates Git with Obsidian for version control.
### **Install and use Obsidian**
**### Step 1: Install Git**
- Install Git if you don't already have it.
[https://git-scm.com/install/Links to an external site.](https://git-scm.com/install/ "(opens in a new window)")
- Git for windows
https://gitforwindows.org/
- To check if you have Git already installed enter this into the command line.  
`git --version`
**### Step 2: Install Obsidian**
- Obsidian Installation:
https://obsidian.md/download
**### Step 3: Set up Obsidian Vault (aka an obsidian folder)**
- Open Obsidian, it will give you the option to create new vault or open folder as vault
- Make a new vault, name it ANEQ505_HW and add it to a location on your computer - probably whatever folder you create on your computer for all class things... Just make sure that the folder doesn't back up to OneDrive or Dropbox. 
- Delete the welcome note in the vault
**### Step 4: Set up GitHub repository**
- Login to [githubLinks to an external site.](https://github.com/ "(opens in a new window)") or create a github account and make a new repository (Click the little cat on the upper left hand corner and click the green button that says new).
- You may have to set up 2 factor authentication for GitHub first in order to log in. 
- Name the repository ANEQ505_HW
- Make sure the visibility is set to public so we can see your homework.
- Make sure add readme is off. 
**### Step 5: Set up access token in GitHub.** 
- Click your profile picture
- Go to settings
- Scroll all the way down and on the left had side click developer settings.
- Click personal access tokens
- Click tokens (classic)
- Click generate new token
- Then generate new token (classic)
- You can put in ANEQ505 for note
- Set expiration to No expiration
- Select repo
- Scroll to bottom and generate token
- Copy the long string of letters and numbers and store it somewhere for later
**### Step 6: Initialize Git in the vault**
- Go to the directory with your vault (aka folder) in the command line/terminal. 
- In macs you can go to the file then right click, click services, then open terminal at file

<span style="color:rgb(0, 112, 192)">**Enter the following commands in your local computers terminal (NOT alpine)</span>
Open PowerShell in your vault folder
4. Open File Explorer
5. Go to: Desktop → ANEQ505_HW
6. Click the address bar, type: powershell
7. Press Enter
8. Initialize Git and prepare the repo
	1. Run the following commands:
		1. git init
		2. git config --global core.editor "notepad"
			<span style="color:rgb(0, 112, 192)"><b>Create a .gitignore file:</b></span>
		notepad .gitignore
		<span style="color:rgb(0, 112, 192)"><b>Paste and save:</b></span>
		.obsidian/workspace*
		.obsidian/cache
10. 5. First commit and push
	1. git add -A
		git commit -m "Initial commit"
11. If prompted for name/email
	1. git config --global user.name "Your Name"
		git config --global user.email "your_github_email@example.com"
		git commit -m "Initial commit"
12. Rename branch and connect to GitHub:
	1. git branch -M main
	git remote add origin https://github.com/YOUR_USERNAME/ANEQ505_HW.git
13. Push
	1. git push --set-upstream origin main
			If prompted, sign in using your browser.
### Normal Workflow
Editing and submitting work
1. Create or edit notes in Obsidian
2. In PowerShell, inside the vault folder, run:
		git add -A
		git commit -m "Update notes"
		git push
3. OPTIONAL: PUSH FROM INSIDE OBSIDIAN
	1. Obsidian Settings → Community plugins
	2. Install and enable “Obsidian Git”
	3. Press Ctrl + P and run:
			Obsidian Git: Commit all changes
			Obsidian Git: Push

IMPORTANT NOTES
• Obsidian notes are plain .md files
• File names in Obsidian are the same on GitHub
• Use PowerShell or Git Bash on Windows
• Do not use WSL
• Do not place the vault in Dropbox or OneDrive
• Workspace layout files are ignored to prevent conflicts

TROUBLESHOOTING
If Git says “Please tell me who you are”:
git config --global user.name "Your Name"
git config --global user.email "your_github_email@example.com"
If Git says “no upstream branch”:
git push --set-upstream origin main

FINAL CHECK
If this says your branch is up to date, everything is working
		-git status

**### Step 7: Set up Git in your obsidian vault**
- Go to settings in your obsidian vault 
- Click community plugins, and click turn on
- Then click browse
- Then search for Git
- Select Git and then Install
- Then click enable. 
- Go to the obsidian git settings by clicking options
- make sure that Custom base path is blank
- If you don't see custom base path in your settings then it is already blank
- Restart obsidian

**### Step 8: Push and Commit in obsidian**
- Reopen your vault and add a new note (pencil and paper icon) and make a change to the note. 
- Then open the command pallet (command/control + p)
- Search for "Git: commit" and select it
- In the command palette again search for Git: push and select it. 
- Go to the obsidian git settings and select a time for automatic push/pull from git hub so you don't have to manually do it. 
    - Click the settings button
    - Go to community plug-ins
    - Scroll down to Git under installed plugins and click it
    - Click options
    - Under Auto commit-and sync intervals (minutes) put in the time you want between automatic commits I have mine at 15 mins. 
    - For the Auto pull interval (minutes) choose how often you want to pull changes. I have mine at 15 mins. 

**### Step 9: Check your GitHub online to see if the files were committed.** 
- You should see the file that you edited and an .obsidian file
- The .obsidian file has all the settings and community plugins so that If you or someone else downloads the vault they have those settings built in. 

**### Step 10: Create folders for class organization.** 
1. 01_homework
2. 02_class_project
3. 03_paper_discussions
4. 04_extra_credit

**#### Some Notes**

For bigger pushes 
- This includes a lot of figures or if you haven't pushed in a while 
- Use this in the Terminal from anywhere to Ingrease Git's HTTP buffer. 
- This prevents GitHub from dropping the connection during larger pushes. 

```r

git config --global http.postBuffer 524288000

```

___
# **1.30.26 - Intro to QIIME2 & Decomposition Tutorial**
# QIIME2
**What is QIIME2 (pronounced "chime")?**

"QIIME2 is a powerful, extensible, and decentralized microbiome analysis package with a focus on data and analysis transparency. QIIME2 enables researchers to start an analysis with raw DNA sequence data and finish with publication-quality figures and statistical results."

QIIME2 is pretty popular:
- As of January 2022, there have been over 32,000 total QIIME1 & 2 citations
- The official QIIME2 citation published in 2019 ([linked hereLinks to an external site.](https://www.nature.com/articles/s41587-019-0209-9 "(opens in a new window)")) has almost 9,000 citations
- Compared to mothur, another common microbiome analysis package, QIIME2 outpaces other amplicon-based microbiome bioinformatic tools 
 ![[Pasted image 20260214221148.png|400]]
- - QIIME2 can be used with a HUGE variety of studies
- QIIME2 has succeeded QIIME1 as of January 2018, and QIIME1 is no longer supported! Learn more about what that means [hereLinks to an external site.](https://qiime.wordpress.com/2018/01/03/qiime-2-has-succeeded-qiime-1/ "(opens in a new window)")
    - Similar to a lot of other popular bioinformatic tools (like R studio), QIIME2 is constantly being updated to fix bugs and add more functionality. The development team does take some feedback from users for improving qiime as well!

**Some high-level features:
- Provides the **latest microbiome bioinformatics methods and visualizations**
- It's **highly accessible** through accurate and detailed documentation and tutorials (we will do many of these tutorials throughout the semester)
- There is a **community** of microbiome scientists, developers, and bioinformaticians in the QIIME2 world. 

**Some low-level features:
- It has **decentralized provenance tracking** that automates bioinformatics record keeping, facilitating **reproducibility**
 ![[Pasted image 20260214221346.png|400]]
- - QIIME2 offers **multiple user interfaces** (command line, Artifact API, graphical user interface, and q2view) which make this bioinformatic tools broadly accessible.
![[Pasted image 20260214221406.png]]
- **Plugin architecture** allows the software to keep pace with the field. Any developer can create and distribute a QIIME2 plugin (even you!)
## **QIIME2 Resources**
- QIIME2 website: [https://qiime2.org/Links to an external site.](https://qiime2.org/ "Link (opens in a new window)")
- Documentation (you will use this link a lot): [https://docs.qiime2.org/2022.11/Links to an external site.](https://docs.qiime2.org/2023.9/ "(opens in a new window)")
- Forum (go here when you have a question): [https://forum.qiime2.org/Links to an external site.](https://forum.qiime2.org/ "(opens in a new window)")
- Code behind QIIME2: [https://github.com/qiime2Links to an external site.](https://github.com/qiime2 "Link (opens in a new window)")
- For viewing visualizations in your browser (I would bookmark this): [https://view.qiime2.org/Links to an external site.](https://view.qiime2.org/ "(opens in a new window)")

## **Core Concepts**

QIIME2 Data Formats (.qza and .qzv)
- QIIME2 data exists as _Artifacts_ 
    - This is a fancy way to basically say zipped file. Within Qiime2, these artifacts (zipped files) contain the core data (e.g. your fasta file with your DNA sequences) and additional information about the data (see figure below).
    - Using artifacts instead of just the singular file allows QIIME2 to track important information about the data (e.g., data type, format, how the data was generated, qiime2 version used, etc.)
- **Two types of _Artifacts_**
    - .qza data files ("a" stands for "artifact") = so "qiime zipped artifact"
    - .qzv visualization files ("v" stands for "visualization") = so "qiime zipped visualization"
- Unzipping .qza or .qzv file will generate a folder with the underlying data and associated metadata. Files structure looks like this:
    - this save a lot of the background information about your data analysis so you dont have to!
    - For example, you can see that the data stored in a .qza file is only a small part of what's stored in a qza
    - Conceptual: 
![[Pasted image 20260214221622.png|500]]
 - what it actually looks like in your terminal when you do "unzip faith_pd_vector.qza"
 - you would see files and directories that contain everything about your file.

## Q2view

- Web-based QIIME2 artifact viewer - it's just a browser link ([https://view.qiime2.org/Links to an external site.](https://view.qiime2.org/ "(opens in a new window)"))
- Reads QIIME2 outputs (.qza and .qzv files)
- You can easily share links over email or Dropbox with your collaborators
- No uploading - your data stays on your computer
- Let's take a look - [taxonomic composition bar plotLinks to an external site.](https://view.qiime2.org/visualization/?type=html&src=https%3A%2F%2Fdocs.qiime2.org%2F2018.6%2Fdata%2Ftutorials%2Fmoving-pictures%2Ftaxa-bar-plots.qzv "(opens in a new window)")

## Semantic Types

- There are MANY types of .qza files you can create.
    - These can be referred to as different "**semantic types**" but really they are just different types of data. 
    - A semantic type of assigned to every QIIME2 _artifact_ 
    - Allows QIIME2 to know what the data is and whether it's appropriate for a type of analysis since different parts of the analysis only accept certain file types.
- Examples:
    - <mark style="background: #FFF3A3A6;">FeatureTable[Frequency]</mark>: A feature table that indicates how many times a sequence was observed in a sample  
        - we will use the term feature a lot! You can think of this for now as an individual microbe. 
        - so a feature table is just a table that contains the abundance of each microbe within each sample.
    - <mark style="background: #FFF3A3A6;">FeatureTable[RelativeFrequency]</mark>: A feature table that indicates the relative abundance each sequences is present in a sample
    - <mark style="background: #FFF3A3A6;">FeatureData[Taxonomy]</mark>: Table that indicates taxonomic assignment of each sequence (or feature).
- More about semantic types can be found here: [https://docs.qiime2.org/2022.11/semantic-types/Links to an external site.](https://docs.qiime2.org/2022.11/semantic-types/ "Link (opens in a new window)")
    
## Plugins
- Software packages that provide all the methods for performing a specific analysis (e.g., `q2-demux`, and `q2-diversity`)
- Plugins are Python 3 "method annotations" that QIIME2 can interpret
- They can wrap methods not written in Python 3 (e.g., DADA2 is written in R, and mafft is a binary)
- Many plugins are automatically installed with base QIIME2 ([https://docs.qiime2.org/2022.11/plugins/Links to an external site.](https://docs.qiime2.org/2022.11/plugins/ "Link (opens in a new window)")), but other, newer plugins require installation ([https://library.qiime2.org/plugins/)Links to an external site.](https://library.qiime2.org/plugins/ "(opens in a new window)")
- Any developer can create and distribute a plugin:

## How commands/methods work in QIIME2:

- These are **actions** within each plugin that can be performed _on_ QIIME2 artifacts
- Can produce intermediate artifacts or terminal artifacts
- For example, the `classify-sklearn` method is part of the `q2-feature-classifier` plugin which allows you to assign taxonomy to your microbes. 

Putting it all together
![[Pasted image 20260214222005.png]]
So a .qza would be our input, you would run a qiime2 action on that file, and you get out another artifact (sometimes in the form of another .qza (intermediate output) and sometimes it will be a qzv (terminal output).

More info about these concepts can be found here: [https://docs.qiime2.org/2023.9/concepts/Links to an external site.](https://docs.qiime2.org/2023.9/concepts/ "(opens in a new window)") 

## **Additional Educational Resources**

- Here is an overview of QIIME2 plugin workflows. This is a great starting point for understanding QIIME2: [https://docs.qiime2.org/2022.11/tutorials/overview/Links to an external site.](https://docs.qiime2.org/2022.11/tutorials/overview/ "(opens in a new window)")
- If you are already an experienced microbiome researcher, this is a good resource for switching to QIIME2: [https://docs.qiime2.org/2022.11/tutorials/qiime2-for-experienced-microbiome-researchers/Links to an external site.](https://docs.qiime2.org/2022.11/tutorials/qiime2-for-experienced-microbiome-researchers/ "(opens in a new window)")

- - Note - all of the above links to the QIIME2 website you can find by searching in the Table of Contents. To become familiar with finding things on the QIIME2 website on your own, a great exercise would be to click around and see what you find.
- Want to learn more about basic research computing? Take a software carpentry workshop: [https://www.software-carpentry.org/Links to an external site.](https://www.software-carpentry.org/ "(opens in a new window)")
- Here is a free, open source, interactive text developed by Greg Caporaso (who is also a major developer of QIIME2) that provides an introduction to applied bioinformatics: [http://readiab.org/Links to an external site.](http://readiab.org/ "(opens in a new window)")
- This is a relatively older course, but this coursera course gives a great introduction to the human gut microbiome, how to study it, and make sense out of the data: [https://www.coursera.org/learn/microbiomeLinks to an external site.](https://www.coursera.org/learn/microbiome "Link (opens in a new window)")
### **Human Decomposition Tutorial Dataset**

**Study Background:** For the first portion of this class we will be working with an example dataset. The dataset is a subset of a larger study conducted by Burcham et al. (2024) designed to determine **whether geographical location/climate and season impact assembly and succession patterns of the cadaver microbiome**. This tutorial demonstrates what a "typical" QIIME2 analysis of 16S rRNA gene amplicon data would look like and will be a great reference for you when analyzing your own data.

- Estimating time since death (i.e. postmortem interval) is crucial for law enforcement to establish accurate timelines and verify alibis
- Current methods are most accurate within 24 hours of death due to environmental factors (e.g., temperature, humidity) that impact decomposition rates
- Studies have shown microbial community response over the course of terrestrial human cadaver decomposition and across a range of mammals, results in a repeatable patterns across individuals
- Patterns appear somewhat similar across different soil types
- Unclear how the effects of environmental variability, such as differences in climate, geographic location and season, may affect the assembly processes and interactions of microbial decomposers

**Study Design:** To study impact of climate and season, **36 human cadavers** were placed at **3 anthropological research facilities** in **2 climate zones**. ARF, located in Knoxville, Tennessee, and STAFS, located in Huntsville, Texas reside in a temperate forest climate zone while FIRS in Grand Junction, Colorado is located within an semi-arid steppe. At each facility, **3 cadavers** were place in the **spring, summer, fall, and winter** (_n_=12 cadaver per facility). Decomposition took place over **21 days**. 16S rRNA amplicon samples were collected daily from **skin and soil** near decomposing cadaver. 
![[Pasted image 20260214222223.png|300]]
**We’ll look at a subset of data from:**
- 9 cadavers
- 3 facilities
- 1 season (spring)
- 21 consecutive days
- 2 sample types (skin and soil)
- Sequences have been subsampled to approximately 5000 sequences per sample to allow the tutorial to run in a short time

This experimental design is useful for learning data analysis for several reasons:
- It's easy to **subset into a small dataset** so we can analyze it with fewer computational resources 
- It gives us **several variables to use for comparisons**
- It includes **both categorical and longitudinal (i.e. timeseries) data**
- It is a **hypothesis-driven experiment**

**Hypothesis**: Geographical location will impact microbial composition of cadaver microbiome  

**One quick profile edit before running qiime2**
- navigate to On Demand: **[ondemand-rmacc.rc.colorado.edu.](http://ondemand-rmacc.rc.colorado.edu/ "(opens in a new window)")** 
- go to your home directory
- click "show dot files"
- edit the .bashrc file so that your temporary files are saved in your scratch directory. Copy and paste these two lines into the .bashrc file. 
```r
# Redirect tmp directory to scratch folder
export TMPDIR=/scratch/alpine/$USER/tmp
```
#### **Remember to keep track of your commands!<!-- omit -->**
- Helps you remember what you did six months from now
- Allows you to edit copied text before pasting into terminal
- Ensures your research will be reproducible to others, some journals now require you to upload your data processing commands
- Great reference for future data analysis projects, or sharing scripts with others
### **Let's Begin!**

Since the commands we are going to run today are simple (and computationally don't require many resources), we will work in an **s****interactive node**. But, because there are many of you trying to run commands at the same time, our two-hour class uses a lot of Alpine's resources at once, so we have a reserved node all to ourselves to mitigate any delay in analysis.

1. Relaunch OnDemand if you aren't still on the webpage: **[ondemand-rmacc.rc.colorado.edu.](http://ondemand-rmacc.rc.colorado.edu/ "(opens in a new window)")**  
2. Open a new Alpine shell
3. Move to an sinteractive node (see below)

**During our Friday tutorial time only, we can use this node by first running the following command:**
```r
sinteractive --reservation=aneq505 --time=01:00:00 --partition=amilan --nodes=1 --ntasks=2 --qos=normal
```
Now, anything we type at the prompt will be sent to our reserved class node. Keep in mind that our access to this will end when class if over, so any commands that you run at 12:49 may not finish on this node and you will need to rerun on an ainteractive node (so you would just type in ainteractive and exclude the --reservation).

**1. Make a new directory for the analysis:** 
First, we'll build a directory to conduct the analysis in. We will do this in our **scratch** directory, but remember that the files in this directory are **deleted after 90 days of inactivity**, so if we wanted to retain these results we'd need to transfer them to projects for storage. We just need to run commands in the scratch directory because there are more resources available to us here. 

Use the following commands (adding in **your** username for the first line) to log into our scratch directory, create a "decomp_tutorial" directory, and move into that directory.
```r
# replace "username" with YOUR CSU EID   
cd /scratch/alpine/c832916267@colostate.edu  

# make a directory using the mkdir command   
mkdir decomp_tutorial  
  
# move into that directory using cd   
cd decomp_tutorial
```
Let's make sure we're in the correct place. Use `pwd` to make sure we're in the **decomp_tutorial** folder we just created. If you're in the wrong directory, you will want to `cd` to the correct one. It should say /scratch/alpine/username@colostate.edu/decomp_tutorial

Next, we will upload the metadata for the tutorial.

## **Metadata**

Metadata is very, very important. It is all of the information associated with your samples. When you analyze your data, the metadata that you have associated with your samples is how you can notice trends (e.g., is there a trend by time, treatment, body site, geography, etc). Otherwise, you would have a bunch of microbiome data and nothing to associate it with! Metadata is all information that you as a researcher should record thoroughly for your own project. You should include anything that you think might have an effect on your hypothesis, such as environmental variables, host sex and age (if applicable), date of collection, any potential biological or technical confounders, etc. _It's better to have too much metadata than not enough_.

_Why do I need metadata?_ Metadata are created and maintained because they **improve use of research data and enable their re-use**. Good metadata can make up for human shortcomings. People forget and misplace things, and leave research projects, taking their knowledge of the research methodology and the data with them.

Some notes about metadata formatting:

- Make sure to **never include spaces** in your column names. If you need a space, use an underscore instead. In some cases you can get away with using periods in place of spaces, but I have personally run into issues with this so it's best to avoid them.
- When you can, **make your sample names informative** because this can be helpful with troubleshooting. In some cases samples need to be anonymized, and that's okay. never have duplicate sample names for different samples.
- If you're looking for a place to start to understand what metadata you should collect, [this paperLinks to an external site.](https://www.nature.com/articles/nbt.1823 "(opens in a new window)") sets some good standards.

**2. Import Decomp Tutorial Metadata and turn "on" qiime2**

```r
#first purge any loaded modules from the node we are on, this ensures no conflicting modules are "on"
#modules are preloaded packages of things people commonly use
module purge
#We can turn "on" or "load" QIIME2
module load qiime2/2024.10_amplicon
```

Warning - it may take a few minutes to cache current deployment! You may see the following message: "QIIME is caching your current deployment for improved performance. This may take a few moments and should only happen once per deployment."  that is totally normal!

**Copy the metadata from the decomp project into a new metadata folder:**
```r
`# make a directory for the metadata using the mkdir command while you are INSIDE of the decomp_tutorial directory   
mkdir metadata`  
  
`# move into that directory using cd   
cd metadata`
```
Now, we will use `cp` command (this is a new one! this stand for "copy") to download the metadata for this study:
* don't forget that period at the end!

```r
cp /pl/active/courses/2025_summer/CSU_2025/q2_workshop_final/QIIME2/metadata_q2_workshop.txt .
```
**Rename the metadata using the  `mv`  (move) command**
```r
mv metadata_q2_workshop.txt metadata.txt
```
**Visualize the metadata file:**
```r
qiime metadata tabulate \
--m-input-file metadata.txt \
--o-visualization metadata.qzv
```
- <span style="color:rgb(0, 112, 192)">Plug ins and stuff. uses chime visualization. PCs do weird things and adds a character when mac presses enter, so must keep everything on the same line.</span>
To look at this, **transfer the file metadata.qzv from alpine to your local computer using OnDemand** and go to [view.qiime2.orgLinks to an external site.](http://view.qiime2.org/ "(opens in a new window)"). Take some mental notes about the kinds of variables you see - _why do you think they will be important for this study?_
<span style="color:rgb(0, 112, 192)">-go back </span><span style="color:rgb(0, 112, 192)">into</span><span style="color:rgb(0, 112, 192)"> folder and download qzv. then upload into ciime view. <br></span>
Note that you can always edit your metadata file in Excel if needed and continue to use it in QIIME2. Converting your metadata file into a .qzv isn't always necessary because you can always look at it in Excel, but it is good practice for those just learning to use QIIME2.
___
# **2.6.26 - Demultiplexing**
## **Importing Data**
Before we can analyze sequencing data in QIIME2, we first need to import it. This step tells QIIME2 where your data lives and how they are formatted.

There are two common ways to import data into QIIME 2, you will get practice with both:
1. **Using a <span style="color:rgb(255, 255, 0)">manifest fil</span>e** – This method uses a text file that lists each sample and the location of its corresponding sequencing files. This is the approach you will use for the decomposition dataset because the sequences have already been demultiplexed.
2. **Using<span style="color:rgb(255, 255, 0)"> compressed raw sequencing file</span>s** – This method is typically used when you receive raw data directly from a sequencing facility, is not demultiplexed first. This is the approach we will use for the homework.
- <span style="color:rgb(0, 112, 192)">Using manifest <span style="color:rgb(0, 112, 192)">file</span> in class</span>
## Demultiplexing##

Let's review demultiplexing, which we talked about in lecture. Remember that after extracting the DNA, we can use a barcoded PCR technique to assign a unique 12 base-pair barcode to all of the sequenc<span style="color:rgb(0, 112, 192)">es within a single sample. After pooling and sequencing everything in one sequencing ru</span>n (multiplexing; makes sequencing cheaper!), we can bioinformatically demultiplex to separate the sequences based on the sample they came from using the 12 base pair (bp) barcode. 
- <span style="color:rgb(0, 112, 192)">if we didnt do this would have to sequence each sample at a time, so here we can put all in one with each having its own unique barcode and tab</span>

**_Simply put, demultiplexing means we take the sequences and assign to them to the appropriate samples_**

How does this work? When we incorporate the barcode during PCR, we know exactly what 12 bp barcode corresponds to each sample. We can record this in our metadata (YAY more metadata!). From the sequencer, we will get a **s<mark style="background: #FFF3A3A6;">equences.fast</mark>q** file (if it also has a .gz extension, that just means it is a zipped file), and a **b<mark style="background: #FFF3A3A6;">arcodes.fast</mark>q** file.

**<span style="color:rgb(255, 0, 0)">M</span><span style="color:rgb(255, 0, 0)">ultiplexed data</span> has all the sequences from all the samples in one file (the sequences.fastq file)!** 

- **barcodes.fastq.gz will have the barcodes for each sample**
- **your metadata file will contain the sample names and barcodes**
- **These 3 files together will enable you to demultiplex the sequences.** 
- **your output will be a fastq file per sample** ##each smple has its own individual fastq

-**<span style="color:rgb(255, 255, 0)">Input</span>![[Pasted image 20260206102739.png]]
##barcodes.fastq shows the <span style="color:rgb(255, 0, 0)">barcodes</span>. sequences file shows the fastq for all sequences. EAch of the 6 lines corresponds to one sample. Have them for each sample

-<span style="color:rgb(255, 255, 0)">Output</span> 
![[Pasted image 20260206102902.png|300]]

When we tell QIIME2 to demultiplex, we will give it our sequences.fastq and barcodes.fastq files, and it will use these files along with the barcode that we give it in our metadata file to separate the sequences by sample name. This is the whole workflow from the bench (adding the barcodes to each sample) so the output files we get from the sequencer:

![[Pasted image 20260206102927.png]]
##whole workflow from the bench

Recall that the output sequences.fastq file is a specific file type that comes from the sequencer that contains 4 line types, where each set of 4 lines corresponds to one sequencing read. The first line is a sequencer identifier (for pc to read not us but has info about the run). This line you will likely never use, but it has some information about the sequencing run.

![[Pasted image 20260206102939.png]]
The second line is either the sequence (150 or 250 bp long depending on your chemistry for an Illumina Miseq run), or it is your barcode. 

![[Pasted image 20260206102959.png]]
The "<span style="color:rgb(255, 255, 0)">+</span>" in the third line is just a placeholder. The purpose of this is to signal the end of the sequence and the start of the next line. 

The fourth and final line looks like a bunch of gibberish, but it has information about the quality of your sequence. This line will always be the same length as the sequence (or barcode), as each character corresponds to the quality of a single base pair. 
- <span style="color:rgb(0, 112, 192)">rarely used, only really need if you dont know if the file has phred scores since that affects how files are imported into Qiime. (if just amplicon seq then theres no phred scores, so need to know that ahead of time if you can). Should know what data looks like and what it means</span>
![[Pasted image 20260206103013.png]]
### Phred Quality Scores
The quality scores can then be converted into **phred quality scores** using an ASCII coding system. A phred score is a single number that corresponds to a single base pair within a sequence, and that number tells us the probability that the base at that position is correct. See the below chart on the left for phred scores and what they mean for how accurate a base might be.
- <span style="color:rgb(0, 112, 192)">score of how confident bp is correct. Ex. If this is a C, its 34. Want scores to be at least 30 proba of incorrect basecall at 30 is 1 in a thousand, so 99.99% accuracy. In our example we have an A, which means were at 32 so still 99.99% accuracy.</span>

![[Pasted image 20260206103027.png]]**What Phred Score should you aim to have?** 
While there is no "correct" phred score, it's good to shoot for one of **at least 30**. It is okay if you need to keep some bases with a phred score of lower than this, but it is always dependent on the dataset itself. This is something we will explore later in our tutorials.  

**Why review this?** 
For the decomp dataset we will skip demultiplexing and instead import by a manifest file, but for the homework you will practice demultiplexing from raw data files. For both datasets you will practice reviewing the demultiplexed data for its quality which will inform your denoising choices. ##sometimes things can go wrong w/ sequencer so always check in on things 

**Importing Sequences Using Manifest File for the Decomp Tutorial**
The decomp dataset sequences have already been demultiplexed (some sequencing facilities will demultiplex for you, some wont so its important to know how to import for both ways), so we will import them using the **<span style="color:rgb(255, 255, 0)">fastq manifest format</span>**. This is when we have a single fastq file for each sample, and when we import, we need to give the path for each fastq sample file in a .tsv or .txt file. This tells QIIME2 where to look for the sequences for each sample and it will combine them into one .qza file (#artifact for Qiime so Qiime can read it and work through it) that we can use for downstream analyses. We will import the sequences as SampleData[PairedEndSequencesWithQuality], because we have paired end data that contains Phred scores in the sequences file. 
- <span style="color:rgb(0, 112, 192)">were using paired end but can also have single, dont use single end a ton anymore but still possible</span>
![[Pasted image 20260214225009.png]]
### **Let's get started!**

**Alpine On-Demand:** [ondemand-rmacc.rc.colorado.edu](http://ondemand-rmacc.rc.colorado.edu/ "(opens in a new window)")

##start by opening in terminal in scratch. Always start with pwd and ls to make sure your in the right place

Load the class reservation node:
```r
sinteractive --reservation=aneq505 --time=01:00:00 --partition=amilan --nodes=1 --ntasks=8 --qos=normal
```

Load your Qiime environment:
```r
module purge  #cleans everything out from last run
  
module load qiime2/2024.10_amplicon
```

Let's navigate to the correct place. Use `cd` to move into your **decomp****_tutorial** folder. Use `pwd` to make sure you're in the correct place. 
```r
cd /scratch/alpine/$USER/decomp_tutorial 
  
pwd
```

### **Create directories**
**for the different analyses so you can keep your files organized:**
```r
mkdir slurm

mkdir taxonomy

mkdir tree

mkdir taxaplots

mkdir dada2

mkdir demux

mkdir core_metrics #can do this now, but future coremetrics command will also make directory for us
#do ls at end to make sure they all showed up
```

#### **Create a directory for the manifest and import the manifest files
(we made them for you):** 
In your study you would need to construct the manifest file yourself, 

```r
mkdir manifest  #tells qiime where sequencing files are and to put in qza
cd manifest

#Two commands because data was broken into two groups so need to run twice

cp /pl/active/courses/2025_summer/CSU_2025/q2_workshop_final/QIIME2/manifest_run2.txt . 

cp /pl/active/courses/2025_summer/CSU_2025/q2_workshop_final/QIIME2/manifest_run3.txt .

##Move out of manifest directory
cd ../
```  

#### **Copy the demultiplex reads into your analysis directory:**
```r
#Have these reads in an alpine file that can be copied, can also upload them into file of choice or can use Globus if theyre huge. Each of these directories has one file for each sample, if you cd reads_run2 can see all the files in there

cp -r /pl/active/courses/2025_summer/CSU_2025/q2_workshop_final/QIIME2/reads_run2 . 

cp -r /pl/active/courses/2025_summer/CSU_2025/q2_workshop_final/QIIME2/reads_run3 .
```

Let's take a look at the data we just imported, we can do this by moving into the reads folder, and typing ls
```r
cd reads_run2

ls
```

these are the individual sample fastq files!
```r
cd ../
```

#### **Import data into QIIME2:**
```r
# run the import command to generate a qiime2 readable artifact for the reads, save the demux results in the demux folder.  
#input format = shows format, input-path - where to find manifest, input path is the file its pulling from, output is where the info is being put
  
#import run 2  
qiime tools import \
--type "SampleData[PairedEndSequencesWithQuality]" \
--input-format PairedEndFastqManifestPhred33V2 \
--input-path manifest/manifest_run2.txt \
--output-path demux/demux_run2.qza  
  
#import run 3  
qiime tools import \
--type "SampleData[PairedEndSequencesWithQuality]" \
--input-format PairedEndFastqManifestPhred33V2 \
--input-path manifest/manifest_run3.txt \
--output-path demux/demux_run3.qza
```

### **Check Data Quality** 
The first important step of data analysis is to check the data quality. Now that we've gotten our data into QIIME2 it's simple to get a visualization that we can use to evaluate our sample quality.

```r
# generate a demux visualization file  
  
cd demux  
  
#run2, puts into qiime that we can visualize
qiime demux summarize \
--i-data demux_run2.qza \
--o-visualization demux_run2.qzv  
  
#run3  
qiime demux summarize \
--i-data demux_run3.qza \
--o-visualization demux_run3.qzv
```

Just like above, transfer this output file to your local computer and visualize it in [view.qiime2.orgLinks to an external site.](http://view.qiime2.org/ "(opens in a new window)"). 

**Questions to Answer with this Output:**
- What is the per-sample sequencing count?
- Which sample had the fewest sequences per sample? The most?
- How many samples were in this run?
- ![[Recording 20260206121734.m4a]]
- How does the quality look?
- Does your data look the exact same as your neighbors?
- How long are the reads? 
- **Where would you trim and/or truncate these sequences to maintain the highest quality data?**

### **Binned Quality Scores**
Some sequencing platforms (e.g. newer Illumina instruments like the MiSeq i100) do not report **individual Phred quality scores** for each base. Instead, they use **binned quality scores**, where ranges of quality values are grouped and reported as a single representative score. This reduces file size and improves data compression, but it changes how quality information is encoded in your FASTQ files. (instead of 1-40 it has 4 different calls)

Rather than seeing the full range of Phred scores (e.g., 0–41), bases are assigned to one of **four quality bins**, which are reported as:

**<span style="color:rgb(0, 112, 192)">2</span>** → very low quality bases
<span style="color:rgb(0, 112, 192)">9</span> → low quality bases
**<span style="color:rgb(0, 112, 192)">23</span>** → moderate quality bases
<span style="color:rgb(0, 112, 192)"><b>38</b></span> → high quality bases

Each of these values represents a _range_ of underlying Phred scores rather than an exact estimate of sequencing accuracy. For example, a base reported with a quality score of 38 may represent any base whose true quality falls within the highest bin.
![[Pasted image 20260206110806.png]]
This means that your demultiplexed sequences from your demux.qzv might look like this:at around >30 which are good quality except for last base, at end of run tend to see a drop of quality so thats why we trim. 
![[Pasted image 20260206123343.png]]
<span style="color:rgb(0, 112, 192)">- Need 10 bp to be able to merge. So if ever designing primers want them to be at least 25 so they actually bind</span>

#### Explanation of Interactive Quality Plots
![[q1lab.mp4]]
Interactive Quality Plot of demux_run2
##Want mean score for each base to be ~30. At ~140 start to see means drop below 140, so since theyre under 30 no longer confident the base is read right. Hover over each of the lines in the plot to see what each of the box values are. Lab wants middle of box to be at 30 at least to be confident
 - careful becuase 2x150 can only cut off ~36 nucleotides because need atleast 10 nucleotide overlap to merge forward and reverse. If you cut too many reads at the reverse theres not enough overlap to merge and itll mess up your computational workflow. 
 - Bit better at 2x250 and can cut more nucleotides and keep the merge ok. 
 - Also talks about designing primers and stuff like that

Interactive Quality Plot of demux_run3
##quality plot looks ALOT better here, none of the middle box are below 30, so here can just keep. Extraction controls should have fewest sequencing runs

Run the denoising commands and then take a Break

### **Denoising sequences to ASVs using DADA2**
Now, we will denoise using [DADA2Links to an external site.](https://www.nature.com/articles/nmeth.3869 "(opens in a new window)") to generate our amplicon sequence variants. These 'ASVs' represent the 'features' in the resulting feature and representative sequences table.

Recall that denoising does three important things:
**Quality filtering:** trims off low-quality reads at a designated location on the sequence.
- **Chimera checking:** Chimeric reads are generally considered artifacts in sequencing applications (i.e. amplicon sequencing) and need to filtered out from the data during processing. Chimeras occur when two DNA sequences join together. This is a contaminate and if they are not removed, can lead to the user falsely interpreting the sequence as a novel organism. 
- **Paired- end read joining:** Here, we have paired-end data, but each read is sequenced on its own (leading to the production of a "forward" and "reverse" read). We need to join these reads together, so that the overlapping region between them can also be used for correcting sequencing errors and potentially yield sequences of higher quality for better taxonomic annotation.  Before joining, trimming low quality bases is necessary for optimizing taxonomy annotation and sequence clustering

![[Pasted image 20260206110843.png]]

___
# **2.13.26 - Denoising & Intro to HW**

___

Start class node
```r
sinteractive --reservation=aneq505 --time=02:00:00 --partition=amilan --nodes=1 --ntasks=8 --qos=normal
```
Load your Qiime environment:
```r
module purge  
  
module load qiime2/2024.10_amplicon
```
Let's navigate to the correct place. Use `cd` to move into your **decomp****_tutorial** **dada2** folder. Use `pwd` to make sure you're in the correct place.

```r
cd /scratch/alpine/$USER/decomp_tutorial/dada2
```
## **Denoising sequences to ASVs using DADA2**
![[Pasted image 20260213093459.png]]

<span style="color:rgb(0, 176, 80)">Taking our sequences that rep microbial sequence and denoise into exact sequence variance or amplicon sequence variances. We're removing what shouldnt be there (primer dimers, pcr errors, chimeras etc.). Were also taking up sequences and making them back into haplotypes ( amplicon sequence variants). Can also cluster things into operational taxonomix units (OTUs). Just turn all of the sequences into features or asvs. In class use ASVs cuz more exact/ unique than OTUs. </span>

![[Recording 20260213112912.m4a]]

Now, we will denoise using [DADA2Links to an external site.](https://www.nature.com/articles/nmeth.3869 "(opens in a new window)") to generate our amplicon sequence variants. <span style="color:rgb(0, 176, 80)">These 'ASVs' represent the 'features' in the resulting feature and representative sequences table. Denoise into sequences</span>
![[Recording 20260213112937.m4a]]Recall that denoising does three important things:

- **Quality filtering:** trims off low-quality reads at a designated location on the sequence. <span style="color:rgb(0, 176, 80)">Like a quality filter</span>
- **Chimera checking:** Chimeric reads are generally considered artifacts in sequencing applications (i.e. amplicon sequencing) and need to filtered out from the data during processing. Chimeras occur when two DNA sequences join together. This is a contaminate and if they are not removed, can lead to the user falsely interpreting the sequence as a novel organism. <span style="color:rgb(0, 176, 80)">Done in background</span>
- **Paired- end read joining:** Here, we have paired-end data, but each read is sequenced on its own (leading to the production of a "forward" and "reverse" read). We need to join these reads together, so that the overlapping region between them can also be used for correcting sequencing errors and potentially yield sequences of higher quality for better taxonomic annotation(cant work off forward or reverse independently).  Before joining, trimming low quality bases is necessary for optimizing taxonomy annotation and sequence clustering.
- Output = Give it all of our demultiplexed sequences (one fast a file). runs all the previous filtering steps so we can output feature table (ASV table). Have features on the columns and have rows as the samples, each gets assigned a certain relative abundance of that feature or ASV in the sample.
- Representative sequences file is also output
	- For every feature gives you the sequence for it.
- We have 150bp amplicons so get 150bp sequence here. Can also check it. Wont really use this for much investigating but will use it as a key input
- 

*audio notes looking at qiime visualization*
![[Pasted image 20260213093528.png]]
![[Recording 20260213113409.m4a]]

Use DADA2 to denoise our sequences. Because we have two sequencing runs, we have to do this twice.

```r
#this will take about 22 mins total  
  
  
#denoise for run 2  
qiime dada2 denoise-paired \
--i-demultiplexed-seqs ../demux/demux_run2.qza \
--p-trunc-len-f 150 \ #truncating a certain length from forward read. number based on where our quality dips in demux plot
--p-trunc-len-r 150 \ #truncates reverse read
--p-n-threads 8 \
--o-table table_run2.qza \
--o-representative-sequences seqs_run2.qza \
--o-denoising-stats dada2_stats_run2.qza  
  
  
#denoise for run 3  
qiime dada2 denoise-paired \
--i-demultiplexed-seqs ../demux/demux_run3.qza \
--p-trunc-len-f 150 \
--p-trunc-len-r 150 \
--p-n-threads 8 \--o-table table_run3.qza \
--o-representative-sequences seqs_run3.qza \
--o-denoising-stats dada2_stats_run3.qza
```

You might notice the --p-trunc-len line, and may be wondering what the 150 means. If you remember when we looked at **demux_seqs.qzv** in QIIME2 View, the interactive quality plot showed that these data were pretty good quality. So, we decided to keep the whole length (150 base pairs) of all reads. In practice, if you start to see the quality dip below 30, you may want to decide to trim some of those low quality base calls off, that way you can be confident in any later analyses. If you were to see quality dip at the beginning of the sequence (usually due to sequencing chemistry), or if you need to trim off your barcodes, you can use --p-trim-left to do this.

## **Important notes about denoising:**
1. Here, we have 2 sequencing runs, this happened because we had so many samples that it needed to be spread out over multiple runs. in this case its important to randomize across runs to account for batch affects. so in order to get all samples we need for the subset we chose for the tutorial data, we needed to use runs "two and three". thats why we have to do some steps twice. after we denoise, we can then merge the sequences and table into singular files for the rest of the analysis.
2. when you merge two runs together, you <mark style="background: #FFF3A3A6;">MUST</mark> use the same trim and truncate parameters or they wont merge properly.
3. if you use different primers than the ones we discuss in the class (reminder we use the Earth Microbiome Protocol 515f/806r primers which also act as the illumina sequencing primers), then you will need to trim them out using cutadadpt.

#### if denoising must check parameters must be exactly the same, cant trim and truncate at different lengths, amplicons MUST be the same length. So if u have mult seq runs cant merge unless theyre the same length. <!-- omit -->
#### if you have different primers than what were using in class, youl have to cut them out with cutadapt. But class is on earth microbiome primers specifically designed for illumina seq so that primers you use to amplify amplicon are the same ones that are used for the sequencers (ie bridge amplification from jessica henleys talk). Seq part attaching to flow cell is same as earth microbiome primers. Good cuz we can start sequencing reads at the amplicon itself and not at the primer, if you dont sequence through the primer dont have to trim it out, buit if you use diff primers will have to trim them out or wont work and will look crappy , everything gets thrown out since just looking for primers. <!-- omit -->
**Check if I need to do this my own data

Amplicon itself - Sequencing primers and barcodes
#### Plot: all goes into the sample. Have forward primer (515 on 3 prime end of forward read) where arrow starts is where youre actually sequencing. Get full 150bp of sequencing, dont have to trim anything out. Good for denoising cuz more room for overlap.  If you didnt have these special primers would only have about 10 bp, so youd lose alot. Definitely a difference between different primers<!-- omit -->
#### Denoising - merging two parts back together<!-- omit -->

![[Recording 20260213114043.m4a]]

Why is #3 important? lets review the amplicon itself:![[Pasted image 20260213093641.png]]
 since we are teaching on the EMP primers which specifically also act as sequencing primers, we get more reads dedicated to our amplicon (in green) and also we dont need to trim out primers.
- additionally, remember that we have paired end reads (so we have a forward and a reverse read) that need to be joined back together. Denoising is doing that for us!

![[Recording 20260213114214.m4a]]
Let's get a visualization of each of these stats outputs for each run
```r
#Turns .qza files into .qzv files
qiime metadata tabulate \--m-input-file dada2_stats_run2.qza \--o-visualization dada2_stats_run2.qzv  
  
qiime metadata tabulate \--m-input-file dada2_stats_run3.qza \--o-visualization dada2_stats_run3.qzv
```
*Somewhat of an explanation below*![[20260213-1842-55.0239756.mp4]]
## **DADA 2
DADA2 denoising creates three output files:**
- <mark style="background: #FFF3A3A6;">stats.qza</mark> (denoising statistics)
	- AKA QC data output
- <mark style="background: #FFF3A3A6;">table.qza</mark> (ASV feature table)
	- AKA microbiome count matrix. Use to look at composition of microbiome for samples and do A + B diversity analysis
- <mark style="background: #FFF3A3A6;">seqs.qza</mark> (sequences associated with ASVs)
	- No other name?
 You will have a set of files for each denoising command (e.g., one set for run 2 and one set for run 3, so six files total).

### **1. DADA2 STATS FILE:** 
Each row is a sample. 

Columns: 
- <span style="color:rgb(0, 112, 192)"><b>Input</b></span>: number of reads per sample (same number as in the demux.qzv)
- <span style="color:rgb(0, 112, 192)"><b><span style="color:rgb(0, 112, 192)">Filtered</b></span></span>: number of reads that passed dada2 filtering
- **<span style="color:rgb(0, 112, 192)"><span style="color:rgb(0, 112, 192)">% of input passed filter</span></span>**: % of reads that passed dada2 filtering
- **<span style="color:rgb(0, 112, 192)">Denoised</span>**: number of reads kept after denoising
- <span style="color:rgb(0, 112, 192)"><span style="color:rgb(0, 112, 192)"><span style="color:rgb(0, 112, 192)">Merged</span></span></span> (only when using paired end data: number of reads merged)
- **<span style="color:rgb(0, 112, 192)">Non-chimeric</span>**: number of reads that are non-chimeric (recall that a chimera is a PCR artifact that is created when two different reads are combined together).
- **<span style="color:rgb(0, 112, 192)">% of input non-chimeric:</span>** % of reads that are non-chimeric 

On this visualization, you are checking to see if there are any samples where a large portion of the reads are removed. Having a few samples like this is ok, but if all of your samples have a large portion of reads removed, then there may be an issue with your data. 

### **2. DADA2 TABLE FILE (AKA Denoised Table, Feature Table):** 
**Merge denoised tables - required when you have samples from multiple sequencing runs like we have here. Then visualize the merged table so all samples are in one place.**

![[Recording 20260213120609.m4a]]
- In dada2 folder

```r
#Important: you have 2 seq runs, denoise separately, have 2 feature tables. But we want to combine to 1 feature table. This will dereplicate all unique features and combine everything together into 1 merge table so you just have one table to work with.
qiime feature-table merge \ #merges table
--i-tables table_run2.qza \
--i-tables table_run3.qza \
--o-merged-table table.qza  
  
#Then well take that table and turn it into a visualization. We need metadata to do this.
qiime feature-table summarize \
--i-table table.qza \
--o-visualization table.qzv \
--m-sample-metadata-file ../metadata/metadata.txt
#When finished, download table .qzv, and visualize in Qiime2 view.
```
- After merging checkpoint – Can look at table now and see 443 samples in one file (combo of two indivs)
The table file contains the following: 

**Features = ASVs**

**Overview tab:** this tab gives you an overview of the summary statistics. 
- **Table summary**: # of samples, number of total features (ASVs) in the dataset, and total feature frequency (how many times were the features observed across all samples?). 
- **Frequency per sample**: stats on reads per sample. Ex: on average, the frequency of features observed in a sample ~4,000. 

**Frequency per feature:** stats on how much a feature/ASV shows up. Ex: on average a feature is observed 683 times.
![[20260213-1905-55.3689156.mp4]]
### **3. DADA2 SEQS FILE:**
```r
#Merge sequence files (Sequences associated w/ ASVs)
qiime feature-table merge-seqs \
--i-data seqs_run2.qza \
--i-data seqs_run3.qza \
--o-merged-data seqs.qza  
  
qiime feature-table tabulate-seqs \
--i-data seqs.qza \
--o-visualization seqs.qzv
```
This file contains the ASV Feature ID and its corresponding sequence

In your seqs.qzv file, _what happens when you click on one of the blue sequences?_

if fasta files crash things, can change them to a .txt
![[20260213-1911-25.8720785.mp4]]

### **Running jobs on Alpine 
- **you will use this for demultiplexing the samples for your homework assignment** 
- Some analysis can take a while (especially with large datasets), so we'll submit it as a "**job**".
- So far we’ve been working on the **interactive node**, for jobs we’ll be using the **computing node**
- **Batch jobs:** are resource provisions that run applications on compute nodes and do not require supervision or interaction.
- **Job script:** is a set of Linux commands paired with the resource requirements for your batch job 

**Create a test job and run it**
*sometimes need to submit a job for more power. its a text file where u put info and tell alpine what you need with commands and time and resources*

![[Recording 20260213122642.m4a]]

1. Go to OnDemand and create a new file in your **slurm directory**, call it "test.sh"
2. click edit to open and edit the file. 

This can be pasted into your example script. Note, to submit jobs, the .sh file must ALWAYS have the <span style="color:rgb(255, 0, 0)">#!/bin/bash</span> line as the first line. the rest are job parameters and are also required, but the order does not matter.


```r
#!/bin/bash  #Shebang: needs to start with this followed by job parameters. These are the main things youll need in your job script (this is a test dummy one)
#SBATCH --job-name=test #name of job  
#SBATCH --nodes=1  #how many nodes, usually can only do 1 at a time
#SBATCH --ntasks=2  #changing dep on how many resources you need
#SBATCH --partition=amilan  #partition were on, affects number of cores/tasks you can request.This has 12 cores and only runs for 24 hours. READ ALPINE DOCS TO KNOW
#SBATCH --time=01:00:00  # how much time you need
#SBATCH --mail-type=ALL  #Mailing system that emails during certain times, can change this to just when start, ends, etc. but all helps
#SBATCH --mail-user=c832916267@colostate.edu  #your email address
#SBATCH --output=slurm-%j.out  #output slurm file. %j is a jobid.out, will give the error codes if your job fails. No red on the terminal with these
#SBATCH --qos=normal  #just telling it its a normal job and not high memory
  
#Activate qiime  
  
module purge  
module load qiime2/2024.10_amplicon  #always run these two when starting alpine
#Dont need to put in sinteractive node becuase we already told it were in the amilan partition, just need to tell it what to load  
  
# command goes here  
OUTFILE="message_${SLURM_JOB_ID}.txt"  
echo "status report" > $OUTFILE  
echo "Job ID: $SLURM_JOB_ID" >> $OUTFILE  
echo "Node: $(hostname)" >> $OUTFILE  
echo "Timestamp: $(date)" >> $OUTFILE  
echo "You ran your first job!" >> $OUTFILE
```
The md above goes into the slurm file
*More instructions for this starting at ~6 min in above audio file*

Click Save, and exit the file.
*when running in command line need to be in the slurm folder where the above script was pasted*
FOR PC ONLY USERS RUN THIS COMMAND FIRST BEFORE running sbtach
  ```r
 dos2unix test.sh
  ```

To submit the job to the compute cluster, use:
```r
sbatch test.sh #runs that test.sh file in the slurm folder
```
- Jobs MUST end in .sh

You will then get a batch job ID as an output. It might be good to note this job ID in case you need to kill the job or check its status. We can also use the On-Demand portal to look at our job status. You should receive an email as well with your job status!

If you get a "failed" email, you can check your slurm directory for the slurm output file (looks like slurm-JOB-ID.out to see what went wrong. This is how you troubleshoot failed code from a job. 

---

![[Recording 20260213123601.m4a]]

### **Summary**
Let's review what we've done so far!
1. There are many ways to study microbial ecology, one of which is to profile the microbial community composition.
2. We use **16S** (a small section of taxonomically informative target DNA, here we use 16S rRNA gene) to **study microbial composition and diversity**.

**Why this gene?**
1. It’s <span style="color:rgb(0, 176, 80)"><b>ubiquitous</b>.</span>
2. Contains regions that identical across diverse organisms (**conserved regions**), and regions that are variable across organisms (**variable regions**).
3. We target for a **specific region of the 16S gene**, PCR amplify, DNA sequence, and then use **qiime2 for sequence data processing**. 

 **Today:**
4. We used **Qiime2** to begin **processing sequence data.** 
5. Qiime2 **requires some type of input data** (today we input demultiplexed reads, your homework will input raw data)
      -  **Decomposition Tutorial:** 
    1. We began with pre-demultiplexed paired end data (2x150nt). # Cow dataset will by 2x250, so more data
    2. We denoised each run separately. # In cow dataset dont need to denoise seperately, just one sequencing one.
    3. Merged quality controlled data together

---

#### **Cow Dataset for Homework:**  

Now its time to practice, we will use a new dataset, called the "cow dataset" to practice the commands you have learned: 

- **20 adult dairy cattle** were sampled (via swabbing) to determine the microbial community composition between **5 different body sites**.  
- Samples were sequenced with 2x250 bp chemistry on an Illumina miseq w/ same primers
- You will be given the raw sequencing file, the barcodes file, and the metadata file (On alpine on public directory). # barcodes and metadata on canvas
- We want you to determine **if and how the microbial composition changes between different cow sites**
- These data are **paired end fastq files** that you will copy from a public folder on Alpine. 
- For homework 1, you will practice **importing** raw data, **demultiplexing paired ends reads**, and **denoising**.
- You will gain experience with editing code and importing paired end reads, and demultiplexing and denoising those samples.
	- In modules, theres a nodule for HW. HW_MD= markdown file for you to use as template. Add file to obsidian vault
- What you will submit: 
    - you will push your homework (via an obsidian note) to GitHub with the **commands** used to import, demultiplex, and denoise the samples.
    - you will include the **answers to the questions** in the note as well 

**To start your homework, go to the modules in Canvas, and download the homework_1.md file. this is the template you will follow to submit your homework (due Feb 19th at midnight)..**

# # Week 5 Tutorial: Taxonomy, Taxa Barplots, Filtering, Phylogenetic Tree
**This week:**

- Quiz #2
- Homework 1 review
- Assign taxonomy using Greengenes2
- Taxa barplots
![[Recording 20260220114557.m4a]]

- Filtering the feature table
- Phylogenetic tree

**Taxonomy**
![[Pasted image 20260220090735.png|400]]

## **Taxonomic Classification**

Last week we denoised the sequences into ASVs. That representative sequences file contained Feature IDs and the sequences. Now, let's classify our ASVs to assign each one its microbial taxonomy. Recall that there are a **variety of databases** that can be used to classify taxonomy:

- [Greengenes2Links to an external site.](https://www.nature.com/articles/s41587-023-01845-1 "(opens in a new window)") - reference tree that includes integrated 16S rRNA regions, full-length 16S rRNA full genes, and whole genome information
- [SILVALinks to an external site.](https://www.arb-silva.de/ "(opens in a new window)") - high quality ribosomal RNA database
- [Ribosomal Database ProjectLinks to an external site.](https://www.glbrc.org/data-and-tools/glbrc-data-sets/ribosomal-database-project "(opens in a new window)") (RDP) - quality controlled, aligned, and annotated 16S rRNA sequences
- [NCBI Reference Sequence DatabaseLinks to an external site.](https://www.ncbi.nlm.nih.gov/refseq/ "(opens in a new window)") (RefSeq) - a comprehensive, integrated, non-redundant, well-annotated set of reference sequences including genomic, transcript, and protein.
- [GenBankLinks to an external site.](https://www.ncbi.nlm.nih.gov/genbank/ "(opens in a new window)") - the NIH genetic sequence database, an annotated collection of all publicly available DNA sequences
- [Genome Taxonomy DatabaseLinks to an external site.](https://gtdb.ecogenomic.org/about "(opens in a new window)") (GTDB) - a standardised microbial taxonomy based on genome phylogeny
- [Web of LifeLinks to an external site.](https://biocore.github.io/wol/ "(opens in a new window)") (WoL) - reference phylogeny for microbes

After some internal testing in our lab, we find we get the best, and most, classifications by using [Greengenes2Links to an external site.](https://www.nature.com/articles/s41587-023-01845-1 "(opens in a new window)").

**Key takeaways about Greengenes2 (gg2):**

- A reference tree that unifies genomic and 16S rRNA databases into a consistent, integrated source
- Includes nearly 16K bacterial and archaeal whole genomes, 18K full-length 16S rRNA sequences, 1.7 million near-complete 16S rRNA genes, and over 23 million 16S rRNA V4 region sequences, all compiled from a variety of sources
- Yields a reference tree covering over 21 million sequences from 31 different types of environments

![[Recording 20260220114904.m4a]]

For the purpose of this class, we will use the pre-trained classifier version of gg2 that was trained on the V4 hypervariable region (corresponding to the 515F-806R primers). 

**Let's get started!**

1. Log onto Alpine On-Demand: [ondemand-rmacc.rc.colorado.edu](http://ondemand-rmacc.rc.colorado.edu/ "(opens in a new window)")
2. Navigate to your decomp tutorial directory
3. open a terminal
4. Go to the taxonomy folder:
```r
cd /scratch/alpine/$USER/decomp_tutorial/taxonomy
```

**5. Load the class node:**
```r
sinteractive --reservation=aneq505 --time=02:00:00 --partition=amilan --nodes=1 --ntasks=6 --qos=normal
```

 **6. Load your Qiime environment:**
```r
module purge  
  
module load qiime2/2024.10_amplicon
```

**7.  wget the classifier:**
tells qiime we want to copy and paste this file from this repository. Creater of grreengenes stores all of the classifiers we need on the index. Have different release form so u can go back to diff releases. Make sure versions are compatible with Qiime (sometimes trial and error, usually similar years works well)
```r
wget --no-check-certificate https://ftp.microbio.me/greengenes_release/2024.09/2024.09.backbone.v4.nb.qza
#Different types
#Versiosn 2024.09 against backbone but using v4 region. We sequence these and we know we need taxonomic classifier trained on V$ region alone
#nb- naive baize, a pretrained clasifier, tells qiime what these are
```

![[Recording 20260220120138.m4a]]



the next command, uses the **q2-feature-classifier** plugin to classify the ASVs by its **classify-sklearn method. This** is machine-learning-based classification, in which **classifiers must be** **_trained_**, e.g., to learn which **features best distinguish each taxonomic group**, adding an additional step to the classification process.

![[Recording 20260220120516.m4a]]

Here is how you can visualize a "classifier":
![[Pasted image 20260220091003.png|500]]
- data on left is what we give??
- REference database in green has these two files that have our sequences and its ID and that ID is linked to some known taxonomy. use these files from pre trained classifier to compare unknown ASVs to output some taxonomy to feature ID. 
we give the **classifier** a **reference database (**with **sequences** and the **associated taxonomic classification**) (green boxes)

And tell the classifier, if you see a sequence like this, you should be returning a taxonomy label like this one (red boxes)

**8.  use our representative reads to classify taxonomy, which will give us our taxonomy.qza output. This will take ~2 minutes.**
```r
qiime feature-classifier classify-sklearn \
--i-reads ../dada2/seqs.qza \ #sequences
--i-classifier 2024.09.backbone.v4.nb.qza \ #pre trained classifier
--o-classification taxonomy_gg2.qza #your ASVs
```

![[Recording 20260220120605.m4a]]
- green genes has its own database, can paste ur amplicon and it can do BLAST search. Can also type in species name.  Itll give you great info about where stuff came from and whatnot and you can start there to understand info about microbe and where it originated from. 

**9. Let's make our taxonomy into a visualization, transfer to our local computers, and look at it in QIIME2 View.**
```r
qiime metadata tabulate \
--m-input-file taxonomy_gg2.qza \
--o-visualization taxonomy_gg2.qzv
```
- always check confidence scores
- will show taxonomic identification
![[20260220-1909-54.4751804.mp4]]
- can sort by confidence
	- So if u find something important ALWAYS check confidence
- Will classify to the best of its ability, but will put _ for any uncertain classifications
	- ex. d_bacteria, p_pseudomonodota, c_ alphaproteobacteira, o_ricketsiales, f_mitochondria, g__, s__

Let's take a look at our taxonomy file using [view.qiime2.orgLinks to an external site.](https://view.qiime2.org/ "(opens in a new window)")!

- we can see that we get an output file with each Feature ID, its taxonomy, and the confidence level.
- Helpful QIIME2 forum post on how confidence is calculated (hint: it isn’t simple): [https://forum.qiime2.org/t/how-is-the-confidence-calculated-with-taxa-assignments/179/3](https://forum.qiime2.org/t/how-is-the-confidence-calculated-with-taxa-assignments/179/3 "(opens in a new window)")
**10. Since there are over 400 samples, we are first going to group by type_days - a metadata column that includes both sample type and days since placement. This will make our results a lot more interpretable**
```r
#creates new metadata column by merging type and days
qiime feature-table group \
--i-table ../dada2/table.qza \
--m-metadata-file ../metadata/metadata.txt \
--m-metadata-column type_days \
--p-mode mean-ceiling \
--p-axis sample \
--o-grouped-table ../dada2/table_type_days.qza
```

**11. Now, using our grouped feature table, we will remove contaminating features like chloroplasts and mitochondria.** 

- Things we often want to remove are plant-derived DNA or eukaryotically-derived DNA, but you can remove other contaminates too here by adding that taxonomy to the --p-exclude line. 
- The reason these show up in our data is due to the endosymbiotic theory, that mitochondria & chloroplasts, likely originating as bacteria, became symbionts in cytoplasm of eukaryotic cells. 
- note that sp004296775 is another chloroplast, it MUST also be removed. see this forum [post.](https://forum.qiime2.org/t/silva-vs-greengenes2-2024-9-yield-different-taxonomic-classifications-of-chloroplast/31889/7 "(opens in a new window)")
```r

qiime taxa filter-table \
--i-table ../dada2/table_type_days.qza \
--i-taxonomy taxonomy_gg2.qza \
--p-exclude mitochondria,chloroplast,sp004296775 \ #genomes to exclude
--o-filtered-table ../dada2/table_type_days_miochloro.qza #trying to filter out hippo gene? Maybe this filters out features?
../dada2/table_type_days_nomitochloro.qza
```

![[Recording 20260220121949.m4a]]

## **Taxa Barplots**
Now that we have ASVs classified taxonomically, let's generate a taxa barplot to visualize it, transfer it to our local computers, and look at it in QIIME2 View.
- use files to generate pretty bar plits
**1. Move to the taxa plots directory**
```r
cd ../taxaplots
```
**2. Generate the taxa bar plot**
```r
qiime taxa barplot \
--i-table ../dada2/table_type_days_nomitochloro.qza \
--i-taxonomy ../taxonomy/taxonomy_gg2.qza \
--o-visualization taxa_barplot_type_days_nomitochloro.qzv
```
- Make sure you are adjusting the taxonomic level so you know what youre looking at. Level 1- genus, level 7= species
Fun tree game - metazoa.com. guess an animal and itll show you where we diverge and goal is to guess what the animal of the day is based on phylogenetic tree
- taxonomic levels = do kinky people come over for good sex

Ignore first 2:45 minutes
![[20260220-1924-12.1936846.mp4]]
![[20260220-1928-52.6799869.mp4]]
A quick way to search for specific taxa is to open your taxa bar plot and use command-F (or ctrl-F for PC) to search for that taxa. Check that chloroplasts and mitochondria were filtered out.
...
Even though these taxa weren't present, it's good practice for you now because it is standard for these things to be filtered out. It's also a good habit to think about taxa you might need to filter out when doing your own analyses.

**3. Check the controls**
First, you will need to filter down to just the controls
First filter samples
```r
qiime feature-table filter-samples \
--i-table ../dada2/table.qza \ #table where column hasnt been combined because were filtering by something in that table where weve altered that column
--m-metadata-file ../metadata/metadata.txt \ metadata file
--p-where "[sample_type]='control'" \ #control= sample type. if youre filtering by something in table make sure to check it in combined table. p= flat when youre filtering. control is type of sample were filtering to
--o-filtered-table ../dada2/table_controls.qza #output of tables with controls in it, so gets rid of every sample that is not a control
```

![[Recording 20260220123157.m4a]]

Create a taxa barplot with our table with just controls
```r
qiime taxa barplot \
--i-table ../dada2/table_controls.qza \
--i-taxonomy ../taxonomy/taxonomy_gg2.qza \
--m-metadata-file ../metadata/metadata.txt \
--o-visualization taxa_barplot_controls.qzv #outputs taxa barplot with just controls
```

![[Recording 20260220123226.m4a]]

Why do we check controls?
- Contamination. trying to prep microbiome samples will always have some mild contamination, sometimes reagents can be contaminated. 
- Workflow validation. Sometimes PCR can be biased. Way that youre doing workflow should be appropriate for samples

![[Recording 20260220123552.m4a]]

Why include positive controls?
- Positive controls contain known combinations and quantities of microbiomes. This allows you to confirm whether your DNA extraction and library preparation protocol was successful and is suitable for identifying a variety of different organisms.
- always always always look at positive controls before you move on
Positive Control Plot
 ![[Pasted image 20260220091427.png|500]]
- this example has known composition from zyme
- L= two that we sequences, doesnt perfectly match up what was in control but has all the same taxa, has same relative abundance. What u want to look for is that all the taxa in control ar epresnt in the positive control. If taxa in control is missing, something happened during sequences, no good
- 
What should you do with taxa found in extraction controls?
- Report them
- Remove them statistically with tools designed for contamination removal
- In most cases, researchers should avoid simplistic removals of taxa , OTUs, or ASVs appearing in negative controls, as many will be microbes from other samples rather than reagent contaminants.
- Will always see something in extractin controls. Shouldnt amplify enough to see them. Can report them. In severe cases can filter the contaminant out, but this is rare. Just report mild contamination. 
![[20260220-1936-06.7455466.mp4]]
You've just done your first taxonomic analysis, congratulations!

![[Recording 20260220124243.m4a]]

**Questions:**
1. Do you see any differences in taxa between sample type? What about facility? 
	1. yes we do. If we clearly see diff between sample site and facility, need to consider for further analysis. So good to run through metadata to see differences. 
2. What taxa did you notice in the extraction controls?
![[20260220-1942-19.6184252.mp4]]
**Pro tips/extra info:**

1. If you ever want to train your own classifier, here is a link to the tutorial on the QIIME2 website; [https://docs.qiime2.org/2021.11/tutorials/feature-classifier/Links to an external site.](https://docs.qiime2.org/2021.11/tutorials/feature-classifier/ "(opens in a new window)")
2. Here is a link to some data resources for picking out pre-trained classifiers and reference databases: [https://docs.qiime2.org/2021.11/data-resources/Links to an external site.](https://docs.qiime2.org/2021.11/data-resources/?highlight=silva "(opens in a new window)")
3. Instead of filtering your feature table, QIIME2 also allows you to filter your taxonomy.qza. Either way will work just fine.
4. To learn more about filtering data, visit the following filtering tutorial on the QIIME2 website; [https://docs.qiime2.org/2023.9/tutorials/filtering/Links to an external site.](https://docs.qiime2.org/2023.9/tutorials/filtering/ "(opens in a new window)")

## **Phylogenetic Trees**
You have already learned about phylogenetic trees during the lecture portion of our class, but let's quickly review some of that information. Phylogenetic trees are useful tools in diversity analysis because they let us consider the evolutionary relationships between organisms. There are several ways in which trees can be constructed, and shown below are the common ways for constructing trees using 16S rRNA data.![[Pasted image 20260220091500.png|500]]
If you are only interested in a quick and dirty analysis, then you can use a de novo approach to create what is called a "fast tree" using the align-to-tree-mafft-fasttree pipeline in the phylogeny plugin in QIIME2. While we don't recommend this for publishing purposes, it can be useful for quickly exploring your data if needed because the insertion tree method can take a long time.

For this course, we will be using the **sepp method** that is available under the **fragment-insertion** plugin in QIIME2. This method uses a backbone tree from a reference database, in this case from Greengenes2, though this can be changed using the --i-reference-database parameter. It attempts to match the representative sequences in our samples with sequences in the tree, and if it cannot it finds the best branch point and adds the unannotated sequences in. This ensures that all the sequences are included in the tree, and that there is valuable structure. This is the recommended method to construct your phylogenetic trees because it is much more accurate and informative.


![[Recording 20260220124943.m4a]]

delete below
![[Recording 20260220125008.m4a]]
Let's navigate to our tree directory and download the reference tree. You can look at your options at [https://docs.qiime2.org/2021.11/data-resources/Links to an external site.](https://docs.qiime2.org/2021.11/data-resources/ "(opens in a new window)") under the SEPP reference databases heading at the bottom. As mentioned above, we are using the Greengenes2 database today, which can be found at [https://ftp.microbio.me/greengenes_release/2022.10/Links to an external site.](https://ftp.microbio.me/greengenes_release/2022.10/ "(opens in a new window)")

**1. Go to the tree directory**
```r
cd ../tree
```

**2. Get the reference backbone**
```r
wget https://ftp.microbio.me/greengenes_release/2022.10/2022.10.backbone.sepp-reference.qza
```

For a full description of all the options, plus some extras that we didn't use today, look at the [documentation page. Links to an external site.](https://docs.qiime2.org/2021.11/plugins/available/fragment-insertion/sepp/#sepp-insert-fragment-sequences-using-sepp-into-reference-phylogenies "(opens in a new window)")

Note that the plugin we are using is `fragment-insertion`, and the method is sepp. We use the representative sequences here, not the table, because this is completely independent of your samples; all it needs is a list of the sequences in your dataset and the ASV label associated.

As an output, we get the `tree.qza` that we need for diversity analyses. We also get a `tree_placements.qza` file, which has information about where it places your features before the actual rooted tree is generated. We get this because it is an intermediate file that is generated during the tree placement, but we don't actually need to look at it (unless you get really invested in your tree and phylogenetics in the future).

This job can take a while (especially in real datasets), so we'll submit it as a job. Here, it may take around 20 minutes.

Note: Remember that before submitting a job you need to **exit the interactive node first before submitting**.

![[Recording 20260220125330.m4a]]

```r
cd ../slurm
```

```r
nano sepp_script.sh  ###this is a way to create the script in the terminal instead of on demand
```

```r
#!/bin/bash  
#SBATCH --job-name=sepp  
#SBATCH --nodes=1  
#SBATCH --ntasks=24  
#SBATCH --partition=amilan  
#SBATCH --time=04:00:00  
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=c832916267@colostate.edu  
#SBATCH --output=slurm-%j.out  
#SBATCH --qos=normal  
  
#Activate qiime  
  
module purge  
module load qiime2/2024.10_amplicon  
  
# go to your decomp directory  
cd /scratch/alpine/$USER/decomp_tutorial  
  
#fragment insertion sepp  
qiime fragment-insertion sepp \
--i-representative-sequences dada2/seqs.qza \
--i-reference-database tree/2022.10.backbone.sepp-reference.qza \
--o-tree tree/tree_gg2.qza \
--o-placements tree/tree_gg2_placements.qza \
--p-threads 4
```

Run in terminal
```r
dos2unix sepp_script.sh
sbatch sepp_script.sh
```
Congratulations! Now you have a tree! QIIME2 doesn't have a real way to visualize these, since a tree is basically a tool for future analyses, but if you get super interested in what it looks like you can plug it into the iTol visualizer here: _[https://itol.embl.de/upload.cgiLinks to an external site.](https://itol.embl.de/upload.cgi "(opens in a new window)")[.Links to an external site.](https://itol.embl.de/upload.cgi "(opens in a new window)")_

## **Summary**

Let's review what we've done so far!
![[Pasted image 20260220091639.png|500]]

Green = File(s), Yellow = Qiime2 actions, Blue = Visualization

[https://docs.qiime2.org/2023.5/tutorials/overview/#conceptual-overview-of-qiime-2](https://docs.qiime2.org/2023.5/tutorials/overview/#conceptual-overview-of-qiime-2 "(opens in a new window)")


---------
-------------------

# Week 6: Alpha Rarefaction, Core Metrics, Alpha Diversity Plots
Val's follow ups from last week... 

###  **1. Long Amplicons**
Remember those long amplicons we saw in the COW dataset? (the ones that were ~425bp, seemingly they all pop up at 18S sequences when you BLAST them). This happens from some unintended affinity for our primers for the 18S rRNA gene of eukaryotes... we originally thought these would come up as Unclassified and out be filtered out by using --p-include c__, but.... they are classifies as bacteria in GG2 (not sure why yet). regardless, thats not good! they need to go...**  

Even though we have done this tutorial enough times to know that it wont affect the results drastically, the best practice would be to remove those long amplicons. (the technical reason on why we may have missed this in the past is because our lab used to use [DeblurLinks to an external site.](https://journals.asm.org/doi/10.1128/msystems.00191-16 "(opens in a new window)") to denoise our sequences. Deblur uses forward reads only, and that's the type of sequencing we've had in the past. Deblur will automatically filter out any non-16S looking amplicons because of its specific error correcting algorithm).  

With dada2, we use longer reads (2x250) can give you longer reads because technically we have ~500bp of sequences, so thats why we get those longer amplicons- they merge in a different place than 16s v4 region would, and due to the error correcting algorithm from dada2, these longer amplicons are not filtered out because they are technically real biological signal).

![[Recording 20260227111619.m4a]]

_Removing ASVs based on length:_ 

## Remove amplicons larger than 300bp

![[Recording 20260227111732.m4a]]

- Removes amplicons larger than 300bp that are likely from 18s contamination while keeping 16s bacterial sequences. 
#### **1. Filter sequences by length
```r
#filter feature table
qiime feature-table filter-seqs \ #calls the filter-seqs action from QIIME2 feature table plugin to filter sequences
--i-data cow_seqs_dada2.qza \ #input file artifact with representative sequences that were generated from dada2 denoising. Each sequence corresponds to an ASV
--m-metadata-file cow_seqs_dada2.qza \ #uses same sequence artifact as "metadata". Works cuz WIIME2 can treat sequence properties (like length) as metadata fields
--p-where 'length(sequence) < 300' \ #filter out seq less than 300 bp to filter out 18s. This command keeps sequences shorter than 300bp
--o-filtered-data cow_seqs_dada2_filtered300.qza #output file with sequences less than 300bp
```

#### **2. Visualize filtered sequences**
```r
qiime feature-table tabulate-seqs \ #generates visualization of rep sequences
--i-data cow_seqs_dada2_filtered300.qza \ #use filtered sequence file generated from the first codeblock
--o-visualization cow_seqs_dada2_filtered300.qzv #visualization file that can be viewed to confirm sequence lengths
```

#### **3. Filter the feature table & remove corresponding ASVs**
```r
qiime feature-table filter-features \ #Filters features (ASVs) in feature table
--i-table cow_table_dada2.qza \ #input feature table (ASV counts per sample)
--m-metadata-file cow_seqs_dada2_filtered300.qza \ #Uses the filtered sequence file as metadata, only ASVs in this file will be kept
--o-filtered-table cow_table_dada2_filtered300.qza # output feature table with ONLY ASVs that pass the length filter
```

#### **4. Summarize filtered feature table
```r
#visualize, double check all amplicons are gone
qiime feature-table summarize \ #Summary of feature table
--i-table cow_table_dada2_filtered300.qza \ #input new filtered feature table from last step
--m-sample-metadata-file ../metadata/cow_metadata.txt \ #Adds metadata, enablind per group visual summaries
--o-visualization cow_table_dada2_filtered300.qzv #output visualization file
```

### **2. Using taxonomic classifiers
add info on using the pre-trained v4 silva and where to find that**


![[Recording 20260227111932.m4a]]

For qiime2 versions 2024.10 and later, the qiime2 developers **will not be providing pre-trained naive bayes classifiers for Silva 138 for the 515F/806R region**. See my forum [postLinks to an external site.](https://forum.qiime2.org/t/silva-v4-classifier/32410/2 "(opens in a new window)"). 

- One of the forum moderators does have a link for pre trained naive bayes v4 silva classifiers [https://bwsyncandshare.kit.edu/s/zK5zjAbsTFQRpboLinks to an external site.](https://bwsyncandshare.kit.edu/s/zK5zjAbsTFQRpbo "(opens in a new window)")
- the downside is that who knows how long he will keep providing the classifiers for each new qiime2 version... 
- so if you really want to use silva, you have to [train your own classifierLinks to an external site.](https://docs.qiime2.org/2021.11/tutorials/feature-classifier/ "(opens in a new window)"), or you can use an older version of qiime (one that is compatible with the older classifiers)
    - to do this, you would go to the data resources page: [https://docs.qiime2.org/2024.10/data-resources/Links to an external site.](https://docs.qiime2.org/2024.10/data-resources/ "(opens in a new window)")
    - and then use the drop down menu on the left side to chose the version of qiime2 you want to use, install that version of qiime2, and then go through your analysis that way and that way you know the pre-trained classifiers are compatible with your qiime2 version!

See example for qiime2 version 2023.7: 

[https://docs.qiime2.org/2023.7/data-resources/Links to an external site.](https://docs.qiime2.org/2023.7/data-resources/ "(opens in a new window)")
![[Pasted image 20260227081916.png]]

### **3. How to know if a taxonomic classifier is incompatible with your version of qiime2?** 

![[Recording 20260227112046.m4a]]

if you are using an incompatible version of a classifer / qiime2 combo, you will get an error like this: 

```r
Plugin error from feature-classifier: The scikit-learn version (1.4.2) used to generate this artifact does not match the current version of scikit-learn installed (0.24.1). Please retrain your classifier for your current deployment to prevent data-corruption errors.Debug info has been saved to /scratch/alpine/lindsval@colostate.edu/.tmp/qiime2-q2cli-err-n7odp7uq.log
```
so this says that there were different versions of scikit-learn that were used to train the classifier, and that it is incompatible with the version of qiime I was using. In this case, you have to use **QIIME2 version 2024.10** **or later** with the **pre-trained 2024.09 naive bayes v4 version of Greengenes2**.

### **4. Back to the decomp tutorial! 
Lets run the taxa bar plot code for the whole dataset as well & explore**

**Alpine On-Demand: [ondemand-rmacc.rc.colorado.edu](http://ondemand-rmacc.rc.colorado.edu/ "(opens in a new window)")**
```r
sinteractive --reservation=aneq505 --time=02:00:00 --partition=amilan --nodes=1 --ntasks=6 --qos=normal
```

![[Recording 20260227112144.m4a]]

```r
module purge  
module load qiime2/2024.10_amplicon  
  
cd /scratch/alpine/$USER/decomp_tutorial/taxaplots  
```

#### **1. Remove unwanted taxa from feature table
```r  
#takes the table and removes all the mitochondria, chloroplasts, and extra one. also takes out everything that doesnt have a class or lower. Exports a filter table
qiime taxa filter-table \ # calls taxa filter-table method in QIIME 2, filters ASVs in feature table based on taxonomic assignments
--i-table ../dada2/table.qza \ #input feature table (ASV counts per sample) produced from dada2 denoising (has counts but no taxonomy yet)
--i-taxonomy ../taxonomy/taxonomy_gg2.qza \ #input taxonomy assignments coresponding to each ASV. the gg2 indicates its classified against Greengenes2 (generated in last lab). It links each ASV to its taxonomix lineage
--p-exclude mitochondria,chloroplast,sp004296775 \ #removes any ASV w/ taxonomy containing any of these terms (ie. mitochondria, chloroplast and specific reference genome sp004...)
--p-include c__ \ #keeps taxa to at least class level (greengenes command, see below for more). removes unassigned levels and anything only classified above class level (at phylum or kingdom)
--o-filtered-table ../dada2/table_nomitochloro.qza  #new filter table output with extras removed and only classified bacterial ASVs 

#This step is removing crap we dont want ie, non-bacteria, poorly classified sequences and creates a cleaned dataset
```
Greengenes-style taxonomy:
- `k__` = kingdom
- `p__` = phylum
- `c__` = class
- `o__` = order
- `f__` = family
- `g__` = genus

#### **2. Generate Taxa barplot**
```r  
#visualize:   
qiime taxa barplot \ #calls barplot function
--i-table ../dada2/table_nomitochloro.qza \ #input newly filtered feature table from last step
--i-taxonomy ../taxonomy/taxonomy_gg2.qza \ #uses same taxonomy file to label taxa in plot (even though we have filtered table, still gotta provide full taxonomy file so QIIME can match retained ASVs)
--m-metadata-file ../metadata/metadata.txt \ #sample metadata file so samples can be grouped by categories
--o-visualization taxa_barplot_all_samples.qzv #output file
```

To determine which versions of qiime were using
```r
module spider qiime
```
![[20260227-1827-32.5747103.mp4]]
## **Alpha Rarefaction, Core Metrics, Alpha Diversity Plots

### **Alpha Rarefaction**

Now that we have our tree and we are ready to start looking at the diversity of our samples, we need to normalize first. In our last lecture, you learned about rarefying as an option for normalizing your data. As a reminder, we do this because the number of reads differs between samples and this can be unfair when you are doing any statistical analysis. For example, say you have sample 1 with 100 reads and sample 2 with 10,000 reads. Then, say you want to compare how much E. coli is in sample 1 versus sample 2. (**this is a relative abundance comparison, not an absolute abundance comparison. To do absolute comparison can do qPCR or metagenomics**) It's likely that sample 2 will have more E. coli compared to sample 1. Is this because the environment that sample 2 was collected from has a biological relevant reason for having more E. coli, or is this because sample 2 happened to have more reads sequenced in that sample and having more E. coli is just an artifact of the PCR and sequencing chemistry? The answer is that we really don't know. This makes comparing samples that have different numbers of reads in them <span style="color:rgb(0, 112, 192)">unfair and this results in invalid statistical analyses</span>. So, before we can compare our samples to each other, we have to subsample (without replacement) the same number of reads from each sample - this is called rarefying. How do we know how many reads to pull from each sample?

The command below shows one tool we can use to determine where to rarefy. The plot we are generating subsamples our feature table at different depths and calculates the alpha diversity at each of these. We are looking for the point when the graphs level off, which suggests that at that point we are not gaining any more diversity by adding more sequences. Ideally, we would rarefy at a level where all of the samples have leveled off, though this is not always possible.

#### Discussing table.qzv
	 can move around sampling depth to see where sample depth is retained and lost. Just a visualization to see where to rarify
	 
![[Recording 20260227113329.m4a]]


We chose 10000 for the max depth because the sample with the most features has 26204 sequences and when we look at the table.qzv we see that we start losing samples around 11000 sequences, and we want to capture all possibilities. By default, 10 rarefied tables are calculated at each sampling depth to provide an error estimate. 

Before running alpha rarefaction and core metrics we want to filter out the controls from our table.
#### **1. Remove controls**
```r
cd ..  
  #Need to remove control samples (blank extractions, PCR controls etc.). probably removed when rarified but this is good practice.
  #want to be in regular decomp tutorial
qiime feature-table filter-samples \ #call filter samples function to remove entire samples (Not ASVs) from feature table
--i-table dada2/table_nomitochloro.qza \ #input filtered feature table that was generated to remove unwanted taxa
--m-metadata-file metadata/metadata.txt \ #use sample metadata file
--p-where "NOT [sample_type] IN ('control') " \ #metadata query that keeps samples where the "sample type" DOES NOT equal "control". This removes any samples called control and keeps biological samples only
--o-filtered-table dada2/table_nomitochloro_nocontrol.qza #output new feature table with control samples removed and is a good dataset for analysis
```

Now we run alpha rarefaction

#### **2. Create Alpha rarefaction directory**
```r
#Make directory for alpha rarefraction
mkdir alpha_rarefaction  #new folder to store rarefaction results
  
cd alpha_rarefaction   #move to newly built directory
```

#### **3. Run Alpha Rarefaction**
```r  
qiime diversity alpha-rarefaction \ #calls the alpha rarefaction function from diversity plugin to generate rarefaction curves for multiple alpha diversity metrics
--i-table ../dada2/table_nomitochloro_nocontrol.qza \ #input feature table thats had extra crap and controls removed (from last few steps)
--m-metadata-file ../metadata/metadata.txt \ #metadata for grouping samples in plots
--p-max-depth 10000 \ #sets max rarefaction depth to 10,000 reads
#Made because victoria saw lots of drop off around 10000. Max depth must be less than the feature with the highest sequencing depth (most reads) before drop off [or < smallest high quality sample you want retained].
--o-visualization alpha_rarefaction_curves.qzv #output file
```
- Alpha rarefaction (qzv output) shows us the following:
**For each sample:**
- Observed Features (richness)
- Shannon diversity
- Faith’s PD (if phylogeny available)
- Evenness

**Curves show:**
- X-axis → sequencing depth
- Y-axis → diversity metric
- Lines → individual samples

![[20260227-1835-41.0325271.mp4]]
The visualization file will display two plots. The upper plot will display the alpha diversity (observed features or shannon) as a function of the sampling depth. This is used to determine **whether the richness or evenness has saturated based on the sampling depth. The rarefaction curve should “level out” as you approach the maximum sampling depth.**

The second plot shows the number of samples in each metadata category group at each sampling depth. This is useful to determine the sampling depth where samples are lost, and whether this may be biased by metadata column group values.

After we have this visualization, we can use the rarefaction curves and the visualization of our DADA2 feature table to decide where to rarefy. Looking at these two visualizations, where would you rarefy?

![[20260227-1842-34.7406510.mp4]]

![[20260227-1845-38.3161386.mp4]]
### **Alpha Diversity Review**

**_What are we measuring with alpha diversity?_**

![Screenshot 2024-02-13 at 1.47.32 PM.png](https://colostate.instructure.com/courses/177609/files/30566297/preview)

****flower type and color are a metaphor for diversity, we are not talking about the microbes of the flowers. If the left picture were a field of microbes, the alpha diversity would be extremely low because the only two species are yellow tulips, grass and trees. Versus the flower bed on the right has at least 50+ species of flowers so the alpha diversity would be high.** 

Alpha diversity is measuring your **richness** (or presence and absence of organisms), the **evenness** (the abundance of the organisms) and we can also add in the **phylogeny** to "weight" our alpha/within sample diversity based on shared evolutionary history.

Thus, alpha diversity uses the **metadata,** **feature table**, and a **phylogenetic tree,** which describes the evolutionary relationship between your organisms, to determine the within sample diversity. Reminder: alpha diversity is within one organism. Beta diversity is comparing across hosts.

### **Running the Diversity Pipeline (Core-Metrics) to Generate Alpha and Beta Diversity**

One really nice thing about QIIME2 is that we can get nearly all of the diversity metrics (alpha and beta) that we could possibly want with just one command. This command, called **core-metrics**, is what we call a pipeline - it runs a series of analyses with just one command. 

If you look in the [diversity plugin documentationLinks to an external site.](https://docs.qiime2.org/2021.11/plugins/available/diversity/ "(opens in a new window)"), you will see a lot of options! There are a few pipelines, and the one we will be using is the core-metrics pipeline with and without phylogeny (because we just constructed our tree, so we definitely want to incorporate phylogeny into our analyses). You may be interested in the non-phylogenetic pipeline if you don't have a tree. While we recommend using a tree, in some cases you don't have the raw data that you need available to you in order to construct a tree, so QIIME2 still has options for analyses in those cases.

In addition to **pipelines,** there are also **methods.** If you only care about alpha OR beta diversity, you can use the appropriate method to just get those metrics. 

Today we will run the **core-metrics-phylogenetic pipeline** because we are going to be looking at both alpha and beta diversity, and we are going to incorporate phylogeny. 

Note that the output here is a directory. This is because it generates so many files that it would be ridiculous to ask you to name all of them. So, QIIME2 has standard diversity output names, and you just choose the name of the directory where you want them to go.

### **Run Core-Metrics**

![[Recording 20260227120423.m4a]]

- uses the filtered table, **we don't want stats on mitochondria or chloroplasts!**
- the tree allows us to use phylogenetic diversity metrics
- we use the sample depth of 1500 as chosen from the alpha rarefaction plots.
	- PICK SAMPLING DEPTH
- a new directory is created for you with all of the results, and files are pre-named for you.
- This should take about 5 mins
	- Actually took ~40

#### **Calculate standard alpha and beta diversity using phylogenetic tree**
```r
# cd back into the main decomp_tutorial directory  
cd ../  
  
qiime diversity core-metrics-phylogenetic \ #calls core-metrics-phylogenetic pipeline. Wrapper command that runs multiple diversity analyses at once
--i-table dada2/table_nomitochloro_nocontrol.qza \ #input cleaned feature table
--i-phylogeny tree/tree_gg2.qza \ #input phylogenetic tree artifact req for phylogenetic metrics (ie. Faiths PD, UniFrac Distances)
--m-metadata-file metadata/metadata.txt \ #Sample metadata file allowing stats comparisons across groups
--p-sampling-depth 1500 \ #rarefaction depth. Each sample randomly subsampled to 1500 reads, anything with less than 1500 will be excluded
--output-dir core-metrics-results #output directory for all artifacts and visualizations generated
```
### **Core-metrics-phylogenetic Pipeline
#### Automatically computes:
Alpha diversity:
- Observed Features
- Shannon diversity
- Evenness
- Faith’s Phylogenetic Diversity
 **Beta diversity:**
- Bray–Curtis
 - Jaccard
- Weighted UniFrac
- Unweighted UniFrac
**Generates:**
- Distance matrices
- PCoA plots
- Emperor visualizations

#### **Outputs generated
 Alpha diversity artifacts
- `faith_pd_vector.qza`
- `shannon_vector.qza`
- `observed_features_vector.qza`
- `evenness_vector.qza`
**Beta diversity distance matrices:**
- `bray_curtis_distance_matrix.qza`
- `jaccard_distance_matrix.qza`
- `weighted_unifrac_distance_matrix.qza
- `unweighted_unifrac_distance_matrix.qza`
**PCoA results:**
- `*_pcoa_results.qza`
**Emperor plots (interactive ordinations):**
- `*_emperor.qzv`

After this completes (~2mins), explore your output to see what files QIIME2 generated.
```r
#Exploring output directory
cd core-metrics-results  
  
ls core-metrics-results  
  
cd ../ #move out of the core-metrics directory  
  
pwd # make sure you are back in your decomp_tutorial
```
As you can see, this command generates many outputs for the most common alpha and beta diversity test. Some are QIIME 2 artifacts and others are both the artifacts (.qza) and the visualizations (.qzv).

- qzv files used for qiime view and qza for analyses. 

## **Alpha Diversity Files**

**Which core-diversity-metric files are alpha diversity and what do they mean? (ordered here from simplest to more complex):**

**1. Observed features:** this is an alpha diversity metric that counts the number of **_present_ features** in your community. Richness. 

```r
core-metrics-results/observed_features_vector.qza
```
- <span style="color:rgb(217, 129, 129)">Richness: number of unique ASVs in each sample after rarefaction (ie. how many diff taxa are present?) Sensitive to sequencing depth but DOES NOT consider sequencing depth. Higher= More taxa detected.</span>

**2. Pielou's evenness:** an alpha diversity metric that measures relative **evenness** (a comparison of organism abundance) of species richness. If abundance is higher or lower

```r
core-metrics-results/evenness_vector.qza
```
<span style="color:rgb(238, 170, 170)">- Evenness: Are taxa equally abundant?<br>- Scale:  <br>		0 → one taxon dominates  <br>		1 → perfectly even distribution</span>

**3. Faith's Phylogenetic Diversity (pd):** this is an alpha diversity metric that uses **phylogenetic information plus richness** (presence/absence of an organism) to determine alpha diversity. Incorporates phylogeny into it. looks at richness + presence/absence or orgs and looks at phylogeny

```r
core-metrics-results/faith_pd_vector.qza
```
- <span style="color:rgb(238, 170, 170)">Faiths: Sums total branch length of phylogenetic tree represented in a sample. Essentially, how evolutionary diverse is this sample?Very informative for ecological interpretation.<br>Two samples with same richness can differ:<br>- If taxa are closely related → low PD<br>- If taxa span distant clades → high PD</span>


**4. Shannon's Diversity:** this is an alpha diversity metric that uses **richness** (presence/absence of an organism) _and_ **evenness** (organism relative abundance), but does **not** use phylogeny. 

```r
core-metrics-results/shannon_vector.qza
```
-<span style="color:rgb(238, 170, 170)"> Shannons diversity index: formula considers richness, evenness (how evenly taxa are distributed. Essentially, how diverse and balanced is the community? <br>- Lower shannon= one taxon dominates. <br>- Higher shannon= taxa are evenly distributed</span>
![[Recording 20260227120618.m4a]]

### **Alpha group significance**[Links to an external site.](https://docs.qiime2.org/jupyterbooks/cancer-microbiome-intervention-tutorial/030-tutorial-downstream/060-alpha-diversity.html#alpha-group-significance "Permalink to this headline (opens in a new window)")
#### Statistical testing on alpha diversity metrics

For categorical data, we use the **alpha-group-significance visualizer**. Each sample gets one alpha diversity metric because, unlike beta diversity, the math is independent of other samples- more on that later! For now, just remember that **alpha diversity is within-sample diversity**. So, we don't have to input the group of interest here, the only decision we need to make is which diversity metric we're interested in.

First we’ll look for **general patterns in alpha diversity across samples** by comparing different **categorical groupings** of samples to see if there is some relationship to richness. This uses a **Kruskal-Wallis H test** which is a rank-based nonparametric test that can be used to determine if there are **statistically significant differences between two or more groups**. 

To start with, we’ll examine **‘observed features’:**
#### 1. Alpha group significance (categorical comparisons)
```r
qiime diversity alpha-group-significance \ #calls alpha group significance method, performs stat testing & generates boxplots
--i-alpha-diversity core-metrics-results/observed_features_vector.qza \ #input alpha diversity vector that contains one richness value per sample (generated during core metrics)
--m-metadata-file metadata/metadata.txt \ #metadata w/ group variables. QIIME tests richness across all categorical columns in this file
--o-visualization core-metrics-results/observed_features_statistics.qzv #output w/ boxplots of richness per group, Kruskal wallis results, p vals, pairwise comparisons
```
-<span style="color:rgb(238, 170, 170)"> Tests wether alpha diversity differs between categorical metadata groups (ie. tx, diet timepoint)<br>- Kruskal-wallis:<br>	- nonparamtric ANOVA. Used because Alpha diversitys rarely normally distribution and its robust to non normal data</span>

![[20260227-1908-07.0770625.mp4]]
**Is there a significant difference in the number of observed features between any of the categorical data?** 
	<span style="color:rgb(0, 112, 192)">- No statistically significant difference</span>

We'll go ahead and try using the alpha-group-significance visualizer with the **Shannon** index (looks at richness and evenness) and the **Faith's Phylogenetic Diversity** index (looks at richness while incorporating phylogeny).

```r
#Shannon diversity
qiime diversity alpha-group-significance \ #calls alpha group significance method, performs stat testing & generates boxplots
--i-alpha-diversity core-metrics-results/shannon_vector.qza \ #input shannon index vals. Tests if overall diversity (richness+ eveness) difers between groups
--m-metadata-file metadata/metadata.txt \
--o-visualization core-metrics-results/shannon_statistics.qzv  #output file w/ boxplots + kruskal wallis
```

#### 2. Faiths Pd- Phylogenetic comparisons
```r
#Faith's Pd  
qiime diversity alpha-group-significance \ #calls alpha group significance method, performs stat testing & generates boxplots
--i-alpha-diversity core-metrics-results/faith_pd_vector.qza \ #inputs faith pd vals
--m-metadata-file metadata/metadata.txt \
--o-visualization core-metrics-results/faiths_pd_statistics.qzv #output file w/ boxplots + kruskal wallis
```

![[20260227-1914-25.1372778.mp4]]
![[Recording 20260227121651.m4a]]


**When we consider richness and evenness (Shannon's Diversity), is there a significant difference between sample type? What about the facility?**

**Is there a difference in phylogenetic diversity (so Faith's PD) between sample type? facility?**

For continuous covariates that we think could be associated with alpha diversity, we can use the alpha-correlation visualizer. In this study, a couple of variables we could look at include add_0c and days since placement.
#### **3. Alpha Correlation (Continuous vars)
- <span style="color:rgb(238, 170, 170)">Tests correlation w/ numeric variables</span>
```r
qiime diversity alpha-correlation \ #Tests association btwn alpha diveristy & continuous metadatvariables
--i-alpha-diversity core-metrics-results/faith_pd_vector.qza \ #input faith Pd values
--m-metadata-file metadata/metadata.txt \ #metadata file w/ numeric columns
--o-visualization core-metrics-results/faith_pd_correlation_statistics.qzv #output w/ scatterplots, correlation coefficients, P-vals

#doesnt take into acount other variables that may affect results. Here looking at faiths pd. WE can look at it over any continuous variables, but if theres any individual variation that affects the results, this wont take that into account. Its only interpereting how it changes over time with one variable. 
```
- each dot tells u about diversity of samples, and you can see results
<span style="color:rgb(238, 170, 170)">For numeric variables:<br>- Spearman rank correlation (default)<br>- Pearson (if appropriate)<br>Spearman is:<br>- Non-parametric, Robust to non-normal data</span>

![[Recording 20260227121916.m4a]]

### Conceptual Differences

| Command                  | Tests                | Metadata Type | Statistical Test |
| ------------------------ | -------------------- | ------------- | ---------------- |
| alpha-group-significance | Group differences    | Categorical   | Kruskal–Wallis   |
| alpha-correlation        | Association strength | Continuous    | Spearman         |

### What You Are Testing Biologically
#### Observed Features:
Are some treatments richer in taxa?
#### Shannon:
Are some treatments more diverse and evenly distributed?
#### Faith PD:
Are some treatments more evolutionarily diverse?
#### Correlation:
Does diversity increase with a continuous variable (e.g., methane output)?

Since most of our experiments are slightly more complex than just comparing across one categorical or continuous variable, there is the [QIIME2 longitudinal plugin.    Links to an external site..](https://docs.qiime2.org/2021.11/plugins/available/longitudinal/ "(opens in a new window)") We will explore longitudinal analysis of alpha diversity metrics more in-depth during the longitudinal tutorial.

---

**Homework #2**

Now that we have imported the reads, demultiplexed all the samples, and denoised the reads into ASVs (generating the representative seqs file, the feature table, and the denoising stats), we can assign taxonomy to those ASVs and generate some taxa bar plots. Finally, the homework will end with creating a job script for the SEPP tree. 

**Note**: we are adding in the filtering step (the commands are written for you) so you can remove large/contaminating amplicons from the seqs file and the feature table. 

**To do:** 

- Go to the homework 2 materials module to download your respective obsidian file (pc or mac) to get started.
- due March 6th at midnight 
- push to github to submit

Email us with any questions or problems!

---

#### **Note about VS Code and Obsidian**

Here is a [link](https://colostate.instructure.com/courses/220471/files/39191108?wrap=1 "VS_code_in_obsidian.md (opens in a new window)") to instructions to run VS Code through Obsidian using the VS Code in Obsidian community plugin. You can use this plugin to directly open VS Code in your Obsidian vault and then check your code for hidden spaces. Saving the code in VS Code will automatically save it in Obsidian.

- In your files, should see "open in VS code". when that happens, youll get an pop up. can change language to powershell and edit the code, then itll update obsidian code
![[Recording 20260227115302.m4a]]


-open in obsidian, see markdown, change language so itll open in powershell. see stuff??? if you scroll through will see if there are any hidden spaces that cause errors. If you make any changes, just hit save and itll automatically update your files. CAn also do collaborative coding through this. 

# Week 7 Tutorial: Beta Diversity, Longitudinal Analysis, Exporting Qiime2 Data 

**Last week on tutorial Fridays:**
- How to deal with 18S contamination 
- Alpha rarefaction
- Running core metrics
- Alpha diversity visualizations

**This week:**
- Homework 2 review
- Beta diversity, beta diversity stats (part 1) on location
- Beta diversity stats (part 2) on longitudinal. repeated measures, longitudinal data, Linear Mixed Effects (LME) models
- exporting qiime2 data for R
- get started on homework 3

---

## **Beta diversity metrics, PCoA plots, and significance testing**

**_What are we measuring with beta diversity?_** 

****Here we are comparing the entire community of each sample (flower color/type) to each other. Based on this, there aren't many shared flower types between these two pictures, suggesting the beta diversity would be large (closer to 1) and the communities are therefore dissimilar.** 
*image of two gardens that are super different*
- closer=0
- further apart=1

## **Distance Matrices:**

You may recall from **alpha diversity that each sample has its own alpha diversity measure**. If you want, you can add alpha diversity as a column to your metadata because no matter what comparison you're making the alpha diversity for a sample is the same as any other value in the metadata (e.g., day you took the sample). In **beta diversity**, however, it is very dependent on which two samples we’re comparing. So we end up with a matrix like this because **we are comparing the samples to one another**:

![[Pasted image 20260305202900.png]]
- Compare row to column sample.
- 0.201= not that similiar
![[Recording 20260306115008.m4a]]
Notice the sample names at the top are repeated in each row. 

So what we're looking at is the **distance those two samples are from each other _and_ how different they are**. These are usually **calculated on a scale from 0 to 1, with 0 being exactly the same, and 1 being more "distant" or more "dissimilar" and 0 being "closer" or "more similar".** So, these two samples in this top left hand corner are exactly the same (so you'll see that the distance is 0 because they're the same sample- so there's no difference between them).

But! If the samples are somewhat different (**Highlighted red box)**, we see a distance of ~0.6. So again what we're looking at is a **direct comparison** between every single sample in your entire data set to determine how similar or different the microbial communities are. 

**Because of the complexity/size,** we don't usually look at the distance matrices themselves but rather we visualize them with ordination plots like Principal Coordinates Analysis (PCoA)/umaps/NMDS/etc. 

## **Beta Diversity Files**

**Which core-diversity-metric files are beta diversity and what do they mean? Plus, let's check them out in Emperor**

- **Weighted =** considers ASV abundance
- **Unweighted =** does not consider ASV abundance, but rather presence or absence.
- **Phylogenetic** **=** measures the degree to which species are phylogenetically related
- **Non-phylogenetic =** does not account for phylogenetic relatedness.

#### **Unweighted UniFrac:** 
Unweighted,<span style="color:rgb(238, 170, 170)"> phylogenetic</span>. Measures the richness of the species and the number of shared branches in the phylogenetic tree. Unweighted UniFrac is **more sensitive to differences in low-abundance features**. May overrepresent rare features because everything that is present is counted as 1.
`core-metrics-results/unweighted_unifrac_emperor.qzv`

#### **Weighted Unifrac:** 
Weighted, <span style="color:rgb(238, 170, 170)">phylogenetic.</span> Measures the **richness** and **evenness** of the species _and_ the **number of shared branches** in the phylogenetic tree. Weighted UniFrac is useful for **examining differences in community structure.** You may see less separation in this visualization compared to the unweighted, that is because the most prominent features are present similarly in both groups. So taxa abundance is accounted for.
`core-metrics-results/weighted_unifrac_emperor.qzv` 

#### **Jaccard:** 
Unweighted, <span style="color:rgb(238, 170, 170)">non-phylogenetic</span>. Measures how many shared microbes there are between the samples. So the fraction of unique features, regardless of abundance. 
- `core-metrics-results/jaccard_emperor.qzv` 

#### **Bray Curtis:** 
Weighted, <span style="color:rgb(238, 170, 170)">non-phylogenetic</span>. Uses richness and evenness to measure how many features are shared and the abundance of those features. Good when you have highly abundant species!
- `core-metrics-results/bray_curtis_emperor.qzv`

Let's explore the following files: 
- `core-metrics-results/unweighted_unifrac_emperor.qzv` 
- `core-metrics-results/bray_curtis_emperor.qzv` 

**As you click through different metadata colorings, are you seeing any separation between the data points? What metadata category best describes this separation?** 
- Pico A plot. each dot is a smpale
- 3 axes. each axis presents similarities and disimilarities and diff clusters of organisms, depending n B diversity metric
- On R side column can pick btwn diff metadata categories we have.
- Here were looking at body sites, skin vs soil
	- see seperation and clustering of the blue (soil). Seems like another subclustering between soil
- No stats here, just good for visualizing data
- ![[20260306-1853-51.7450582.mp4]]

**Are there any visual differences between unweighted unifrac and bray curtis?**
- Show similar separation, but there is a difference since looking at different aprts of beta diversity

![[Recording 20260306115948.m4a]]

Note that these visualizations are just highlighting the qualitative differences (things we can see that looks like real differences), we need stats to tell us about the actual quantitative differences (what is statistically different).

## **Testing for** **Significance**

**how do we know what is significant or meaningful to our analysis? We test using Beta Group Significance commands in a variety of ways.**

Beta Group Significance will determine whether groups of samples are significantly different from one another using a permutation-based (PERMANOVA) statistical test.

We are testing the hypothesis that <span style="color:rgb(238, 170, 170)">samples within a group are more similar to each other than they are in samples to another group</span>. In other words, it tests whether the within-group distances from each group are different from the between-group distance. Samples that are similar to each other will have smaller distances from each other. Read [this paperLinks to an external site.](https://onlinelibrary.wiley.com/doi/full/10.1111/j.1442-9993.2001.01070.pp.x "(opens in a new window)") for more about PERMANOVA. 

<span style="color:rgb(238, 170, 170)">ASSUMES ALL SAMPLES ARE INDEPENDENT</span>

Use <span style="color:rgb(186, 186, 130)"><b>PERMANOVA</b></span> when:
- You want to test whether **overall community composition differs between groups**
- Your samples are **independent**
- You are testing **categorical predictors** (e.g., treatment, sample type, facility)
- You are working directly with a **distance matrix** (Bray–Curtis, UniFrac, Jaccard, etc.)

<span style="color:rgb(186, 186, 130)"><b>PERMANOVA</b></span> is ideal for:
- Cross-sectional studies
- One-time-point comparisons
- Simple group comparisons

<span style="color:rgb(238, 170, 170)">You cannot use <b>PERMANOVA</b> when:<br></span>
- You have **repeated measures** (longitudinal data), this is because samples from the same subject are **not independent**
- You want to model **continuous predictors** (e.g., time)
- You want to include **multiple covariates with complex structure**
- Longitudinal, studies over time
- Were just using permanova on this dataset for purpose of teaching, wouldnt normally use it for this dataset
- Good for cow dataset

PERMANOVA assumes independence of samples. In longitudinal microbiome data, that assumption is violated because samples from the same subject over time are correlated.

<span style="color:rgb(238, 170, 170)"><b>PERMANOVA is not appropriate for this study; however, we want to demonstrate how to run PERMANOVA so you can use it for your homework assignment, where it is appropriate.</b></span> 

![[Recording 20260306120230.m4a]]

## **Beta diversity stats (part 1)**

**Alpine On-Demand: [ondemand-rmacc.rc.colorado.edu](http://ondemand-rmacc.rc.colorado.edu/ "(opens in a new window)")**

- Using decomp_tutorial data
```r
sinteractive --reservation=aneq505 --time=02:00:00 --partition=amilan --nodes=1 --ntasks=6 --qos=normal

module purge  
module load qiime2/2024.10_amplicon
```

### **Running a PERMANOVA via the beta group significance test command in qiime2**

Here, we are specifying **"body_site"** to determine whether it is associated with significant differences in unweighted UniFrac distance & bray curtis.

```r
# unweighted unifrac significance  
qiime diversity beta-group-significance \
--i-distance-matrix core-metrics-results/unweighted_unifrac_distance_matrix.qza \ #specify which beta diversity metric were looking at
--m-metadata-file metadata/metadata.txt \
--m-metadata-column body_site \ #specify which column to look at (body site in this case)
--o-visualization core-metrics-results/unweighted_unifrac_distance_matrix.qzv #name of output and location
```

```r
# bray curtis significance  
#similiar to last codeblock, but bray curtis
qiime diversity beta-group-significance \
--i-distance-matrix core-metrics-results/bray_curtis_distance_matrix.qza \
--m-metadata-file metadata/metadata.txt \
--m-metadata-column body_site \
--o-visualization core-metrics-results/bray_curtis_distance_matrix.qzv
```

Let's explore these plots in QIIME2 View.

We are looking at what's called a "distance plot", and they can be kind of confusing. If you remember how beta diversity works, we are measuring a distance between two points (remember all of those metrics we made you calculate? That's where these numbers come from). When we make beta diversity box plots, we are plotting the distribution of distances between two things,  and the n is the number of distances in the box plot (not the number of samples you have for that category). 

Now that we understand these distance plots, let's try to interpret them.

- comparing first group to second group??? Relisten

**Does body site contribute to a significant difference in beta diversity?**
- Yes, the p-value is 0.001
- This is overall p value between 2. IF you wanted to compare more, need to add `--p-pairwise` to commands so you can do that. So would just need to change code to indicate multiple comparisons
![[20260306-1905-10.5933673.mp4]]
**After reviewing the PCoA plots again, which other metadata variable would you test with beta group significance (PERMANOVA)?**

![[20260306-1907-14.7512312.mp4]]

![[Recording 20260306120825.m4a]]

---

![[Recording 20260306120947.m4a]]

## **Longitudinal Analysis with Qiime2**

There's a lot you can do:![[Pasted image 20260306083120.png]]
- can make longitudinal volaitility plots - whats happening to alpha diversity over time, but no stats with this. 
- can do linear mixed effects ( show stats for volatility plot)
- First differences RELISTEN
- Distance matrix
	- beta diversity
	- longitudinal first distances
- Linear mixed effects models also an option

![[Recording 20260306122103.m4a]]
- for this lecture will focus on some things but everything for beta can be used for alpha

Here, we will analyze the time-series data associated with this experiment. To use the **longitudinal** plugin, you need a repeated measures sample in which the same site or individual is sampled over time. In the decomp tutorial, **samples from each donor were collected over accumulated degree days**, so we can use this tool to **understand how the microbiome changed during these periods.** Much of these analyses can be done with alpha or beta diversity. For the sake of time, we will focus on just beta diversity. 

Let's open up our **unweighted UniFrac emperor plot** again to help visualize the analysis that we are about to do. **Color the samples by host_subject_id.** Click on the animations tab and select days add_0c for the gradient and **host_subject_id** as the trajectory. Then, click the play button and observe. You can adjust the speed to observe changes in beta diversity at a slower pace.

THIS IS WEIGHTED UniFrac EXAMPLE
- axis 1 almost always time
- axis 1 clustering would be time
- axis 2 other variable. 
- in LMEM in R need to consider all three axes.
![[20260306-1923-00.7454255.mp4]]

We can see visual changes in beta diversity over time, but this wouldn't be very useful for reporting in a manuscript. **Let's look at longitudinal differences with beta diversity so we can start associating numbers and p-values with what we see.**

We want to look at **how samples from an individual donor change along principal coordinates**. So, we will construct a **volatility plot** which will let us **track the beta diversity changes over time within an individual.** Volatility refers to the temporal stability of a metric over time and between subjects. 

This command is a little different than those we've used before because the parameters we are naming can actually be changed in the visualization. That's why they are called the "defaults". Also, note that we are using the output of the core metrics for this analysis.

```r
# make a longitudinal directory  
  
mkdir longitudinal   
cd longitudinal  
  
# construct a volatility plot  
  
qiime longitudinal volatility \
--m-metadata-file ../metadata/metadata.txt \
--m-metadata-file ../core-metrics-results/weighted_unifrac_pcoa_results.qza \
--p-state-column add_0c \
--p-individual-id-column host_subject_id \ #how it accounts for each subject/ repeated sample. Just participants sampling over time
--p-default-group-column 'sample_type' \ #column in metadata to look at over time
--p-default-metric 'Axis 2' \ #time is huge factor on axis 1, but want to know whats on axis 2
--o-visualization pc_vol_sample_type.qzv
```

![[Recording 20260306122805.m4a]]

Let's explore this output in q2view. Using the control bars to the right, look at variation in sample_type and facility along PCs 1, 2, and 3. **What kind of patterns do you see with time along each axis?**
- thick line = mean
- started with sample type. looking at change in axis 2 over time
- most variance in first 3 axes

![[20260306-1929-26.7912624.mp4]]

***Once you have an overview of each axis, you can do linear mixed effects models with the axis of interest as the response variable in R. I will usually test the first 3 axes in R for my variables of interest.** 

Another method is to do a **distance-based analysis using the first-distances method**. We'll use this to test the **hypothesis that sample type affects the magnitude of change in the distance from the first sample collected.** This baseline parameter is used to specify a static time point against which all other time points are compared - if we were to remove this, we would instead look at the rate of change for each individual between each time point. 
 - <span style="color:rgb(238, 170, 170)">can do first differences for alpha diversity</span>
 -<span style="color:rgb(238, 170, 170)"> baseline can be whatever you want</span>

Here, the state column designates the time component in the metadata in this case, add_0c, and the individual id column is used to designate the host subject id sample type. 

Another method is to do a **distance-based analysis using the first-distances method**. In this analysis, we test the hypothesis that **microbial community composition changes over accumulated degree days (ADD)**, measured as the distance from each sample type’s baseline time point (ADD = 0). By specifying a baseline (`--p-baseline 0`), all subsequent time points are compared back to that fixed reference, allowing us to quantify how much each community diverges from its starting composition over time.

The `state` column (`add_0c`) represents accumulated degree days (time), and the `individual-id-column` (`host_subject_id_sample_type`) links repeated measures from the same sample type (soil or skin, facility) across time. If the baseline parameter were omitted, the analysis would instead evaluate change between successive time points (i.e., rate of change rather than deviation from the initial state).

```r
# evaluate using first distances  
  
qiime longitudinal first-distances \
--i-distance-matrix ../core-metrics-results/weighted_unifrac_distance_matrix.qza \
--m-metadata-file ../metadata/metadata.txt \
--p-state-column add_0c \
--p-individual-id-column host_subject_id_sample_type \
--p-baseline 0 \
--o-first-distances from_first_wunifrac.qza
```

We can again use a volatility analysis to visualize the change in beta diversity based on distance.
```r
# visualize volatility  
  
qiime longitudinal volatility \
--m-metadata-file ../metadata/metadata.txt \
--m-metadata-file from_first_wunifrac.qza \
--p-state-column add_0c \
--p-individual-id-column host_subject_id \
--p-default-metric Distance \
--p-default-group-column 'sample_type' \
--o-visualization from_first_wunifrac_vol.qzv
```
- coordinates with stats in R.???????
- These are visualizations. Always look at these before stats and to gudie what you want to test
- skin compared to day 0 hasnt changed in composition too much. If you look in middle left chunk, theres a good slope. 

![[20260306-1934-14.9261593.mp4]]

**Looking at this plot, does one soil type change more over time than the other? What about by facility?**

**Note:** this analysis can also be performed on alpha diversity metrics like Shannon and observed features. From the command above, replace from_first_unifrac.qza file with an alpha diversity file (shannon.qza) and replace the "Distance" metric with "shannon".

Next, we can do a statistical test with a Linear Mixed Effects model. This will let us know if there is a relationship between a dependent variable (diversity) and one or more independent variables, such as accumulated degree days, sample type, facility, etc within the repeated measures. 

Since we are interested in the change in distance from the initial time point, we use Distance for --p-metric.

```r
# run LME - linear mixed effects model
# this code is for QIIME2, but its so much easier in R
  
qiime longitudinal linear-mixed-effects \
--m-metadata-file ../metadata/metadata.txt \
--m-metadata-file from_first_wunifrac.qza \
--p-state-column add_0c \
--p-individual-id-column host_subject_id \ #random effects in individual ID column
--p-formula "Distance ~ add_0c + facility + sample_type" \ #linear mixed effects model formula. distance= dependent, 0=time,??????) model only additive. IF u need to do interactive model MUST be done in R. QIIME cant run interactive models
--o-visualization from_first_wunifrac_lme_formula.qzv
```
- can be done on any of the diversity metrics

![[Recording 20260306124004.m4a]]


There is a new line here, --p-group-columns. While our main question centers around whether accumulated degree day and facility affects the longitudinal change in the microbial community, we also know that sample type plays a large role in shaping the microbial community as well. By including this line, we can account for **all** of these things "**to test whether microbial communities diverge from baseline over accumulated degree days, while controlling for variation due to facility and sample type."**

![[20260306-1938-15.6018810.mp4]]

Let's look at this output in q2view now. **Here, is there a significant association between the sample type and temporal change? What about facility?**  If you need a review from a statistics class to understand what the model outputs mean, the table below may help, but [this videoLinks to an external site.](https://www.youtube.com/watch?v=NIGbYdGErLw&list=PLbVDKwGpb3XmvnTrU40zHRT7NZWWVNUpt&index=23 "(opens in a new window)") may also help (play from about 14 min to 20 min):


![[Pasted image 20260306083303.png]]

### **When is it time to move your longitudinal analysis outside of Qiime2?**
- before u do linear effects models

Here are some R packages that are useful for downstream longitudinal analysis. 

- **Linear Mixed Effects Models (lme4)**
    - Fit linear and generalized linear mixed-effects models.
    - Can model interactions
    - More flexible than the Qiime2 plugin
- **Estimating Marginal Means (emmeans)**
    - Compare treatment groups at specific timepoints
    - Adjust for covariates like age
    - Interpret interactions (e.g., treatment × time)
    - Generate adjusted pairwise comparisons with multiple-testing correction
- **ANCOM-BC2**
    - Adjust for covariates (like age)
    - great for longitudinal studies
    - can include linear effects model in this in R
- **MaAsLin2**
    - Enables multivariable association testing with fixed and random effects

![[Recording 20260306124755.m4a]]

## **Exporting Qiime2 data** 

For downstream analysis outside of qiime2, it is helpful to export qiime2 data so it can be used with other data analysis tools such as R or Python. 

We export QIIME 2 data by unzipping the `.qza` file because a `.qza` artifact is essentially a compressed (zipped) directory. Inside this archive is a structured collection of files, including the actual analysis data (e.g., diversity metrics), as well as supporting files such as provenance information, metadata, version information, and checksums.

The provenance folder records the full analysis history of the artifact, allowing reproducibility and transparency of all upstream steps. The checksums file ensures data integrity by verifying that the contents have not been altered. The `data/` directory contains the primary output of interest (e.g., alpha diversity values or ordination results).

By unzipping the `.qza` file, we can directly access the underlying data files for use in downstream analyses outside of QIIME 2.

**To start, move out of the longitudinal directory.**
```r
cd ../
```

We will export these alpha diversity metrics: **shannon_vector.qza, observed_features_vector.qza, faith_pd_vector.qza, and eveness_vector.qza.** 

**Create a new directory for the data we want to export**
```r
mkdir export
```

**We will start with unzipping the shannon_vector.qza**
```r
unzip core-metrics-results/shannon_vector.qza -d export/shannon
```
 After unzipping, you’ll notice a UUID-named folder inside `export/shannon/`. QIIME 2 stores each artifact inside a uniquely identified directory for provenance tracking. Within that folder, the `data/` directory contains the actual Shannon diversity values (`alpha-diversity.tsv`), which is the file we will use for downstream analysis.

![[Recording 20260306124948.m4a]]

**Let's explore the file structure of the unzipped directory**

**Navigate into the export directory and take a look around.**
```r
cd export   
  
ls
```
What do you see?


**Now navigate into the shannon directory and take a look at what is in the directory.**
```r
cd shannon  
  
ls
```

What do you see? You should see a directory with a bunch of random characters; this is the UUID-named folder.

**Take a look at the data directory**
```r
cd */data  #cd into this folder specifically
  
ls
```

**Now unzip the qza files for the other three alpha diversity metrics.**

Move back into the decomp_tutorial directory
```r
cd ../../../../
#puts us back into the decomp_tutorial folder
```

Run the unzip commands for each diversity metric
```r
# Observed Features  
unzip core-metrics-results/observed_features_vector.qza -d export/observed_features  
  
# Faith's PD  
unzip core-metrics-results/faith_pd_vector.qza -d export/faith_pd  
  
# Pielou's evenness  
unzip core-metrics-results/evenness_vector.qza -d export/evenness
```

**Let's repeat what we did with the alpha diversity metrics with our beta diversity metrics.**
```r
# Bray Curtis  
unzip core-metrics-results/bray_curtis_pcoa_results.qza -d export/bray_curtis  
  
# Jaccard  
unzip core-metrics-results/jaccard_pcoa_results.qza -d export/jaccard  
  
# Unweighted Unifrac  
unzip core-metrics-results/unweighted_unifrac_pcoa_results.qza -d export/unweighted_unifrac  
  
# Weighted Unifrac  
unzip core-metrics-results/weighted_unifrac_pcoa_results.qza -d export/weighted_unifrac
```
- Why do you think for beta diversity we used the pcoa results instead of the distance matrix?


**Let's clean this up a little bit. There are a lot of files, and you only need the .tsv for R.**

Navigate into the export directory
```r
cd export
```

Make an alpha diversity directory
```r
mkdir alpha_div
```

Copy the tsv files into the alpha_div directory
```r
# define alpha metrics  (directory name for each metrics)
metrics=("shannon" "evenness" "faith_pd" "observed_features")  
  
# copy their tsv files into alpha_div/. copies tsv into alpha diversity file
for metric in "${metrics[@]}"; do  
 cp $metric/*/data/alpha-diversity.tsv alpha_div/${metric}.tsv  
done
```
- Organizing the files this way puts all alpha diversity metrics in one clean directory with consistent filenames, making them easier to import into R for downstream statistical analysis and visualization.

**Now we will repeat the same steps with beta diversity.**

Make a beta diversity directory
```r
mkdir beta_div
```

Copy the tsv files into the beta_div directory
```r
# define beta metrics  
metrics=("bray_curtis" "jaccard" "unweighted_unifrac" "weighted_unifrac")  
  
# copy their txt files into beta_div/  
for metric in "${metrics[@]}"; do  
 cp $metric/*/data/ordination.txt beta_div/${metric}.txt  
done
```

Now, if you want to download all of these metrics at once to your local computer, go to OnDemand and select the `export` directory. From there, you can download all of the unzipped files at once. Alternatively, you can navigate into the `export` directory and download only the `alpha_div` or `beta_div` folders, which contain just the TSV files.

We will use these files next week in R. 

---
### Homework #3
- see Homework 3 materials in Modules
- due Thursday at midnight