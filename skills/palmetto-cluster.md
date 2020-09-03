## Palmetto Cluster

The Palmetto Cluster is Clemson's high-performance computing cluster. In our lab we use Palmetto extensively to run experiments that we can't run on our own machines. Full documentation on Palmetto can be found [here](https://www.palmetto.clemson.edu/palmetto/). There is also a [Youtube tutorial](https://www.youtube.com/playlist?list=PLagFkXs2Bczaukd90bUWy0E2qub9alZn1) for Palmetto created by Clemson CITI.

### Accessing Palmetto via JupyterHub

The easiest way to access Palmetto is through [JupyterHub](https://www.palmetto.clemson.edu/jupyterhub). Once you sign in with your Clemson username, you can provision a compute node and run Jupyter notebooks from your home directory on Palmetto.

### Accessing Palmetto via SSH

Once you have an account, you can access Palmetto through SSH with your Clemson username and password:
```
ssh <username>@login.palmetto.clemson.edu
```

__NOTE: The login node should not be used for compute-heavy tasks, and your home directoy has limited storage space.__ Read the __Jobs__ section to learn how to use compute nodes, and read the __Data Storage__ section to learn how to use the scratch directories.

### Jobs

When you log into Palmetto, you will be on the "login node", which should be used only for simple tasks such as moving files around. For more compute-intensive tasks such as building software (e.g. running `make`) and running the face recognition system, you need to first login to a compute node, which can be done by submitting jobs. IF you try to run something on the login node that takes too long, it will be terminated.

#### Interactive jobs

The easiest way to login to a compute node is to run `qsub -I`. When the node is ready, your shell prompt will change to something like `nodeXXXX` and you will be able to run whatever you want. This command is equivalent to the following:
```
qsub -I -l select=1:ncpus=1:mem=1gb,walltime=0:30:00
```

So you can adjust this syntax to request the resources that you need. Below is a good alias to append to your `.bashrc` for logging into a node with a GPU:
```
alias gpu='qsub -I -l select=1:ncpus=8:ngpus=1:mem=16gb:gpu_model=k40,walltime=72:00:00'
```

#### Batch jobs

You can also run `qsub example.pbs`, where `example.pbs` is a shell script with a few PBS directives at the top to specify the resources, like so:
```
#PBS -N example
#PBS -l select=1:ncpus=1:mem=2gb,walltime=00:10:00

pwd
env
module list
```

You can use `qstat` to view the status of your jobs, like so:
```
qstat -xu $USER
```

More information can be found in the Palmetto documentation.

### Data Storage

When you are logged in, your default directory is your home directory `/home/$USER`. You can use this directory for long-term storage; however, you should not run jobs (or other tasks that perform I/O) in your home directory. Instead, use the scratch directories located at `/scratch1/$USER` and `/scratch2/$USER`.

The downside of the scratch directories is that they will not keep your data permanently -- files older than 30 days will be deleted automatically. Therefore, you should try to keep things like code, input data, and results in your home directory, while using the scratch directories to run jobs and store temporary data.

Below are a few aliases that are useful to have in your `.bashrc`. This script is in your home directory, and it is run when you login, so this kind of setup gives you a few shortcuts when working with the scratch directories.
```
alias scratch1="cd /scratch1/$USER/"
alias scratch2="cd /scratch2/$USER/"
```

### Modules

Since you can't install packages on Palmetto through `apt-get`, software packages are provided as modules. Below are some simple commands to get you started:

- `module avail`: list all available modules
- `module list`: list the modules that you have installed
- `module load <name>[/<version>]`: load a module into your environment
- `module rm <name>`: remove a module from your environment
- `module purge`: remove all modules from your environment

These commands are typically very fast because they only update a few environment variables; the software packages themselves are already installed.

Below are a few module commands that are useful to have in your `.bashrc`. This script is in your home directory, and it is run when you login, so this kind of setup allows you to have a few basic modules installed by default.
```
module purge
module load anaconda3/5.1.0-gcc/8.3.1
module load cuda/11.0.2-gcc/8.3.1
module load git/2.27.0-gcc/8.3.1
```

### Transferring Data

You can use `scp` to transfer data from the command-line:
```
scp [-r] <username>@xfer01-ext.palmetto.clemson.edu:<remote-path> <local-path>

scp [-r] <local-path> <username>@xfer01-ext.palmetto.clemson.edu:<remote-path>
```

To manage file transfers from a GUI, you can use [FileZilla](https://filezilla-project.org/), an FTP client which is relatively easy to use and cross-platform. If you're on JupyterHub, you can upload files via the "Upload" button.
