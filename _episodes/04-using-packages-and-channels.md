---
title: "Using Packages and Channels"
teaching: 20
exercises: 10
questions:
- "What are Conda channels?"
- "What are Conda packages?"
- "Why should I be explicit about which channels my research project uses?"
objectives:
- "Install a package from a specific channel."
keypoints:
- "A package is a tarball containing system-level libraries, Python or other modules, executable programs and other components, and associated metadata."
- "A Conda channel is a URL to a directory containing a Conda package(s)."
- "Explicitly including the channels (and their priority!) in a project's environment file is necessary for another researcher to completely re-create that project's software environment." 
---

## What are Conda packages?

A [conda package][conda-pkg-docs] is a compressed tarball file (`.tar.bz2`) that contains:

* system-level libraries
* Python or other modules
* executable programs and other components
* metadata under the `info/` directory
* a collection of files that are installed directly into an `install` prefix.

Conda keeps track of the dependencies between packages and platforms; the conda package format is 
identical across platforms and operating systems.

### Package Structure 

All conda packages have a specific sub-directory structure inside the tarball file. There is a 
`bin` directory that contains any binaries for the package; a `lib` directory containing the 
relevant library files (i.e., the `.py` files); and an `info` directoy containing package metadata. 
For a more details of the conda package specification, including discussions of the various 
metadata files, see the [docs][conda-pkg-spec-docs].

