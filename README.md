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
<p align="center">
  <img width="350" height="250" src="/images/DNN_metrics.png">
</p>

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
