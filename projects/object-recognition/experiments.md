## Experiments

This page describes all kinds of experiments that you can run with the recognition system. These experiments are generally centered around tuning or comparing various aspects of the project, such as hyperparameters, datasets, and architectures.

### Algorithms

The object recognition system uses a variety of machine learning algorithms to perform feature extraction and classification, so we can compare these algorithms across several metrics, including training time, prediction time, and the error rate. Below is an example table for comparing each algorithm. Note that in this example the dataset and hyperparameters are fixed, so they should be specified along with the results.

|           | Train (s) | Pred (s) | Error rate |
|-----------|-----------|----------|------------|
| PCA/kNN   |           |          |            |
| LDA/kNN   |           |          |            |
| ICA/kNN   |           |          |            |
| PCA/Bayes |           |          |            |
| LDA/Bayes |           |          |            |
| ICA/Bayes |           |          |            |

### Architectures

The system can be run on a CPU or a GPU, so we can compare the performance of the system on each architecture. Below is an example table for comparing architectures across each algorithm. Note that with this example, the dataset and hyperparameters are fixed, so they should be specified along with the results.

|           | CPU / Train (s) | CPU / Pred (s) | GPU / Train (s) | GPU / Pred (s) |
|-----------|-----------------|----------------|-----------------|----------------|
| PCA/kNN   |                 |                |                 |                |
| LDA/kNN   |                 |                |                 |                |
| ICA/kNN   |                 |                |                 |                |
| PCA/Bayes |                 |                |                 |                |
| LDA/Bayes |                 |                |                 |                |
| ICA/Bayes |                 |                |                 |                |

### Datasets

One way to demonstrate the robustness of a object recognition system is to show that it performs well with several (good) datasets. Below is an example table for comparing the performance and accuracy of several datasets. Note that with this example, the algorithm and hyperparameters are fixed, so they should be specified along with the results.

|       | Train (s) | Pred (s) | Error rate |
|-------|-----------|----------|------------|
| FERET |           |          |            |
| MNIST |           |          |            |
| ORL   |           |          |            |

### Hyperparameters

Each of the machine learning algorithms has hyperparameters associated with it, such as `pca_n1` and `knn_k`. These hyperparameters typically have trade-offs associated with them, so for each hyperparameter we should test the system across a range of values and compare metrics such as training time, prediction time, and the error rate. Below is an example table for tuning `pca_n1`. Note that in this example, the algorithm, the dataset, and the other hyperparameters are fixed, so they should be specified along with the results.

| pca_n1 | Train (s) | Pred (s) | Error rate |
|--------|-----------|----------|------------|
| 10     |           |          |            |
| 20     |           |          |            |
| 30     |           |          |            |
| 40     |           |          |            |
| ...    |           |          |            |

### Partitions

Related to testing several datasets is testing several different partitions of a dataset. The partition of a dataset refers to how the dataset is "partitioned" into a training set and a test set. For example, a 90/10 partition means that 90% of the samples are in the training set and 10% of the samples are in the test set. Below is an example table for comparing the error rate of the system across several partitions of a dataset. Note that with this example, the algorithm, the dataset, and the hyperparameters are fixed, so they should be specified along with the results.

| Partition | Error rate |
|-----------|------------|
| 90/10     |            |
| 80/20     |            |
| 70/30     |            |
| 60/40     |            |
| 50/50     |            |
| ...       |            |
