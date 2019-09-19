## Python

Amidst the sea of programming languages out there, Python has become one of the most widely used languages for machine learning and data science. Python is similar to C and is actually built upon C, but it is an interpreted language and it has a simpler syntax. More importantly, there is an extensive ecosystem of software written in Python, so whenever you need to implement something, there's a good chance that it's already been done in a Python package which can be installed with a single command.

Many high-performance libraries such as TensorFlow are implemented in C++ but have a thin Python interface, which allows the user to have both the convenience of the Python scripting language and the performance of highly-optimized C++ code. Python is one of the most useful languages that you can learn today: for data analysis, data visualization, machine learning, and so on.

### Learning Python

Because Python is so widely used, there are a _ton_ of resources out there for learning Python. Probably any of them are good to use. If you've used other languages like C/C++ or MATLAB, you can probably just refer to [this cheatsheet](https://learnxinyminutes.com/docs/python3/). Beyond that, the best way to learn is to code it yourself! Try to implement a few small projects, or take some old coding projects in other languages and port them to Python.

### Setup

The recommended way to use Python is through [Anaconda](https://www.anaconda.com/), which provides an easier way to manage Python packages, and installs a lot of useful packages immediately for you. Anaconda can be installed on most platforms. On Palmetto, Anaconda is available as a module:
```
module add anaconda3/5.1.0
```

You can manage packages in Anaconda with the `conda` command-line tool. On Palmetto, you must create a virtual environment before you can install additional packages:
```
# (Palmetto) login to a compute node
qsub -I -l select=1:ncpus=2:mem=8gb:ngpus=2,walltime=02:00:00

# create an environment called "myenv"
conda create -n myenv python=3.6 tensorflow-gpu=1.12.0 ipykernel ipython ipywidgets matplotlib numpy pandas scikit-image scikit-learn seaborn

# add your environment to JupyterHub
source activate myenv
python -m ipykernel install --user --name myenv --display-name "Python 3 (myenv)"
```

If you use a virtual environment then you must use `source activate [env]` and `source deactivate` to enter and exit your environment. Here's an example of checking that your TensorFlow installation worked:
```
# (Palmetto) login to a GPU node
qsub -I -l select=1:ncpus=2:mem=8gb:ngpus=2:gpu_model=k40,walltime=02:00:00

# enable the environment that you created
source activate myenv

# start a Python shell and try to import tensorflow
# if everything is working, it will import with no problems
ipython
>>> import tensorflow as tf

# to disable your environment when you're done
source deactivate
```

### Jupyter Notebooks

Python is now widely used in machine learning for its ease of use and good availability of Python packages for machine learning. But it's still hard to test Python code sometimes, especially on an HPC system. [Jupyter notebooks](https://jupyter.org/) are sort of like "interactive coding worksheets" -- you can run Python code in them, but you can also insert text for notes and explanations, and you can save the output produced by code blocks, even plots. And you're not limited to Python either; there are [Jupyter kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) for all kinds of languages!
 
#### Local

If you have Anaconda installed, Jupyter is included by default and it's easy to start up a server:
```
jupyter notebook
```

The server will initialize and open a browser window where you can browse and run Jupyter notebooks.
 
#### Palmetto (JupyterHub)

You can run Jupyter notebooks from Palmetto through [JupyterHub](https://www.palmetto.clemson.edu/jupyterhub). Once you sign in with your Clemson username, you can provision a compute node and run Jupyter notebooks from your home directory on Palmetto. Here is a good set of defaults for provisioning a compute node:

- 2 cpus
- 6gb memory
- 56g interconnect

Once you're logged in, you can use the "New" button to create a notebook or open a terminal. If you followed the instructions to create a custom Anaconda environment, you should see an option called "Python 3 (myenv)". Creating a notebook with this kernel will allow you to use any Python packages installed in your Anaconda environment.

### JupyterLab

JupyterLab is a web-based IDE for Jupyter notebooks. On a local machine you can run it easily if you have Anaconda installed:
```
jupyter-lab
```

### Further Study

There are a variety of Python tools that are commonly used for working with data. All of these tools can be installed with Anaconda. Each tool has a website with tutorials and documentation, so we refer you to them:

- [keras](https://keras.io/): deep learning but really simple
- [matplotlib](https://matplotlib.org/): visualization
- [numba](https://numba.pydata.org/): high-performance computing
- [numpy](http://www.numpy.org/): linear algebra
- [pandas](http://pandas.pydata.org/): data management
- [scikit-image](http://scikit-image.org/): image processing
- [scikit-learn](http://scikit-learn.org/): machine learning
- [scipy](https://scipy.org/): scientific computing
- [seaborn](http://seaborn.pydata.org/): (better) visualization
- [TensorFlow](https://www.tensorflow.org/): high-performance machine learning

For more information on Jupyter notebooks:

- [Jupyter nbviewer](http://nbviewer.jupyter.org/)
