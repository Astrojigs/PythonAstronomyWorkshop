# Assignment 4: Solutions and Explanations

---

## Part 1: Scatter Plots and Customization

### 1. Plotting Star Positions

**Task:** Create a scatter plot of `RA` vs. `Dec` for stars in `example_catalog.fits`.

**Code:**
```python
from astropy.table import Table
import matplotlib.pyplot as plt

# Load the catalog
catalog_path = '../data/fits_files/example_catalog.fits'
catalog_table = Table.read(catalog_path)

# Extract RA and Dec
ra = catalog_table['RA']
dec = catalog_table['Dec']

# Scatter plot
plt.figure(figsize=(8, 6))
plt.scatter(ra, dec, s=2, color='blue')
plt.title("Star Positions (RA vs Dec)", fontsize=14)
plt.xlabel("Right Ascension (degrees)", fontsize=12)
plt.ylabel("Declination (degrees)", fontsize=12)
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()
```

**Explanation:**  
- The `s=2` argument sets the marker size.  
- Adding a grid and labeling axes improves readability.  
- Scatter plots like this are commonly used to visualize the spatial distribution of stars or galaxies.

---

### 2. Adding Color to the Plot

**Task:** Color the scatter plot points by `Magnitude`.

**Code:**
```python
magnitude = catalog_table['Magnitude']

# Scatter plot with color mapping
plt.figure(figsize=(8, 6))
sc = plt.scatter(ra, dec, c=magnitude, cmap='viridis', s=10)
plt.colorbar(sc, label='Magnitude')
plt.title("Star Positions Colored by Magnitude", fontsize=14)
plt.xlabel("Right Ascension (degrees)", fontsize=12)
plt.ylabel("Declination (degrees)", fontsize=12)
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()
```

**Explanation:**  
- The `c=magnitude` argument assigns colors based on the `Magnitude` column.  
- `cmap='viridis'` specifies the colormap, and `plt.colorbar()` adds a color bar for reference.

---

## Part 2: Histograms and Distributions

### 1. Magnitude Distribution

**Task:** Plot a histogram of star magnitudes.

**Code:**
```python
plt.figure(figsize=(8, 6))
plt.hist(magnitude, bins=20, color='green', edgecolor='black', alpha=0.7)
plt.title("Distribution of Star Magnitudes", fontsize=14)
plt.xlabel("Magnitude", fontsize=12)
plt.ylabel("Number of Stars", fontsize=12)
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()
```

**Explanation:**  
- `bins=20` divides the data into 20 intervals.  
- `edgecolor='black'` and `alpha=0.7` improve the aesthetics of the histogram.  

---

### 2. Overplotting Multiple Distributions

**Task:** Compare distributions of bright and dim stars.

**Code:**
```python
bright_magnitudes = magnitude[magnitude < 14.5]
dim_magnitudes = magnitude[magnitude >= 14.5]

plt.figure(figsize=(8, 6))
plt.hist(bright_magnitudes, bins=20, color='blue', alpha=0.5, label='Bright Stars')
plt.hist(dim_magnitudes, bins=20, color='orange', alpha=0.5, label='Dim Stars')
plt.title("Magnitude Distribution: Bright vs Dim Stars", fontsize=14)
plt.xlabel("Magnitude", fontsize=12)
plt.ylabel("Number of Stars", fontsize=12)
plt.legend()
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()
```

**Explanation:**  
- Creating separate histograms for bright and dim stars allows for comparison.  
- The `alpha=0.5` transparency ensures overlapping areas are visible.

---

## Part 3: Displaying FITS Images

### 1. Basic Image Display

**Task:** Load and display the FITS image.

**Code:**
```python
from astropy.io import fits
import numpy as np

# Load FITS file
image_path = '../data/fits_files/example_image.fits'
hdul = fits.open(image_path)
image_data = hdul[0].data

# Display the image
plt.figure(figsize=(8, 8))
plt.imshow(image_data, cmap='gray', origin='lower')
plt.colorbar(label='Pixel Intensity')
plt.title("FITS Image Display", fontsize=14)
plt.show()

# Close FITS file
hdul.close()
```

---

### 2. Enhancing Contrast

**Task:** Adjust the contrast of the FITS image.

