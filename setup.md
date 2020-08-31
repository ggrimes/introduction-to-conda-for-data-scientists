---
layout: page
title: "Setup"
permalink: /setup/
root: ..
---

# Conda on Eddie

Log into eddie using your terminal application.

Load the anaconda module

~~~
module load anaconda/5.0.1
~~~

WARNING: If you use conda to create environments, your home directory may fill up. 
Please see our documentation at https://www.wiki.ed.ac.uk/display/ResearchServices/Anaconda for advice.

## Create a 'conda' environment

First choose a location to store your conda environment. We recommend using the cluster filesystem (/exports/<COLLEGE>/eddie/...) for this, as the environments can get quite large. If you use your home directory under /home, it could rapidly fill-up and go over quota.

Configure the path for your environments directory, i.e, the directory where all your conda environments will be stored. Note that this should be an existing directory, so you need to create it first if it doesn't already exist, before running the command below.

~~~
$ module load anaconda
$ conda config --add envs_dirs /exports/<COLLEGE>/eddie/<SCHOOL>/groups/<GROUP NAME>/anaconda/envs
~~~

# Installation Instructions

If you wish to get started with this course without installing Conda, then you can use a 
pre-configured instance running on [Binder](https://mybinder.org/) by clicking on the link below.

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/carpentries-incubator/introduction-to-conda-for-data-scientists/binder?urlpath=lab)

## Check to see if Conda is already installed

