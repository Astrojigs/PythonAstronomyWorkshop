# Assignment 2: Detailed Solutions and Explanations

## Overview

In this assignment, we combined the use of Numpy and Pandas—two key Python libraries for data analysis. We worked with synthetic wavelength arrays using Numpy and handled star catalog data using Pandas DataFrames. These exercises mirror common tasks in astronomy, where you might handle large arrays of spectral data alongside tabular catalogs of stars or galaxies.

---

## Part 1: Numpy Basics

### 1. Array Creation and Manipulation

**Task:** Create a Numpy array `wavelengths` from 100 nm to 2000 nm in steps of 100 nm, then compute its mean, median, and standard deviation.

**Explanation:**  
- We can use `np.arange(start, stop, step)` to create a regularly spaced array.  
- Numpy arrays provide handy methods like `np.mean`, `np.median`, and `np.std` for statistics.  
- Wavelength arrays are common in spectroscopy, where you sample fluxes at discrete wavelengths.

**Code:**
```python
import numpy as np

# Create array from 100 to 2000 nm with step of 100 nm:
wavelengths = np.arange(100, 2100, 100)  # 100, 200, 300, ... 2000
print("Wavelengths array:", wavelengths)

# Check shape
print("Shape of wavelengths:", wavelengths.shape)

# Compute statistics
mean_wl = np.mean(wavelengths)
median_wl = np.median(wavelengths)
std_wl = np.std(wavelengths)

print(f"Mean wavelength: {mean_wl} nm")
print(f"Median wavelength: {median_wl} nm")
print(f"Standard Deviation: {std_wl:.2f} nm")
```

**Expected Output Reasoning:**  
- The array should have elements: [100, 200, 300, ..., 2000]. That’s (2000-100)/100 + 1 = 20 elements.  
- The mean should be roughly (100+2000)/2 = 1050 nm because it’s a uniform sequence.  
- The median, with an even count, should be the average of the 10th and 11th elements. Here, since it’s evenly spaced, the median is also ~1050 nm.
- The standard deviation will depend on the distribution around the mean, but it should be a value characteristic of a uniform distribution from 100 to 2000.

### 2. Vectorized Operations

**Task:** Given a simple flux model: \(\text{flux}(\lambda) = \frac{1}{\lambda}\), create `flux_values` from `wavelengths` and print the first 5 elements.

**Explanation:**  
- Numpy enables vectorized operations, so we can directly do `1.0 / wavelengths` without looping.  
- This simulates a toy model where flux inversely depends on wavelength, which is not physically accurate for a star’s spectrum but good for practice.

**Code:**
```python
flux_values = 1.0 / wavelengths
print("First 5 flux values:", flux_values[:5])
```

**Expected Output Reasoning:**  
- If `wavelengths[0] = 100 nm`, then `flux_values[0] = 1/100 = 0.01`.
- The first 5 flux values correspond to wavelengths = [100, 200, 300, 400, 500 nm], so we’ll get [0.01, 0.005, 0.003333..., 0.0025, 0.002].

### 3. Masking and Filtering Arrays

**Task:** Identify wavelengths ≥ 1000 nm and extract corresponding fluxes.

**Explanation:**  
- Astronomers often analyze specific wavelength ranges. Masking and filtering arrays let us focus on certain regions, e.g., infrared (≥ 1000 nm).
- We create a boolean mask and then index the original arrays with it.

**Code:**
```python
mask = wavelengths >= 1000
filtered_wavelengths = wavelengths[mask]
filtered_flux = flux_values[mask]

print("Wavelengths ≥ 1000 nm:", filtered_wavelengths)
print("Corresponding flux values:", filtered_flux)
```

**Expected Output Reasoning:**  
- For wavelengths [100, 200, ..., 2000], the subset ≥ 1000 nm are [1000, 1100, 1200, ..., 2000].  
- Flux at 1000 nm = 1/1000 = 0.001, and so forth.

---

## Part 2: Pandas Data Handling

**Assumption:** We have a file `star_catalog.csv` with columns: `Name`, `RA`, `Dec`, `Magnitude`, `Type`.

A sample data snippet might be:

```csv
Name,RA,Dec,Magnitude,Type
StarA,10.1,-5.2,14.2,Dwarf
StarB,10.2,-5.1,13.7,Giant
StarC,10.3,-5.3,15.1,Supergiant
StarD,10.4,-5.4,16.0,Dwarf
StarE,10.5,-5.0,12.9,Dwarf
```

Place this file in `data/csv_catalogs/star_catalog.csv`.

### 1. Reading Data with Pandas

**Task:** Load the CSV file into a DataFrame, print `head()`, `info()`, and `describe()`.

**Explanation:**  
- Pandas `read_csv` function loads tabular data into a DataFrame.
- `df.head()` shows the first few rows.
- `df.info()` shows column types and data counts.
- `df.describe()` gives summary statistics for numeric columns.

**Code:**
```python
import pandas as pd

df = pd.read_csv('../data/csv_catalogs/star_catalog.csv')
print("First 5 rows:\n", df.head())
print("\nDataFrame info:")
df.info()
print("\nSummary statistics:")
print(df.describe())
```

**Expected Output Reasoning:**  
- `df.head()` should show the first 5 rows including StarA, StarB, etc.  
- `df.info()` displays data types (float64 for RA/Dec/Magnitude, object for Name and Type).  
- `df.describe()` shows count, mean, std, min, max of numeric columns (RA, Dec, Magnitude).

