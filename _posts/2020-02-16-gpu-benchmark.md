---
title: Tesla P100 GPU Benchmark
updated: 2020-02-16 15:00
---

### Tesla P100 GPU Benchmark

I recently got to play with a GPU server at our lab and wanted to measure it's performance. To evaluate the server's performance on a typical deep learning workload I did a simple benchmark with the following setup. First of all, the server itself is equipped with the hardware described in Table 1. To run the experiment, I generated a synthetic dataset of $$20000$$ images with dimensions $$\mathbb{R}^{120 \times 120 \times 3}$$. Next, I created a deep feedforward neural network in TensorFlow with varying number of layers and $$2000$$ units in each layer.  Finally, I measured the run-time to train $$5$$ epochs on the dataset with varying depth of the neural network and with a hardware setup varying between 1 CPU, 16 CPU, 1 GPU, and 2 GPUs.

#### Hardware Setup (Table 1)

| Metric                   | Value                     |
|--------------------------+---------------------------|
| RAM                      | 128GB                     |
| CPU                      | 16 Intel Xeon 3.50Ghz CPU |
| GPU                      | 2 Tesla P100 GPU          |
| Storage                  | 2TB SSD                   |
| GPU-to-GPU Communication | PCIe                      |

#### Experiment Parameters (Table 2)

| Metric                      | Value                                                            |
|-----------------------------+------------------------------------------------------------------|
| Batch Size                  | 2056                                                             |
| Dataset Size                | $$X \in \mathbb{R}^{20000 \times 120 \times 120 \times 3}$$ |
| Number of Classes           | 10                                                               |
| Neurons Per Layer           | 2000                                                             |
| Steps Per Epoch             | 30                                                               |
| Number of Epochs            | 5                                                                |
| Neural Network Architecture | FeedForward Neural Network                                       |
| Hidden Unit Activation      | ReLU                                                             |
| Output Unit Activation      | Softmax                                                          |
| Loss Function               | Cross-Entropy                                                    |
| Multi-GPU Training          | Data-Parallel with Collective-All-Reduce                         |

#### Results (Table 1 & Fig 1)

The results demonstrate that the GPU scale very well with larger neural network models as compared to the CPU and is an order of magnitude faster on all metrics. Of course, to do a proper benchmark one should let variables such as number of epochs and batch size be variable, which I did not evaluate in this benchmark. Moreover, neither the GPUs nor CPUs were "warmed up" when performing these benchmarks, which could have had some effect on the results. Finally, I did not tune any of the default configurations in TensorFlow, nor did I use any of the optimizations that TensorFlow have available for multi-GPU training. Multi-GPU training did not show any significant performance boost in this case, which might be due to data transfer bottleneck, that the batch size was set too low, or that the number of epoch were too low. The code for the benchmarks is available [here](https://github.com/Limmen/gpu_benchmark).

| Hardware                  | 5 Layers | 15 Layers | 30 Layers | 45 Layers | 60 Layers | 75 Layers |
|---------------------------+----------+-----------+-----------+-----------+-----------+-----------|
| 1 Intel Xeon 3.50Ghz CPU  | 1624s    | 2396s     | 3583s     | 4850s     | 6046s     | 7311s     |
| 16 Intel Xeon 3.50Ghz CPU | 235s     | 337s      | 499s      | 663s      | 830s      | 985s      |
| 1 Tesla P100 GPU          | 95s      | 99s       | 98s       | 116s      | 192s      | 199s      |
| 2 Tesla P100 GPU          | 97s      | 101s      | 109s      | 107s      | 115s      | 181s      |

![GPU Benchmark Results](/assets/gpu_benchmark_results.png "GPU Benchmark Results")
