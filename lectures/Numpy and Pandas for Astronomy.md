
# Introduction to Numpy and Pandas for Astronomical Data Analysis

**Lecture Objectives:**  
- Understand how to create and manipulate arrays with Numpy.  
- Learn how to load, filter, and analyze tabular data with Pandas.  
- Apply these tools to simple astronomical datasets.

---

## 1. Introduction and Motivation

In astronomy, we frequently work with large numerical datasets—whether it’s arrays of pixel values from telescopes or catalogs of star positions and magnitudes. Handling such data efficiently requires robust computational tools. Two key Python libraries excel at this:

- **Numpy:** For handling n-dimensional arrays and performing fast numerical operations.
- **Pandas:** For intuitive manipulation of tabular (row-and-column) data.

By the end of this lecture, you’ll be able to create and manipulate arrays with Numpy and read, filter, and analyze tabular data with Pandas.

---

## 2. Getting Started with Numpy

**Numpy** is the fundamental package for scientific computing in Python. It provides the `ndarray` object, which is a fast, space-efficient way to represent arrays of homogeneous data.

### 2.1 Importing Numpy

```python
import numpy as np
```

### 2.2 Creating Arrays

- From a Python list:
```python
arr = np.array([1, 2, 3, 4])
arr
```

- Using built-in functions:
```python
zeros = np.zeros((3,3))
linspace_arr = np.linspace(0, 10, 11)  # 11 points between 0 and 10 inclusive
```

### 2.3 Array Shapes and Reshaping

Check dimensions:
```python
arr.shape   # returns shape tuple, e.g. (4,)
arr.ndim    # returns number of dimensions
```

Reshape arrays:
```python
mat = np.arange(1,13)  # array([1,2,...,12])
mat = mat.reshape(3,4) # reshape into a 3x4 matrix
mat
```

### 2.4 Indexing and Slicing

Access elements:
```python
mat[0,0]       # top-left element
mat[:, 0]      # first column
mat[1, :]       # second row
```

Boolean indexing:
```python
mat[mat > 5]   # returns all elements greater than 5
```

### 2.5 Basic Math and Broadcasting

Arithmetic is element-wise:
```python
mat * 2
mat + 10
```

Two arrays of compatible shapes will broadcast:
```python
arr1 = np.array([1,2,3])
arr2 = np.array([10,20,30])
arr1 + arr2   # array([11,22,33])
```

Summary statistics:
```python
np.mean(mat)
np.std(mat)
np.median(mat)
```

### 2.6 Practical Example: Synthetic Star Magnitudes

Imagine we have magnitudes for 100 stars:

```python
magnitudes = 10 + 2 * np.random.randn(100)  # mean ~10, std ~2
mean_mag = np.mean(magnitudes)
print("Mean magnitude:", mean_mag)
```

---

## 3. Introduction to Pandas

**Pandas** provides the `DataFrame` object, which resembles a spreadsheet or SQL table, making it intuitive to handle columns of different types.

### 3.1 Importing Pandas

```python
import pandas as pd
```

### 3.2 Creating DataFrames

From a dictionary:
```python
data = {
    'StarName': ['StarA', 'StarB', 'StarC'],
    'RA': [10.1, 10.2, 10.3],
    'Dec': [-5.2, -5.1, -5.3],
    'Magnitude': [14.2, 13.5, 14.7]
}
df = pd.DataFrame(data)
df
```

### 3.3 Reading Data from Files

Assume you have a CSV file `stars.csv` in `data/csv_catalogs/`:

```python
df_stars = pd.read_csv('../data/csv_catalogs/stars.csv')
df_stars.head()    # preview first five rows
```

### 3.4 Exploring DataFrames

- Get info about columns and data types:
```python
df_stars.info()
df_stars.describe()   # summary statistics for numeric columns
```

- Selecting columns:
```python
df_stars['Magnitude']
df_stars[['RA','Dec']]
```

### 3.5 Filtering and Indexing

Filter rows:
```python
bright_stars = df_stars[df_stars['Magnitude'] < 15]
bright_stars.head()
```

Sorting:
```python
df_stars_sorted = df_stars.sort_values('Magnitude')
```

### 3.6 Adding and Manipulating Columns

Converting magnitudes to flux (hypothetical formula):
```python
import numpy as np
# Suppose F0 is a reference flux
F0 = 1e-9  
df_stars['Flux'] = F0 * 10**(-0.4 * df_stars['Magnitude'])
df_stars.head()
```

Dealing with missing data:
```python
df_stars = df_stars.dropna(subset=['Magnitude'])  # remove rows without magnitude
```

### 3.7 Saving and Exporting

```python
bright_stars.to_csv('bright_stars_filtered.csv', index=False)
```

---

## 4. Quick Visualization Preview

Although we’ll cover plotting in more depth later, here’s a quick glimpse:

```python
import matplotlib.pyplot as plt
%matplotlib inline

df_stars['Magnitude'].hist()
plt.title("Distribution of Star Magnitudes")
plt.xlabel("Magnitude")
plt.ylabel("Count")
```

---

## 5. Exercises

Try these exercises at the end of the lecture:

1. **Numpy Exercise:**  
   Create a Numpy array of 100 RA values evenly spaced between 0 and 360 degrees, then compute their mean.
   
   ```python
   ra_values = np.linspace(0, 360, 100)
   print("Mean RA:", np.mean(ra_values))
   ```

2. **Pandas Exercise:**  
   Load a provided star catalog from `data/csv_catalogs/stars.csv`. Filter out all stars dimmer than magnitude 16. Calculate the mean RA of the filtered set.
   
   ```python
   df_stars = pd.read_csv('../data/csv_catalogs/stars.csv')
   brighter_stars = df_stars[df_stars['Magnitude'] < 16]
   mean_ra = np.mean(brighter_stars['RA'])
   print("Mean RA of brighter stars:", mean_ra)
   ```

3. **Data Export:**  
   Export your filtered DataFrame to a new file called `filtered_stars.csv`.

   ```python
   brighter_stars.to_csv('filtered_stars.csv', index=False)
   ```

---

## 6. Summary and Next Steps

- **Numpy:** We learned how to create arrays, slice them, and perform computations efficiently.
- **Pandas:** We introduced DataFrames for handling tabular data, filtering rows, selecting columns, and computing statistics.

**Next Steps:**  
In future lectures, we’ll integrate these foundations with Astropy’s specialized functions for handling astronomical images, catalogs, coordinates, and cosmological calculations.

Keep exploring these tools and try out the exercises to gain confidence. In the next sessions, we’ll combine these skills to tackle real astronomical data sets.
