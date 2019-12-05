# Drug Response Project

This project, a continuation of my work with Dr. Anuj Karpatne and Md Abdullah Al Maruf, aims to use the drug-response data on bacterial cells [4] and attempts to utilize the drug-protein and protein-protein interaction networks extracted from [1] such that we could use state-of-the-art graph Machine Learning algorithms [2,3] that could help us better predict the drug-responses given the structural embeddings we learn through the protein interaction networks of the drugs. In essence, [4] and many others have attempted to predict drug-responses solely based on data from lab-based experiments without taking into consideration the fact that certain proteins that these drugs act on can give us valuable structural information that might help us make better predictions on the drug-responses.

## Introduction to the Dataset

For this project, we utilize the dataset from [4] which consists of drugs Chloramphenicol, Doxycycline, Erythromycin, Lincomycin, Ofloxacin, Salicylate, and Trimethoprim. Here, we refer to the effectiveness of a drug, given a certain dosage, as its 'g' value. The dataset, curated from [4], has been tabulated intro three forms: Singletons, Pairs, and Triplets of drugs. Each of these tabulations has the drug combinations, along with their dosage and 'g' values.

### Accessing the Dataset

The tabulated dataset is accessible under the /woodsdata folder in this repository.

## Baseline (Deep Neural Network)

The baseline for this project that we have used is a Deep Neural Network (DNN) with 5 hidden layers with 50 nodes each. The dataset used for training/testing is a shuffled combination of Singletons, Pairs, and Triplets. The suffled set is split 50-50 for training and testing. We further test our model with 100%, 70%, and 30% of our training data respectively to see how it would perform under limited data constraints. We run our DNN for 500 epochs, and perform this iteration 10 times over to ensure that there are no flukes in our metrics.

### Baseline (True Values vs. Predicted Values)
<p align="center">
  <img width="450" height="325" src="/images/dnn_tvsp.png">
</p>

### Baseline Evaluation Metrics
The evaluation metrics used for both the baseline and the EC-Convolution are the MSE (Mean Square Error) and the R2-Score.
<p align="center">
  <img src="/images/DNN_metrics.png">
</p>

Here, we can see that our baseline performs pretty well, with a high R2-Score of 94.3% when the 100% of the training data is used.

### Running the Baseline
```sh
jupyter nbconvert --to python DNN_Baseline.ipynb
```
## Extracting Information from Protein-Protein and Drug-Protein Interaction Networks

To this point, we have only used the drug dosage values and 'g' values to make predictions. As mentioned in the introduction, [1] offers resources for protein-protein interaction (PPI) and drug-protein interaction (DPI) networks. Thus, we utilize the DPI and PPI networks to extract embeddings for the drugs based on the structural properties of the DPI/PPI network. 

For this, we use unsupervised graphSAGE [3] to extract the structural embeddings of the DPI and PPI network. Using undersupervised graphSAGE (with the StellarGraph library https://github.com/stellargraph/stellargraph), we learn 50 dimensional embeddings for each of the 7 drugs and the thousands of proteins in our network.

Using, T-Distributed Stochastic Neighbouring Entities (t-SNE) plots, we can visualize the learned embeddings in two dimensions.

<p align="center">
  <img src="/images/embeddings.png">
</p>

We can see that the embeddings are forming clusters, hinting that the embeddings that we extracted may resemble neighborhoods in the PPI and DPI graphs. Now, through the implementation of unsupervised graphSAGE, we have 50 dimensional embeddings for each of the 7 drugs, which gives us additional information about the proteins they act on.

### Running the Unsupervised GraphSAGE
```sh
jupyter nbconvert --to python graphSAGE/graphSAGE.ipynb
```
