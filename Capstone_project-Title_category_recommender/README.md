### Project Title
Auto Generate Title and Category from Product Images

**Author**
Dhanya Kunnath Rajappan

#### Executive summary
In today’s fast-paced e-commerce and social marketplace environments, minimizing friction in product listing is essential. A major pain point for sellers is the time-consuming manual entry of product details after uploading an image. This often leads to significant drop-off before listing completion.

This project aims to streamline the listing process by automatically predicting key product metadata—specifically title and category—based on just the uploaded product image. Automating this task has the potential to reduce drop-off rates, increase the number of listings, and boost Gross Merchandise Volume (GMV), all while enhancing user experience.

#### Rationale
Why should anyone care about this question?

Can we use computer vision and machine learning to generate product titles and categories automatically from a single product image?

The goal is to assist sellers on social marketplaces by minimizing manual input when listing products. Automatically generating relevant metadata upon image upload will significantly reduce friction, increasing engagement and platform productivity.

#### Research Question
What are you trying to answer?

How accurately can we predict the title and category of a product based solely on an image, using machine learning and image-based feature extraction?

##### Model Objectives & Outcomes
Type of Learning: Supervised Learning

Task: Multi-class Classification

Outputs:
Category Prediction: A label such as "Jeans", "Dress", or "Shoes"
Title Prediction: A plausible title generated from visual features (currently approximated using classifiers)

#### Data Sources
What data will you use to answer you question?

I used the Fashion Product Images Dataset from Kaggle, which contains:
~44,000 product images
Metadata including:  Product title, Gender, Category and subcategory, Color, usage, and image links

This dataset is suitable for multi-label classification tasks in fashion e-commerce.

##### Data Preprocessing
Steps Taken:
Image Resizing: All images resized to a standard 100x100 format for efficient training
Label Cleaning: Removed noisy or inconsistent labels and filtered for categories with sufficient support

Train-Test Split:
80% Training, 20% Test

Encoding: Category labels were one-hot encoded for classification tasks
Missing or ambiguous entries were removed to ensure clean, reliable inputs to the model.

#### Methodology
The overall pipeline involves:

1. Data Preprocessing
Loaded and resized images

Cleaned metadata and encoded categories

2. Exploratory Data Analysis (EDA)
Analyzed class distribution

Visualized sample categories and image metadata

3. Feature Extraction
Flattened image matrices for traditional classifiers

Used color and shape-based pixel intensities

4. Modeling
Evaluated multiple classifiers: Logistic Regression, Random Forest, Support Vector Machines (SVM).
Applied GridSearchCV for hyperparameter tuning

5. Deep Learning for Image Classification
Integrated ResNet50 pretrained CNN for richer feature extraction (for category prediction)

##### Model Evaluation
Metrics Used:
Accuracy, F1 Score, Classification Report

Classifier Results:
Random Forest and SVM outperformed Logistic Regression for category classification.

ResNet50 model achieved notably higher accuracy on category prediction using transfer learning.

Model	Accuracy (Top Categories)
Logistic Regression	~52%
SVM	~60%
Random Forest	~63%
ResNet50 (fine-tuned)	~72%+

#### Results
What did your research find?

Traditional models performed moderately well, but struggled with image complexity.
Deep learning with pretrained CNN (ResNet50) significantly improved classification accuracy.
Title prediction remains challenging due to lack of semantic textual data; future work will integrate NLP models (like BERT or BLIP) for text generation.
More image variety (angles, background styles) and richer metadata (e.g., brand, style) would enhance prediction accuracy.

#### Next steps
What suggestions do you have for next steps?

Use richer datasets: Look for image datasets that include multi-angle product views, brand, and style descriptions.
Implement image captioning: Integrate models like BLIP, CLIP, or ViT-BERT for text generation from image features.
Enhance feature extraction: Use EfficientNetB0 or ViT to replace ResNet50 for even better accuracy and speed.
Develop end-to-end pipeline: Build a working UI that accepts an image and auto-generates metadata for product listing.

#### Outline of project

[Notebook](./generate_category_title_from_image.ipynb)


##### Contact and Further Information
Author: Dhanya Kunnath Rajappan
Email: mailtodkr@gmail.com
GitHub: https://github.com/dhanyakr

