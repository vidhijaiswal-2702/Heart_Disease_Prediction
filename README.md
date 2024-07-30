Sure! Here's the updated README file with the requested changes:

---

# Heart Disease Prediction Model

This project aims to predict the presence of heart disease in a patient using a logistic regression model. The model is built using Python and various machine learning libraries.

## Table of Contents
- [Introduction](#introduction)
- [Dataset](#dataset)
- [Installation](#installation)
- [Usage](#usage)
- [Model](#model)
- [Web Application](#web-application)
- [Results](#results)

## Introduction
Heart disease is one of the leading causes of death worldwide. Early prediction can help in timely treatment and can potentially save lives. This project uses a logistic regression model to predict the likelihood of a patient having heart disease based on various medical attributes.

## Dataset
The dataset used in this project is the Cleveland Heart Disease dataset available from the UCI Machine Learning Repository. It contains 303 records with 14 attributes, including age, sex, chest pain type, resting blood pressure, serum cholesterol, fasting blood sugar, resting electrocardiographic results, maximum heart rate achieved, exercise-induced angina, ST depression induced by exercise, and others.

## Installation
To run this project, you need to have Python installed along with the following libraries:
- Pandas
- NumPy
- Scikit-learn
- Streamlit
- Joblib

You can install the required libraries using:
```
pip install pandas numpy scikit-learn streamlit joblib
```

## Usage
1. Clone this repository:
```
git clone https://github.com/yourusername/heart-disease-prediction.git
```
2. Navigate to the project directory:
```
cd heart-disease-prediction
```
3. Run the Jupyter notebook to train and save the model:
```
jupyter notebook Heart_Disease_Prediction.ipynb
```
4. Start the Streamlit web application:
```
streamlit run app.py
```
5. Open your web browser and navigate to the URL provided by Streamlit to use the application.

## Model
The logistic regression model is used for prediction. The model is trained using the `Heart_Disease_Prediction.ipynb` notebook. 

## Web Application
A simple web application is built using Streamlit to interact with the model. Users can input medical attributes and get a prediction of the likelihood of having heart disease.

## Results
The model achieves an accuracy of around 63.2%. The results are displayed on the web application.
