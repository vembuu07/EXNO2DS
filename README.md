# EXNO2DS
# AIM:
      To perform Exploratory Data Analysis on the given data set.
      
# EXPLANATION:
  The primary aim with exploratory analysis is to examine the data for distribution, outliers and anomalies to direct specific testing of your hypothesis.
  
# ALGORITHM:
STEP 1: Import the required packages to perform Data Cleansing,Removing Outliers and Exploratory Data Analysis.

STEP 2: Replace the null value using any one of the method from mode,median and mean based on the dataset available.

STEP 3: Use boxplot method to analyze the outliers of the given dataset.

STEP 4: Remove the outliers using Inter Quantile Range method.

STEP 5: Use Countplot method to analyze in a graphical method for categorical data.

STEP 6: Use displot method to represent the univariate distribution of data.

STEP 7: Use cross tabulation method to quantitatively analyze the relationship between multiple variables.

STEP 8: Use heatmap method of representation to show relationships between two variables, one plotted on each axis.

## CODING AND OUTPUT
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("titanic_dataset.csv")
print("Original Dataset:")
display(df.head())

print("Dataset Shape:", df.shape)
df.info()

print("\nStatistical Description:")
display(df.describe(include="all"))

print("\nMissing Values:")
print(df.isnull().sum())

df["Age"] = df["Age"].fillna(df["Age"].median())
df["Cabin"] = df["Cabin"].fillna("Unknown")
df["Embarked"] = df["Embarked"].fillna(df["Embarked"].mode()[0])

print("\nMissing Values After Filling:")
print(df.isnull().sum())

plt.figure(figsize=(12, 6))
sns.boxplot(data=df[["Age", "Fare", "SibSp", "Parch"]])
plt.title("Boxplot for Outlier Analysis")
plt.show()

num_cols = ["Age", "Fare", "SibSp", "Parch"]

Q1 = df[num_cols].quantile(0.25)
Q3 = df[num_cols].quantile(0.75)

IQR = Q3 - Q1
lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR

df_clean = df[~((df[num_cols] < lower) |
                (df[num_cols] > upper)).any(axis=1)]

print("Original Dataset:", df.shape)
print("Dataset After Outlier Removal:", df_clean.shape)
display(df_clean.head())

sns.countplot(data=df_clean, x="Survived")
plt.title("Passenger Survival Count")
plt.xlabel("Survived (0 = No, 1 = Yes)")
plt.ylabel("Number of Passengers")
plt.show()

sns.displot(data=df_clean, x="Age", kde=True)
plt.title("Distribution of Passenger Age")
plt.xlabel("Age")
plt.ylabel("Count")
plt.show()

cross_tab = pd.crosstab(df_clean["Sex"], df_clean["Survived"])
print("Cross Tabulation of Gender and Survival:")
display(cross_tab)

plt.figure(figsize=(10, 6))
correlation = df_clean.select_dtypes(include="number").corr()
sns.heatmap(correlation, annot=True, cmap="coolwarm")
plt.title("Correlation Heatmap of Titanic Dataset")
plt.show()

df_clean.to_csv("Titanic_Cleaned.csv", index=False)
print("Cleaned Titanic dataset saved successfully!")
```

## 1:
<img width="650" height="457" alt="image" src="https://github.com/user-attachments/assets/526b2c4f-82b0-45eb-a501-a9fc7cbf04d7" />

## 2:
<img width="640" height="208" alt="image" src="https://github.com/user-attachments/assets/46b539f6-a0c6-47ae-8e1d-a0f1036373a3" />

## 3:
<img width="681" height="388" alt="image" src="https://github.com/user-attachments/assets/5683f7b1-e4e1-4430-82b2-57322e5e64a9" />

## 4:
<img width="617" height="175" alt="image" src="https://github.com/user-attachments/assets/8bcfc4c2-7a40-4faf-8e55-303b771f6d6f" />

## 5:
<img width="642" height="381" alt="image" src="https://github.com/user-attachments/assets/5bf0ad2c-db0a-423c-8b31-b0005c53d6f0" />

## 6:
<img width="662" height="410" alt="image" src="https://github.com/user-attachments/assets/9042302b-3789-48d9-978b-c6f0cc0cb620" />

## 7:
<img width="445" height="157" alt="image" src="https://github.com/user-attachments/assets/f4436d24-62da-4a22-8284-a76a39d84892" />

## 8:
<img width="680" height="498" alt="image" src="https://github.com/user-attachments/assets/f90010c1-38fc-4b87-ad53-252b4f70208d" />

## Summary:
I understood that Exploratory Data Analysis (EDA) is used to examine and understand a dataset before performing further analysis.
1. Load and inspect the dataset
- Methods used: read_csv(), head(), shape, info(), describe(), isnull().sum().
- Why: To understand the dataset's structure, number of rows and columns, data types, statistical details and missing values.
2. Handle missing values
- Methods used:
  - Median: Filled missing Age values using the median because it is less affected by extreme values.
  - Mode: Filled missing Embarked values using the most frequently occurring category.
  - Suitable replacement: Filled missing Cabin values with "Unknown" because the cabin information was unavailable.
- Why: To avoid missing data affecting further analysis.
3. Identify potential outliers using boxplots
- Method used: Boxplot (sns.boxplot()).
- Why: To visually identify values that are unusually high or low compared with the remaining data. We checked Age, Fare, SibSp and Parch columns.
4. Remove outliers using the IQR method
- Method used: Interquartile Range (IQR).
- Why: To calculate the lower and upper limits and remove rows containing values outside these limits.
\[
IQR = Q3 - Q1
\]

\[
Lower = Q1 - 1.5(IQR)
\]

\[
Upper = Q3 + 1.5(IQR)
\]

5. Visualize categorical and numerical data
- Countplot: Used to visualize categorical data, such as the number of passengers who survived and did not survive.
- Displot: Used to visualize the distribution of numerical data, such as passenger ages, and understand how frequently different age values occur.
- Why: To understand the distribution and patterns in the dataset through graphs.
6. Cross-tabulation
- Method used: pd.crosstab().
- Why: To compare two categorical variables and understand their relationship. We compared Gender and Survival to see how many male and female passengers survived.
7. Correlation heatmap
- Method used: sns.heatmap() with .corr().
- Why: To understand the correlation between numerical variables. It helps identify positive, negative and weak relationships using values from -1 to +1.
8. Save the cleaned dataset
- Method used: to_csv().
- Why: To save the cleaned dataset as Titanic_Cleaned.csv so that it can be used for future analysis without repeating the cleaning process.


# RESULT
Thus, the performance of EDA on the given data set is Verified successfully along with the Visualization .
