# Visualization for Astronomy with Matplotlib

**1. Introduction**  
- **Motivation:**  
  Visualization is key in astronomy to understand large datasets and reveal underlying patterns.  
- **Goals:**  
  - Learn to create line plots, scatter plots, histograms, and images with Matplotlib.  
  - Customize plot styles, labels, and colors.  
  - Use visualization techniques to explore astronomical catalogs and images.

**2. Getting Started with Matplotlib**  
- **Imports and Setup:**
  ```python
  import matplotlib.pyplot as plt
  import numpy as np
  from astropy.table import Table
  %matplotlib inline
  ```
- **Basic Plot:**  
  ```python
  x = np.linspace(0, 10, 100)
  y = np.sin(x)
  
  plt.plot(x, y)
  plt.title("Simple Sine Wave")
  plt.xlabel("X")
  plt.ylabel("sin(X)")
  plt.show()
  ```
- **Plot Customization:**  
  - Line styles, markers, colors (`'r-'`, `'bo'`, etc.).  
  - Adding legends, grids.
  
  ```python
  plt.plot(x, y, 'r--', label='sin(x)')
  plt.legend()
  plt.grid(True)
  ```

**3. Scatter Plots for Catalog Data**  
- **Loading Astronomical Catalog:**
  ```python
  gaia_table = Table.read('../data/fits_files/gaia_sample.fits')
  ra = gaia_table['ra']
  dec = gaia_table['dec']
  mag = gaia_table['phot_g_mean_mag']
  ```
- **Basic Scatter Plot:**
  ```python
  plt.scatter(ra, dec, s=1, c='blue')
  plt.xlabel("RA (deg)")
  plt.ylabel("Dec (deg)")
  plt.title("Position of Gaia Stars")
  ```
- **Color Mapping:**  
  - Use magnitude values to color stars:
    ```python
    plt.scatter(ra, dec, s=1, c=mag, cmap='viridis')
    plt.colorbar(label='G Magnitude')
    plt.xlabel("RA (deg)")
    plt.ylabel("Dec (deg)")
    plt.title("Gaia Stars Colored by Magnitude")
    ```
  
**4. Histograms and Distributions**  
- **Visualizing Distributions:**
  ```python
  plt.hist(mag, bins=30, color='green', edgecolor='black')
  plt.xlabel("Magnitude")
  plt.ylabel("Number of Stars")
  plt.title("Distribution of Gaia Star Magnitudes")
  ```
- **Overplotting Multiple Distributions:**
  ```python
  bright = mag[mag < 15]
  dim = mag[mag >= 15]

  plt.hist(bright, bins=20, alpha=0.5, label='Bright (<15 mag)')
  plt.hist(dim, bins=20, alpha=0.5, label='Dim (≥15 mag)')
  plt.xlabel("Magnitude")
  plt.ylabel("Count")
  plt.title("Bright vs. Dim Stars")
  plt.legend()
  ```

**5. Displaying FITS Images**  
- **Loading FITS Image:**
  ```python
  from astropy.io import fits
  hdu = fits.open('../data/fits_files/example_image.fits')
  image_data = hdu[0].data
  hdu.close()
  ```
- **Image Display with imshow:**
  ```python
  plt.imshow(image_data, origin='lower', cmap='gray', vmin=np.percentile(image_data,5), vmax=np.percentile(image_data,95))
  plt.colorbar(label='Pixel Value')
  plt.title('Astronomical Image')
  ```
- **Colormaps and Scaling:**
  - Experiment with different colormaps (`'viridis'`, `'magma'`, `'inferno'`, etc.).
  - Apply logarithmic scaling for better dynamic range.

**6. Error Bars and Annotations**  
- **Plotting Data with Error Bars:**
  ```python
  x = np.arange(0, 5, 0.5)
  y = x**2
  y_err = 0.5 * x
  plt.errorbar(x, y, yerr=y_err, fmt='o', ecolor='red', capsize=3)
  plt.xlabel("X")
  plt.ylabel("Y")
  plt.title("Data with Error Bars")
  ```
- **Annotations:**
  - Adding text or arrows to highlight features:
    ```python
    plt.plot(x, y, 'bo-')
    plt.annotate('Quadratic Behavior', xy=(3, 9), xytext=(4, 15),
                 arrowprops=dict(facecolor='black', shrink=0.05))
    ```

**7. Subplots for Multiple Figures**  
- **Multiple Plots in One Figure:**
  ```python
  fig, axes = plt.subplots(1, 2, figsize=(12, 5))
  
  axes[0].hist(mag, bins=30)
  axes[0].set_title("Magnitude Distribution")
  
  axes[1].scatter(ra, dec, s=1)
  axes[1].set_title("Star Positions")
  ```
- **Tight Layout:**
  ```python
  plt.tight_layout()
  ```

**8. Styles and Aesthetics**  
- **Changing Styles:**
  ```python
  plt.style.use('seaborn-notebook')  # Experiment with different styles
  ```
- **Save Figures:**
  ```python
  plt.savefig('star_distribution.png', dpi=300)
  ```

**9. Exercises**  
- **Exercise 1:**  
  Load `gaia_sample.fits`, plot RA vs. Dec with a color scale representing magnitude. Add a colorbar and title.
  
- **Exercise 2:**  
  Create a histogram of the RA values, and then make a subplot with RA histogram on one side and a Dec histogram on the other.
  
- **Exercise 3:**  
  Load another FITS image (if available), display it with a chosen colormap, and annotate a particularly bright region.
  
- **Challenge Exercise:**  
  Plot a 2D histogram (using `plt.hist2d`) of RA and Dec for the star sample. Experiment with different bins and color maps to see underlying structure.

**10. Summary and Next Steps**  
- **Recap:**  
  - Learned basic plotting with Matplotlib.
  - Created line plots, scatter plots, histograms, and displayed FITS images.
  - Learned about customization and subplots.
- **Next Notebook:**
  Dive deeper into astronomy-specific tasks, such as working with Astropy’s units and coordinates, so you can correctly interpret and transform astronomical data in plots.
