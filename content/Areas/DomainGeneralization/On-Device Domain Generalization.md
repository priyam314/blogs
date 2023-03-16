---
math: true
---

## Author Affiliation

Kaiyang Zhou, Yuanhan Zhang, Yuhang Zang, Jingkang Yang, Chen Change Loy, Ziwei Liu  
**S-Lab, Nanyang Technological University, Singapore**

DOI: [Paper Link](https://doi.org/10.48550/arXiv.2209.07521) 

DG:: [[Domain Generalization Introduction | Domain Generalization]] 
KD:: [[Knowledge Distillation]] 
OOD:: [[Deep Learning/Out-of-Distribution/Introduction | Out-of-Distribution]]
ID:: [[Deep Learning/In-Distribution/Introduction | In-Distribution]]
ERM:: [[Deep Learning/Empirical Risk Minimization/Introduction | Empirical Risk Minimization]]  
RSC:: Representation Self-Challenging

<br>

# Problem
> [!warning]- Present research focus on large models
> Present research in machine learning has been heavily focusing on large models of huge number of parameters and model size to store weights

> [!warning]- Low cost edge device constraint
> Low cost edge devices have certain constraints which limits the usage of large models and call for tiny neural networks of approximately similar predictive power.
>> [!info]- More...
>>  some edge devices have 512Kb SRAM which is not enough for most neural network to fit.

> [!warning]- DG non-trivial for tiny neural nets
> Making tiny neural networks Domain Generalizable is non-trivial since tiny nets have much smaller number of parameters than larger neural networks, hence smaller capacity.

<br>

# Motivation

Low cost edge devices are used in random environment which makes <abbr title="Domain Generalization" class="abbr-style">DG</abbr> problem more relevant for tiny neural networks.

![[Pasted image 20220918221558.png| "upp"]]

Looking at the following table we can say that tiny neural networks tend to bring not much significant difference to <abbr title="Out-of-Distribution" class="abbr-style">OOD</abbr> data in comparision to large neural networks

<br>

# Related Works

> [!summary]- Domain Generalization
> Some DG methods
>> [!summary]- Domain Alignment
>> Employ a distance measure like Maximum Mean Discrepancy or Adversarial Leanring to reduce feature distribution between two or multiple source domains.
>
>> [!summary]- Meta-Learning
>> They adopt the notion of Learning to Learn 
>
>> [!summary]- Data Augmentation
>> Can use Generative model or mixing data at input or feature level 
> - Recent Research has explored test-time adaptation which updates the model on-the-fly using a test data point or minibatch.
> 
> - Researchers have also explored MultiModal Learning with joint embedding space of image and language.

> [!summary]- Knowledge Distillation
>  Transferring representations from Teacher to Student KD methods
>  - Hints
>
>  - Probabilistic Distribution
>
>  - Attention Maps
>
>  - Mutual Information
>
>  - inter-instance correlations based on some measure like
> 	 - Contrastive Learning
> 
> 	 - Similarity Matrices

<br>

# Preliminary Observation

> [!summary]- Improvement over ERM model using DG methods
> 1-2% improvement over <abbr title="Empirical Risk Minimization" class="abbr-style">ERM</abbr> model using <abbr title="Domain Generalization" class="abbr-style">DG</abbr> state-of-the-art methods for tiny neural networks.
> 
>> [!note]- Note re ERM Models
>> According to [[Research_Papers/In Search of Lost Domain Generalization | In Search of Lost Domain Generalization]], ERM Model known to be strong baseline method

> [!summary]- KD boosts DG performance more than DG methods
>  <abbr title="Knowledge Distillation" class="abbr-style">KD</abbr> boosts <abbr title="Domain Generalization" class="abbr-style">DG</abbr> performance more than DG methods. Following figure shows that KD methods give more improvement over ERM for <abbr title="Out-of-Distribution" class="abbr-style">OOD</abbr> data.
> ![[Pasted image 20220919144828.png | 300x200]]

> [!summary]- Large performance gap on OOD data 
> Even after significant amount of improvment in <abbr title="Knowledge Distillation" class="abbr-style">KD</abbr> methods, performance gap in teacher and students' accuracy on <abbr title="Out-of-Distribution" class="abbr-style">OOD</abbr> data is still ~12% for various <abbr title="Knowledge Distillation" class="abbr-style">KD</abbr> based methods
> ![[Pasted image 20220919145753.png | 300x200]]

> [!summary]- Capacity mismatch of model on OOD data vs ID data
> Gap on <abbr title="Out-of-Distribution" class="abbr-style">OOD</abbr> data is much bigger in comparision to <abbr title="In-Distribution" class="abbr-style">ID</abbr> data for same <abbr title="Knowledge Distillation" class="abbr-style">KD</abbr> methods
> ![[Pasted image 20220919150913.png | 300x200]]
> The following suggests that capacity mismatch constrains the transfer of generalizable knowledge from teacher to student.

> [!summary]- Student never been taught to handle OOD data
>> [!note]- Note re KD methods
>> Conventional KD loss is computed on <abbr title="In-Distribution" class="abbr-style">ID</abbr> data only.
>
>  Student has never been taught how to handle <abbr title="Out-of-Distribution" class="abbr-style">OOD</abbr> data and therefore failed to match teachers' OOD performance

<br>

# Proposition
1. Tiny neural networks should not be trained in same way to that of large neural networks for <abbr title="Domain Generalization" class="abbr-style">DG</abbr>.

2. Use <abbr title="Knowledge Distillation" class="abbr-style">KD</abbr>, since they are well established for compressing large models.

3. <abbr title="Out-of-Distribution Knowledge Distillation" class="abbr-style">OKD</abbr> which extends KD by adding another KD loss computed on <abbr title="Out-Of-Distribution" class="abbr-style">OOD</abbr> data that is synthesized using image tranformations. 

<br>

# Contributions
1. DG datasets, which constitute the *DOmain Shift in Context* (DOSCO) benchmark. 

2. DOSCO is diverse in both dimensions 
	1. **Cover broader categories** - generic objects, fine-grained categories like aircrafts and animals, and human actions
	2. **Wider spectrum of domain shift** -  image style, background, viewpoint, object pose etc

</br>

# Approach
Idea of <abbr title="Out-of-Distribution Knowledge Distillation" class="abbr-style">OKD</abbr> is simple. Its' to teach student that how teacher handles <abbr title="Out-Of-Distribution" class="abbr-style">OOD</abbr> data. 

## OOD Data Generator
1. Collecting extra OOD data is impractical, so used image transformations to synthesize OOD data.

2. Selected best Augmentation Function or OOD data generator  $A(\cdot)$ after thorough research from list of candidate image transformations. 

3. Best OOD data generator came out to be **CutMix+MixUp** 

4. Teachers' prediction over soft probability distribution provides valuable insight for any $A(x)$ which might not belong to any existing class for the student to learn. 

</br>

## Out-of-Distribution Knowledge Distillation
<abbr title="Out-of-Distribution Knowledge Distillation" class="abbr-style">OKD</abbr> loss follows as 
$$
L_{OKD} = \lambda \textcolor{CornflowerBlue}{H(y, f_{S}(x))}+(1-\lambda)\textcolor{LimeGreen}{D_{KL}(f_{s}(x), f_{T}(x))}+ \textcolor{OrangeRed}{D_{KL}(f_{S}(A(x)), f_{T}(A(x)))}
$$
where,  
$f_{S}(\cdot)$ is Student Encoder  
$f_{T}(\cdot)$ is Teacher Encoder  
$D_{KL}$ is [[KL Divergence]] 

- $\textcolor{CornflowerBlue}{H(\cdot)}$ is cross-entropy loss for student to learn classification of input data where $\textcolor{CornflowerBlue}{y}$ is output label and $\textcolor{CornflowerBlue}{f_{S}(x)}$ is model output of Student for input $x$.  

- $\textcolor{LimeGreen}{D_{KL}(f_{s}(x), f_{T}(x))}$ makes student to learn how teacher is predicting the output of $x$.

- $\lambda$ is weight balancing term, which is set to $\textcolor{Cerulean}{0.1}$ 

- $\textcolor{OrangeRed}{D_{KL}(f_{S}(A(x)), \, f_{T}(A(x)))}$ makes student to learn how teacher is predicting <abbr title="Out-Of-Distribution" class="abbr-style">OOD</abbr> data. $A(\cdot)$ creates a data which most likely lie outside of domain.

This addition of $\textcolor{OrangeRed}{red \ term}$ adds negligible overhead in training since no model architecture has been changed e.g. number of parameters, cost function time complexity, image size are constant 

</br>

## Model Architecture
![[Pasted image 20221005160757.png]]

</br>

# Experiments

## Model Architecture
- **MobileNetV3-Small** as the Student Model 

- **ResNet-50** as the Teacher Model

- all models pre-trained on *ImageNet* 

</br>

## Training Details
- **Batch Size**: 32

- **Optimizer**: SGD with Momentum

- **Learning Rate**: 0.01

- **Learning Rate Scheduler**: Cosine Annealing

- **Max Epochs**: 100

- **Augmentations**: Random Crop, Flip

</br>

## Baseline Methods
DOSCO-2k does not provide training domain labels, hence chose those <abbr title="Domain Generalization" class="abbr-style">DG</abbr> training strategies that do not need labels, example:-

1. [[Deep Learning/Empirical Risk Minimization/Introduction | ERM]] 

2. [RSC](https://arxiv.org/abs/2007.02454) 

3. [MixStyle](https://arxiv.org/pdf/2104.02008.pdf)

4. [EFDMix](https://arxiv.org/pdf/2203.07740.pdf) 

</br>

## Choosing OOD data Generator

- Compared some commonly used image transformations and found best image transformation out of those.

- below is list of image transformations that authors have used.
	- ![[Pasted image 20221006101157.png|300x200]]
- Following are the results on DOSCO-2K over KD for candidate image transformations
	- ![[Pasted image 20221006101724.png|300x200]]


- Adversarial Gradient *worse* than Gaussian Noise
	- which suggest even Teacher is consfused about the samples close decision boundary

- Jigsaw destroys the global structure

- MixUp maintains the global structure 

- CutMix only twists the local statistics 

- Following are probably the reason **CutMix + MixUp** are best for synthesizing OOD samples. 

</br>

# Conclusion
- Current SOTA <abbr title="Domain Generalization" class="abbr-style">DG</abbr> methods do not work consistently well for different tiny networks
	- MixStyle and EFDMix can improve upon ERM when using MobileNetV3-Small

	- ERM performs better than above two when it comes to MobileNetV2-Tiny and MCUNet 
		- Both MobileNetV2-Tiny and MCUNet designed for MCU

- OKD framework provides great potential for future research in this field.

</br>

# Limitation
- Even after <abbr title="Out-of-Distribution Knowledge Distillation" class="abbr-style">OKD</abbr>, performance gap with large models is still huge
	- 10% difference compared with ResNet50 on DOSCO-2K

</br>

# Future Work
- Can we design more advanced OOD data generators or even make them fully learnable?

- Is it possible to combine KD with DG methods while pursuing efficacy?

- Will state-of-the-art generative models such as GAN or diffusion models help?

- Making model learning fully on-device, i.e., allowing model updates to be performed on tiny devices?

- Can we try some parameter-efficient designs that can offer a good trade-off between performance and efficiency?
