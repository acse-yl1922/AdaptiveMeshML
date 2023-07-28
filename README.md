# Physics-driven machine learning and AdaptiveMeshRefinement
This repo is mainly established based on (https://github.com/nbouziani/physics-driven-ml.git). Go to there for more about training and cnn model.

## Table of Contents
* [Setup](#setup)
* [Generate dataset](#generate-dataset)
* [Training](#training)
* [Evaluation](#evaluation)
* [Reporting bugs](#reporting-bugs)
* [Contributing](#contributing)
* [Citation](#citation)

## Setup

This work relies on the Firedrake finite element system and on PyTorch, which both need to be installed.

# Install Firedrake with adaptive mesh function

This is a repo containing scripts to install firedrake with adaptive mesh function.

Most of the materials comes from [pyroteus](https://github.com/pyroteus/pyroteus/tree/main/install) and [firedrake](https://github.com/firedrakeproject).

If you are using conda, it is the best to deactivate it before running the installation by using (`conda deactivate`)

## Install:

1. Replace `SOFTWARE=<your/install/path>` in `install_firedrake.sh` to the project root folder (where you can find `install_firedrake.sh` and `petsc_options.txt`).


2. Run `source ./install_firedrake.sh` under project root folder. This will set up a folder for firedrake and relavent envs.

## For mac users:

### `-w` error in making PETSc

One gotcha users could encounter is listed [here](https://github.com/firedrakeproject/firedrake/issues/2793) so if you are using gmake 4.4.1, you should run

```{shell}
brew tap-new $USER/local-make
brew extract --version=4.4 make $USER/local-make
brew install make@4.4
brew unlink make
brew link make@4.4
```

before executing the install script.

Note this is caused by a bug produced by gmake 4.4.1, so in the future, if gmake get around with this bug, you do not necessarily need to do this step.

PS: some users reflected that the `4.4` should be replaced with `4.4.0`. If the above instructions failed, please use `4.4.0` instead of `4.4`

### Parallel error

After installation, if you find parallel tests are failing, then please do some editing as suggested [here](https://firedrakeproject.org/download.html#testing-the-installation)

To be more specific, add below statement in `/etc/hosts` (if not editable, use sudo vim /etc/hosts to force writting)

(use `hostname` command to find LOCALHOSTNAME and the LOCALHOSTNAME below)

```
127.0.0.1       LOCALHOSTNAME.local
127.0.0.1       LOCALHOSTNAME
```

# Activate firedrake

firedrake installation come along with a virtual env. To use firedrake, you need to activate it by doing:

`source /path/to/firedrake/src/firedrake_adapt/bin/activate`

## Other settings

if you wish to set a command to activate firedrake instead of typing in a long path to your activate file, you could wirite following line to your .bash_rc or equivalent bash init file:

`alias fire_drake='source </path/to/your/acivate/file>'

then execute `fire_drake` in a new shell.

to deactivate, simply use `deactivate`.

## Installing PyTorch

Then, you need to install [PyTorch](https://pytorch.org/) (see [here](https://pytorch.org/get-started/locally/#start-locally)). Make sure to have your Firedrake virtual environment activated when you install it.

## Some math background
So mesh adaptivity is a  general technique to adapt the mesh during a computational simulation to focus resolution where it is needed in order to make it more efficient. There are various different ways to choose/predict where resolution will be needed and various different ways to adapt the mesh. You could have a look at part II of this thesis: 
(https://spiral.imperial.ac.uk/handle/10044/1/92820)[1](It will also attached in this project folder at /materials).


One way to adapt a mesh is so called "mesh movement" which means you only move the vertices without changing the connections, and try to "squeeze" resolution where it's needed most. Again deciding where the nodes should move to is the expensive part. One method is solving the Monge Ampere equations 

(see the thesis [1] above and e.g. https://www.researchgate.net/publication/220411930_Moving_Mesh_Generation_Using_the_Parabolic_Monge-Ampere_Equation). 

The paper (https://arxiv.org/pdf/2204.11188.pdf)[2] is in fact about machine learning to replace the solution of these equations. There are a number of other ways to make this decision of where to move the nodes: see here 

(https://spiral.imperial.ac.uk/bitstream/10044/1/57583/2/IC15-ecse-tech-report.pdf) 

for an overview of different approaches (worked in our group) - this document should give various ideas of things you could work on (again with ML or not)
	
for mesh movement (r adaptivity) the actual changing the mesh is very straightforward: you can just tell firedrake to change the coordinates of the vertices - the hard part is to decide where you want to move them to. For that there is a bit of code here: (https://github.com/pyroteus/movement) To install it, just make a local checkout and install it with pip install -e <path-to-that-checkout> with you firedrake environment activated. The -e option means "editable", which means you can change the python code and it will take immediate effect when you import that module. Some example notebooks are here: (https://github.com/pyroteus/movement-notebooks)   For mesh movement you don't actually need the special build instructions for firedrake with mesh (h) adaptivity functionality, but could also use a standard firedrake build (it doesn't matter)

