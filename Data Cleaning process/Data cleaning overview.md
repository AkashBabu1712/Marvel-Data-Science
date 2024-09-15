### Data Cleaning Course Introduction

Data cleaning is a crucial step in the data preprocessing pipeline. It involves identifying and correcting errors, inconsistencies, and missing values in a dataset to improve its quality and reliability. In this course introduction, we'll explore various aspects of data cleaning and demonstrate techniques to effectively clean and preprocess data.

---

### What is Data Cleaning & How to use it?

Data cleaning is the process of identifying and correcting errors, inconsistencies, and missing values in a dataset to improve its quality and reliability. It ensures that the data is accurate, complete, and suitable for analysis. Data cleaning involves tasks such as removing duplicate records, handling missing values, correcting errors, and standardizing data formats. It is essential for ensuring the integrity of data analysis and modeling results.

Example:
Suppose you have a dataset containing information about customer transactions. Data cleaning would involve tasks such as removing duplicate entries, correcting misspelled customer names, and filling in missing values in the transaction amount column.

---

### What is a missing value & How to find it?

A missing value is a placeholder that indicates the absence of data in a particular field or column within a dataset. Missing values can occur due to various reasons, such as data entry errors, equipment malfunction, or intentional omission. It is essential to identify missing values in a dataset accurately to handle them appropriately during data cleaning and preprocessing.

Example:
In a dataset containing information about students' exam scores, a missing value in the "mathematics_score" column could indicate that the score for a particular student is not available.

To find missing values in a dataset, you can use various techniques such as:

1. **Visual inspection**: Manually scanning the dataset for blank cells or placeholders indicating missing values.
2. **Summary statistics**: Calculating summary statistics such as count, mean, median, etc., to identify columns with fewer non-missing values.
3. **Data visualization**: Plotting histograms or heatmaps to visualize the distribution of missing values across different variables.

---

### How to Handling Missing Values (Dropping)

Handling missing values involves deciding how to deal with them effectively to prevent them from adversely affecting the analysis or modeling process. One approach to handling missing values is to simply drop the rows or columns containing missing values from the dataset. While this approach is straightforward, it may result in loss of valuable information, especially if a large proportion of the data is missing.

Example:
If a dataset contains a column representing the age of individuals, and a significant number of entries in this column are missing, you might choose to drop those rows from the dataset.

```python
import pandas as pd

# Drop rows with missing values
cleaned_data = original_data.dropna()
```

---

### How to Handling Missing Values (Imputing Category Data)

Another approach to handling missing values is to impute them with appropriate values. For categorical data, imputation involves replacing missing values with the mode (most frequent value) of the respective column. This approach helps retain the information present in the dataset while dealing with missing values effectively.

Example:
In a dataset containing information about car models and their colors, if some entries in the "color" column are missing, you might choose to impute those missing values with the mode of the "color" column.

```python
import pandas as pd

# Impute missing values with mode
mode_color = original_data['color'].mode()[0]
original_data['color'].fillna(mode_color, inplace=True)
```

---

### How to Handling Missing Values (Scikit-Learn)

Scikit-learn provides various techniques for handling missing values, including imputation using statistical methods such as mean, median, or mode. The `SimpleImputer` class in scikit-learn allows you to easily apply these imputation techniques to your dataset.

Example:
Using the `SimpleImputer` class to impute missing values with the mean of the respective column.

```python
from sklearn.impute import SimpleImputer

# Create an instance of SimpleImputer
imputer = SimpleImputer(strategy='mean')

# Fit and transform the data
imputed_data = imputer.fit_transform(original_data)
```

---

### What are One Hot Encoding & Dummy Variables?

One hot encoding is a technique used to represent categorical variables as binary vectors. It involves creating a new binary column for each category present in the categorical variable, where the presence of a category is indicated by a value of 1 and the absence by a value of 0. Dummy variables are similar to one hot encoding but involve creating \( n-1 \) binary columns for \( n \) categories to avoid multicollinearity.

