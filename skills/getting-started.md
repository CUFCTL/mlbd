## Getting Started

There is a lot you will have to learn in order to be able to do machine learning / data science work on a high-performance computing system. So for now, let's just focus on setting everything up. Here's the summary:

1. Get access to Palmetto
2. Login to Palmetto OnDemand
3. Create an Anaconda environment
4. Create a Jupyter kernel for your environment
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

### Login to Palmetto OnDemand

Once you have an account, go to [Palmetto OnDemand](https://openod.palmetto.clemson.edu/). You will be asked to sign in with your Clemson username, and then you will arrive at the Dashboard. Feel free to explore the many features, then go to Interactive Apps -> Jupyter Notebook. This form will allow you to provision a JupyterLab instance. Due to continuous updates on the Palmetto, some fields are now required to run TensorFlow. Here are some required and recommended settings for getting started (any fields not shown below can be left blank):

[comment]: <> (- 1 CPU)
[comment]: <> (- 15 GB memory)
[comment]: <> (- 1 GPU [K20, K40, or P100])
[comment]: <> (- 24 hr walltime)
__Required__
- Anaconda Version: 
```anaconda3/2022.05-gcc/9.5.0```
- List of modules to be loaded: 
```bash 
cudnn/8.1.0.77-11.2-gcc/9.5.0 cuda/11.2.2-gcc/9.5.0
```
- Notebook Workflow: ```Tensorflow Notebook```

__Recommended__ (These can be varied based on computational needs)
- Number of resource chunks (select): ```1```
- CPU cores per chunk (ncpus): ```8```
- Amount of memory per chunk (mem): ```15gb```
- Number of GPUs per chuck (ngpus): ```1```
- GPU Model (gpu_model): ```P100``` or ```V100``` (K series are very old)
- Interconnect: ```any```
- Walltime: However long you plan to use the notebook or node
- Queue: ```work1```


Select "Launch" and wait for your node to be provisioned. You may have to refresh the page, but when it's ready you will see a "Connect to Jupyter" button that will take you to your JupyterLab instance. JupyterLab is the central hub from which you can do everything you need -- browse files, edit files, run a terminal or notebook, view images, and so on.

### Create an Anaconda environment

Our Jupyter notebooks use Python, so we will create an Anaconda environment to manage the Python packages required by the notebooks. From the File Browser, click the "+" icon and open a new terminal. Run `module list` to see what modules you have loaded. You may see the `anaconda3` module, but we need `anaconda3/2022.05-gcc/9.5.0` specifically, so run these commands:
```bash
module purge
module load anaconda3/2022.05-gcc/9.5.0
```
Occasionally, Palmetto modules are updated so the anaconda version may be different than what is shown above. If this anaconda version does not work, use the command `module avail` to list the current modules and their versions on the Palmetto.  

You should also append these commands to your `.bashrc` file in your home directory so that they are run every time you log in.

The `anaconda3` module provides the `conda` command, which we will use to actually create an Anaconda environment:
```bash
wget https://raw.githubusercontent.com/cufctl/mlbd/master/environment.yml
conda env create -f environment.yml
```

Note: Conda's package mangager seems to be breaking the TensorFlow installation so we will install TensorFlow in the next step with pip once we are inside our environment. Like the other installations, TensorFlow only has to be installed once.

This command will ask you to confirm the installation, and then it will take a while to install everything. Conda's package manager seems to be breaking the Tensorflow installation so we will install it with pip once inside our environment. Once it finishes you will have an envionrment which you can use like so:
```bash
# enter the environment
source activate mlbd

# install tensorflow
pip install tensorflow==2.9

#You must run this command in order to use it in a Jupyter notebook
python -m ipykernel install --user --name mlbd --display-name "Python 3 (mlbd)"
```


### Create a Jupyter kernel for your environment

Now click the "+" icon again and look for a notebook option called "Python 3 (mlbd)". Creating a notebook with this kernel will allow you to use any Python packages installed in your Anaconda environment.

You may have to refresh JupyterLab or even restart your server in order to see the change take affect. To restart your server, go to "Control Panel", select "Stop My Server", wait for the button to disappear, and then select "My Server" to request a new compute node.

### Download the Jupyter notebooks to Palmetto

There are a few ways to get the Jupyter notebooks onto Palmetto. You can clone this repository from Github:
```bash
git clone git@github.com:cufctl/mlbd
```

The Jupyter notebooks are located in `mlbd/notebooks`. You can also download each notebook individually from Github:
```bash
wget https://cufctl.github.io/mlbd/notebooks/introduction.ipynb
```

Lastly, you can download the notebooks from the main page and upload them to JupyterLab using the "Upload" button, or dragging and dropping them into the File Browser.

### Run the Introduction notebook

Open the "Introduction" notebook and look around. A Jupyter notebook consists of code cells and Markdown cells. You can edit a cell by selecting it and pressing Enter or double-clicking it, and you can run a cell by pressing Shift+Enter. If everything is set up correctly, you should be able to run every cell without any errors. This notebook is also a sanity check so if it works then all of the other notebooks should work for you.

Now you're ready to go!