If you have ever installed the [Anaconda Python distribution](https://www.anaconda.com/distribution/) 
on your local machine, then you already have Conda installed! Mac and Linux users can check 
whether Conda is installed by running the following command in a terminal.

~~~
$ which conda
/Users/$USERNAME/miniconda3/bin/conda
~~~
{: .language-bash}

If Conda has already been installed on your machine, then you this command should return the 
absolute path to the conda executable. 

Windows users should search for "Anaconda" to see if the "Anaconda Command Prompt" shows up as an 
option, if it does then you already have Conda installed.

## Install Python 3 version of Miniconda

If Conda has not been installed on your machine, then install the Python 3 version of 
[Miniconda](https://docs.conda.io/en/latest/miniconda.html) from Anaconda for your OS. As the name 
suggests, Miniconda is a "mini" version of the 
[Anaconda Python distribution](https://www.anaconda.com/distribution/) that includes only Conda, a 
Python 3 distribution, and any necessary OS-specific dependencies.

For convenience here are links to the 64-bit GUI Miniconda installers.

* [Windows](https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe)
* [Mac OSX](https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.pkg)
* [Linux](https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh) 

> ## Prefer Miniconda to Anaconda
>
> I suggest installing Miniconda which combines Conda with Python 3 (and a small number of core 
> systems packages) instead of the full Anaconda distribution. Installing only Miniconda will 
> encourage you to create separate environments for each project (and to install only those packages 
> that you actually need for each project!). Project specific environments enhance portability and 
> reproducibility of your research and workflows. 
> 
> Besides, if you *really* want the full Anaconda distribution you can always create an new conda 
> environment and install it using the following command.
>
> ~~~
> $ conda create --name my-anaconda-env anaconda=5.3
> ~~~
> {: .language-bash}
>
> We will discuss the above command in great depth in the workshop.
{: .callout}

### Linux installation

I will walk through the steps for installing on Linux systems below as installing on Linux systems 
is slightly more involved. First, download the 64-bit Python 3 install script for Miniconda 
(clicking the link above will download the same script!).

~~~
wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
~~~
{: .language-bash}

Run the Miniconda install script. Follow the prompts on the installer screens. If you are unsure 
about any setting, accept the defaults (you can change them later if necessary).

~~~
bash Miniconda3-latest-Linux-x86_64.sh
~~~
{: .language-bash}

Once the install script completes, you can remove it.

~~~
rm Miniconda3-latest-Linux-x86_64.sh
~~~
{: .language-bash}

## Verifying your Conda installation

In order to verify that you have installed Conda correctly run the `conda help` command. Output 
of the command should look similar to the following.

~~~
$ conda help
usage: conda [-h] [-V] command ...

conda is a tool for managing and deploying applications, environments and packages.

Options:

positional arguments:
  command
    clean        Remove unused packages and caches.
    config       Modify configuration values in .condarc. This is modeled
                 after the git config command. Writes to the user .condarc
                 file (/Users/drpugh/.condarc) by default.
    create       Create a new conda environment from a list of specified
                 packages.
    help         Displays a list of available conda commands and their help
                 strings.
    info         Display information about current conda install.
    init         Initialize conda for shell interaction. [Experimental]
    install      Installs a list of packages into a specified conda
                 environment.
    list         List linked packages in a conda environment.
    package      Low-level conda package utility. (EXPERIMENTAL)
    remove       Remove a list of packages from a specified conda environment.
    uninstall    Alias for conda remove.
    run          Run an executable in a conda environment. [Experimental]
    search       Search for packages and display associated information. The
                 input is a MatchSpec, a query language for conda packages.
                 See examples below.
    update       Updates conda packages to the latest compatible version.
    upgrade      Alias for conda update.

optional arguments:
  -h, --help     Show this help message and exit.
  -V, --version  Show the conda version number and exit.

conda commands available from other packages:
  env
~~~
{: .language-bash}

At the bottom of the help menu you will see a section with some optional arguments for the 
`conda` command. In particular you can pass the `--version` flag which will return the version 
number. Again output should look similar to the following.

~~~
$ conda --version
conda 4.8.2
~~~
{: .language-bash}

## Make sure you have the most recent version

Once Conda exists on your machine, then run the following command to make sure that you 
have the most recent version and patches.

~~~
$ conda update --name base --channel defaults --yes conda
~~~
{: .language-bash}

You can re-run this command at any time to update to the most recent version of Conda.

## Initializing your shell for Conda

Key parts of Conda's functionality require that it interact directly with the shell within which 
Conda commands are being invoked as such each shell must be configured to make use of them. The 
`conda init` command initializes a shell for use with Conda by making changes to your system that 
are specific and customized for each shell. Conda supports a number of different shells and you 
can run `conda init --help` to see the complete list.

Mac OSX and Linux users will want to initialize Conda for Bash as follows. If you are installing 
on Linux, then you may be prompted to initialize Conda for your shell when running the installation 
script. If so, then you can safely skip this step.

~~~
$ conda init bash
~~~
{: .language-bash}

Windows users can either use the Anaconda Command Prompt or the Anaconda Powershell Prompt which 
are already initialized for Conda or they can initialize Conda for Powershell as follows.

~~~
> conda init powershell
~~~

After running `conda init` you will need to close and restart your shell for changes to take 
effect. Alternatively, Mac OS and Linux users can reload your `~/.bashrc` profile (which was 
changed by running the `conda init` command). To reload your `~/.bashrc` profile, use the 
following command.

~~~
$ source ~/.bashrc
~~~
{: .language-bash}

If you want to reverse or “undo” the changes made by `conda init`, then you can re-run the 
`conda init` command and pass the `--reverse` option. Again, in order for the reversal to take 
effect you will likely need to close and restart your shell session.

## Workspace Setup

In order to maintain a consistent workspace for all learners create an 
`introduction-to-conda-for-data-scientists` directory on your Desktop to serve as the working 
directory for the duration of the lesson. On Mac OSX and Linux running following commands in the
Terminal will create the required directory on the Desktop.

~~~
$ cd ~/Desktop
$ mkdir introduction-to-conda-for-data-scientists
$ cd introduction-to-conda-for-data-scientists
~~~
{: .language-bash}


For Windows users you will need to reverse the direction of the slash (because Windows) and run 
the commands from the command prompt.

~~~
> cd ~\Desktop
> mkdir introduction-to-conda-for-data-scientists
> cd introduction-to-conda-for-data-scientists
~~~

Alternatively, you can always "right-click" and "create new folder" on your Desktop.

