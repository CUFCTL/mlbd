## High-Performance Computing

High-performance computing (HPC) is a field which employs large-scale computing systems like [supercomputers](https://en.wikipedia.org/wiki/Supercomputer) to tackle large-scale problems such as [climate analytics](https://arxiv.org/pdf/1810.01993.pdf). Most HPC systems are built and run by organizations such as the [DOE National Labs](https://www.energy.gov/national-laboratories) and universities for science research. Our own [Palmetto Cluster](palmetto.md), maintained by Clemson University, is one of the fastest public university clusters in the country. Large companies such as Google and Facebook sometimes build their own datacenters as well for training AI models and deploying web services, while smaller companies typically use the cloud instead.

The first supercomputers were basically just bigger computers, but today a supercomputer is (basically) a _cluster_ of hundreds or thousands of regular computers. As a result, writing code for supercomputers is a bit more complicated than for regular computers. High-performance applications typically use an interface called Message Passing Interface (MPI) to distribute a problem across many compute node. Furthermore, every new supercomputer is bigger and more power-hungry than the last, so it's also becoming important for supercomputers to be power-efficient. As a result many supercomputers now also feature accelerators such as GPUs and FPGAs which provide a much higher performance-per-watt. But once again, writing code for these accelerators is more complicated than for CPUs.

All of that is to say, knowing how to effectively use an HPC system is a skill unto itself. There are a plethora of programming models and applications that can be used for HPC, catering to every level of expertise. Skilled programmers can use libraries like CUDA to have the most control and squeeze the most performance out of their code, while domain scientists may prefer to use something simpler like OpenACC, which will allow them to write code much more quickly but may not be quite as performant, and novice users may avoid programming altogether and use existing HPC applications such as LAMMPS or TensorFlow to accomplish their work.

Yet even then there is another problem -- many HPC applications are on the order of 10,000 or even 100,000 lines of code, which means that "porting" a code from one programming model to another is a monstrous effort. And since every HPC system is slightly different, even just building your application can be an adventure for every new system. And not the fun kind, either. As a result, there are now "performance portability" libraries such as Kokkos and RAJA which allow you to write code once and compile it for multiple architectures. There are also package managers like [Anaconda](https://anaconda.org/) and [Spack](https://spack.io/) which make it easier to manage dependencies (usually). While [Docker](https://www.docker.com/) is the most commonly used container software, most HPC systems do not allow Docker due to security conerns, so there are alternatives such as [Singularity](https://sylabs.io/singularity/) and [Shifter](https://www.nersc.gov/research-and-development/user-defined-images/) that essentially enable the secure use of containers on HPC systems.

Whatever your desired skill level, here we have tried to curate some helpful resources for learning how to use any of these technologies.

### Distributed Computing

- [OpenMPI Documentation](https://www.open-mpi.org/doc/)
- [mpi4py](https://mpi4py.readthedocs.io/en/stable/)

### GPU Computing

CUDA:
- [CUDA Toolkit Documentation](http://docs.nvidia.com/cuda/)
- [An Even Easier Introduction to CUDA](https://devblogs.nvidia.com/even-easier-introduction-cuda/)
- [Numba: High-Performance Python with CUDA Acceleration](https://devblogs.nvidia.com/numba-python-cuda-acceleration/)
- [RAPIDS Accelerates Data Science End-to-End](https://devblogs.nvidia.com/gpu-accelerated-analytics-rapids/)

OpenCL:
- [OpenCL Reference Pages](https://www.khronos.org/registry/OpenCL/sdk/1.2/docs/man/xhtml/)
- [NVIDIA OpenCL SDK Code Samples](https://developer.nvidia.com/opencl)

### Performance Portability

- [OpenMP](https://computing.llnl.gov/tutorials/openMP/)
- [OpenACC](https://www.openacc.org/)
- [Kokkos](https://github.com/kokkos/kokkos-tutorials)
- [RAJA](https://raja.readthedocs.io/en/main/index.html)

### Dependency Management, Containers

- [Anaconda](https://anaconda.org/)
- [Spack](https://spack.io/)
- [Singularity](https://sylabs.io/singularity/)
- [Shifter](https://www.nersc.gov/research-and-development/user-defined-images/)
