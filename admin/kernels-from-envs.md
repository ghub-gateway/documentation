# Creating Anaconda kernels from conda envs

* Don't conda install into base env
* If you must pip install into an env (e.g. hublib!), do so after conda installs
* In the env, install any packages from base that the user will need. We can't stack kernels on base.

## Create the env

1. Become the apps user, enable anaconda, note environment:

```
sudo su - apps
use -e anaconda-6

which python
which conda
```

1. Create the env, install packages. 

* Using conda-forge is ok. Don't get too weird.
* Limit python version
* Install any packages from base that the user will need
* Also install ipykernel package, needed to create kernels 

```
conda create -n environment_name python=3.7 
source activate environment_name
conda install -c conda-forge ipykernel package1 package2 package3
```

Any pip installs should be installed last for safety, e.g.

```
pip install hublib
```

NOTE: You can continue to install more packages into the env, even after it's deployed as a kernel. 
A simple refresh of the Jupyter UI will make new packages available.

## Install the env as a kernel

As apps user: activate the desired env and ensure ipykernel is installed in it. 

To create the kernel spec run (as apps user):

`python -m ipykernel install --user --name myenv --display-name "Python (myenv)"`

This installs a kernel spec (JSON text file).
(https://jupyter-client.readthedocs.io/en/latest/kernels.html#kernelspecs).  
Kernel spec files are just JSON files, and can be viewed and changed with a normal text editor.

It specifies:

* env name 
* kernel's display name 

The spec is created in:

* current user's user space (`~/.local/share/jupyter/kernels/`)
* current python installation

As the apps user, create a directory in /apps with the env name and copy into it the kernel.json and associated
(logo) files. The appropriate location should be something like:
`/apps/share64/debian10/anaconda/anaconda-6/share/jupyter/kernels/`

Ensure you verify the ownership and permissions of the new directory and its contents.

NOTE: To pick up newly deployed kernels, you must restart your Jupyter tool on Hubzero.

### Cheat sheet

Which kernels are available?

https://jupyter-client.readthedocs.io/en/stable/kernels.html

`$ jupyter kernelspec list`

TODO
