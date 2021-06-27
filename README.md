# Model Card

This repository is managed and updated by [Adel Hassen](https://github.com/adelhassen). For questions or concerns, please reach out at adelhassen17@gmail.com.

## Intended Use

The model enables the user to identify whether the annual percentage rate (APR) charged for a mortgage is 1.5% or more above a survey-based estimate of similar mortgages.


### Users

- **Consumer Financial Protection Bureau (CFPB)**: A government agency with the purpose of ensuring financial companies treat their members fairly.
- **U.S. Financial Instituions**: They distribute mortgagaes so they can use this tool in-house to assess themselves for fairness prior to government evaluation.
- **Machine learning due diligence organizations**: Third-party assessment of financial institutions.

### Primary inteded uses

This model is designed to flag predatory loan behavior exhibited by banks. It focuses on home mortgages. It accepts certain features related to an applicant’s financial situation and information on the mortgage itself.


### Secondary uses

This model is designed to predict whether a mortgage is high-priced or not. The model has not been tested for any other purposes, but it has the potential to be extended to analyze lending methods other than mortgages.


## Training Data

### Source

This model was trained on data collected by the CFPB under the [Home Mortgage Disclosure Act (HMDA)](https://www.consumerfinance.gov/data-research/hmda/). HMDA mandates financial institutions to provide mortgage data for the general public’s scrutiny. The data can be used to help public officals make decisions or in our case, we investigate whether lending patterns of financial institutions are discriminatory or predatory. 

The training data can be accessed from this Github [repository](https://github.com/jphall663/GWU_rml/blob/master/assignments/data/hmda_train_preprocessed.zip).

### Training data information

The training data was randomly split into training and validation set using the Python NumPy package. 
- Training data: rows = 112253, columns = 23
- Validation data: rows = 48085, columns = 23

### Training data columns

The following features were included:

- high_priced: Binary target, whether (1) or not (0) the annual percentage rate (APR) charged for a mortgage is 150 basis points (1.5%) or more above a survey-based estimate of similar mortgages. (High-priced mortgages are legal, but somewhat punitive to borrowers. High-priced mortgages often fall on the shoulders of minority home owners, and are one of many issues that perpetuates a massive disparity in overall wealth between different
demographic groups in the US.)

- conforming: Binary numeric input, whether the mortgage conforms to normal standards (1), or whether the loan is different (0), e.g., jumbo, HELOC, reverse mortgage, etc.

- debt_to_income_ratio_std: Numeric input, standardized debt-to-income ratio for mortgage applicants.

- debt_to_income_ratio_missing: Binary numeric input, missing marker (1) for debt to income ratio std.

- income_std: Numeric input, standardized income for mortgage applicants.

- loan_amount_std: Numeric input, standardized amount of the mortgage for applicants.

- intro_rate_period_std: Numeric input, standardized introductory rate period for mortgage applicants.

- loan_to_value_ratio_std: Numeric input, ratio of the mortgage size to the value of the property for mortgage applicants.

- no_intro_rate_period_std: Binary numeric input, whether or not a mortgage does not include an introductory
rate period.

- property_value_std: Numeric input, value of the mortgaged property.

- term_360: Binary numeric input, whether the mortgage is a standard 360 month mortgage (1) or a different type of mortgage (0).

## Evaluation Data

### Source

The test data can be accessed from this Github [repository](https://github.com/jphall663/GWU_rml/blob/master/assignments/data/hmda_test_preprocessed.zip).

### Test data information

•	Validation data rows = 48085, columns = 23

### Test data columns

The columns used in the test data are the same as the columns used in the training data.

## Model Details

### Input features

- conforming
- debt_to_income_ratio_std
- debt_to_income_ratio_missing
- income_std
- loan_amount_std
- intro_rate_period_std
- loan_to_value_std
- no_intro_rate_std
- property_value_std
- term_360

### Target feature

- high_priced

### Model type

Binary classification algorithm was used. The model selected was an Explainable Boosting Model (EBM) which is an interpretable model that builds upon GAMs.

### Software version

Interpret ML version 0.2.4 was used to develop the EBM model.

### Hyperparameters of model

- max_bins: 128
- max_interaction_bins: 32
- interactions: 15
- outer_bags: 8
- inner_bags: 0
- learning_rate: 0.01
- validation_size: 0.25
- min_samples_leaf: 5
- max_leaves: 3
- early_stopping_rounds: 100.0
- n_jobs: 4
- random_state: 12345

## Quantitative Analysis

### Metrics

<img width="666" alt="Accuracy" src="https://user-images.githubusercontent.com/56606588/123524555-22602a00-d699-11eb-81c9-c5c7da0183f7.png">

AUC, accuracy, and F1 are the metrics used to evaluate the model on training, validation, and test data. The metrics are fairly stable across the different folds of our dataset.

### Plots

#### Correlation Plot

![debt to income_ratio_std](https://user-images.githubusercontent.com/56606588/123524910-3ad14400-d69b-11eb-8f34-43f796c1954c.png)

A simple pearson correlation plot allows us to quickly identify the relationship each input feature has with our target feature high_priced. Property_value_std and loan_amount_std have the strongest negative correlations. Debt_to_income_ratio_std and loan_to_value_ratio_std have the strongest positive correlations.

#### Variable Importance

![Overall Importance](https://user-images.githubusercontent.com/56606588/123524987-d662b480-d69b-11eb-827c-4fe2320ea2c3.png)

The variable importance is displayed above. The plot includes both individual features and interactions between features. Loan_to_value_ratio_std is weighed much more heavily than the other features which raises concern. When a model depends heavily on only one feature; if someone can figure out a way to manipulate that variable, then they have learned a large amount of information about our model,

#### Variable Importance from Model Extraction Attack
![Variable Importance H2O DRF](https://user-images.githubusercontent.com/56606588/123525291-8553c000-d69d-11eb-92d8-692e76c36f70.png)

To compare, this is the variable importance plot of the decision tree used in our model extraction attack test. It values different features heavily like intro_rate_period_std, and loan_to_value_ratio_std received almost no importance. 

#### Partial Dependence Plots

<img width="999" alt="Best EBM Partial Dependence" src="https://user-images.githubusercontent.com/56606588/123525404-5db12780-d69e-11eb-8329-d71b9db8bcda.png">

These are partial dependence plots for each feature. It shows the marginal effect a feature has on the prediced outcome of the model. Debt_to_income_ratio_std has the steepest partial dependence suggesting the model is sensitive to changes in that feature.

#### Model Remediation

![Screen Shot 2021-06-25 at 12 10 01 PM](https://user-images.githubusercontent.com/56606588/123526097-956e9e00-d6a3-11eb-82d8-5bbde584fe53.png)

This plot shows adverse impact ratio (AIR) vs AUC. AIR was used for model remediation. The AIR uses the protected group of black people and the reference group of white people. The minimum required threshold is 0.8 as shown by the red dotted line. The best performing remediated model has an AUC of 0.8243 and ann AIR OF 0.8364.

#### Global Logloss Residuals

![Global Logloss Residuals](https://user-images.githubusercontent.com/56606588/123526254-8f2cf180-d6a4-11eb-8305-26430196fd16.png)

This plot of logloss shows that the residuals are unbalanced. The model is much better at predicting when an applicant will not receive a high-priced loan, but it does not do well predicting when an applicant will receive a high-rpiced loan.

### Alternative Models

Only glassbox models were trained. Other models including a GBM, a glm model, and an unremediated EBM were trained. When selecting our top model; not only did we consider performance, we also highly valued fairness. Initially, the EBM model was the top performing model. All the models suffered from fairness issues. After remediating the model to a certain level, the EBM model only slightly dropped in performance and we selected the remediated version as our top model.

## Ethical Considerations


### Fairness

A well known issue with mortgages and loans in general is that minority populations tend to receive worse rates. A major concern with our model is whether it discriminated against these minority groups. We tested for discrimination and bias to increase the fairness in our model. Despite not explicitly including demographic features in our model, we suspect that there is a strong correlation between different demographic features and economic indicators. We checked for fairness issues by calculating the adverse impact ratio (AIR) for a certain protected group versus its respective reference group. Some of the comparisons made included black people versus white people and men versus women. After calculating AIR, we remediated the mode to make sure AIR was above 0.8 for all protected groups. Further testing using different metrics can and should be conducted in the future.

### Privacy and Security 

A major concern is the sensitivity of the data. The data includes demographic information which is not directly included in our model and personal finance metrics. It is crucial that the we keep our data secure and we constantly monitor our model for security issues. If a membership inference attack is successfully conducted on our model, personal information will be leaked. We attempted to white-hat hack our model to test for security issues. This included a surrogate model inversion attack and an adversarial example attack.

