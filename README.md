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

```
version: '3'

services:
  postgres:
    image: postgres:13
    environment:
      - POSTGRES_USER=airflow
      - POSTGRES_PASSWORD=airflow
      - POSTGRES_DB=airflow
    volumes:
      - postgres-db-volume:/var/lib/postgresql/data

  airflow-init:
    image: apache/airflow:2.10.2
    depends_on:
      - postgres
    environment:
      - AIRFLOW__CORE__EXECUTOR=LocalExecutor
      - AIRFLOW__DATABASE__SQL_ALCHEMY_CONN=postgresql+psycopg2://airflow:airflow@postgres/airflow
    volumes:
      - ./dags:/opt/airflow/dags
    entrypoint: >
      /bin/bash -c "airflow db init && airflow users create --username admin --password admin --firstname Air --lastname Flow --role Admin --email admin@example.com"

  webserver:
    image: apache/airflow:2.10.2
    depends_on:
      - postgres
      - airflow-init
    environment:
      - AIRFLOW__CORE__EXECUTOR=LocalExecutor
      - AIRFLOW__DATABASE__SQL_ALCHEMY_CONN=postgresql+psycopg2://airflow:airflow@postgres/airflow
    volumes:
      - ./dags:/opt/airflow/dags
    ports:
      - "8080:8080"
    command: webserver

  scheduler:
    image: apache/airflow:2.10.2
    depends_on:
      - webserver
      - postgres
    environment:
      - AIRFLOW__CORE__EXECUTOR=LocalExecutor
      - AIRFLOW__DATABASE__SQL_ALCHEMY_CONN=postgresql+psycopg2://airflow:airflow@postgres/airflow
    volumes:
      - ./dags:/opt/airflow/dags
    command: scheduler

volumes:
  postgres-db-volume:

```

```
from airflow import DAG
from airflow.operators.bash import BashOperator
from datetime import datetime, timedelta

# Default args for all tasks
default_args = {
    'owner': 'vidhi',
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
}

with DAG(
    dag_id='daily_spark_ingestion',
    default_args=default_args,
    description='Run daily Spark ingestion job (Java JAR)',
    start_date=datetime(2025, 9, 1),
    schedule_interval='@daily',   # run daily
    catchup=False,
) as dag:

    ingest_task = BashOperator(
        task_id='run_spark_ingestion',
        bash_command="""
        spark-submit \
          --class org.example.Injestion \
          --master local[*] \
          /opt/airflow/dags/airline-delay-ingestion.jar \
          /opt/airflow/dags/Airline_Delay_Cause.csv \
          /opt/airflow/dags/output/ingested_data
        """
    )
```
```
FROM apache/airflow:2.10.2

USER root
RUN apt-get update && apt-get install -y openjdk-11-jdk curl wget tar bash procps \
    && curl -O https://archive.apache.org/dist/spark/spark-3.4.2/spark-3.4.2-bin-hadoop3.tgz \
    && tar -xvf spark-3.4.2-bin-hadoop3.tgz -C /opt/ \
    && mv /opt/spark-3.4.2-bin-hadoop3 /opt/spark \
    && rm spark-3.4.2-bin-hadoop3.tgz

ENV SPARK_HOME=/opt/spark
ENV PATH=$PATH:$SPARK_HOME/bin

USER airflow

