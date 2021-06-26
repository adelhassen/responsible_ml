# Model Card

This repository is managed and updated by [Adel Hassen](https://github.com/adelhassen). For questions or concerns, please reach out at adelhassen17@gmail.com

## Intended Use

The model enables the user to identify whether the annual percentage rate (APR) charged for a mortgage is 1.5% or more above a survey-based estimate of similar mortgages.


### Users

- **Consumer Financial Protection Bureau (CFPB)**: A government agency with the purpose of ensuring financial companies treat their members fairly.
- **U.S. Financial Instituions**: They distribute mortgagaes so they can use this tool in-house to assess themselves for fairness prior to government evaluation.
- **Machine learning due diligence organizations**: Third-party assessment of financial institutions,

### Primary inteded uses

This model is designed to flag predatory loan behavior exhibited by banks. It focuses on home mortgages. It accepts certain features related to an applicant’s financial situation and information on the mortgage itself.


### Secondary uses

This model is designed to predict whether a mortgage is high-priced or not. The model has not been tested for any other purposes, but it has the potential to be extended to analyze lending methods other than mortgages.


## Training Data

### Source

This model was trained on data collected by the CFPB under the [Home Mortgage Disclosure Act (HMDA)](https://www.consumerfinance.gov/data-research/hmda/). HMDA mandates financial institutions to provide mortgage data for the general public’s scrutiny. The data can be used to help public officals make decisions or in our case, we investigate whether lending patterns of financial institutions are discriminatory or predatory. 

The training data can be accessed from this Github [repository](https://github.com/jphall663/GWU_rml/blob/master/assignments/data/hmda_train_preprocessed.zip).

### Training data information

The training data was randomly split into training and validation set using the Python NumPy. 
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

Interpret ML version 0.2.4 is used to develop the EBM model.

### Hyperparameters of model









