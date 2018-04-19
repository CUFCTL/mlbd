## Python Tools

Amidst the sea of programming languages out there, Python has become one of the most widely used languages for machine learning and data science. Many high-performance libraries such as TensorFlow are implemented in C++ but have a thin Python interface, which allows the user to have both the convenience of the Python scripting language and the performance of highly-optimized C++ code. Python also has an extensive ecosystem of packages which are easy to install, which means that whenever you need to implement something, there's a good chance that it's already been done in a Python package. If you want to be able to do anything with machine learning, Python is one of the most useful languages that you can learn.

### Setup (Anaconda)

The recommended way to use Python is through [Anaconda](https://www.anaconda.com/), which provides an easier way to manage Python packages, and installs a lot of useful packages immediately for you. Anaconda can be installed on most platforms. On Palmetto, Anaconda is available as a module:
```
module add anaconda3/4.3.0
```

You can manage packages in Anaconda with the `conda` command-line tool. On Palmetto, you must create a virtual environment before you can install additional packages:
```
# create an environment called "tensorflow"
conda create -n tensorflow python=2.7

# install packages in the virtual environment
conda install -n tensorflow tensorflow-gpu=1.3.0
```

If you use a virtual environment then you must use `source activate [env]` and `source deactivate` to enter and exit your environment. Here's an example of checking that your TensorFlow installation worked:
```
# (Palmetto) login to a GPU node
qsub -I -l select=1:ncpus=2:mem=8gb:ngpus=2:gpu_model=k40,walltime=02:00:00

# enable the environment that you created
source activate tensorflow

# start a Python shell and try to import tensorflow
# if everything is working, it will import with no problems
python
>>> import tensorflow as tf

# to disable your environment when you're done
source deactivate
```

### Tools

There are a variety of Python tools that are commonly used for working with data. All of these tools can be installed with Anaconda. Each tool has a website with tutorials and documentation, so we refer you to them:

- [matplotlib](https://matplotlib.org/): visualization
- [numpy](http://www.numpy.org/): linear algebra
- [pandas](http://pandas.pydata.org/): data management
- [scikit-image](http://scikit-image.org/): image processing
- [scikit-learn](http://scikit-learn.org/): machine learning
- [seaborn](http://seaborn.pydata.org/): (better) visualization
- [TensorFlow](https://www.tensorflow.org/): high-performance machine learning