**Code:**
```python
# Compute contrast limits
vmin, vmax = np.percentile(image_data, [5, 95])

# Display the image with contrast enhancement
plt.figure(figsize=(8, 8))
plt.imshow(image_data, cmap='gray', origin='lower', vmin=vmin, vmax=vmax)
plt.colorbar(label='Pixel Intensity')
plt.title("FITS Image with Enhanced Contrast", fontsize=14)
plt.show()
```

**Explanation:**  
- The 5th and 95th percentiles of pixel intensity values set `vmin` and `vmax`.  
- This enhances the contrast by clipping extreme values.

---

## Part 4: Subplots and Combining Plots

### 1. Side-by-Side Plots

**Task:** Create two side-by-side plots.

**Code:**
```python
fig, axes = plt.subplots(1, 2, figsize=(12, 6))

# Scatter plot (RA vs Dec)
axes[0].scatter(ra, dec, s=2, color='blue')
axes[0].set_title("Star Positions")
axes[0].set_xlabel("RA")
axes[0].set_ylabel("Dec")
axes[0].grid(True)

# Histogram of Magnitudes
axes[1].hist(magnitude, bins=20, color='green', edgecolor='black')
axes[1].set_title("Magnitude Distribution")
axes[1].set_xlabel("Magnitude")
axes[1].set_ylabel("Count")
axes[1].grid(True)

plt.tight_layout()
plt.show()
```

**Explanation:**  
- `plt.subplots()` creates a grid of subplots.  
- Each axis object (`axes[0]`, `axes[1]`) is customized individually.

---

### 2. Multi-Panel Display

**Task:** Create a 2x2 grid of plots.

**Code:**
```python
fig, axes = plt.subplots(2, 2, figsize=(12, 12))

# Top-left: Scatter plot
axes[0, 0].scatter(ra, dec, c=magnitude, cmap='viridis', s=10)
axes[0, 0].set_title("RA vs Dec")
axes[0, 0].set_xlabel("RA")
axes[0, 0].set_ylabel("Dec")
axes[0, 0].grid(True)

# Top-right: Histogram of all magnitudes
axes[0, 1].hist(magnitude, bins=20, color='green', edgecolor='black')
axes[0, 1].set_title("Magnitude Distribution")
axes[0, 1].set_xlabel("Magnitude")
axes[0, 1].set_ylabel("Count")
axes[0, 1].grid(True)

# Bottom-left: Bright vs Dim histogram
axes[1, 0].hist(bright_magnitudes, bins=20, color='blue', alpha=0.5, label='Bright')
axes[1, 0].hist(dim_magnitudes, bins=20, color='orange', alpha=0.5, label='Dim')
axes[1, 0].set_title("Bright vs Dim Stars")
axes[1, 0].set_xlabel("Magnitude")
axes[1, 0].set_ylabel("Count")
axes[1, 0].legend()

# Bottom-right: FITS image
axes[1, 1].imshow(image_data, cmap='gray', origin='lower', vmin=vmin, vmax=vmax)
axes[1, 1].set_title("FITS Image")
axes[1, 1].axis('off')

plt.tight_layout()
plt.show()
```

**Explanation:**  
- Combining multiple types of plots gives an overview of the data in one figure.  
- Titles, labels, and legends make the figure self-explanatory.

---

## Part 5: Exercises and Reflection

### 1. Filtering Bright Stars

**Code:**
```python
# Filter for bright stars (Magnitude < 15)
bright_stars = catalog_table[catalog_table['Magnitude'] < 15]

# Scatter plot
plt.figure(figsize=(8, 6))
plt.scatter(bright_stars['RA'], bright_stars['Dec'], s=2, color='red')
plt.title("Bright Stars (Magnitude < 15)", fontsize=14)
plt.xlabel("RA")
plt.ylabel("Dec")
plt.grid(True)
plt.savefig("bright_stars_scatter.png", dpi=300)
plt.show()
```

---

### 2. Reflection

**Sample Answer:**  
"Visualization is crucial in astronomy for identifying patterns, trends, and anomalies in data. I found scatter plots most useful for spatial distributions, as they provide an intuitive representation of celestial coordinates. The most challenging part was enhancing the contrast in FITS images, as it required understanding percentile-based clipping."

---

By completing this assignment, students will gain experience in creating, customizing, and combining visualizations to analyze and present astronomical data effectively.
