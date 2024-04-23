# Installing Software with Mamba

### April 23, 2024

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)* 


# Mamba <img src="../images/mamba.png" width="80"/> 

[Mamba](https://mamba.readthedocs.io/en/latest/index.html) (a C++ implementation of the python-based Conda software) is a software package manager that allows you to easily install much of the software required for this workshop on your computer. Software is installed into "environments" that can be activated and deactivated from the command line. By setting up Mamba/Conda environments you can have multiple versions of the same software on one computer and avoid conflicts between different versions of software packages or incompatible software. This will also allow you to easily install software and any other pre-requisite programs that are needed to run that software in one step. Another major advantage of Mamba/Conda (and a reason while we'll be using it for this workshop) is that you can be sure that everyone is using the same version of each software application regardless of when they download it and what computer they are using (good for reproducibility).

For a nice introduction to the basic functionality and commands of Mamba/Conda (essentially the same for both), see [this tutorial](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533) or [here](https://docs.conda.io/projects/conda/en/latest/index.html) for more detail. 


### Step 1 - Download and install micromamba:

For this workshop we'll be using [micromamba](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html) as it is very stippped down, easy to install, and fast. 

If you already have Miniconda, Conda, or Anaconda installed, you can continue to use those for package management or follow the instructions [here](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html) to install Mamba. 

Otherwise, installing micromamba should be a snap:

```
"${SHELL}" <(curl -L micro.mamba.pm/install.sh)
```

If you are on a Mac and would rather use Homebrew:

```
brew install micromamba
```

*Now whenever you want to use a `conda` command in this workshop or for installing other software packages, just use the `micromamba` command instead.*

**Optional extra step:** If you only have micromamba on your system and you think you'll always want to use it in place of Conda, you can create a system alias to call micromamba any time you type the `conda` command:

```
echo 'alias conda="micromamba"' >> ~/.bashrc
source ~/.bashrc
```

### Step 2 - Set up bioconda

Enter the following commands, either one at a time or cut and paste all of them into your terminal. The order of the commands is important, though. For more information, check out [Bioconda](https://bioconda.github.io/)

```Shell
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
```

### <a name="condainstall"></a>Step 3 - Install packages in environments:

Conda environments can be set up by 1) manually creating a new environment and then adding software packages to the environment one at a time, 2) listing the software you want added to the environment when you create it, or 3) by using a specially formatted `environment.yml` file to create the environnment and install all the correct packages in one step. If you want more detail about setting up Conda environments, take a look [here](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

Since they're relatively small and uncomplicated, we'll just create the environments manually here:

---

**A. `assembly` Environment**

```
conda create \
  -n assembly \
  -c conda-forge \
  -c bioconda \
  -c defaults \
  fastqc quast fastp spades
```

---

**B. `annotation` Environment**

```
conda create \
  -n annotation \
  -c conda-forge \
  -c bioconda \
  -c defaults \
  prokka=1.14.6
```

><img src="../images/warn.png" width="25" /> WARNING: If you are using a Mac, the Conda version of prokka is too buggy for us to use. Instead, use the following commands to install prokka:
>
>If you have [HomeBrew](../part_1/1A_computer_preparation.md#step-3---mac-only-install-xcode-command-line-tools-and-homebrew) installed: 
>
>```Shell
>brew install brewsci/bio/prokka
>sudo cpan install Bio::SearchIO::hmmer
>```
>
>If you don't have HomeBrew:
>
>```Shell
>sudo cpan Time::Piece XML::Simple Digest::MD5 Bio::Perl Bio::SearchIO::hmmer
>git clone https://github.com/tseemann/prokka.git $HOME/prokka
>$HOME/prokka/bin/prokka --setupdb
>```

---

**C. `alignment` Environment**

```
conda create \
  -n alignment \
  -c conda-forge \
  -c bioconda \
  -c defaults \
  ivar snippy=4.6.0 snpeff=4
```

> <img src="../images/warn.png" width="25" /> NOTE: On MacOS, the conda version of snippy is broken. To install manually if you have [HomeBrew](../part_1/1A_computer_preparation.md#step-3---mac-only-install-xcode-command-line-tools-and-homebrew): 
> 
> ```Shell
> brew install brewsci/bio/snippy
> brew install brewsci/bio/vt
> ```
> 
> If you don't have HomeBrew:
> 
> ```Shell
> git clone https://github.com/tseemann/snippy.git $HOME/snippy
> echo "export PATH=$HOME/snippy/bin:\$PATH" >> ~/.bashrc
> source ~/.bashrc
> snippy --check
> 
> ``` 
> 
> Then install just ivar in the conda environment:
> 
> ```Shell
> conda create -n alignment -c conda-forge -c bioconda -c defaults ivar
> ``` 
>
> 

---

**D. `phylogenetics` Environment**

```
conda create \
  -n phylogenetics \
  -c conda-forge \
  -c bioconda \
  -c defaults \
  treetime iqtree mafft
```


---

# [Back to table of contents](../README.md)

---

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.