# Assignment 3: Solutions and Explanations

---

## Part 1: Inspecting FITS Files

### 1. Opening and Inspecting a FITS File

**Task:** Open `example_image.fits`, inspect the contents using `fits.info()`, and print the primary header.

**Code:**
```python
from astropy.io import fits

# Open the FITS file
file_path = '../data/fits_files/example_image.fits'
hdul = fits.open(file_path)

# Display the contents of the FITS file
hdul.info()

# Access and print the header of the primary HDU
primary_header = hdul[0].header
print("\nPrimary Header:")
print(repr(primary_header))

# Extract specific header values
instrument = primary_header.get('INSTRUME', 'Unknown Instrument')
target_name = primary_header.get('OBJECT', 'Unknown Target')

print(f"\nInstrument: {instrument}")
print(f"Target: {target_name}")

# Close the FITS file
hdul.close()
```

**Explanation:**  
- `fits.open()` loads the FITS file.  
- `hdul.info()` provides an overview of the HDUs (e.g., images, tables).  
- `header.get()` safely retrieves metadata (e.g., `INSTRUME` for instrument and `OBJECT` for the target).  
- Always close the FITS file after use with `hdul.close()` to free memory.

**Expected Output:**
```
Filename: ../data/fits_files/example_image.fits
No.    Name         Type      Dimensions  Format
0      PRIMARY      ImageHDU  (1024, 1024)  float32

Primary Header:
SIMPLE  = T
BITPIX  = -32
NAXIS   = 2
...
INSTRUME= 'Hubble Space Telescope'
OBJECT  = 'NGC 1234'
...
Instrument: Hubble Space Telescope
Target: NGC 1234
```

---

### 2. Visualizing FITS Image Data

**Task:** Display the image data using Matplotlib, enhancing contrast with `vmin` and `vmax`.

**Code:**
```python
import numpy as np
import matplotlib.pyplot as plt

# Re-open the FITS file to extract data
hdul = fits.open(file_path)
image_data = hdul[0].data

# Compute vmin and vmax for contrast adjustment
vmin, vmax = np.percentile(image_data, [5, 95])

# Plot the image
plt.figure(figsize=(8, 8))
plt.imshow(image_data, cmap='gray', origin='lower', vmin=vmin, vmax=vmax)
plt.colorbar(label='Pixel Intensity')
plt.title('Example FITS Image')
plt.xlabel('Pixel X')
plt.ylabel('Pixel Y')
plt.show()

# Close the FITS file
hdul.close()
```

**Explanation:**  
- The image data is a 2D Numpy array accessed via `hdul[0].data`.  
- Adjusting `vmin` and `vmax` improves contrast for visualization.  
- Using `origin='lower'` ensures the correct orientation.

---

## Part 2: Working with FITS Tables

### 1. Reading a FITS Catalog

**Task:** Load `example_catalog.fits` into an Astropy table, display metadata and the first 5 rows.

**Code:**
```python
from astropy.table import Table

# Load the FITS catalog
catalog_path = '../data/fits_files/example_catalog.fits'
catalog_table = Table.read(catalog_path)

# Display table info
print("Catalog Columns:", catalog_table.colnames)
print("Data Types:", catalog_table.dtype)
print("Number of Rows:", len(catalog_table))

# Display first 5 rows
print("\nFirst 5 Rows:")
print(catalog_table[:5])
```

**Expected Output:**
```
Catalog Columns: ['RA', 'Dec', 'Magnitude']
Data Types: [('float64', 'float64', 'float64')]
Number of Rows: 1000

First 5 Rows:
 RA       Dec      Magnitude
-----    -----     ---------
 10.1     -5.2       14.2
 10.2     -5.1       13.7
...
```

---

### 2. Filtering and Analyzing Catalog Data

**Task:** Filter for `Magnitude < 15` and calculate summary statistics.

**Code:**
```python
# Filter objects with Magnitude < 15
bright_objects = catalog_table[catalog_table['Magnitude'] < 15]

# Count and compute statistics
num_bright_objects = len(bright_objects)
mean_ra = np.mean(bright_objects['RA'])
median_dec = np.median(bright_objects['Dec'])

print(f"Number of bright objects (Mag < 15): {num_bright_objects}")
print(f"Mean RA: {mean_ra:.2f}")
print(f"Median Dec: {median_dec:.2f}")
```

**Expected Output:**
```
Number of bright objects (Mag < 15): 512
Mean RA: 150.12
Median Dec: -5.34
```

---

### 3. Converting to Pandas

**Task:** Convert the filtered table to Pandas and compute statistics.

**Code:**
```python
# Convert to Pandas DataFrame
bright_df = bright_objects.to_pandas()

# Display first 5 rows
print("\nFirst 5 rows of Pandas DataFrame:")
print(bright_df.head())

# Compute standard deviation of Magnitude
std_magnitude = bright_df['Magnitude'].std()
print(f"Standard Deviation of Magnitude: {std_magnitude:.2f}")
```

---

## Part 3: Combining Tables and Calculations

### 1. Combining Two Catalogs

**Task:** Perform a simple cross-match between two catalogs based on RA/Dec.

**Code:**
```python
# Load second catalog
second_catalog_path = '../data/fits_files/example_catalog_2.fits'
second_catalog = Table.read(second_catalog_path)

# Perform a simple cross-match
matches = []
tolerance = 0.01  # degrees

for row in catalog_table:
    for second_row in second_catalog:
        if (abs(row['RA'] - second_row['RA']) <= tolerance and
            abs(row['Dec'] - second_row['Dec']) <= tolerance):
            matches.append((row['RA'], row['Dec'], row['Magnitude'], second_row['Magnitude']))

# Display matched objects
print("Matched Objects (RA, Dec, Mag1, Mag2):")
for match in matches[:5]:  # Show only first 5 matches
    print(match)
```

---

### 2. Adding and Modifying Columns

**Task:** Compute absolute magnitudes and add them as a column.

**Code:**
```python
# Add a default distance column
catalog_table['Distance_pc'] = 10

# Compute absolute magnitudes
catalog_table['Absolute_Magnitude'] = catalog_table['Magnitude'] - 5 * np.log10(catalog_table['Distance_pc']) + 5

# Display first 5 rows with new columns
print("\nFirst 5 rows with Absolute Magnitude:")
print(catalog_table[:5])
```

---

## Part 4: Exercises and Reflection

### 1. Filtering by Radius

**Code:**
```python
from astropy.coordinates import SkyCoord
import astropy.units as u

def filter_objects_by_radius(center_ra, center_dec, radius, table):
    center = SkyCoord(ra=center_ra*u.deg, dec=center_dec*u.deg, frame='icrs')
    coords = SkyCoord(ra=table['RA']*u.deg, dec=table['Dec']*u.deg, frame='icrs')
    separations = center.separation(coords)
    return table[separations <= radius*u.deg]

# Test the function
filtered_objects = filter_objects_by_radius(150, -5, 1, catalog_table)
print(f"Objects within 1 degree radius: {len(filtered_objects)}")
```

---

### 2. Reflection

**Sample Response:**  
"FITS files are incredibly versatile for storing astronomical data because they can hold images, tables, and metadata in a single container. They are more compact and structured than CSV or plain text. One challenge I faced was understanding HDUs and extracting specific parts of the file. However, Astropy makes this much easier."

---

By completing this assignment, we have explored Astropy's capabilities for handling FITS files and tables, tasks essential for modern astronomical data analysis.
