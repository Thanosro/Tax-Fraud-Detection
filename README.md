# Tax-Fraud-Detection
## Tax fraud detection in a New York City (NYC) real estate dataset.
<hr />

### Background
In this project, we analyze New York City’s (NYC’s) real estate data to specifically identify property tax fraud. The main indicators of property tax fraud were property tax assessments that were too high or too low (outlier values). The dataset consists of records of more than a million property valuations and assessments on NYC real estate, and was intended for calculating property taxes and granting eligible properties exemptions and/or abatements during NYC’s 2010/2011 fiscal year. The dataset contains 32 features for each record, such as zip and borough of the property, size of each dimension of the building and the lot (heigh, width, lenght), full market value of the property, etc. Our goal is to use statistical models in an unsupervised manner to detect outlier records (potentially fraudulent). 
### Approach
#### Feature Engineering
Initially, we filled any missing values of the dataset. For that we grouped records using a subset of the features and we filled the missing record with the median or average of that feature of the grouped records. We repeated the process, using different groups of features each time untill all the missing values were filled. Next, we created new features based on the initial ones in the dataset, to help us detect fraud patterns. Our goal was to create features to evaluate property value in relation to building and lot size. Hence, we calculated the area and volume of the lot and building of each record and we used them to normalize the assesed and total values of the building and the land of each record. Following, we calculated grouped and total averages of these new features which were grouped by zipcode and borough (among others). This would divide the features by location and type of building which will help assess whether a property assessment value is too high or too low. 
### Dimensionality Reduction
#### PCA
#### Autoencoder

