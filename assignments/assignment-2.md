
# Assignment 2: Working with Numpy and Pandas for Astronomical Data


**Instructions:**  
In this assignment, you will use Numpy and Pandas to explore and manipulate astronomical data. Make sure you have the required packages installed and that you have placed any data files as instructed. If no real data file is provided, you can create synthetic data or use the examples given below.

---

## Part 1: Numpy Basics

1. **Array Creation and Manipulation**  
   - Create a Numpy array `wavelengths` that spans from 100 nm to 2000 nm in steps of 100 nm. (Hint: convert nm to meters if you wish, but it’s not mandatory here. Just be consistent.)
   - Print the shape of `wavelengths` and confirm it looks as expected.
   - Compute the mean, median, and standard deviation of the `wavelengths` array and print these statistics.

2. **Vectorized Operations**  
   - Suppose we have a simple model of flux as a function of wavelength: \(\text{flux}( \lambda ) = \frac{1}{\lambda}\) (in arbitrary units).
   - Using `wavelengths`, create a new array `flux_values` by applying the above formula.  
   - Print the first 5 elements of `flux_values` to see if it makes sense.

3. **Masking and Filtering Arrays**  
   - Create a mask that identifies all wavelengths greater than or equal to 1000 nm. Use this mask to extract the `flux_values` for those wavelengths.
   - Print the filtered wavelengths and corresponding flux values.

**Reflection:** Numpy arrays allow fast, vectorized computations. In astronomy, working efficiently with large arrays is crucial when dealing with spectra, image pixel values, or large catalogs.

---

## Part 2: Pandas Data Handling

For this section, assume you have a CSV file named `star_catalog.csv` located in the `data/csv_catalogs/` directory. If you don’t have a real file, create a small synthetic one with columns such as `Name`, `RA`, `Dec`, `Magnitude`, and `Type`. For example:

```csv
Name,RA,Dec,Magnitude,Type
StarA,10.1,-5.2,14.2,Dwarf
StarB,10.2,-5.1,13.7,Giant
StarC,10.3,-5.3,15.1,Supergiant
StarD,10.4,-5.4,16.0,Dwarf
StarE,10.5,-5.0,12.9,Dwarf
```

1. **Reading Data with Pandas**  
   - Use Pandas to read `star_catalog.csv` into a DataFrame called `df`.  
   - Print the first 5 rows using `df.head()` to confirm the data is loaded correctly.
   - Print `df.info()` and `df.describe()` to get a summary of the DataFrame.

2. **Filtering and Selecting Data**  
   - Select only the rows where `Magnitude < 14.5` and assign this filtered DataFrame to `bright_stars`. Print `bright_stars`.
   - From `bright_stars`, select only the columns `Name` and `Magnitude` and print them.
   - Count how many stars are classified as `Dwarf` in the entire DataFrame.

3. **Computations and Adding Columns**  
   - Assume `Magnitude` is measured in the visual band. Create a new column `Flux` in `df` by converting magnitude to an approximate flux. Use a simplified formula (you can reuse the flux-to-magnitude concept from Assignment 1 or invent a linear relationship if you prefer):  
     For example:  
     
     $\text{Flux} = F_0 \times 10^{(-0.4 \times \text{Magnitude})}$

     Choose an arbitrary $(F_0)$ (e.g., $(3.63 \times 10^{-20}$)) and calculate the `Flux` column.
   - Print the updated DataFrame `df` and confirm `Flux` has been added.

4. **Sorting and Grouping**  
   - Sort the DataFrame by `Magnitude` in ascending order. Print the first 5 rows of the sorted DataFrame to see the brightest stars at the top.
   - Group the stars by `Type` and compute the mean magnitude for each type. Print these mean values.

**Reflection:** Pandas allows you to load catalogs of stars, galaxies, or other astronomical objects and apply powerful filtering, grouping, and statistical operations. This mirrors the real workflow astronomers use when dealing with large survey catalogs.

---

## Part 3: Integration

1. **Combining Numpy and Pandas**  
   - From your `flux_values` array in Part 1, select the middle value (the wavelength that lies roughly at the center of your array) and print it.
   - Using the `Flux` column in your DataFrame `df`, compute the median flux and print it.
   
   Think about how these computations relate to real data pipelines, where Numpy arrays might represent spectral or image data and Pandas DataFrames represent catalogs of sources.

2. **Short Discussion (Markdown Cell)**  
   In a Markdown cell (no code needed), discuss a scenario where you might want to join data from a Pandas DataFrame (e.g., star catalogs) with Numpy arrays (e.g., spectral data). How would combining these tools help in a real astronomical research project?

---

## Submission Guidelines

- Place your code in a Jupyter notebook named `your_name_solution_2.ipynb`.
- Include comments to explain each step.
- Make sure your code runs without errors and that you’ve answered all parts of the assignment.
- Submit your notebook by the due date.

---

**Good Luck!**  
By completing this assignment, you’ll gain confidence in handling arrays and tables—two fundamental data structures in astronomy data analysis.