### 2. Filtering and Selecting Data

**Task:**  
- Filter stars with `Magnitude < 14.5`.
- Print only `Name` and `Magnitude` of these filtered stars.
- Count how many stars are `Dwarf`.

**Explanation:**  
- We use boolean indexing to filter rows.
- Selecting columns is done by `df[['Name', 'Magnitude']]`.
- Counting a condition can be done with `value_counts()` or a sum of booleans.

**Code:**
```python
bright_stars = df[df['Magnitude'] < 14.5]
print("Bright stars (Mag < 14.5):\n", bright_stars)

# Select only Name and Magnitude
print("\nNames and Magnitudes of bright stars:\n", bright_stars[['Name', 'Magnitude']])

# Count how many are Dwarf
num_dwarfs = (df['Type'] == 'Dwarf').sum()
print("\nNumber of Dwarf stars in the catalog:", num_dwarfs)
```

**Expected Output Reasoning:**  
- `bright_stars` should include StarA (14.2), StarB (13.7), and StarE (12.9) because they are <14.5.
- Only StarC and StarD are excluded since they have magnitudes 15.1 and 16.0.
- If the sample data has StarA, StarD, and StarE as Dwarfs, we have 3 Dwarfs total.

### 3. Computations and Adding Columns

**Task:**  
- Compute flux from magnitude using \(F = F_0 \times 10^{-0.4 \times M}\).  
- We’ll choose \(F_0 = 3.63 \times 10^{-20}\) (arbitrary).  
- Add this `Flux` column, print the DataFrame, sort by Magnitude, and group by Type.

**Explanation:**  
- Converting magnitude to flux is a standard operation in astronomy.
- Sorting by `Magnitude` puts the brightest stars (lowest magnitude) first.
- Grouping by `Type` is common when comparing populations of objects (e.g., Dwarfs vs. Giants).

**Code:**
```python
F0 = 3.63e-20
df['Flux'] = F0 * 10**(-0.4 * df['Magnitude'])

print("\nDataFrame with Flux column:\n", df)

# Sort by Magnitude
sorted_df = df.sort_values('Magnitude')
print("\nStars sorted by Magnitude (brightest first):\n", sorted_df.head())

# Group by Type and compute mean magnitude
mean_mag_by_type = df.groupby('Type')['Magnitude'].mean()
print("\nMean Magnitude by Type:\n", mean_mag_by_type)
```

**Expected Output Reasoning:**  
- After adding `Flux`, each row now has a flux value.  
- Sorting by magnitude puts the smallest magnitude star (e.g., StarE with Mag=12.9) at the top.
- The `mean_mag_by_type` might show something like: Dwarf ~14.3, Giant ~13.7, Supergiant ~15.1 (based on the sample data).

---

## Part 3: Integration

### 1. Combining Numpy and Pandas

**Task:**  
- From `flux_values` (Numpy array), get the middle value.  
- From `df['Flux']`, compute the median flux.  
- Reflect on how these tools integrate.

**Explanation:**  
- The “middle value” of `flux_values` is at index `len(flux_values)//2`.
- Pandas Series objects have a `.median()` method for quick statistics.
- In astronomy, you might have wavelength arrays (Numpy) and catalogs (Pandas), and you want to combine insights from both: for example, selecting certain objects from the catalog to analyze their spectra represented by arrays.

**Code:**
```python
mid_index = len(flux_values) // 2
middle_flux_value = flux_values[mid_index]
print("Middle value in flux_values array:", middle_flux_value)

median_flux_df = df['Flux'].median()
print("Median flux in DataFrame:", median_flux_df)
```

**Expected Output Reasoning:**  
- If `flux_values` had 20 elements, `mid_index = 20//2 = 10`, the 11th element (index 10) might be something like 1/1100 nm ≈ 0.000909...
- The median flux in the DataFrame depends on the stars’ magnitudes. Brighter stars yield higher flux.

### 2. Discussion (Markdown)

**Sample Reflection Answer (No code needed):**  
"In a real astronomical research project, you might have a large Numpy array representing a spectrum—intensity values at different wavelengths. At the same time, you have a Pandas DataFrame representing a catalog of objects with their positions, magnitudes, and spectral classifications. By combining these tools, you can select a subset of objects from the DataFrame based on certain criteria (e.g., only Dwarf stars brighter than a certain magnitude) and then use corresponding parts of the Numpy arrays to analyze their spectral characteristics. This unified approach streamlines data handling, letting you quickly move from object properties in the catalog to examining their spectral flux distributions or comparing them across large samples."

---

## Summary of Learning

- **Numpy:** We learned how to create arrays, perform vectorized arithmetic, and filter data using masks. These skills are fundamental for handling pixel-level image data or spectral arrays.
- **Pandas:** We learned how to load data from CSV files, explore the DataFrame with `head()`, `info()`, and `describe()`, filter rows based on conditions, compute new columns, and aggregate data by groups. This reflects real-world scenarios where astronomers handle large catalogs from surveys (e.g., Gaia, SDSS).
- **Integration:** Combining Numpy arrays (like wavelength or flux arrays) with Pandas DataFrames (catalog info) is a common workflow in astronomy, allowing researchers to link observational metadata with quantitative data analysis.

By completing this assignment, you’ve strengthened your data handling capabilities and gained a better understanding of the tools you’ll need for more complex analyses in astronomy.
