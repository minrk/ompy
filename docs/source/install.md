## Installation
Start off by downloading ompy:
``` bash
git clone --recurse https://github.com/oslocyclotronlab/ompy/
```
where the `--recurse` flag specifies, that all submodules shall be downloaded as well.

### Dependencies
 - Get and compile MultiNest (use the cmake version from [github.com/JohannesBuchner/MultiNest](https://github.com/JohannesBuchner/MultiNest)). The goal is to create lib/libmultinest.so
    ``` bash
    git clone https://github.com/JohannesBuchner/MultiNest
    cd MultiNest/build
    cmake ..
    make
    sudo make install
    ```
    Multinest had following hard dependencies: `lapack` and `blas`. To use MPI, additionally openmp has to be installed (probably does not work for MAC users, see below.). With apt-get you may fix the dependencies by:
    ```bash
    sudo apt-get install liblapack-dev libblas-dev libomp-dev
    ```
    If you still get an error like:
    ``` bash
    OSError: libmultinest.so: cannot open shared object file: No such file or directory
    ```
    visit http://johannesbuchner.github.io/PyMultiNest/install .
 - We require `python>=3.7`. Make sure you use the correct python version and the correct `pip`.
   You may need to replace `python` by `python3` and `pip` by `pip3` in the examples below. Run
   `python --version` and `pip --version` to check whether you have a sufficient python version.
 - All other dependencies can be installed automatically by `pip` (see below). Alternatively,
   make sure to install all requirements listed in `requirements.txt`, eg. using `conda` or `apt-get`.
   You may try following in `conda` (untested)
    ``` bash
   conda install --file requirements.txt
   ```
 - For openMP support (optional), install `libomp`. Easiest on linux/ubuntu: `sudo apt-get install libomp-dev` or MAC `brew install libomp`.
 - Many examples are written with [jupyter notebooks](https://jupyter.org/install), so you probably want to install this, too.

### OMpy package

There are two main options on how to install OMpy. We will start off with our recommendation, that is with the `-e` flag is a local project in “editable” mode. This way, you will not have to reinstall ompy if you pull a new version from git or create any local changes yourself.

Note: If you change any of the `cython` modules, you will have to reinstall/recompile anyways.
```bash
pip install -e .
```

If you want to install at the system specific path instead, use
```bash
pip install .
```

For debugging, you might want to compile the `cython` modules "manually". The first line here is just to delete any existing cython modules in order to make sure that they will be recompiled.
```bash
rm ompy/*.so
rm ompy/*.c
python setup.py build_ext --inplace
```

### Troubleshooting
#### Docker container
If you don't succeed with the above, we also provide a [Docker](https://www.docker.com/get-started) container via dockerhub, see https://hub.docker.com/r/oslocyclotronlab/ompy. However, for everyday usage, we think it's easier to install the package *normally* on your machine

#### Python version
If you had some failed attempts, you might try to uninstall `ompy` before retrying the stepts above:
```bash
pip uninstall ompy
```
Note that we require python 3.7 or higher. If your standard `python` and `pip` link to python 2, you may have to use `python3` and `pip3`.

#### Try to reinstall
If you changed / if after a `git pull` there have been any changes to one of the `cython` modules, you will have to reinstall/recompile anyways: `pip install -e .`.

#### OpenMP / MAC
If you don't have OpenMP / have problems installing it (see above), you can install without OpenMP. Type `export ompy_OpenMP=False` in the terminal before the setup above.

#### Cloned the repo before September 2019
**NB: Read this (only) if you have cloned the repo before October 2019:**
We cleaned the repository from old comits clogging the repo (big data files that should never have been there). Unfortunetely, this has the sideeffect that the history had to be rewritten: Previous commits now have a different SHA1 (git version keys). If you need anything from the previous repo, see [ompy_Archive_Sept2019](https://github.com/oslocyclotronlab/ompy_Archive_Sept2019). This will unfortunately also destroy references in issues.
The simplest way to get the new repo is to rerun the installation instructions below.

### General usage
All the functions and classes in the package are available in the main module. You get everything by importing the package

```py
import ompy
```

The overarching philosophy is that the package shall be flexible and transparent to use and modify. All of the "steps" in the Oslo method are implemented as classes with a common structure and call signature. If you understand one class, you'll understand them all, making extending the code easy.

As the Oslo method is a complex method involving dozen of variables which can be daunting for the uninitiated, many class attributes have default values that should give satisfying results. Attributes that _should_ be modified even though it is not strictly necessary to do so will give annoying warnings. The documentation and docstrings give in-depth explanation of each variable and its usage.
