# CS5824: Drug Response Project

Mandar Sharma

This project, a continuation of my work with Dr. Anuj Karpatne and Md Abdullah Al Maruf, aims to use the drug-response data on bacterial cells [4] and attempts to utilize the drug-protein and protein-protein interaction networks extracted from [1] such that we could use state-of-the-art graph Machine Learning algorithms [2,3] that could help us better predict the drug-responses given the structural embeddings we learn through the protein interaction networks of the drugs. In essence, [4] and many others have attempted to predict drug-responses solely based on data from lab-based experiments without taking into consideration the fact that certain proteins that these drugs act on can give us valuable structural information that might help us make better predictions on the drug-responses.

## Hardware/Software Development Environement

This project was developed in a LINUX system and all the neural networks were run through a GPU cluster. The language used for development was Python 3.6. Please make sure that the following libraries are installed in your Python environment before running the code provided here.

```sh
keras
pytorch
pytorch-geometric
stellargraph
sklearn
pandas
numpy
networkx
pickle
tqdm
```

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
  <img width="500" height="125" src="/images/DNN_metrics.png">
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
  <img width="450" height="325" src="/images/embeddings.png">
</p>

We can see that the embeddings are forming clusters, hinting that the embeddings that we extracted may resemble neighborhoods in the PPI and DPI graphs. Now, through the implementation of unsupervised graphSAGE, we have 50 dimensional embeddings for each of the 7 drugs, which gives us additional information about the proteins they act on.

### Running the Unsupervised GraphSAGE
```sh
jupyter nbconvert --to python graphSAGE/graphSAGE.ipynb
```
## EC-Convolution for Graph-level Predictions

Now that we have established our baseline and have extracted embeddings for our 7 drugs based on their DPI and PPI networks, now we can use the state-of-the-art graph classification algorithm EC-CONV [2] to make graph level predictions.

Each instance of our graph is as following:
<p align="center">
  <img width="450" height="325" src="/images/graph.png">
</p>

Where, the nodes represent the 7 drugs. Each node feature is associated with an instance of the the training sample, such that the node features represent the drug dosage information. For singletons, only one of the 7 drugs will have a node feature associate with its dosage and the node features for the other drugs will be zero. The same protocol is followed for pairs and triplets of drugs.

For the edge features, we use the Hadamard product of the learned embeddings for each drugs. Thus, we have a 50 dimensional edge feature for each edge in the graph.

Finally, each instance of our graph will have an associated 'g' value for training.

We have used an EC-Convolution Network (EC-Conv) with 10 input channels and 10 output channels. The dataset used for training/testing, as same as the baseline, is a shuffled combination of Singletons, Pairs, and Triplets. The difference being that for EC-Conv, as described above, each data instance is represented as a graph with node features corresponding to the dosage values and each features corresponding to the learned DPI/PPI embeddings. The suffled set is split 50-50 for training and testing. We further test our model with 100%, 70%, and 30% of our training data respectively to see how it would perform under limited data constraints.

### EC-Convolution (Training Loss)
<p align="center">
  <img width="450" height="325" src="/images/ECCONV_trainingloss.png">
</p>

### EC-Convolution (True Values vs. Predicted Values)
<p align="center">
  <img width="450" height="325" src="/images/ecconv_tvsp.png">
</p>

### EC-CONV Evaluation Metrics
The evaluation metrics used for both the baseline and the EC-Convolution are the MSE (Mean Square Error) and the R2-Score.
<p align="center">
  <img width="500" height="125" src="/images/ECCONV_metrics.png">
</p>

Here, we can see that EC-CONV outperforms our baseline by a significant margin on all sizes of the training data. Thus, the structual embeddings learned from the DPI/PPI graph networks did help us enhance our predictions.

### Running the EC-CONV
```sh
jupyter nbconvert --to python EC_Convolution.ipynb
```

## Future Directions

An important analysis that we could further do, is to use random noise (perhaps generated from some distribution), as edge features and run EC-CONV. If we get a reduced accuracy or increased error, we could confirm that what we extracted from the structural information of the DPI/PPI graphs is truly a signal and that EC-CONV is not generally a better model than a DNN.

## References
[1]  String protein interaction network.  https://string-db.org/.

[2]  Justin  Gilmer,  Samuel  S.  Schoenholz,  Patrick  F.  Riley,  Oriol  Vinyals,  and  George  E.  Dahl.   Neural message passing for quantum chemistry. CoRR, abs/1704.01212, 2017.

[3]  William L. Hamilton, Rex Ying, and Jure Leskovec.  Inductive representation learning on large graphs. CoRR, abs/1706.02216, 2017.

[4]  Kevin  B.  Wood,  Satoshi  Nishida,  Eduardo  D.  Sontag,  and  Philippe  Cluzel.   Mechanism-independent method  for  predicting  response  to  multidrug  combinations  in  bacteria. Proceedings  of  the  NationalAcademy of Sciences of the United States of America, 109 30:12254–9, 2012.
