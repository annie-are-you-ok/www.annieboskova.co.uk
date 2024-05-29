---
title: Exploring Reading Habits and Trends with Seaborn
summary: Personal project, looking at books and reading patterns. #shows on the homepage
date: 2024-05-29

# Place an image named `featured.jpg/png/gif` in this page's folder and customize its options here.
image:
  #placement: 1 = Full column width, 2 = Out-set, 3 = Screen-width 
  placement: 1
  caption: 'Image credit: [**giphy**](https://www.giphy.com/)'
  #focal point = Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, or BottomRight
  focal_point: 'Smart'
  preview_only: false

authors:
  - admin

tags:
  - Pandas
  - Matplotlib
  - Seaborn
  - Data Visualisation
  - Personal Project
  - Books
  - Reading
---
As an avid reader with an interest in social trends and data visualisation, for this project I wanted to explore reading habits and draw comparisons to my own habits.

I wanted to explore some datasets that would help me answer the following questions:
1. Do females read more than males? 
2. Do men (18 yrs+) read more non-fiction than women? **top_goodreads_df**
3. Do more people listen to audiobooks than read physical books? 

#### General trends;
- Most popular genre **top_goodreads_df**
- Trends by gender, age, education, income (reading_habits_df) **reading_habits_df** 

### Data Collecting
I used the following datasets from kaggle.com:
1. **Reading habits**: https://www.kaggle.com/datasets/vipulgote4/reading-habit-dataset/data 

This dataset looks at the reading habits and demographics of 2832 people, accross 14 columns. It includes age, gender, and number of books read in a 12 moth period (the data is about 4 years old).

![screen reader text](reading_habits_df_head.png "reading_habits_df.head(2)")
![screen reader text](reading_habits_df_shape.png "reading_habits_df.shape")

2. **Top Goodreads Books (1980-2023)**: https://www.kaggle.com/datasets/cristaliss/ultimate-book-collection-top-100-books-up-to-2023 

This dataset looks at the top 100 books for each year between 1980 and 2023, according to Goodreads, and includes genres, book length, average rating, and the number of people interested in reading each book. It is made up of 4400 indexes and 20 columns.

![screen reader text](top_goodreads_df_head1.png)
![screen reader text](top_goodreads_df_head2.png "top_goodreads_df.head(2)")
![screen reader text](top_goodreads_df_shape.png "top_goodreads_df.shape")

3. **Personal dataset**

This dataset is created from my own personal library spreadsheet which documents my physical and electronic book collection as well as some details about each book (e.g. whether I have read it and what genre it is). It is made up of 278 indexes and 12 columns.

![screen reader text](df_sample(5).png "df.sample(5)")
![screen reader text](df_shape.png "df.shape-")


### Importing Libraries
After I found some datasets that I liked, I import some python libraries to help with my analysis and visualisations; **Pandas**, **Matplotlib**, and **Seaborn**:
```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import seaborn as sns
```
I was then able to use the **.read_csv()** method to read my datasets into dataframes. After this, I explored the datasets, checked if anything needed to be amended or removed, looked for NaNs, and got to cleaning.  

### 1. **Reading habits**
#### Data Cleaning

With this dataset I was happy with the data types of the columns, but I decided to drop the last 3 columns as I wasn't interested in them. I did this by first creating a copy of my dataframe to allow me to track my changes, created a list of the columns I wanted to drop (I called this list *cols_to_drop*); by creating a list of the columns I wanted to alter, I was able to pass the list name through a method rather than naming each column, which in this case would have been a long and messy piece of code due to the column names being so long.
I then used the **.drop()** method, passing my list of columns as the parameter.
```python
cols_to_drop = ['Last book you read, you…',	'Do you happen to read any daily news or newspapers?',	'Do you happen to read any magazines or journals?']

clean_df = reading_habits_df.copy()
clean_df = clean_df.drop(cols_to_drop, axis=1)
```
I debated keeping the 'Last book you read, you…' column, which outputs the following values:
![screen reader text](screenshot-1.png "screenshot-1") 

But after utilising the **.unique()** (to return all the unique values, including NaNs) and **.value_counts()** (to return the count of all unique values, excluding any NaNs) methods, it didn't make sense to include this in my analysis as it wasn't clear what some of the values represented ('8' and '9') and changing them into 'unknown' wouldn't contribute any value to the data. I also decided that I was more interested in how many books people read in a year and whether they were physical books, ebooks, or audiobooks - though I wish the data included how many books where of each format.

