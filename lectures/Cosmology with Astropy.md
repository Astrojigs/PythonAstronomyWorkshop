
# *Cosmology with Astropy*

**1. Introduction**  
- **Motivation:**  
  Cosmology connects observable quantities like redshift to fundamental parameters of the universe’s expansion. Astropy’s `cosmology` module enables straightforward computations of distances, ages, and other cosmological properties without delving into the low-level math every time.
- **Goals:**  
  By the end of this notebook, you will:
  - Understand how to select and use built-in cosmologies (e.g., Planck15, WMAP9).
  - Compute luminosity distance, angular diameter distance, and lookback time for given redshifts.
  - Explore differences between cosmological models.
  
**2. Getting Started with the Astropy Cosmology Module**  
- **Imports:**
  ```python
  import numpy as np
  import matplotlib.pyplot as plt
  from astropy.cosmology import Planck15, WMAP9, FlatLambdaCDM
  import astropy.units as u
  ```
- **What Are Cosmologies?**  
  Explain that each cosmology object encodes values like H0, Ω_matter, Ω_lambda, etc.

**3. Using Built-In Cosmologies**  
- **Inspecting Planck15:**
  ```python
  print(Planck15)
  print("Hubble parameter (H0):", Planck15.H0)
  print("Matter density (Om0):", Planck15.Om0)
  ```
- **Computing Distances:**
  ```python
  z = 0.5
  lum_dist = Planck15.luminosity_distance(z)
  ang_dist = Planck15.angular_diameter_distance(z)
  lookback = Planck15.lookback_time(z)
  
  print(f"Luminosity distance at z={z}:", lum_dist)
  print(f"Angular diameter distance at z={z}:", ang_dist)
  print(f"Lookback time at z={z}:", lookback)
  ```

**4. Comparing Different Cosmologies**  
- **Compute Distances for a Range of Redshifts:**
  ```python
  z_values = np.linspace(0, 2, 50)
  ld_planck = Planck15.luminosity_distance(z_values)
  ld_wmap = WMAP9.luminosity_distance(z_values)
  ```
- **Plotting the Differences:**
  ```python
  plt.plot(z_values, ld_planck, label='Planck15')
  plt.plot(z_values, ld_wmap, label='WMAP9', linestyle='--')
  plt.xlabel("Redshift (z)")
  plt.ylabel("Luminosity Distance")
  plt.title("Comparing Cosmologies")
  plt.legend()
  plt.show()
  ```

**5. Custom Cosmologies**  
- **Defining Your Own Cosmology:**
  ```python
  # Example: A flat ΛCDM cosmology with custom parameters
  custom_cosmo = FlatLambdaCDM(H0=70*u.km/u.s/u.Mpc, Om0=0.3)
  print(custom_cosmo)
  ```
- **Compute Distances with Custom Cosmology:**
  ```python
  ld_custom = custom_cosmo.luminosity_distance(z_values)
  plt.plot(z_values, ld_planck, label='Planck15')
  plt.plot(z_values, ld_custom, label='Custom Cosmology')
  plt.xlabel("Redshift (z)")
  plt.ylabel("Luminosity Distance")
  plt.title("Planck15 vs. Custom Cosmology")
  plt.legend()
  plt.show()
  ```

**6. Realistic Applications**  
- **Example: Estimating Absolute Magnitude from Redshift and Apparent Magnitude**
  ```python
  # Suppose we have a supernova observed at z=0.5 with an apparent magnitude m
  m = 24.0
  distance = Planck15.luminosity_distance(0.5)
  # Distance modulus: μ = 5*log10(D/10pc)
  distance_modulus = 5 * np.log10((distance.to(u.pc).value)/10)
  M = m - distance_modulus
  print("Absolute magnitude:", M)
  ```

- **Check Variation with Different Cosmologies:**
  Compare the absolute magnitude using WMAP9 and Planck15 to illustrate how cosmological assumptions influence derived distances and intrinsic properties.

**7. Exercises**  
- **Exercise 1:**  
  For a given redshift `z=1.0`, compute the lookback time, luminosity distance, and angular diameter distance for both Planck15 and WMAP9. Which cosmology predicts a larger distance at z=1.0?
  
- **Exercise 2:**  
  Create a plot of lookback time vs. redshift for three cosmologies (Planck15, WMAP9, and a custom one). Identify the redshift range where they differ the most.
  
- **Exercise 3:**  
  Suppose you have an apparent magnitude `m=25` for a supernova at `z=0.7`. Compute its absolute magnitude under Planck15 and under your custom cosmology. Comment on the difference.
  
- **Challenge Exercise:**  
  Explore how changing H0 in a FlatLambdaCDM cosmology affects the luminosity distance–redshift relation. Plot results for H0 = 67, 70, and 73 km/s/Mpc and discuss the implications.

**8. Summary and Next Steps**  
- **Recap:**
  - Introduced Astropy cosmologies and how to compute cosmic distances and times.
  - Explored differences between published cosmological parameters and custom models.
- **Next Step:**
  Move toward modeling and fitting data in astronomy, using `scipy.optimize` or `astropy.modeling` to extract physical parameters from observational data.

