# Managing conda envs for the Hub

Hard won notes by JMS


## Current versions

* anaconda-6
* debian7, debian10 (latter needs to be fixed!!)
* python 3.7

## Notes

### Install Python packages into envs

New python packages should be installed with conda if possible. Sets of packages used together can be collected into different envs. Don't add package installations to the base env!

The point is when we install lots of (potentially conflicting) packages into
a *clean new python environment* there's a simplicity to ensuring that versions
are proper. We round up the packages that interoperate, and install them.

### No need to clone 'base'

Recall that when we are running in the Jupyter installation we are always piggybacking any env we
%use on top of base so we get the base packages for free (scipy, hublib, etc.)!

### Be cautious with upgrades

I've recently experienced headaches with an env that installed with Python 3.9 that necessitated
reinstall specifying 3.7. Don't go too far afield from what's on the base env.

[Conda package management](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html)
[Conda environment management](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)

## Procedures

### Checking for packages

- use anaconda.org to search for some packages

### Install procedure

Recommend redirecting/teeing the install output to a text file for later scrutiny.

1. Ensure you start out in the appropriate container (Workspace for debian7; Workspace10 for debian10)

1. Enable the anaconda installation
```
   sudo su - apps
   source /etc/environ.sh
   use anaconda-6

```

1. Check the environment:
```
   which python
   which conda
```

1. Create the env (limit Python version as appropriate); install packages. Many packages will come
from conda-forge. It's best to install them all at once if possible, to ensure dependencies are met.
```
   conda create -n environment_name python=3.7
   conda activate environment_name
   conda install -c conda-forge package1 package2 package3

```
1. Test in the Python command line--run python and import a package or two to verify.

    Additionally, check `env` for useful environment variables--you may need to set some in the use
    config.

1. Create 'use' configs

    The use config is what actually enables importing of the env's packages, inside the Hub's
    Jupyter notebooks. If you fat-finger some syntax there is not a lot of helpful feedback!
    
    * Refer to other use configs in `/apps/share64/debianX/environ.d/`
    * Prepend the new env's `bin/` directory to the `$PATH`
    * Have a look at other environment variables, which are sometimes needed (apparently not [PYTHONPATH](https://stackoverflow.com/questions/17386880/does-anaconda-create-a-separate-pythonpath-variable-for-each-new-environment), however... 


1. Check your work from the Jupyter install: hublib.use

 Import the hublib.use library into the jupyter notebook, which allows the %use magic to be invoked to bring the python environment in.
	
[https://hubzero.github.io/hublib/use.html](https://hubzero.github.io/hublib/use.html)
	
NOTE that conda will not always report the env as expected even once `%use` has been invoked. You still have access to the packages and binaries!!

In the notebook we say:
```
  import hublib.use
  %use *my_important_env*

  import *env_package*
```
