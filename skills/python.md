## Python

Amidst the sea of programming languages out there, Python has become one of the most widely used languages for machine learning and data science. Python is similar to C and is actually built upon C, but it is an interpreted language and it has a simpler syntax. More importantly, there is an extensive ecosystem of software written in Python, so whenever you need to implement something, there's a good chance that it's already been done in a Python package which can be installed with a single command.

Many high-performance libraries such as TensorFlow are implemented in C++ but have a thin Python interface, which allows the user to have both the convenience of the Python scripting language and the performance of highly-optimized C++ code. Python is one of the most useful languages that you can learn today: for data analysis, data visualization, machine learning, and so on.

### Learning Python

Because Python is so widely used, there are a _ton_ of resources out there for learning Python. Probably any of them are good to use. If you've used other languages like C/C++ or MATLAB, you can probably just refer to [this cheatsheet](https://learnxinyminutes.com/docs/python3/). Beyond that, the best way to learn is to code it yourself! Try to implement a few small projects, or take some old coding projects in other languages and port them to Python.

### Anaconda

The recommended way to use Python is through [Anaconda](https://www.anaconda.com/), which provides an easier way to manage Python packages, and installs a lot of useful packages immediately for you. Anaconda can be installed on most platforms.

### Jupyter Notebooks

Python is now widely used in machine learning for its ease of use and good availability of Python packages for machine learning. But it's still hard to test Python code sometimes, especially on an HPC system. [Jupyter notebooks](https://jupyter.org/) are sort of like "interactive coding worksheets" -- you can run Python code in them, but you can also insert text for notes and explanations, and you can save the output produced by code blocks, even plots. And you're not limited to Python either; there are [Jupyter kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) for all kinds of languages!

If you have Anaconda installed, Jupyter is included by default and it's easy to start up a server:
```
jupyter notebook
```

The server will initialize and open a browser window where you can browse and run Jupyter notebooks.

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

And here are some good websites for testing your Python programming skills:

- [Project Euler](https://projecteuler.net/)
- [Rosalind Problems](http://rosalind.info/)