Example:
Suppose you have a categorical variable "color" with three categories: red, blue, and green. One hot encoding would create three binary columns: "color_red," "color_blue," and "color_green," where each column represents the presence or absence of a particular color.

```python
import pandas as pd

# Perform one hot encoding
encoded_data = pd.get_dummies(original_data, columns=['color'])
```

---

### What is Label Encoding?

Label encoding is a technique used to transform categorical variables into numerical values. It involves assigning a unique numerical label to each category present in the categorical variable. Label encoding is suitable for ordinal categorical variables, where there is a meaningful order among the categories.

Example:
Suppose you have a categorical variable "size" with categories: small, medium, and large. Label encoding would assign the labels 0, 1, and 2 to the categories, respectively.

```python
from sklearn.preprocessing import LabelEncoder

# Initialize LabelEncoder
label_encoder = LabelEncoder()

# Encode categorical variable
encoded_size = label_encoder.fit_transform(original_data['size'])
```

---

### What is Ordinal Encoding?

Ordinal encoding is similar to label encoding but is specifically designed for ordinal categorical variables, where there is a meaningful order among the categories. It involves assigning numerical labels to categories based on their order or ranking.

Example:
Suppose you have an ordinal categorical variable "education_level" with categories: high school, bachelor's degree, master's degree, and PhD. Ordinal encoding would assign numerical labels 0, 1, 2, and 3 to the categories, respectively, based on their increasing level of education.

```python
from sklearn.preprocessing import OrdinalEncoder

# Define the order of categories
education_order = ['high school', "bachelor's degree", "master's degree", 'PhD']

# Initialize OrdinalEncoder with specified order
ordinal_encoder = OrdinalEncoder(categories=[education_order])

# Encode ordinal categorical variable
encoded_education = ordinal_encoder.fit_transform(original_data[['education_level']])
```

---

### What is an Outlier and How to Handle It?

An outlier is an observation that significantly deviates from the rest of the data points in a dataset. Outliers can occur due to various reasons, such as measurement errors, experimental errors, or natural variation in the data. Handling outliers is essential to prevent them from skewing statistical analyses and machine learning models.

Example:
In a dataset containing information about the salaries of employees in a company, an unusually high salary compared to the rest of the salaries could be considered an outlier.

To handle outliers, you can use various techniques such as:

1. **Removing outliers**: Exclude data points that are identified as outliers from the dataset.
2. **Transforming data**: Apply mathematical transformations to the data to reduce the impact of outliers.
3. **Using robust statistical methods**: Utilize statistical methods that are less sensitive to outliers, such

 as median instead of mean.

---

### How to Remove Outliers using IQR?

The Interquartile Range (IQR) method is a robust statistical technique used to identify and remove outliers from a dataset. It involves calculating the IQR, which is the difference between the third quartile (Q3) and the first quartile (Q1), and then defining a range based on the IQR to identify outliers.

Example:
To remove outliers from a dataset using the IQR method, you would first calculate the IQR and then define a range within which data points are considered normal. Data points outside this range are considered outliers and can be removed from the dataset.

```python
def remove_outliers_iqr(data, column):
    Q1 = data[column].quantile(0.25)
    Q3 = data[column].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    cleaned_data = data[(data[column] >= lower_bound) & (data[column] <= upper_bound)]
    return cleaned_data

# Remove outliers from the 'salary' column using IQR method
cleaned_data = remove_outliers_iqr(original_data, 'salary')
```

---

### How to Remove Outliers using Z Score?

The Z Score method is another statistical technique used to identify and remove outliers from a dataset. It involves calculating the Z score for each data point, which represents the number of standard deviations a data point is from the mean. Data points with a Z score exceeding a certain threshold are considered outliers and can be removed from the dataset.

Example:
To remove outliers from a dataset using the Z Score method, you would calculate the Z score for each data point and then define a threshold beyond which data points are considered outliers.

```python
from scipy import stats

def remove_outliers_zscore(data, column, threshold=3):
    z_scores = stats.zscore(data[column])
    outliers = (abs(z_scores) > threshold)
    cleaned_data = data[~outliers]
    return cleaned_data

# Remove outliers from the 'age' column using Z Score method
cleaned_data = remove_outliers_zscore(original_data, 'age')
```

