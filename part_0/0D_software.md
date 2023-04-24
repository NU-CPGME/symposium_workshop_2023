# Installing and managing software: Conda, GitHub, and Docker

### April 24, 2023

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)* 


# Step 1 - Conda <img src="../images/conda.png" width="50"/> 

Conda is a software package manager that allows you to easily install much of the software required for this workshop on your computer. Software is installed into "environments" that can be activated and deactivated from the command line. By setting up conda environments you can have multiple versions of the same software on one computer and avoid conflicts between different version of software packages or incompatible software. This will also allow you to easily install software and and any other programs that are needed to run that software in one step. Another major advantage of Conda (and a reason while we'll be using it for this workshop) is that you can be sure that everyone is using the same version of each software application regardless of when they download it and what computer they are using (good for reproducibility).

For a nice introduction to conda, see [this tutorial](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533) or [here](https://docs.conda.io/projects/conda/en/latest/index.html) for more detail. 

> <img src="../images/warn.png" width="20"/> You may want to consider looking into installing [Mamba](https://mamba.readthedocs.io/en/latest/index.html) instead of Conda. Although Conda is very well-developed, it still often runs into slow-downs or difficulties installing software that can be very hard to puzzle out. Mamba performs the same functions as Conda, but seems to install software much more quickly and smoothly. If you choose to try Mamba instead, for the purposes of this workshop I'd recommend installing Micromamba instead. To do so, replace steps A and B below with the following code for Linux or WSL:
> 
> ```Shell
> curl micro.mamba.pm/install.sh | bash
> ```  
> 
> or on Mac:
> 
> ```Shell
> brew install micromamba
> ```
> 
> Then in every step below everywhere you seen `conda` type `micromamba` instead.
> 


### Step 1.A - Download Miniconda installer:

System requirements and installer downloads for Mac, PC, and Linux can be found at [this site](https://docs.conda.io/en/latest/miniconda.html). The links below are for the latest Python 3.9 versions of the packages for each system:  

* Linux (including Windows WSL): [Miniconda3 Linux 64-bit](https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh)  
* Mac: [Miniconda3 macOS 64-bit pkg](https://repo.anaconda.com/miniconda/Miniconda3-py39_4.11.0-MacOSX-x86_64.pkg) (visual installer)


### Step 1.B - Install Miniconda:  

* Mac: Double-click the downloaded .pkg file. Accept all the default settings.
* Linux: In the terminal window, run

```Shell
bash Miniconda3-latest-Linux-x86_64.sh
```

### Step 1.C - Verify installation:

```Shell
conda list
```

If you see a list of installed packages in your Terminal window, you're good!  

### Step 1.D - Set up bioconda

Enter the following commands, either one at a time or cut and paste all of them into your terminal. The order of the commands is important, though. For more information, check out [Bioconda](https://bioconda.github.io/)

```Shell
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
```

### <a name="condainstall"></a>Step 1.E - Install packages in environments:

Conda environments can be set up by 1) manually creating a new environment and then adding software packages to the environment one at a time, 2) listing the software you want added to the environment when you create it, or 3) by using a specially formatted `environment.yml` file to create the environnment and install all the correct packages in one step. If you want more detail about setting up Conda environments, take a look [here](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

**Option 1**: We will start by creating an environment called "ws_test1", activating the environment, and then installing a single software package called "circlator" with its dependencies.

```Shell
conda create -n ws_test1
conda activate ws_test1
conda install circlator
```

Circlator is a program that can be used to circularise genome assemblies. More info [here](https://github.com/sanger-pathogens/circlator).

When you are done using the environment, you can exit it using the `conda deactivate` command to return to your base environment.

**Option 2**: You can also create an environment and install software in the environment in a single step like so:

```Shell
conda create -n ws_test1 circlator
```

New software can be installed in an existig environment by activating the environment and then using the `conda install` command.

<img src="../images/warn.png" width="20"/> As a warning, the more software packages you install in an environemnt, the more risk you'll have that conflicts will arise. Sometimes it can take a very long time (minutes or hours) for Conda to resolve those conflicts or else it can't resolve them at all. Just be aware. 

**Option 3**: Environemnt files that contain lists of packages and other information can be used to quickly create environments. These are formatted as .yml configuration files.

To create an environment from a .yml environemnt file called "environment.yml", use the following command:

```Shell
conda env create -f environment.yml
```

We are going to use software in conda environments for several exercies in the rest of the workshop.  Use the link below to download the environment files for installation using the method above. 

### [Download Conda environments](https://downgit.github.io/#/home?url=https://github.com/NU-CPGME/symposium_workshop_2023/tree/main/conda_environments) 

> If you get a browser error trying to follow the link above, you should instead go to the DownGit website [https://minhaskamal.github.io/DownGit/#/home](https://minhaskamal.github.io/DownGit/#/home), paste the following link into the box: 
> 
> https://github.com/NU-CPGME/aku_genomics_workshop_2022/tree/master/conda_environments
> 
> and click "Download". 


**Other useful Conda commands**:

* To get a list of all of your installed conda environemnts: `conda env list`
* To remove a conda environment (like the ws_test1 environment we created above): `conda remove -n ws_test1 --all`

# Step 2 - GitHub <img src="../images/github.png" width="50"/> 

We are using Github right now for the workshop documents, but Github's primary use is for software source code development and version control. Often newer versions of software will be available on Github than in Conda. A folder containing source code and supporting documents for a piece of software is called a "repository."

Software or source code can be downloaded directly from the Github website, but it's often more convenient to use the command line to get code from Github.

As an example, we're going to download the source code of [Filtlong](https://github.com/rrwick/Filtlong), a program for quality trimming long sequencing reads generated by Nanopore or PacBio platforms. 

Below are the commands to "clone" (copy) the Github repository for Filtlong onto your computer and compile and test it.

```Shell
mkdir ~/applications
cd ~/applications
git clone https://github.com/rrwick/Filtlong
cd Filtlong
make
bin/filtlong -h
```

Using git and GitHub for software development and version control could be the topic of a whole other workshop. Just know they are very powerful tools for software and other development.

# Step 3 - Docker <img src="../images/docker.png" width="50"/>

Docker is a system for packaging software and all required supporting materials independent of the other software and setup of the computer. Everything is packaged in "containers" that are self-contained. This allows for maximum reproducibility of results and security.

For more information on Docker and containers, [here](https://docs.docker.com/get-started/overview/) is a nice introduction.

---

# [Back to table of contents](../README.md)

---

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.