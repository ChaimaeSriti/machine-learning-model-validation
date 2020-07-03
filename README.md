# Introduction

Machine learning models and their validation differ substantially from classical models and classical model validation. 
The use of machine learning models quickly expands to business areas with strong regulation, such as finance and medicine.
Regulators, model validation teams and model developers need to adjust to different requirements on model validation and model development.

These notes aim to go into detail what topics model validation of machine learning should address.

The term model validation is used in multiple contexts and can mean at least the following three things:

* Model evaluation: Estimating a model's performance,

* Code and algorithm review: Fixing technical and methodological bugs,

* Regulatory model validation: To be allowed to use the model in production.

We will focus on the latter two: How to prevent any errors from happening during model deployment, and how to ensure regulatory approval. Model evaluation will play an important role for both.



These notes are split in the following sections:

[Data](#data)
: plays a central part of any machine learning model, during training, testing, and deployment.

[Design](#design)
: This chapter is centered around mathematical appropriateness of the chosen model approach.

[Implementation](#implementation)
: This chapter highlights how errors could occur during the programming part of model development, and how to possibly prevent them.

[Output](#output)
: This chapter is all about verifying the model based on its outputs, a central way of validating any machine learning model.

[Documentation](#documentation)
: Machine learning models have some special requirements on the documentation that differs from classical models.

[Governance & Use](#governance-use)
: Questions around fulfillment of regulatory requirements and model usage.


Version history:

* July 2020: initial version, comments welcome!


**Disclaimer:**
These notes are my personal view and do not necessarily express the opinion of any of my current or former employers. 

If you have any questions, suggestions, other comments, or want to collaborate with me on topics around machine learning model validation please reach out to me at adrianscheerer@gmail.com.



# Data

As machine learning models heavily rely on data, this chapter comes first.


## Data properties

Analyze the data set used with respect to the following properties.

**Representativeness:** Are the train and test data representative of all future use-cases? This is to ensure the
model behavior is known on its intended domain.

**Data set shift:** Do train and test data have the same statistical properties? 

**Data bias:** Data can be biased in many ways. Bias is such an important topic in machine learning, it would deserve its own chapter. Here, we restrict ourselves to a few examples to raise awareness of how bias may arise and influence machine learning models. 

Examples of biased data are:

* In a spam email identification model: All messages of a minority group are labeled as spam.
* Tank vs. no tank classification problem: All pictures of tanks were taken at day. As a consequence, all pictures that were taken at night, even pictures of tanks at night, were classified as not a tank.
* Dog vs. Husky classification: All pictures of huskies were taken before snowy background. As a consequence, all pictures of dogs in front of snowy background were classified as huskies.

**Human bias:** Assess to what extend humans have been part in data generating, data selection and the data labeling process and could have introduced bias.

**Skewed data:** Are test and training data for categorical features evenly distributed over the classes? If not, this could be a source of bias and model assumptions might be violated.

**Data stability:** Is the proposed model stable when the data set is changing? This is important for example if the model will be retrained on new or updated data during production. Assess the stability of the training data over time.

**Outlier treatment:** How are outliers treated? If outliers have been removed from the training data, how can you ensure that the model works correctly once deployed? Outliers are likely to happen during production.


## Data preprocessing

During data preprocessing, modifications are made to the data that will influence model performance. Among others, data preprocessing consists of data imputation, data rescaling and feature engineering.

The following points can be assessed during data preprocessing validation:

* Has there been data leakage, i.e., has information about test data been used during model training? For example, data leakage can happen during data preprocessing if statistical properties of the combined train and test set have been used during data normalization of the training set. 
* How have missing values been treated? For example, see how the model changes if N/A values are no longer replaced by the mean of a feature, but by a random sample from available values of the feature. Does the specific way missing values are replaced make sense from a business perspective?
* If feature selection is part of the model: What features have been selected? Is there a business explanation for why these exact features have been chosen? How do different features influence model performance? Are the features meaningful or are any spurious patterns selected?
* How is data with special properties handled, such as sparse data, wide data, or categorical features with many categories?
* Is the input data scaled in accordance with the assumptions of the algorithm?
* Have train and test data been preprocessed the same way?
* Look at preprocessing of individual input instances when applying the model.


## Data privacy

* If you use cloud-based model training or deployment: Does your institution’s privacy policy allow you to upload the data?
* Is the data anonymized?
* Are you obeying all licenses that are attached to your data?


## Data other

* Is the data labeled correctly?
* Is the data appropriate and meaningful? For example, you would use balance sheet data to predict credit ratings, but not what is the favorite color of the employees.
* Is the data extensive? Has nothing been left out or forgotten during data collection?
* Are there logical errors or inconsistencies in the data? An example of inconsistent data would be that results of a poll show that 73% of the respondents consider option A, 78% consider option B, whereas 40% consider both options.
* Is the data real, as opposed to generated?
* The train-test split needs to respect if several observations come from the same underlying source. For example, if several observations have been collected from the same patient, these observations should not occur in both train and test data.
