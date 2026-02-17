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
        - 2. DADA2 TABLE FILE:
        - 3. DADA2 SEQS FILE:
        - Running jobs on Alpine
        - Summary
            - Cow Dataset for Homework:
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
    - **Primarily used for script editing and job submissions only**   
- **2. Compile node**
    - Translates human-readable source code that we give into computer-executable machine code
    - **Primarily used to install software**
    - Use `acompile` to start a compile session  
-  **3. Compute node**
    - We do not navigate into these
    - **This is where jobs run after submission**
    - There are different kinds of compute nodes
    - Here is a list of the compute node resources available on Alpine ([link to more detailed info hereLinks to an external site.](https://curc.readthedocs.io/en/latest/clusters/alpine/alpine-hardware.html "(opens in a new window)") )
    - Use `sbatch` to submit jobs  
- **4. Interactive node**
    - **We can navigate here to run jobs and commands interactively** 
    - We will use an interactive node to run our Qiime2 commands
    - Use `ainteractive` to start an interactive session
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
- <mark style="background: #FFF3A3A6;">table.qza</mark> (ASV feature table)
- <mark style="background: #FFF3A3A6;">seqs.qza</mark> (sequences associated with ASVs)
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

### **2. DADA2 TABLE FILE:** 
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