Next I used the **.fillna()** method to changed some NaNs into "Don’t know"; I chose this as the columns already had "Don’t know" as a value. Again I created a list of columns (this time of the ones with NaNs I wanted to convert to "Don’t know") and called it *nan_cols*. 
```python
nan_cols = ['Education', 'Read any printed books during last 12months?', 'Read any audiobooks during last 12months?', 'Read any e-books during last 12months?']
clean_df[nan_cols] = clean_df[nan_cols].fillna('Don’t know')
```

During my initial exploration, I also found that one of the columns had a typo in some of the values. The **.value_counts()** method revealed that 212 values in the *'Incomes'* column were equivalent to *9$100,000 to under $150,000* which clearly was a typo. 

![screen reader text](screenshot-2.png "screenshot-2") 

To combat this, I called the **.replace()** method to allow me to fix this typo by removing the '9' at the start:
```python
reading_habits_clean = clean_df.copy() #tracking changes
reading_habits_clean['Incomes'] = reading_habits_clean['Incomes'].replace('9$100,000 to under $150,000', '$100,000 to under $150,000')
```
I also applied the following function onto the column to fix the formatting as the values were being squished which made them difficult to read; I did this by removing the '$' sign.

```python
def fix_format(x):
  if '$' in x:
    return x.replace('$','')
  else:
    return x

reading_habits_clean['Incomes'] = reading_habits_clean['Incomes'].apply(fix_format)
```


Finally, I needed to rename one of the columns as it was misspelt ('Employement'). I did this with the following piece of code.
```python
reading_habits_clean = reading_habits_clean.rename(columns={'Employement':'Employment'})
```

My resulting clean dataframe looked like this:
![screen reader text](screenshot-3.png "screenshot-3") 


### Age Distribution
I decided to create a boxplot next to allow me to see the central tendency, dispersion, and outliers within age. This showed that the majority of the sample were aged between 30-60 years, with some outliers on both ends. 

![screen reader text](boxplot-1.png "age boxplot") 

By calling the **.describe()** method, I was able to see that the youngest person was 16 years old and the oldest was 93 years old, with the average age being 47.3 years old.  

A histogram showed that a large amount of the sample were in their 20s, though the majority of people aged between 50-60 years old. And though it looks like there were many twenty-something year olds, 20-29 year olds only made up 13% of the group, whilst 50-60 year olds made up 21.6%.

![screen reader text](histogram-1.png "age distribution") 


### Who Reads More?

<!-- 1. Do females read more than males? -->

I first used a histogram to look at the distribution of the number of books read over a 12 month period. 
![screen reader text](histogram-2.png "histogram") 

From this histogram I can see that the data is skewed to the left, i.e. most people read around 0-15 books (calling the **.describe()** method showed that the average books read were 16.7), though a large reason for this might be that the highest count seems to be for 0-2 books (i.e. around 700 people said they read 0 to 2 books).

By running the following line of code I was able to work out that 390 people said they didn't read any books (i.e. 13.8% of the people in the dataset).
```python
none_read = len(reading_habits_clean[reading_habits_clean['How many books did you read during last 12months?']==0])
none_read
```

The histogram also showed several outliers, for example, around 100 people said they read 90-95 books. In fact, 141 people said they read 90+ books:
```python
high_readers = len(reading_habits_clean[reading_habits_clean['How many books did you read during last 12months?']>=90])
high_readers
```

I then used a countplot which supported the theory that females read more than males - interestingly, it also showed that nearly twice as many males read 0-2 books in the 12 months leading up to the data collection, than females.  
![screen reader text](countplot-1.png "countplot") 


And lastly, the pie chart below shows that, of all the books read (according to the 'How many books did you read during last 12months?' column), 61.7% of them were read by females. 

![screen reader text](piechart-1.png "piechart") 

<!-- ### Reading Trends


books read x age
books read x education
books read x income
books read x marital status





### 2. **Top Goodreads Books (1980-2023)**:





color='orchid'
color='lightblue'
color='mediumpurple'











### Aftermath - What did I learn? -->