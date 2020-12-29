---
layout: post
title: Averaging Weights leads to Wider Optima and Better Generalization
comments: True
excerpt: 
tags: ['2018', 'Stochastic Gradient Descent', 'UAI 2018', AI, Generalization, SGD, SWA, UAI]


---



## Introduction

* The paper proposes Stochastic Weight Averaging (SWA) procedure for improving the generalization performance of models trained with SGD (with cyclic or constant learning rate).

* Specifically, the model is checkpointed at several points along the training trajectory, and these checkpoints are averaged (in the parameter space) to obtain a single model.

* [Link to the paper](https://arxiv.org/abs/1803.05407)

## Idea

* "Stochastic" in the name refers to the idea that with cyclical or constant learning rate, SGD proposals are approximately sampled from a neural network's loss surface and are hence stochastic.

* SWA uses a learning rate schedule that allows exploration in the weight space.

* SGD with cyclical and constant learning rates explore points (model instances) at the periphery of high-performing networks.

* With different initializations, SGD will find different points (of low training loss) on this boundary, but will not move inside it.

* Averaging the points provide a mechanism to move inside this periphery.

* The train and the test error surfaces, while being similar, are not perfectly aligned. Hence, averaging several models (along the optimization trajectory) could lead to a more robust model.

## Algorithm

* Given a model $w$ and some training budget $B$, train the model in the conventional way for approx 75% of the budget. 

* Starting from that point, continue training with the remaining budget, with a constant or cyclical learning rate. 

* For fixed learning rate, checkpoint models at each epoch. For cyclical learning rate, checkpoint the model at the lowest learning rate in the cycle.

* Average all the models to get the SWA model.

* If the model has Batch Normalization layers, run an additional pass to compute the SWA model's running mean and standard deviation.

* The computational and space complexity of computing the SWA model is relatively low.

* The paper highlights the ensembling like the effect of SWA by showing that if the model checkpoints ($w_i$) are generated by training with Fast Geometric Ensembling (FGE), the difference between averaging the weights and averaging the predictions is of the order $O(\Delta)$ where $\Delta = max \|\|w_i - w_{SA}\|\|$.

* Note that SWA does not have the overhead of an extra-forward pass during inference.

## Experiments

* Datasets: CIFAR10, CIFAR100, ImageNet

* Models: VGG16, WideResNet, 164-layer preactivation ResNet, ShakeShake, Pyramid Net.

* Baselines: Conventional SGD, Exponentially decaying average with SGD and FGE.

* In all the CIFAR experiments, SWA consistently outperforms SGD in one budget and consistently improves with training.

* SWA also achieves performance comparable to FGE, despite FGE being an ensemble method.

* On ImageNet, SWA is run on a pre-trained model, and it improves performance in all the cases.

* An ablation experiment (on CIFAR-100) shows that it is possible to train a network (with SWA) using a fixed learning rate. In that setup, using SWA improves performance by 16%.