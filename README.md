# Physics-driven machine learning and AdaptiveMeshRefinement
This repo is mainly established based on (https://github.com/nbouziani/physics-driven-ml.git)

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

### Installing Firedrake

[Firedrake](https://www.firedrakeproject.org/) for this project, please refer to (https://github.com/acse-yl1922/firedrake_install_helper) for more detailed instructions

Finally, you will need to activate the Firedrake virtual environment:

```activate_venv
source firedrake-adapte/bin/activate
(please refer to your own files and dirs for correct activation)
```

### Installing PyTorch

Then, you need to install [PyTorch](https://pytorch.org/) (see [here](https://pytorch.org/get-started/locally/#start-locally)). Make sure to have your Firedrake virtual environment activated when you install it.

