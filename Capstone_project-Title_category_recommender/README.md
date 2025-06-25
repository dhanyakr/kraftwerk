### Project Title
Tilte and cateogry recommender for a product from images
**Author**
Dhanya

#### Executive summary
In today’s fast-paced world, everyone wants to get things done with a single click. On social marketplaces, sellers typically upload pictures of the items they want to sell. They then have to manually fill in all the remaining information, which is very time-consuming. This step often leads to a high drop-off rate among users during the item listing process.

If we could automatically generate the title, description, category, color, brand, and style information as soon as the image is uploaded, it would significantly reduce the time required to list an item. This improvement could help prevent user drop-off, increase the number of listings on the platform, and ultimately lead to more orders and higher Gross Merchandise Volume (GMV).


#### Rationale
Why should anyone care about this question?

This would help the sellers to list their items within a minute with minimal clicks. This will motivate them to list more and they could accelerate the business on these marketplaces.

#### Research Question
What are you trying to answer?

On social marketplaces, sellers typically upload pictures of the items they want to sell. They then have to manually fill in all the remaining information, which is very time-consuming. This step often leads to a high drop-off rate among users during the item listing process. This is to address the biggest pain point of the seller in listing their items.


#### Data Sources
What data will you use to answer you question?

I am looking for fashion product image dataset which should have a collection of fashion product images along with detailed metadata like category, color, brand, and style descriptions etc.
I came across this data set on Kaggle - https://www.kaggle.com/datasets/paramaggarwal/fashion-product-images-dataset. I believe this would be suitable for my project.


#### Methodology
What methods are you using to answer the question?

Data Collection
Data Preprocessing
Data analysis
Data Clustering using the appropriate algorithm
Principal component analysis (PCA)
Data Modeling using K-Nearest Neighbors (KNN), Decision Trees, Logistic Regression, and Support Vector Machines (SVM) Classifiers. Also will be using GridSearchCV for hyper parameter optimization for each algorithm

#### Results
What did your research find?

To make more accurate recommentation, we need a dataset which includes images of a product with various angles. Also, the data should have more info about the brand and styling information of the product, so that we can predict those fields for the seller to make things easy for them. 

#### Next steps
What suggestions do you have for next steps?

To identify a larger dataset which has more info about the product such as brand, subcategory, style info etc. also images of a product from different angle. Wiith which we could recommend more accurate suggestions.


#### Outline of project

[Notebook](./generate_category_title_from_image.ipynb)


##### Contact and Further Information
