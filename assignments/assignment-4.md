
# Assignment 4: Visualization for Astronomy

**Instructions:**  
In this assignment, you will use Matplotlib to create various plots to visualize astronomical data. You will plot data from catalogs and images, explore distributions, and customize plots to improve clarity and aesthetics.

---

## Part 1: Scatter Plots and Customization

### 1. Plotting Star Positions
- Load the FITS catalog `example_catalog.fits` (from Assignment 3) into an Astropy Table.
- Create a scatter plot of `RA` vs. `Dec` for all stars in the catalog.
- Customize the plot:
  - Set the marker size to 2.
  - Add a title, axis labels, and a grid.

**Example Output:**  
A scatter plot showing the positions of stars on the celestial sphere.

---

### 2. Adding Color to the Plot
- Use the `Magnitude` column to color the points in the scatter plot.
- Add a color bar to indicate the range of magnitudes.
- Use the colormap `'viridis'`.

**Hint:** Pass the `c` argument to `plt.scatter()` for coloring points and use `plt.colorbar()` for the color bar.

---

## Part 2: Histograms and Distributions

### 1. Magnitude Distribution
- Create a histogram of the `Magnitude` column from the same catalog.
- Customize the histogram:
  - Use 20 bins.
  - Set the color to `'green'`.
  - Add axis labels and a title.

**Example Question:** What is the peak magnitude range?

---

### 2. Overplotting Multiple Distributions
- Split the stars into two groups:  
  - Bright stars (`Magnitude < 14.5`).  
  - Dim stars (`Magnitude >= 14.5`).  
- Create a histogram for each group on the same plot with:
  - Different colors (e.g., `'blue'` for bright, `'orange'` for dim).
  - Set transparency (`alpha=0.5`) to distinguish overlapping areas.
- Add a legend to the plot.

---

## Part 3: Displaying FITS Images

### 1. Basic Image Display
- Load the FITS file `example_image.fits`.
- Extract the image data and display it using `plt.imshow()`.
- Use the colormap `'gray'`.

---

### 2. Enhancing Contrast
- Adjust the contrast of the image by setting `vmin` and `vmax` to the 5th and 95th percentiles of the pixel values.
- Add a color bar and a title.

---

## Part 4: Subplots and Combining Plots

### 1. Side-by-Side Plots
- Create two plots side-by-side:
  - The first plot is the scatter plot of `RA` vs. `Dec` from Part 1.
  - The second plot is the histogram of magnitudes from Part 2.
- Use `plt.subplots()` with 1 row and 2 columns.

---

### 2. Multi-Panel Display
- Create a 2x2 grid of subplots:
  - Top-left: The scatter plot of `RA` vs. `Dec`.
  - Top-right: Histogram of all magnitudes.
  - Bottom-left: Histogram of bright stars.
  - Bottom-right: The FITS image (with enhanced contrast).
- Add titles to all subplots and adjust spacing using `plt.tight_layout()`.

---

## Part 5: Exercises and Reflection

1. **Exercises**
   - Modify the scatter plot in Part 1 to display only stars with `Magnitude < 15`.  
   - Save the modified scatter plot to a file named `bright_stars_scatter.png` with 300 dpi resolution.

2. **Reflection (Markdown Cell)**
   - Write a short paragraph on why visualization is important in astronomy. Mention one type of plot you found most useful or challenging.

---

## Submission Guidelines

- Save your code and outputs in a Jupyter notebook named `solution_day4.ipynb`.
- Include comments and Markdown cells to explain each step.
- Ensure all code runs without errors.
- Submit your notebook by the due date.

---

**Good Luck!**  
By completing this assignment, you’ll gain practical experience in creating and customizing visualizations that are essential for analyzing and presenting astronomical data.
