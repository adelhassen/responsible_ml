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

## Training data columns

The following features were included:








