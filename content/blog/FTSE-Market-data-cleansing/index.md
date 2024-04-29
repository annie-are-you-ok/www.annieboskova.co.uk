---
title: FTSE Market - Data Cleansing
summary: As part of the L4 Data Analysis Bootcamp with Cambridge Spark, I completed several assignments to test my knowlegde. This was a Data Cleansing Assignment.
date: 2024-04-29

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
# image: ''
#   caption: '' #'Image credit: [**Unsplash**](https://unsplash.com)'

authors:
  - admin

tags:
  - FTSA
  - Pandas
  - NumPy
  - Data Cleansing
---

<!-- ## FTSE Market - Data Cleansing  -->

The Financial Times Stock Exchange 100 (FTSE 100) Index data is a share index of the 100 companies listed on the London Stock Exchange with the highest market capitalisation. 

Before we were able to perform any data analysis, some data cleansing was required which was the focus of this project. What do I mean by data cleansing? That is the removal of missing or incorrect values, and superfluous data. Transforming some of the data involved changing data types which allowed me to utilise arithmetic operators to create new columns within the data set before analysis took place.


To begin data cleansing, I first had to import Pandas, this is because I was working with a dataframe. By calling the ```python .read_csv()``` function, I was able to import (or ‘read’) the CSV file into a dataframe and take a look at the dataset. Calling the `.head()` method on the dataframe allowed me to print out the first 5 rows so that I could get an idea of what the dataset looks like:

![screen reader text](Screenshot-1.png)