As an example of Conda package structure consider the [Conda](https://pytorch.org/) package for 
Python 3.6 version of PyTorch targeting a 64-bit Mac OS, `pytorch-1.1.0-py3.6_0.tar.bz2`.

<div class="highlight-bash notranslate"><div class="highlight"><pre><span></span>.
├── bin
│   └── convert-caffe2-to-onnx
│   └── convert-onnx-to-caffe2
├── info
│   ├── LICENSE.txt
│   ├── about.json
│   ├── files
│   ├── git
│   ├── has_prefix.json
│   ├── hash_input.json
│   ├── index.json
│   ├── paths.json
│   ├── recipe/
│   └── test/
└── lib
    └── python3.6
        └── site-packages
            ├── caffe2/
            ├── torch/
            └── torch-1.1.0-py3.6.egg-info/
</pre></div>
</div>

A complete listing of available PyTorch packages can be found on 
[Anaconda Cloud](https://anaconda.org/pytorch/pytorch/files).

## What are Conda channels?

Again from the [Conda documentation][conda-channels-docs], conda packages are downloaded from 
remote channels, which are URLs to directories containing conda packages. The `conda` command 
searches a default set of channels, and packages are automatically downloaded and updated from the 
[Anaconda Cloud channels](https://repo.anaconda.com/pkgs/). 

*   `main`: The majority of all new Anaconda, Inc. package builds are hosted here. Included in 
    conda's defaults channel as the top priority channel.
*   `r`: Microsoft R Open conda packages and [Anaconda, Inc.'s R conda packages](https://anaconda.org/r/repo). 
    This channel is included in conda's defaults channel. When creating new environments, MRO is 
    now chosen as the default R implementation.

Collectively, the Anaconda managed channels are referred to as the `defaults` channel because, 
unless otherwise specified, packages installed using `conda` will be downloaded from these 
channels. 

> ## The `conda-forge` channel
>
> In addition to the `default` channels that are managed by Anaconda Inc., there is another 
> channel called that also has a special status. The [Conda-Forge](https://github.com/conda-forge) 
> project "is a community led collection of recipes, build infrastructure and distributions for 
> the conda package manager."
>
> There are a few reasons that you may wish to use the `conda-forge` channel instead of the 
> `defaults` channel maintained by Anaconda:
> 
> 1. Packages on `conda-forge` may be more up-to-date than those on the `defaults` channel.
> 2. There are packages on the `conda-forge` channel that aren't available from `defaults`.
> 3. You may wish to use a dependency such as `openblas` (from `conda-forge`) instead of `mkl` 
> (from `defaults`).
{: .callout}

> ## The `bioconda` channel
>
> Another useful channel is  [bioconda](https://bioconda.github.io/) 
> Bioconda is a channel for the conda package manager specializing in bioinformatics software
{: .callout}

## How do I install a package from a specific channel?

You can install a package from a specific channel into the currently activate environment by 
passing the `--channel` option to the `conda install` command as follows.

~~~
$ conda install bedtools --channel bioconda
~~~
{: .language-bash}

You can also install a package from a specific channel into a named environment (using `--name`) 
or into an environment installed at a particular prefix (using `--prefix`). For example, the 
following command installs the `scipy` package from the `conda-forge` channel into the environment 
called `my-first-env` which we created eariler.

~~~
$ conda install bedtools --channel bioconda --name my-first-env
~~~
{: .language-bash}

This command would install `bedtools` package from `bioconda` channel into an environment 
installed into the `env/` sub-directory.

~~~
$ conda install bedtools --channel bioconda --prefix ./env
~~~
{: .language-bash}

Here is another example for R users. The following command would install 
[`r-tidyverse`](https://anaconda.org/r/r-tidyverse) package from the `r` channel into an 
environment installed into the `env/` sub-directory.

~~~
$ conda install r-tidyverse --channel r --prefix ./env
~~~
{: .language-bash}

In this case the `--channel` option is unnecessary because the `r` channel is included by default. 
The following works just as well!

~~~
$ conda install r-tidyverse --prefix ./env
~~~
{: .language-bash}

> ## Channel priority
> 
> You may specify multiple channels for insalling packages by passing the `--channel` argument 
> multiple times.
> 
> ~~~
> $ conda install scipy --channel conda-forge --channel bioconda
> ~~~
> {: .language-bash}
>
> Channel priority decreases from left to right - the first argument has higher priority than the 
> second. For reference, bioconda is a channel for the conda package manager specializing in 
> bioinformatics software. For those interested in learning more about the Bioconda project, 
> checkout the project's [GitHub](https://bioconda.github.io/) page.
{: .callout}

## What actually happens when I install packages?

During the installation process, files are extracted into the specified environment (defaulting to 
the current environment if none is specified). Installing the files of a conda package into an 
environment can be thought of as changing the directory to an environment, and then downloading 
and extracting the package and its dependencies.

For example, when you `conda install` a package that exists in a channel and has no dependencies, 
conda does the following.

1. looks at your configured channels (in priority)
2. reaches out to the repodata associated with your channels/platform
3. parses repodata to search for the package
4. once the package is found, conda pulls it down and installs

The [conda documentation][conda-install-docs] has a nice decision tree that describes the package installation process.

<p align="center">
    <img alt="Installing with Conda" src="../fig/installing-with-conda.png" width="250">
</p>

> ## Specifying channels when installing packages
>
> Like many projects, [PyTorch](https://pytorch.org/) has its own 
> [channel](https://anaconda.org/pytorch) on Anaconda Cloud. This channel has several interesting 
> packages, in particular `pytorch` (PyTorch core) and `torchvision` (datasets, transforms, and 
> models specific to computer vision).
> 
> Create a new directory called `my-computer-vision-project` and then create a Python 3.6 
> environment in a sub-directory called `env/` with the three packages listed above. Also include 
> the most recent version of `jupyterlab` in your environment (so you have a nice UI) and 
> `matplotlib` (so you can make plots).
> 
> > ## Solution
> > 
> > In order to create a new environment you use the `conda create` command as follows.
> > 
> > ~~~
> > $ mkdir my-computer-vision-project
> > $ cd my-computer-vision-project/
> > $ conda create --prefix ./env --channel pytorch \
> > > python=3.6 \
> > > jupyterlab \
> > > pytorch \
> > > torchvision \
> > > matplotlib
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

> ## Alternative syntax for installing packages from specific channels
> 
> There exists an alternative syntax for installing conda packages from specific channels that 
> more explicitly links the channel being used to install a particular package.
> 
> ~~~
> $ conda install conda-forge::tensorflow  --prefix ./env
> ~~~
> {: .language-bash}
>
> Repeat the previous exercise using this alternative syntax to install `python`, `jupyterlab`, 
> and `matplotlib` from the `conda-forge` channel and `pytorch` and `torchvision` from the 
> `pytorch` channel.
>
> > ## Solution
> > 
> > One possibility would be to use the `conda create` command as follows.
> > 
> > ~~~
> > $ mkdir my-computer-vision-project
> > $ cd my-computer-vision-project/
> > $ conda create --prefix ./env \
> > > conda-forge::python=3.6 \
> > > conda-forge::jupyterlab \
> > > conda-forge::matplotlib
> > > pytorch::pytorch \
> > > pytorch::torchvision
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

{% include links.md %}

