## Jupyter Notebooks

Python is now widely used in machine learning for its ease of use and good availability of Python packages for machine learning. But it's still hard to test Python code sometimes, especially on an HPC system. [Jupyter notebooks](https://jupyter.org/) are sort of like "interactive coding worksheets" -- you can run Python code in them, but you can also insert text for notes and explanations, and you can save the output produced by code blocks, even plots. And you're not limited to Python either; there are [Jupyter kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) for all kinds of languages!

### Setup (local)

If you have Anaconda installed, Jupyter is included by default and it's easy to start up a server:
```
jupyter notebook
```

The server will initialize and open a browser window where you can browse and run Jupyter notebooks.

### Setup (Palmetto)

You can run Jupyter notebooks from Palmetto through [JupyterHub](https://www.palmetto.clemson.edu/jupyterhub). Once you sign in with your Clemson username, you can provision a compute node and run Jupyter notebooks from your home directory on Palmetto.

By default you can create notebooks for Python and R, but you will probably want to be able to run notebooks using a custom Anaconda environment -- this is because you need a custom Anaconda environment in order to be able to install any custom Python packages like Tensorflow. To do this you'll need to set up some things on Palmetto:
```
module add anaconda3/4.3.0

# create an environment called "myenv"
conda create -n myenv python=3.6

# export the kernel for JupyterHub to use
source activate myenv
python -m ipykernel install --user --name myenv --display-name "Python 3 (myenv)"
```

After taking these steps, whenever you log in to a node on JupyterHub you should see an option under "New" called "Python 3 (myenv)". Creating a notebook with this kernel will allow you to use any Python packages installed in your Anaconda environment.

### JupyterLab

JupyterLab is a web-based IDE for Jupyter notebooks. On a local machine you can run it easily if you have Anaconda installed:
```
jupyter-lab
```
