## Python

Amidst the sea of programming languages out there, Python has become one of the most widely used languages for machine learning and data science. Python is similar to C and is actually built upon C, but it is an interpreted language and it has a simpler syntax. More importantly, there is an extensive ecosystem of software written in Python, so whenever you need to implement something, there's a good chance that it's already been done in a Python package which can be installed with a single command.

Many high-performance libraries such as TensorFlow are implemented in C++ but have a thin Python interface, which allows the user to have both the convenience of the Python scripting language and the performance of highly-optimized C++ code. Python is one of the most useful languages that you can learn today: for data analysis, data visualization, machine learning, and so on.

### Learning Python

Because Python is so widely used, there are a _ton_ of resources out there for learning Python. Probably any of them are good to use. I personally recommend [Codecademy](https://www.codecademy.com/) because that is where I (Ben) learned Python. They have a [basic Python course](https://www.codecademy.com/learn/learn-python) which is free and interactive. Beyond that, the best way to learn is to code it yourself! Try to implement a few small projects, or take some old coding projects in other languages and port them to Python.

### Setup (Anaconda)

The recommended way to use Python is through [Anaconda](https://www.anaconda.com/), which provides an easier way to manage Python packages, and installs a lot of useful packages immediately for you. Anaconda can be installed on most platforms. On Palmetto, Anaconda is available as a module:
```
module add anaconda3/5.1.0
```

You can manage packages in Anaconda with the `conda` command-line tool. On Palmetto, you must create a virtual environment before you can install additional packages:
```
# create an environment called "myenv"
conda create -n myenv python=3.6

# install packages in the virtual environment
source activate myenv
conda install ipython
conda install tensorflow-gpu==1.7.0
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

### Tools

There are a variety of Python tools that are commonly used for working with data. All of these tools can be installed with Anaconda. Each tool has a website with tutorials and documentation, so we refer you to them:

- [keras](https://keras.io/): deep learning but really simple
- [matplotlib](https://matplotlib.org/): visualization
- [numpy](http://www.numpy.org/): linear algebra
- [pandas](http://pandas.pydata.org/): data management
- [scikit-image](http://scikit-image.org/): image processing
- [scikit-learn](http://scikit-learn.org/): machine learning
- [seaborn](http://seaborn.pydata.org/): (better) visualization
- [TensorFlow](https://www.tensorflow.org/): high-performance machine learning
