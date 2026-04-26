<h1>Mechanisms of Action (MoA) Prediction</h1>
Can you improve the algorithm that classifies drugs based on their biological activity?

[Link to competetion](https://www.kaggle.com/competitions/lish-moa/overview)

<h2>Description</h2>
The Connectivity Map, a project within the Broad Institute of MIT and Harvard, the Laboratory for Innovation Science at Harvard (LISH), and the NIH Common Funds Library of Integrated Network-Based Cellular Signatures (LINCS), present this challenge with the goal of advancing drug development through improvements to MoA prediction algorithms.

<br>
<b>What is the Mechanism of Action (MoA) of a drug? And why is it important?</b>
<br>
In the past, scientists derived drugs from natural products or were inspired by traditional remedies. Very common drugs, such as paracetamol, known in the US as acetaminophen, were put into clinical use decades before the biological mechanisms driving their pharmacological activities were understood. Today, with the advent of more powerful technologies, drug discovery has changed from the serendipitous approaches of the past to a more targeted model based on an understanding of the underlying biological mechanism of a disease. <u>In this new framework, scientists seek to identify a protein target associated with a disease and develop a molecule that can modulate that protein target.</u> As a shorthand to <strong>describe the biological activity of a given molecule</strong>, scientists assign a label referred to as mechanism-of-action or MoA for short.

<br>
<b>How do we determine the MoAs of a new drug?</b>
<br>
One approach is to treat a sample of human cells with the drug and then analyze the cellular responses with algorithms that search for similarity to known patterns in large genomic databases, such as libraries of gene expression or cell viability patterns of drugs with known MoAs.

In this competition, you will have access to a unique dataset that combines gene expression and cell viability data. The data is based on a new technology that measures simultaneously (within the same samples) human cells’ responses to drugs in a pool of 100 different cell types (thus solving the problem of identifying ex-ante, which cell types are better suited for a given drug). In addition, you will have access to MoA annotations for more than 5,000 drugs in this dataset.

As is customary, the dataset has been split into testing and training subsets. Hence, your task is to use the training dataset to develop an algorithm that automatically labels each case in the test set as one or more MoA classes. Note that since drugs can have multiple MoA annotations, <b>the task is formally a multi-label classification problem.</b>

<br>
<b>How to evaluate the accuracy of a solution?</b>
<br>
Based on the MoA annotations, the accuracy of solutions will be evaluated on the average value of the logarithmic loss function applied to each drug-MoA annotation pair.
<br>
If successful, you’ll help to develop an algorithm to predict a compound’s MoA given its cellular signature, thus helping scientists advance the drug discovery process.


<h2>Evaluation</h2>

For every sig_id you will be predicting the probability that the sample had a positive response for each `MoA` target. For 
 sig_id rows and `MoA` targets, you will be making $N \times M$  predictions. Submissions are scored by the log loss:

$$\text{score} = - \frac{1}{M}\sum_{m=1}^{M} \frac{1}{N} \sum_{i=1}^{N} \left[ y_{i,m} \log(\hat{y}_{i,m}) + (1 - y_{i,m}) \log(1 - \hat{y}_{i,m})\right]$$

where:

* $N$ is the number of sig_id observations in the test data ($i=1,..,N$)
* $M$ is the number of scored MoA targets ($m=1,..,M$)
* $\hat{y}_{i,m}$ is the predicted probability of a positive MoA response for a sig_id
* $y_{i,m}$ is the ground truth, 1 for a positive response, 0 otherwise
* $log()$ is the natural (base e) logarithm

Note: the actual submitted predicted probabilities are replaced with $max(min(p,1-10^{-15}),10^{-15})$. A smaller log loss is better.