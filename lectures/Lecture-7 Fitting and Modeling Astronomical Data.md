
# *Fitting and Modeling Astronomical Data*

**1. Introduction**  
- **Motivation:**  
  In astronomy, we often measure quantities (fluxes, magnitudes, positions, velocities) and compare them to theoretical models to derive physical parameters (e.g., temperature, luminosity). Fitting allows us to find the best parameters that make the model agree with observed data.
- **Goals:**  
  By the end of this notebook, you will:
  - Understand the basics of model fitting and the least-squares optimization.
  - Use `scipy.optimize.curve_fit` to fit simple functions to data.
  - Learn how to estimate uncertainties in fitted parameters.
  - Explore `astropy.modeling` for more advanced model fitting scenarios.

**2. Introduction to Model Fitting**  
- **Concept of Residuals and Chi-Squared:**  
  Explain that the best-fit parameters minimize the sum of squared residuals (observed - model).
- **Imports:**
  ```python
  import numpy as np
  import matplotlib.pyplot as plt
  from scipy.optimize import curve_fit
  from astropy.modeling import models, fitting
  %matplotlib inline
  ```

**3. A Simple Example: Polynomial Fitting**  
- **Synthetic Data Generation:**
  ```python
  x = np.linspace(0, 10, 50)
  # True relationship: y = 3*x + 2 + noise
  y_true = 3*x + 2
  np.random.seed(42)
  y_obs = y_true + np.random.normal(0, 2, size=len(x))
  
  plt.scatter(x, y_obs, label='Data')
  plt.plot(x, y_true, 'r--', label='True Relationship')
  plt.title("Synthetic Data")
  plt.xlabel("x")
  plt.ylabel("y")
  plt.legend()
  ```

- **curve_fit:**
  ```python
  def linear_model(x, a, b):
      return a*x + b

  popt, pcov = curve_fit(linear_model, x, y_obs)
  a_fit, b_fit = popt
  a_err, b_err = np.sqrt(np.diag(pcov))
  
  print("Fitted slope:", a_fit, "+/-", a_err)
  print("Fitted intercept:", b_fit, "+/-", b_err)

  plt.scatter(x, y_obs, label='Data')
  plt.plot(x, linear_model(x, *popt), 'g-', label='Best Fit')
  plt.legend()
  plt.title("Fitted Linear Model")
  ```

**4. Handling Measurement Uncertainties**  
- **Weighting Data Points:**
  If uncertainties σ are known for each data point:
  ```python
  # Assume 2.0 is the standard deviation of uncertainty for all points
  sigma = np.full_like(x, 2.0)
  popt_w, pcov_w = curve_fit(linear_model, x, y_obs, sigma=sigma)
  ```
- Discuss how including σ ensures that points with smaller uncertainty have more influence on the fit.

**5. Fitting an Astronomical Function: Blackbody Curve Example**  
- **Context:**
  A blackbody spectrum describes the flux distribution of a star’s emission. By fitting a blackbody curve to observed fluxes at different wavelengths, one can estimate the star’s temperature.

- **Defining a Blackbody Model:**
  ```python
  from astropy import units as u
  from astropy.modeling.blackbody import blackbody_lambda

  def blackbody_model(wavelength, T, scale):
      # wavelength in meters, T in K
      # scale is a normalization factor for flux
      # convert wavelength to a Quantity
      wave_q = wavelength * u.m
      return scale * blackbody_lambda(wave_q, T*u.K).value
  ```

- **Synthetic Blackbody Data:**
  ```python
  wavelengths = np.linspace(100e-9, 2000e-9, 50)  # 100 nm to 2000 nm
  T_true = 6000
  scale_true = 1e-9
  flux_true = blackbody_model(wavelengths, T_true, scale_true)

  # Add noise
  flux_obs = flux_true + np.random.normal(0, 1e-10, size=len(flux_true))

  plt.plot(wavelengths*1e9, flux_obs, 'o', label='Observed')
  plt.plot(wavelengths*1e9, flux_true, 'r--', label='True Model')
  plt.xlabel("Wavelength (nm)")
  plt.ylabel("Flux")
  plt.title("Synthetic Blackbody Data")
  plt.legend()
  ```

- **Fitting the Blackbody Curve:**
  ```python
  popt_bb, pcov_bb = curve_fit(blackbody_model, wavelengths, flux_obs, p0=[5000, 1e-9])
  T_fit, scale_fit = popt_bb
  T_err, scale_err = np.sqrt(np.diag(pcov_bb))
  
  print("Fitted Temperature:", T_fit, "+/-", T_err, "K")
  print("Fitted Scale:", scale_fit, "+/-", scale_err)

  plt.plot(wavelengths*1e9, flux_obs, 'o', label='Data')
  plt.plot(wavelengths*1e9, blackbody_model(wavelengths, T_fit, scale_fit), 'g-', label='Best Fit')
  plt.xlabel("Wavelength (nm)")
  plt.ylabel("Flux")
  plt.title("Fitted Blackbody Curve")
  plt.legend()
  ```

**6. Using Astropy Modeling**  
- **Astropy Model Fitting Framework:**
  ```python
  # Fit a polynomial with astropy.modeling
  from astropy.modeling import models, fitting

  poly_init = models.Polynomial1D(degree=1)
  fitter = fitting.LinearLSQFitter()
  poly_fit = fitter(poly_init, x, y_obs)
  print(poly_fit)
  
  plt.scatter(x, y_obs, label='Data')
  plt.plot(x, poly_fit(x), 'm-', label='Astropy Fit')
  plt.legend()
  ```

- **Advantages of Astropy Modeling:**
  Discuss the availability of built-in model classes and flexible fitters (Levenberg-Marquardt, Simplex, etc.).

**7. Exercises**  
- **Exercise 1:**  
  Generate synthetic data from a quadratic function (e.g., y = 2x² + 3x + 1), add noise, and fit it with `curve_fit` to recover the parameters.
  
- **Exercise 2:**  
  Load a small dataset of star fluxes at different wavelengths (if provided). Attempt to fit a blackbody curve and estimate the star’s temperature. Compare to a known value.
  
- **Exercise 3:**  
  Use `astropy.modeling` to fit a Gaussian profile to a synthetic emission line. Adjust parameters and see how uncertainties affect the fit.

- **Challenge Exercise:**  
  Implement a chi-squared calculation for your fitted model and see how the reduced chi-squared changes if you artificially inflate or reduce the assumed uncertainties.

**8. Summary and Next Steps**  
- **Recap:**
  - Learned how to use `curve_fit` and `astropy.modeling` for fitting.
  - Extracted physical parameters from fits (e.g., temperature from a blackbody curve).
  - Understood the importance of uncertainties and weighting.
- **Next Step:**
  If you plan on the optional time-series analysis or move directly to the final project, you now have all the basic tools to deal with data, visualize it, handle units, coordinates, and fit models.
