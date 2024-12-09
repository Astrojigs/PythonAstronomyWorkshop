

# *Data Handling with Astropy* : Tables and FITS Files

**1. Introduction**  
- **Motivation:**  
  Astronomical data often comes in FITS (Flexible Image Transport System) format for images and binary tables. Astropy provides convenient tools to read, write, and manipulate these files. Also, Astropy Tables provide a structure similar to Pandas DataFrames but optimized for astronomical use cases.
- **Goals:**  
  By the end of this notebook, you’ll know how to:
  - Open and inspect FITS images and headers.
  - Access and interpret FITS table data.
  - Use Astropy Tables to manage and manipulate catalog data.
  - Convert between Astropy Tables and Pandas DataFrames for convenience.

**2. Getting Started with Astropy**  
- **Imports:**
  ```python
  from astropy.io import fits
  from astropy.table import Table
  import numpy as np
  import pandas as pd
  import matplotlib.pyplot as plt
  %matplotlib inline
  ```
- **Brief Introduction to FITS Files:**  
  - FITS is a standard data format in astronomy.
  - A FITS file can contain multiple "HDUs" (Header Data Units), which may hold images, tables, or metadata.

**3. Inspecting FITS Files**  
- **Opening a FITS File:**
  ```python
  hdul = fits.open('../data/fits_files/example_image.fits')
  hdul.info()
  ```
- **Understanding HDUs:**  
  - Show how to index into `hdul[0]`, `hdul[1]`, etc.
  - Examine headers:
    ```python
    header = hdul[0].header
    print(repr(header))
    ```
- **Image Data:**  
  - Accessing the image array:
    ```python
    image_data = hdul[0].data
    print(image_data.shape)
    ```
  - Display the image using Matplotlib:
    ```python
    plt.imshow(image_data, origin='lower', cmap='viridis')
    plt.colorbar(label='Pixel value')
    plt.title('Example FITS Image')
    ```
- **Closing the File:**
  ```python
  hdul.close()
  ```

**4. FITS Tables**  
- **Reading a FITS Table:**
  ```python
  # Suppose the second HDU is a binary table
  hdul = fits.open('../data/fits_files/example_catalog.fits')
  hdul.info()
  table_data = hdul[1].data
  ```
- **Accessing Columns:**
  ```python
  print(table_data.columns) 
  ra = table_data['RA']
  dec = table_data['DEC']
  mag = table_data['MAG']
  ```
- **Converting to Astropy Table:**
  ```python
  cat_table = Table.read('../data/fits_files/example_catalog.fits')
  cat_table
  ```
- **Examining Astropy Table:**
  - Show column names, units, and metadata.
    ```python
    cat_table.info()
    ```
  - Slice and filter the table (similar to Pandas):
    ```python
    bright_stars = cat_table[cat_table['MAG'] < 15]
    print(len(bright_stars))
    ```

**5. Working with Astropy Tables**  
- **Creating Tables from Scratch:**
  ```python
  names = ['StarA', 'StarB', 'StarC']
  distances = [2.6, 5.1, 0.9] # in parsecs
  new_table = Table([names, distances], names=('Name','Distance_pc'))
  new_table
  ```
- **Adding and Removing Columns:**
  ```python
  new_table['Distance_ly'] = new_table['Distance_pc'] * 3.26
  del new_table['Distance_pc']
  ```

- **I/O with Astropy Tables:**
  - Write the table to a FITS or ASCII file:
    ```python
    new_table.write('my_new_catalog.fits', overwrite=True)
    ```

**6. Interfacing with Pandas**  
- **Converting Astropy Table to Pandas DataFrame:**
  ```python
  df_from_table = cat_table.to_pandas()
  df_from_table.head()
  ```
- **Converting Pandas DataFrame to Astropy Table:**
  ```python
  table_from_df = Table.from_pandas(df_from_table)
  table_from_df
  ```
- **When to use which?**  
  - Astropy Tables handle units, meta information, and FITS compatibility well.
  - Pandas may offer more flexible data wrangling tools.
  
**7. Practical Example: Selecting Stars from a FITS Catalog**  
- **Load a real FITS catalog (e.g., Gaia subset) and filter by magnitude:**
  ```python
  gaia_table = Table.read('../data/fits_files/gaia_sample.fits')
  bright_gaia = gaia_table[gaia_table['phot_g_mean_mag'] < 10]
  print("Number of bright stars:", len(bright_gaia))
  ```
- **Plotting RA/Dec Distribution:**
  ```python
  plt.scatter(bright_gaia['ra'], bright_gaia['dec'], s=1)
  plt.xlabel('RA (deg)')
  plt.ylabel('Dec (deg)')
  plt.title('Bright Gaia Stars')
  ```
  
**8. Exercises**  
- **Exercise 1:**  
  Open `example_image.fits`, print out the header information, and identify the instrument name (if available in the header).
  
- **Exercise 2:**  
  Read in `example_catalog.fits` and filter the table to include only objects with `MAG < 14`. Print how many objects satisfy this criterion.

- **Exercise 3:**  
  Create a new Astropy table with columns `['Object', 'RA', 'DEC']` and a few made-up entries. Write it out to a FITS file. Then read it back in and verify the contents.

- **Exercise 4 (Challenge):**  
  Convert the filtered catalog from Exercise 2 into a Pandas DataFrame. Compute the mean, median, and standard deviation of their magnitudes using Pandas methods.

**(Optional) Advanced Challenge:**  
- If time permits, demonstrate reading a multi-extension FITS file with multiple HDUs containing both images and tables, and selecting a particular HDU for analysis.
  
**9. Summary and Next Steps**  
- **Recap:**  
  - You learned how to open and inspect FITS files, read image data, and understand HDUs.
  - Introduced Astropy Tables as a flexible alternative to Pandas DataFrames, tailored for astronomy data.
  - Showed how to convert between Astropy Tables and Pandas.
- **Next:**  
  In the following notebooks, you’ll use these skills to explore coordinates, units, and more complex data manipulation, eventually applying them to real-world astronomical analysis tasks.
