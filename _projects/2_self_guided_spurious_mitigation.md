---
layout: post
title: Learning Robust Classifiers with Self-Guided Spurious Correlation Mitigation
description:  We tackle an annotation-free setting and propose a self-guided spurious correlation mitigation framework. Our framework automatically constructs fine-grained training labels tailored for a classifier obtained with empirical risk minimization to improve its robustness against spurious correlations.
img: /assets/images/self_guided_debiasing_method.png
---

Deep neural classifiers tend to rely on spurious correlations between spurious attributes of inputs and the targets to make predictions, which could jeopardize their generalization capability.
Training classifiers robust to spurious correlations typically rely on data with spurious correlation annotations, e.g., group information, which is often expensive to get. In this paper, we tackle an annotation-free setting and propose a self-guided spurious correlation mitigation framework. 
Our framework automatically constructs fine-grained training labels tailored for a classifier obtained with empirical risk minimization to improve its robustness against spurious correlations. The fine-grained training labels are formulated with different prediction behaviors of the classifier identified in a novel spuriousness embedding space. We construct the space with automatically detected conceptual attributes and a novel spuriousness metric which measures how likely a class-attribute correlation is exploited for predictions. 
We demonstrate that training the classifier to distinguish different prediction behaviors reduces its reliance on spurious correlations without knowing them a priori and outperforms prior methods on five real-world datasets.

![method](/assets/images/self_guided_debiasing_method.png){:.centered}

Method overview. (a) Detecting attributes with a pre-trained VLM. (b) Quantifying the spuriousness of correlations between classes and detected attributes. (c) Clustering in the spuriousness embedding space for relabeling  the training data. (d) Diversifying the outputs of the classifier and training the classifier with balanced training data.

