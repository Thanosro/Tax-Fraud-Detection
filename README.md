# Tax-Fraud-Detection
## Tax fraud detection in a New York City (NYC) real estate dataset.
<hr />

### Background
In this project, we analyze New York City’s (NYC’s) real estate data to specifically identify property tax fraud. The main indicators of property tax fraud were property tax assessments that were too high or too low (outlier values). The dataset consists of records of more than a million property valuations and assessments on NYC real estate, and was intended for calculating property taxes and granting eligible properties exemptions and/or abatements during NYC’s 2010/2011 fiscal year. The dataset contains 32 features for each record, such as zip and borough of the property, size of each dimension of the building and the lot (heigh, width, lenght), full market value of the property, etc. Our goal is to use statistical models in an unsupervised manner to detect outlier records (potentially fraudulent). 
### Approach
#### Feature Engineering
Initially, we filled any missing values of the dataset. For that we grouped records using a subset of the features and we filled the missing record with the median or average of that feature of the grouped records. We repeated the process, using different groups of features each time untill all the missing values were filled. Next, we created new features based on the initial ones in the dataset, to help us detect fraud patterns. Our goal was to create features to evaluate property value in relation to building and lot size. Hence, we calculated the area and volume of the lot and building of each record and we used them to normalize the assesed and total values of the building and the land of each record. Following, we calculated grouped and total averages of these new features which were grouped by zipcode and borough (among others). This would divide the features by location and type of building which will help assess whether a property assessment value is too high or too low. After this step, our dataset contained 45 features. 
### Dimensionality Reduction
#### Principal Component Analysis (PCA)
Initially, we standardized the dataset and performed PCA. PCA projects the dataset to the axes (principal components) of those features that have maximum variancve. Additionally, the features of the transformed dataset are ordered according to variance and therefore those features with small variance are considered to have a high linear correlation with other features. Hence, dimensionality reduction is performed by choosing the first 𝑘 features of the resulting dataset that contain a desirable percentage of the total variance. In the following figure, we can see that ~97% of the total variance of the dataset is explained by 8 axes of the features. 

<img src="https://user-images.githubusercontent.com/39418469/142396988-aa3c2263-9217-44b3-a661-563f6d9fe723.png" width="450" height="300"> 
In the following scree plot, we present the variance of each principal component in decreasing order. It is pretty obvious that any further principal components have variance close to 0. 
<img src="https://user-images.githubusercontent.com/39418469/142398202-c4fc59eb-d2c9-4717-be21-c244b16c45bb.png" width="450" height="300">

#### Autoencoder
Next, we used the reduced dimension dataset from the PCA as input to an autoencoder and we standardized it with zero mean and one standard deviation. The autoencoder had 3 layers and 5 nodes in the hidden layer, which performed the dimensionality reduction. The autoencoder reduces the dimenension of the dataset in the hidden layer and then reconstructs it in the output layer, thus offering noise reduction and outlier detection in the dataset. In the following figure we can observe the loss of the autoencoder for different number of nodes in the hidden layer. 

<img src="https://user-images.githubusercontent.com/39418469/142399855-f697f69e-090b-4148-b671-c28530fcb314.png" width="450" height="300">

### Evaluation of the Results
In this section we assign fraud scores at each record based on the outputs of the PCA and autoencoder to indicate a measure of fraud for each data entry. As our 1st score, we use the distance (2-norm) of each record after the PCA step from the origin. Any outliers in the original dataset would indicate high variance and PCA maximizes variance by projecting in the principal components. Hence, an outlier value in a dimension would be included in the dimensionality reduction performed by PCA and could be easily detected by a distance metric in the standardized dataset. If ![image](https://user-images.githubusercontent.com/39418469/142498168-faa167ae-73fb-4ec9-af72-74870cd83907.png)
is the output dataset of PCA and ![image](https://user-images.githubusercontent.com/39418469/142552945-c16b4f40-404b-4ff6-a7af-796f57968bc6.png) is the first score, we have ![image](https://user-images.githubusercontent.com/39418469/142552995-69b2de03-9cd3-4d08-b93e-501f2dd8cd55.png).

Our second score ![image](https://user-images.githubusercontent.com/39418469/142553050-191a3ebe-4566-40b6-824a-55f03cdba3cf.png)
 is derived from the output of the autoencoder and is the difference of the output ![image](https://user-images.githubusercontent.com/39418469/142553805-d021653b-008f-4f35-9817-379bd5995f37.png) to the input ![image](https://user-images.githubusercontent.com/39418469/142498183-205b3f69-94d0-40d9-ad46-499e9e98b4fb.png) of the autoencoder: ![image](https://user-images.githubusercontent.com/39418469/142553851-8964b0dc-6731-4001-8f60-239a1698d7dc.png). Finally, we combine the two scores either by either calculating their harmonic mean or their average to have a final outlier score. The histogram final score is presented below:

<img src="https://user-images.githubusercontent.com/39418469/142493148-a4a504f3-2bd3-4bb7-8466-f743bf57c033.png" width="450" height="300">

To have a better understanding of the outlier values and their scores, we plot the top 150 final scores in decreasing order. We notice from the plot below that the final score decreases significanlty after the top 10 records, Hence, we opted to examine 10 records for potential fraud activity. 

<img src="https://user-images.githubusercontent.com/39418469/142554387-98a625c3-bd50-4067-9982-0b11966fdbaf.png" width="450" height="300">

Finally, for a better illustration of the outlier values, we present the scatter plot of normalized Score 1 and Score 2 and point out these 10 values with red:

<img src="https://user-images.githubusercontent.com/39418469/142554549-f40cefdf-ffc1-4f90-9f50-201a1c404a6a.png" width="450" height="300">

### Interpretation of the Results
In the final step of this project, we analyzed the top ten properties that had the highest fraud scores. We looked at the overall data points for each property to highlight why the property resulted in a high fraud score. Some variables had strangely low or strangely high values - for example, for property or lot size. These could represent a fraudulent record, clerical error, etc. Further investigation and discussion will be needed will real estate experts if these records are indeed fraudulent or not. Also, we looked at the normalized dataset for the top ten records with the highest fraud scores to see which variables had unusual vales.

After examination of those records, we noticed that most were either properties owned by goverment entities or properties that had significanlty low values for propety size (i.e. 1 foot for building depth). Those extreme low values yielded high property values after the standardization step, thus part of next steps would be to determine if those low values were due to administrative errors, due to our potential errors while filling missing values, or are in fact fraudulent values.  
