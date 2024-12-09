# Assignment 3: Data Handling with Astropy (Tables and FITS)

**Instructions:**  
This assignment focuses on using Astropy to handle astronomical data. You will work with FITS files and tables to explore, manipulate, and analyze data. Make sure you have the required FITS files available in the appropriate directory. If no real FITS files are available, use the provided synthetic data.

---

## Part 1: Inspecting FITS Files

### 1. Opening and Inspecting a FITS File
- Open the FITS file `example_image.fits` from the directory `data/fits_files/`.
- Use `fits.info()` to list the contents of the file.
- Print the header of the primary HDU (Header Data Unit).
- Identify the following from the header:
  - The instrument used to capture the image.
  - The target name or object observed (if available).

**Hint:** Use `hdu[0].header` to access the primary header.

---

### 2. Visualizing FITS Image Data
- Extract the image data from the primary HDU and store it in a Numpy array.
- Use Matplotlib to display the image using `plt.imshow()`.
- Customize the plot:
  - Set the color map to `'gray'`.
  - Add a color bar to indicate pixel intensity values.
  - Use `vmin` and `vmax` to enhance contrast (e.g., set `vmin` to the 5th percentile and `vmax` to the 95th percentile of the pixel values).

---

## Part 2: Working with FITS Tables

### 1. Reading a FITS Catalog
- Open the FITS file `example_catalog.fits` located in `data/fits_files/`.
- Use `Table.read()` to load the catalog into an Astropy `Table`.
- Print the column names, data types, and the number of rows in the table.
- Display the first 5 rows using `table[:5]`.

---

### 2. Filtering and Analyzing Catalog Data
- The catalog contains columns `RA`, `Dec`, and `Magnitude`. Perform the following:
  - Filter the table to select objects with `Magnitude < 15`.
  - Count the number of objects that meet this criterion.
  - Compute the mean and median RA and Dec values of the filtered objects.

---

### 3. Converting to Pandas
- Convert the filtered table to a Pandas DataFrame using `.to_pandas()`.
- Display the first 5 rows of the DataFrame.
- Use Pandas to calculate the standard deviation of the `Magnitude` column.

---

## Part 3: Combining Tables and Calculations

### 1. Combining Two Catalogs
- Suppose you have a second FITS catalog `example_catalog_2.fits` with similar columns (`RA`, `Dec`, `Magnitude`, `Object_Type`).
- Read the second catalog into another Astropy `Table`.
- Perform a simple cross-match:
  - Find objects from the first catalog whose RA and Dec match (within a tolerance of 0.01 degrees) with objects in the second catalog.
  - Print the matched objects with their RA, Dec, and Magnitudes.

---

### 2. Adding and Modifying Columns
- Add a new column `Distance_pc` to the first catalog, assuming all objects are at a default distance of 10 parsecs.
- Use this distance to calculate the absolute magnitude for each object using the formula:
  \[
  M = m - 5 \times \log_{10}(d) + 5
  \]
  Here, \(M\) is the absolute magnitude, \(m\) is the apparent magnitude, and \(d\) is the distance in parsecs.
- Add the computed absolute magnitudes as a new column in the table.

---

## Part 4: Short Exercises and Reflection

1. **Exercises:**
   - Write a function `filter_objects_by_radius(center_ra, center_dec, radius)` that selects all objects within a given radius (in degrees) from a central RA/Dec coordinate. Test this function on the first catalog.
   - Calculate the angular separation between two objects in the first catalog using the formula:
     
     $\text{sep} = \sqrt{(\Delta \text{RA} \cdot \cos(\text{Dec}))^2 + (\Delta \text{Dec})^2}$
     
     Use Astropy's `SkyCoord` instead of manual calculations if you're familiar with it.

2. **Reflection (Markdown Cell):**
   - Discuss why FITS files are the standard in astronomy and what advantages they provide over CSV or plain text formats. Mention one challenge you faced while working with FITS files.

---

## Submission Guidelines

- Place your code in a Jupyter notebook named `your_name_solution_3.ipynb`.
- Include comments to explain each step and Markdown cells for explanations or reflections.
- Make sure your code runs without errors and provides the required outputs.
- Submit your notebook by the due date.

---

**Good Luck!**  
By completing this assignment, you’ll strengthen your skills in working with FITS files and astronomical catalogs, which are essential for analyzing data from telescopes and surveys.
