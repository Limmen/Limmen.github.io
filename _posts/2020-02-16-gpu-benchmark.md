---
title: Tesla P100 GPU Benchmark
updated: 2019-05-11 18:30
---

### Tesla P100 GPU Benchmark

I recently got to play with a GPU server at our lab and wanted to measure it's performance. To evaluate the server's performance on a typical deep learning workload I did a simple benchmark with the following setup. First of all, the server itself is equipped with the hardware described in Table 1. To run the experiment, I generated a synthetic dataset of $20000$ images with dimensions $$\mathbb{R}^{120 \times 120 \times 3}$$. Next, I created a deep feedforward neural network in TensorFlow with varying number of layers and $2000$ units in each layer.  Finally, I measured the run-time to train $5$ epochs on the dataset with varying depth of the neural network and with a hardware setup varying between 1 CPU, 16 CPU, 1 GPU, and 2 GPUs. The experiment parameters are specified in Table 2 and the experiment results are visualized in Fig. TODO. The data is listed in Table 3.

The results demonstrate that the GPU scale very well with larger neural network models as compared to the CPU and is an order of magnitude faster on all metrics. Of course, to do a proper benchmark one should let variables such as number of epochs and batch size be variable, which I did not evaluate in this benchmark. Moreover, neither the GPUs nor CPUs were ``warmed up'' when performing these benchmarks, which could have had some effect on the results. Finally, I did not tune any of the default configurations in TensorFlow, nor did I use any of the optimizations that TensorFlow have available for multi-GPU training.

| Metric                   | Value                     |
|--------------------------+---------------------------|
| RAM                      | 128GB                     |
| CPU                      | 16 Intel Xeon 3.50Ghz CPU |
| GPU                      | 2 Tesla P100 GPU          |
| Storage                  | 2TB SSD                   |
| GPU-to-GPU Communication | PCIe                      |

| Metric                      | Value                                                          |
|-----------------------------+----------------------------------------------------------------|
| Batch Size                  | 2056                                                           |
| Dataset Size                | $\bm{X} \in \mathbb{R}^{20000 \times 120 \times 120 \times 3}$ |
| Number of Classes           | 10                                                             |
| Neurons Per Layer           | 2000                                                           |
| Steps Per Epoch             | 30                                                             |
| Number of Epochs            | 5                                                              |
| Neural Network Architecture | FeedForward Neural Network                                     |
| Hidden Unit Activation      | ReLU                                                           |
| Output Unit Activation      | Softmax                                                        |
| Loss Function               | Cross-Entropy                                                  |
| Multi-GPU Training          | Data-Parallel with Collective-All-Reduce                       |
