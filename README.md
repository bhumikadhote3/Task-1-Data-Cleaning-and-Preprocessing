# Task-1-Data-Cleaning-and-Preprocessing
step by step code
i used spyder for debuggeing.

import pandas as pd
import numpy as np
 
 #step 1 
 # load the data
df = pd.read_csv("netflix_titles.csv")
print(df.head())

print(df.shape)       # Rows and columns
print(df.columns)     # Column names
print(df.info())      # Data types and nulls
print(df.describe())  # Summary stats (numeric columns)

# Check missing values
print(df.isnull().sum())

# Drop rows where title is missing (essential field)
df = df.dropna(subset=['title'])

# Fill other missing values with placeholder or 'Unknown'
df['director'] = df['director'].fillna('Unknown')
df['cast'] = df['cast'].fillna('Unknown')
df['country'] = df['country'].fillna('Unknown')
df['rating'] = df['rating'].fillna('Not Rated')
df['date_added'] = df['date_added'].fillna('Unknown')

# Convert 'date_added' to datetime
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce')

# Check again
print(df.dtypes)

 # Trim whitespace and lowercase string fields
df['title'] = df['title'].str.strip().str.title()
df['director'] = df['director'].str.strip()
df['cast'] = df['cast'].str.strip()
df['country'] = df['country'].str.strip()
df['type'] = df['type'].str.strip()
df['year_added'] = df['date_added'].dt.year
print("Duplicates:", df.duplicated().sum())
df = df.drop_duplicates()
# Example: Count of shows by type
print(df['type'].value_counts())

# Example: Number of titles by year
print(df['year_added'].value_counts().sort_index())
df.to_csv('netflix_cleaned.csv', index=False)
print("Cleaned data saved.")
