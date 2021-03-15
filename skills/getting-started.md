## Getting Started

There is a lot you will have to learn in order to be able to do machine learning / data science work on a high-performance computing system. So for now, let's just focus on setting everything up. Here's the breakdown:

1. Get access to Palmetto
2. Login to JupyterHub
3. Create an Anaconda environment
4. Add your environment to JupyterHub
5. Download the Jupyter notebooks to Palmetto
6. Run the Introduction notebook

### Get Access to Palmetto

If you do not already have a Palmetto account, follow the instructions [here](https://www.palmetto.clemson.edu/palmetto/basic/new/) to apply for one. Fill out the application form as completely and accurately as possible. Some guidance for each field is provided below. If you fill out everything correctly, you should receive your account within a few days. Otherwise, please ask one of the CI mentors for help.

- __Title__: Student
- __Account Type__: Educational
- __Rank__: Undergraduate
- __Research Abstract__: ML/BD Creative Inquiry with Dr. Melissa Smith
- __Usage Questionaire__: Tensorflow, Python, Interactive jobs, GPUs
- __Assistance__: Will receive startup instructions in the CI

### Login to JupyterHub

Once you have an account, go to [JupyterHub](https://www.palmetto.clemson.edu/jupyterhub). You will be asked to sign in with your Clemson username, and then you will see a list of "spawner" options. This form is how you specify the compute resources that you need. Here is a good set of defaults for provisioning a compute node:

- 1 cpu
- 14gb memory
- 1 gpu (K20, K40, or P100)
- 24hr walltime

Submit the form and then wait for your node to be provisioned. JupyterHub will automatically refresh when it's ready. Once you're logged in, you will see your home directory on Palmetto.

### Create an Anaconda environment

Our Jupyter notebooks use Python, so we will create an Anaconda environment to manage the Python packages required by the notebooks. From JupyterHun, click "New -> Terminal" to open a command-line prompt. Run `module list` to see what modules you have loaded. You may see the `anaconda3` module, but we need `anaconda3/5.1.0-gcc/8.3.1` specifically, so run these commands:
```bash
module purge
module load anaconda3/5.1.0-gcc/8.3.1
```

You should also append these commands to your `.bashrc` file in your home directory so that they are run every time you log in.

The `anaconda3` module provides the `conda` command, which we will use to actually create an Anaconda environment:
```bash
conda create -n mlbd python=3.7 tensorflow-gpu=1.15.0 ipykernel ipython ipywidgets matplotlib numpy pandas scikit-image scikit-learn seaborn
```

This command will ask you to confirm the installation, and then it will take a while to install everything. Once it finishes you will have an envionrment which you can use like so:
```bash
# enter the environment
source activate mlbd

# start a Python shell, make sure tensorflow is working
ipython
>>> import tensorflow as tf
>>> print(tf.reduce_sum(tf.random.normal([1000, 1000])))

# exit the environment
source deactivate
```

### Add your environment to JupyterHub

Once you create the environment, you can use it in the terminal, but there is one more command you must run in order to make it visible to JupyterHub:
```bash
source activate mlbd
python -m ipykernel install --user --name mlbd --display-name "Python 3 (mlbd)"
```

From the JupyterHub main page, click "New" and look for an option called "Python 3 (mlbd)". Creating a notebook with this kernel will allow you to use any Python packages installed in your Anaconda environment.

You may have to refresh JupyterHub or even restart your server in order to see the change take affect. To restart your server, go to "Control Panel", select "Stop My Server", wait for the button to disappear, and then select "My Server" to request a new compute node.

### Download the Jupyter notebooks to Palmetto

There are a few ways to get the Jupyter notebooks onto Palmetto. You can clone this repository from Github:
```bash
git clone https://github.com/cufctl/creative-inquiry.git
```

The Jupyter notebooks are located in `creative-inquiry/notebooks`. You can also download each notebook individually from Github:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/introduction.ipynb
```

Lastly, you can download the notebooks from the main page and upload them to JupyterHub using the "Upload" button.

### Run the Introduction notebook

Open the "Introduction" notebook and look around. A Jupyter notebook consists of code cells and Markdown cells. You can edit a cell by selecting it and pressing Enter or double-clicking it, and you can run a cell by pressing Shift-Enter. If everything is set up correctly, you should be able to run every cell without any errors. This notebook is also a sanity check so if it works then all of the other notebooks should work for you.

Now you're ready to go!
