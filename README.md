# Beer Preference Prediction System

Beer outlets need to strategically position themselves and maximize market share in the booming global beer market. Understanding customer preferences is crucial. This project leverages advanced data analytics and Natural Language Processing (NLP) techniques to predict beer preferences based on review text. The goal is to provide an automatic rating system and drive insights that can help beer outlets refine their inventory selection and improve customer satisfaction.


  ![Local Image](beer.jpg)



## 1. Data

The dataset used in this project comprises detailed customer reviews and ratings from a variety of beers.  
**Source:** StrataScratch - Beer Data Analysis

## 2. Method

There are several key methods and models used in this project:

1. **Text preprocessing:** The review text was cleaned and preprocessed using techniques such as tokenization, stop words removal, and lemmatization. These steps were crucial to reduce noise and improve the quality of the data for model training.
2. **Sentiment Analysis:** A model predicted the sentiment of each review, aiding in understanding customer opinions.

## 3. Data Cleaning

### Data Cleaning Report

In this project, missing values in `review_text` were imputed with 'Unknown' to maintain data integrity and ensure a consistent dataset. Outliers in the ABV values were replaced with the median ABV to preserve the overall distribution and prevent distortion from extreme values.

- **Problem 1:** Inconsistent data entries were common in user-generated content, particularly with beer styles and names.
  - **Solution:** Standardization and normalization techniques were applied to ensure consistency across the dataset.
- **Problem 2:** The review text contained a significant amount of noise, including irrelevant words and variations in word forms.
  - **Solution:** Advanced NLP techniques were applied to clean and preprocess the text, improving its suitability for modeling.

## 4. Exploratory Data Analysis (EDA)

EDA revealed key trends and patterns in the data. For instance, the distribution of review scores highlighted that most customers rate beers positively, with few low scores. Model training considered this typical skew by using weighted parameters.

This analysis also highlighted key themes in reviews, such as beer characteristics like "head hop" and "taste malt." Bigrams guided feature creation for models and enhanced sentiment analysis performance. By correlating bigrams with sentiments, more accurate models were built.

## 5. Algorithms & Machine Learning

### Sentiment Analysis with TextBlob

In this section, sentiment analysis was used on beer reviews to classify them as positive or negative based on their textual content. The TextBlob library, a simple yet accurate tool for processing textual data, was utilized.

- **Sentiment Analysis Using TextBlob:** TextBlob analyzes each review text to determine its sentiment polarity, ranging from 0 (negative sentiment) to 1 (positive sentiment). Based on the polarity score, reviews are classified as 'positive' or 'negative.' The project primarily uses a Support Vector Machine (SVM) for predicting beer preferences, chosen for its computational efficiency in binary classification tasks.

## 6. Model Selection

After evaluating various models, the TF-IDF and LinearSVC combination, using the SVM framework, proved effective for handling high-dimensional data. TF-IDF transforms text into a high-dimensional vector space, and SVM is ideal for this environment due to its efficiency and accuracy.

### Why Use This Pipeline?

#### TF-IDF Vectorization:
The `TfidfVectorizer` transforms text data into a numerical matrix where each value reflects the importance of a word within a document relative to the entire corpus. This helps the model prioritize words indicative of sentiment. Parameters like `max_features=10000`, `min_df=5`, and `max_df=0.7` further refine the features, focusing on those most relevant to sentiment classification.

#### Linear SVM Classifier:
The SVM algorithm aims to find a plane that has the maximum margin, i.e., the maximum distance between data points of both classes. The `LinearSVC` classifier is well-suited for binary classification tasks, such as distinguishing between positive and negative reviews. Including `class_weight=class_weight_dict` in the pipeline is crucial for handling class imbalance.

### Benefits of This Approach

- **Accurate Sentiment Classification:** The combination of TF-IDF and Linear SVM allows the model to concentrate on the most critical aspects for determining sentiment, resulting in accurate predictions of whether a review is positive or negative.
- **Handling Class Imbalance:** Applying a `class_weight` dictionary helps the model adjust to allocate more significance to the underrepresented class, enhancing its performance on negative reviews.

## 7. Predictions

**Final Predictions Notebook:**  
Users can input a review text, and the model will predict the likely overall rating and beer style, offering valuable insights into customer preferences.

## 8. Future Improvements

- **Hybrid Models:** Explore hybrid models that combine content-based and collaborative filtering techniques to enhance prediction accuracy.
- **Real-time Deployment:** Deploy the model in a real-time application to provide instant predictions based on new reviews.
- **Expanded Feature Set:** Incorporate additional features such as user demographics or purchase history to improve prediction accuracy.
