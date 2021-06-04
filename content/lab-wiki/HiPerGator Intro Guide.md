+++
title = "HiPerGator Intro Guide"
category = "Computing"
+++

# So you want to run your R or python script on the HiperGator
 
## Introduction
This guide gives a high level overview of how one goes about running R or python scripts on a high performance cluster (HPC). There are no coding examples here, and instead is designed to give someone a frame of reference for how to approach things, and where other more detailed tutorials fit in the larger picture. The expected user is someone who is comfortable doing analysis and writing scripts in R using RStudio, or python with various IDEs, and has no HPC experience.

This is written for users of the [UFL HiperGator](https://www.rc.ufl.edu/services/hipergator/) who code in R or python, but most information will apply to potential users for any HPC system and with any scripting language.

## HPC Use cases
There are two scenarios where you may want to run your analysis script on the HPC.  

1. **Your code takes a very long time to run**  
If your code is taking several hours, days or more to run on your personal computer then using an HPC is likely a good option for two reasons. One is the servers on an HPC have processors much faster than desktops and laptops, so with minimal changes your code will run significantly faster. Second is scripts on an HPC run independently of your personal computer, so you can shutdown your personal computer while the script on the HPC runs overnight or over the weekend. HPC systems can have a time limit of several weeks to a month for any single job.  
If your script takes so long that it seems like it will never finish then an HPC can be especially beneficial. Say you do a test run on 1% of your data and it takes 2 days to run. Theoretically it will then take 200 days to run on your full dataset. In this case the benefit of the HPC is its parallel processing power.  
By default R and python scripts run on a single processor. Most computers today have 4-8 processors though. And HPC servers have upwards of 64. If you spread the work out to multiple processors you can significantly decrease the amount of time it takes to run. For example: a script that takes 1 hour to run can potentially take 0.5 hours with 2 processors, or 15 minutes with 4 processors, or 7.5 minutes with 4 processors, and so on. There is computational overhead with parallel processing though so halving the time with a doubling of the number of processors is only a general rule. This is not a straightforward change to your code though and it will take time. See below about making scripts parallel and whether it’s even worth it. 


2. **Your script fills up your computer's memory and crashes when it runs.**  
When you have large datasets, such as raster files, it’s easy to use up all the memory and freeze your computer. As opposed to just waiting on long running scripts, in this case it makes it nearly impossible to do analysis. Just like HPC servers have powerful processors, they also have extremely large amounts of memory. Usually greater than 100GB. There is a good chance that they can handle whatever large datasets you throw at them.

## Should I bother with an HPC?  
Your analysis and data can be of *any* size. There is no minimum computational requirement to use an HPC. But understand there is a time cost involved with learning how to interact with an HPC and also optimizing your code so it runs most efficiently. Therefore, in some cases it isn’t worth it porting scripts to an HPC system. 

Consider an example where you have a script that takes 1 hour on your laptop, and you must run it once a month. It’s likely reasonable to just keep that workflow. But if a script takes 10 hours and you must run it once a week, then it’s worth considering doing it on an HPC. Especially since that will decrease the wear and tear on your laptop and free up that 10 hours for other uses. 

Whether it’s worth it or not is unique to every situation. Also remember that once you learn all the HPC basics the first time, then that time cost isn't needed for your next project. 

Also consider that the two use cases described above might also be solvable by code optimization. If you can find a section of code which is slow and make it run fast enough to meet your needs, that is preferable over running the code on an HPC. There is no one solution to this, but a good starting point is Hadley Wickham’s Advanced R tutorial on [Performance and Profiling](http://adv-r.had.co.nz/Performance.html). This [45 minute video](https://youtu.be/K_90QGUPYCA) also gives a great overview of profiling, optimization, parallel processing, and the implications in R. 

## What exactly is an HPC?
A high performance cluster (HPC) is primarily two things.  

1. It’s hundreds of individual servers in a data center. Each server is a computer just like your personal computer, but has more powerful components, and does not have a graphical user interface or even a monitor. You interact with the servers via the command line. If you’ve never used the command line consider it like the Rstudio console, or a python prompt, was your *only* way to interact with a computer. More on this below. 

2. It’s a system for scheduling, prioritizing, and running scripts from hundreds of users. This is how the hundreds of servers can be used as “one”. Access to them is controlled by scheduling programs which you interact with, which then put your scripts in a queue to be run when resources are free. Slurm is probably the most popular scheduler (and the one used on the HiperGator) but some HPC systems may use other ones like [PBS or MOAB](https://en.wikipedia.org/wiki/Job_scheduler#Batch_queuing_for_HPC_clusters).

## Primary Steps to running your code on an HPC.
1. **You need an account**.  
    Signup for HiperGator HPC account [here](https://www.rc.ufl.edu/access/account-request/).

2. **You must be able to login to the HPC via SSH and use the command line.**  
    The command line can also be referred to as the “unix shell”. With this you use text commands (just like the RStudio console) to copy files, edit text files, interact with the scheduler, view job status, etc. See the [Hipergator Connection guide](https://help.rc.ufl.edu/doc/Getting_Started#Connecting_to_HiPerGator).

    Some unix tutorials:
        - Scinet [Geospatial Unix Intro](https://geospatial.101workbook.org/IntroductionToCommandLine/Unix/unix-basics-1.html)
        - Data Carpentry [tutorial on the unix shell](https://swcarpentry.github.io/shell-novice/)

    If you work on a Mac computer you have a full unix shell available already called the Terminal. For Windows users there are several options available. See the bottom of the Setup page for the Data Carpentry unix shell tutorial. 


3. **You must optimize your code to run on the HPC.**  
    This is potentially the trickiest part. At a minimum your code must be able to run independently without any interaction from you. Do you have one large (or even several) analysis script where you highlight different parts to run in the correct order? Or to check output before moving on? That will not work on an HPC. A single R or python file (aka a script) must run from start to finish and write some results to a file to be able to be useful on the HPC. 

    For R, a good test for this is the Jobs tab in RStudio (next to Console and Terminal tabs. Not available on older versions of RStudio). This is *very* analogous to running a script in an HPC environment. If your script can run as an RStudio Job *without* copying the local environment or copying the job results anywhere (your script should write results to some file) then it should be able to run on the HPC.

    For python a good test is being able to run the script via the command line (ie. `python my_analysis.py`). If you are using an IDE (like spyder or pycharm) then running a full script from start to finish using the “Run” option should also be sufficient.

    Having a script run without any interaction does not necessarily mean it needs to have parallel processing. See Should I make my code run parallel? below. 

4. **You have to get your code and data onto the HPC.**  
    You’ll need to use special programs to transfer files (both data and scripts) from your local computer to the HPC. For windows this will be the WinScp program, which will have the same username and password as logging into the command line. For mac or linux users you can use the Terminal to transfer files via the command line using the scp command. Read more about the [scp command here](https://scinet.usda.gov/guide/ceres/#using-scp-to-transfer-data). More on data transfer for HiperGator can be found here. Something you’ll see mentioned a lot is Globus, which is a useful (but not strictly required) tool when you need to transfer 100GB+ of data. 

5. **You need to ensure you have the correct packages.**  
    Most HPC systems will have common packages installed and ready to use. If not you’ll have to install them yourself. If you do this then the latest versions will be installed on the HPC, so it’s good practice to make sure all packages on your personal computer are up to date to so they match (in RStudio use Tools-> Check for package updates, in python use conda or pip to update all package to the latest version).
    For python packages for your projects you’ll want to use environments with either conda or python virtual environments.  

    Also take note of the [module](https://help.rc.ufl.edu/doc/Modules_Basic_Usage) command on the HiperGator. This is used to load preloaded software, including R and python themselves. This is covered is most tutorials about batch scripts (see next section).

6. **You can now submit your scripts to the scheduler.**  
    Once your data and scripts are on the HPC system you can submit them to the scheduler to run. This involves defining “jobs” where you tell the scheduler what you need. Specifically: the location of your script, the resources needed (cpus and memory), the time needed to run your script, the location for putting script output and logs, etc. 

    Jobs are defined via batch scripts which have a line for each piece of information.

    Some examples:  
    
    - A [USDA tutorial on batch scripts](https://geospatial.101workbook.org/Workshops/2-Session2-intro-to-ceres.html#batch-computing-on-ceres).
    - A [sample of Hipergator batch scripts](https://help.rc.ufl.edu/doc/Sample_SLURM_Scripts).
    - An [annotated Hipergator batch script](https://help.rc.ufl.edu/doc/Annotated_SLURM_Script).

7. **You might need to debug your script if it doesn’t run correctly.**  
    It’s very common for scripts to not run the first time because they were written on a personal computer and things like directory paths and packages may be different. In this case it’s useful to debug your script on the HPC directly. 
    A good place to do this is an “interactive node” or “interactive session”. For these instead of submitting a job to the queue, you request a new unix shell where a small amount of resources are available. Here you can run your scripts via the Rscript or python command and see the output directly, and make adjustments as needed until it runs successfully. 

    - [HiperGator guide on interactive sessions](https://help.rc.ufl.edu/doc/Development_and_Testing)
    - [HiperGator guide on interactive sessions when using GPUs](https://help.rc.ufl.edu/doc/GPU_Access#Interactive_Access)

8. **You can now get your results back.**  
    This is the same process as putting scripts and data onto the HPC but in reverse. 

## Should I make my code run on parallel processes?
Before you dive into making your script parallel, do a quick cost/benefit analysis. It may indeed take a full day or more to redo your code to take advantage of parallel processing, but the benefits could be extremely large. If your code already runs in a relatively short time, like a few hours on your laptop and less than an hour on the HPC without any modification, and you're happy with that then making it parallel might not be worth it. 

If you do not use parallel processing then your jobs will always request just a single processor. This is perfectly fine as there is no minimum requirement for using an HPC. 

## Make your scripts use parallel processing
By default R and python run on a single processor. Most computers today have 4-8 processors. If you spread the work out to multiple processors you can significantly decrease the amount of time it takes to run. 
For example: a script that takes 1 hour to run can potentially take 30 minutes with 2 processors, or 15 minutes with 4 processors. To make your scripts run across multiple processors, you'll have to make some adjustments to your code.

For R users, if your code uses lapply to run your main function to many items (e.g. fitting the same model to many species), you can swap it for mclapply from the parallel package without making any substantial changes. For more details and advanced uses, here are some short tutorials that go over this:  

- [A brief foray into parallel processing with R](https://beckmw.wordpress.com/2014/01/21/a-brief-foray-into-parallel-processing-with-r/)
- [Software Carpentry Parallel Processing in R](http://resbaz.github.io/r-intermediate-gapminder/19-foreach.html)
- [Software Carpentry Parallel Processing in python](https://swcarpentry.github.io/python-intermediate-mosquitoes/04-multiprocessing.html)
    
Some notes:

- If your code already uses functions and for loops, it should be straightforward to make it parallel, unless each pass through the loop depends on the outcome from previous passes.
- On your own computer, never set the amount of processors used to the max available. This will take away all the processing power needed to run the operating system, browser, and other programs, and could potentially crash your computer. To test out parallel code on my computer I set the number of processors to use at 2 (out of 8 available). Then when the scripts are moved to the HPC I set the amount to something higher.

## What about distributed computing?
The links and examples for parallel computing above show you how to utilize the multiple processors in a single system. In the case of the HPC this means up to (usually) 64-128 processors on a single server. But what if you still need more processing power? In this case it’s possible to write parallel code which takes advantages of the processors on *numerous* individual servers. This is how one utilizes 100’s or even 1000’s of processors.

Going from single system parallel processing to distributed computing with your script is possible but will likely take even more work on your part. For this you might come across tutorials using MPI technology. MPI (Message-Passing Interface) is the protocol for analysis scripts to communicate between servers in an HPC environment, thus enabling distributed computing. Packages to use MPI are available in all common languages such as R, python, julia, and matlab. 

Newer packages are available which either deal with MPI in the background for you, or implement newer protocols. The R package [batchtools](https://mllg.github.io/batchtools/index.html) has many high end functions for distributed computing. The python package [dask](https://docs.dask.org/) is a state of the art package for distributed computing, and the accompanying [jobqueue](https://jobqueue.dask.org) package integrates it with SLURM and other HPC schedulers. 

## Other considerations and important points
The Research Computing group has a [wiki on HiperGator usage](https://help.rc.ufl.edu/doc/UFRC_Help_and_Documentation).  

Some common tasks and scripts are outlind in the [HiPerGator Reference Guide](/lab-wiki/hipergator-reference-guide/)

**Login Node**: When you sign into the HPC there is a single landing server which you’ll start on. It’s important to never run actual scripts on this initial server. It should be used to submit jobs, request development nodes or interactive sessions, or transfer data in or out.  

**Partitions**: HiperGator resources are divided into partitions, where each partition has a specific set of hardware and resource and tim**e limits. Whenever you request resources you’ll specify which partition you want to use. See the [partitions wiki page](https://help.rc.ufl.edu/doc/Available_Node_Features).  

**Account limits**: The resources you request (eg. number of processors and amount of memory) is limited by how many credits your group has purchased. The number of jobs which can be running concurrently is also determined by this. See more on the [account limits wiki page](https://help.rc.ufl.edu/doc/Account_and_QOS_limits_under_SLURM). This is also referred to as QOS (quality of service), a term coined in the [early internet days](https://en.wikipedia.org/wiki/Quality_of_service).  

**Processors/CPU/Cores/Sockets/Threads**: Each of these things is technically different and has a distinct definition. For most users of an HPC system they can be thought of interchangeably though. When you request resources via a batch script, or other method, you’ll usually ask for multiple CPUs to implement parallel processing and leave it at that. Advanced users can read about different terminology [here](https://login.scg.stanford.edu/faqs/cores/) or [here](https://slurm.schedmd.com/mc_support.html).  
