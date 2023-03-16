---
type: Paper Explanation
tags: [Self-Supervised, Data-Augmentation]
society: CVPR
status: ongoing
publishD: 2022
id: PECVPRSSLDAn001
---

# Learning Where to Learn in Cross-View Self-Supervised Learning
Lang Huang, Shan You, Mingkai Zheng, Fei Wang, Chen Qian, Toshihiko Yamasaki

DOI: [Paper Link](https://doi.org/10.48550/arXiv.2203.14898)
# Problem
We use random cropping, random erasing etc augmentations over image-level data input to learn invariant behaviour. This encourage the model to find hypothesis that learn better representations. Self Supervised Representation learning is mainly guided by projection into an *embedding space*. During the projection, current method simply adopt **uniform agregation of pixels** for embedding which states that at any point of time we consider approximately fixed amount of pixels to form a projection in embedding space for e.g random cropping crop fixed size of patch from the image and make the model to perform spatial alignment over it: however this risks involving **object irrelevant nuisances** like background crop consisting grassland which could be aligned with any dog, cat etc and **spatial misalignment** for different augmentations. Augmentations like Random cropping capture the spatial information and then we try to align this with the anchor input or any positive or negative pair of it in spatial domain. But it will result in spatial misalignment if input involves object irrelevant nuisances. Well good degree of spatial misalignment is beneficial according to [[What makes for good views for Contrastive Learning]],it remains unclear how to choose optimal degree of misalignment. 

# Proposed Approach
Authors argue that good 
	self-supervised representation learning algorithm should not leverage task specific priors but learn local representations spontaneously. 

They proposed a new self-supervised learning approach
Learning Where to Learn (LEWEL) in a pure end-to-end manner. 

> [!ATTENTION]
> Learn Global Average Pooling(Gap), Alignment Maps

$$sin(x) = x-\frac{x^{3}}{3}+\frac{x^{5}}{5}...$$