---

### What is Feature Scaling (Standardization)?

Feature scaling is a preprocessing technique used to standardize the range of independent variables or features in a dataset. It involves transforming the features so that they have a mean of 0 and a standard deviation of 1. Standardization ensures that all features are on a similar scale, which can improve the performance of machine learning algorithms, particularly those sensitive to the scale of features.

Example:
Suppose you have a dataset containing features such as age, income, and education level, which have different scales. Standardization would transform these features so that they have a mean of 0 and a standard deviation of 1.

```python
from sklearn.preprocessing import StandardScaler

# Initialize StandardScaler
scaler = StandardScaler()

# Fit and transform the data
scaled_features = scaler.fit_transform(original_data[['age', 'income', 'education_level']])
```

---

### What is Feature Scaling (Normalization)?

Normalization is another preprocessing technique used to scale the range of features in a dataset to a uniform range, typically between 0 and 1. It involves transforming the features so that they fall within the specified range while preserving their relative relationships. Normalization is particularly useful when the features have different ranges or units.

Example:
Suppose you have a dataset containing features such as temperature, humidity, and pressure, which have different scales. Normalization would scale these features to a uniform range between 0 and 1.

```python
from sklearn.preprocessing import MinMaxScaler

# Initialize MinMaxScaler
scaler = MinMaxScaler()

# Fit and transform the data
normalized_features = scaler.fit_transform(original_data[['temperature', 'humidity', 'pressure']])
```

---

### How to Handle Duplicate Data?

Duplicate data refers to records or observations that are identical or nearly identical to one another in a dataset. Duplicate data can arise due to various reasons, such as data entry errors, system failures, or merging datasets improperly. Handling duplicate data is essential to ensure the accuracy and integrity of analyses and modeling results.

Example:
In a dataset containing information about customer transactions, duplicate entries may occur if the same transaction is recorded multiple times due to system errors.

To handle duplicate data, you can use various techniques such as:

1. **Identifying and removing duplicates**: Detecting and eliminating duplicate records from the dataset.
2. **Preventing duplicate entries**: Implementing measures to prevent the occurrence of duplicate data, such as enforcing unique constraints in databases.
3. **Data deduplication**: Combining duplicate records into a single, representative record while preserving relevant information.

```python
# Identify and remove duplicate rows
cleaned_data = original_data.drop_duplicates()
```

---

### How to Replace and Change Data Types?

Replacing and changing data types involves modifying the values or types of variables in a dataset to ensure consistency and compatibility with analysis or modeling tasks. This process may include converting categorical variables to numerical format, changing data types to better represent the underlying data, or replacing specific values with alternative values.

Example:
Suppose you have a dataset containing a column representing gender with values 'male' and 'female.' To facilitate analysis, you might want to replace these categorical values with numerical labels, such as 0 and 1.

```python
# Replace categorical values with numerical labels
data['gender'] = data['gender'].replace({'male': 0, 'female': 1})
```

---

### Function Transformer

Function Transformer is a utility provided by scikit-learn that allows you to apply custom transformations to the data using functions. It is particularly useful for scenarios where you need to perform complex or custom preprocessing steps that are not directly supported by existing scikit-learn transformers.

Example:
Suppose you want to apply a custom transformation to a dataset to extract specific features or engineer new features. You can define a custom function and use Function Transformer to apply this function to the data.

```python
from sklearn.preprocessing import FunctionTransformer

# Define custom transformation function
def custom_transform(X):
    # Perform custom transformation
    transformed_data = X ** 2
    return transformed_data

# Initialize FunctionTransformer
transformer = FunctionTransformer(func=custom_transform)

# Apply custom transformation to the data
transformed_data = transformer.fit_transform(original_data)
```

---

These examples provide an overview of various concepts and techniques related to data cleaning and preprocessing. By understanding and applying these techniques effectively, you can ensure that your data is clean, consistent, and well-prepared for analysis or modeling tasks.
