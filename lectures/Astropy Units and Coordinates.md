
# *Astropy Units and Coordinates*

**1. Introduction**  
- **Motivation:**  
  Astronomical measurements often require careful unit handling (e.g., mixing parsecs and light years or km/s and m/s). Similarly, dealing with different coordinate systems (equatorial, galactic) is common. Astropy makes these tasks straightforward.
- **Goals:**  
  By the end of this notebook, you will:
  - Understand how to use `astropy.units` to attach, convert, and manipulate units.
  - Use `astropy.coordinates` to represent positions on the sky.
  - Perform coordinate transformations between frames (e.g., RA/Dec to Galactic).
  - Compute angular separations between astronomical objects.

**2. Introduction to Astropy Units**  
- **Imports:**
  ```python
  import astropy.units as u
  from astropy.coordinates import SkyCoord
  import numpy as np
  ```
- **Defining Units:**
  ```python
  distance = 10 * u.parsec
  speed = 3000 * u.km/u.s
  ```
- **Unit Conversions:**
  ```python
  distance_ly = distance.to(u.lightyear)
  print("Distance in light years:", distance_ly)
  
  speed_m_s = speed.to(u.m/u.s)
  print("Speed in m/s:", speed_m_s)
  ```
- **Combining Units:**
  ```python
  time = (distance / speed).to(u.yr)
  print("Travel time:", time)
  ```

**3. Common Astronomical Unit Conversions**  
- **Angles:**
  ```python
  angle_deg = 30 * u.deg
  angle_rad = angle_deg.to(u.rad)
  print("Angle in radians:", angle_rad)
  ```
- **Flux and Magnitudes (Optional):**  
  Introduce flux density units (e.g., Jansky) and how to convert them to SI units if relevant.

**4. Introduction to Astropy Coordinates**  
- **SkyCoord Basics:**
  ```python
  from astropy.coordinates import SkyCoord
  
  # RA/Dec in degrees
  c = SkyCoord(ra=10*u.deg, dec=-5*u.deg, frame='icrs')
  print(c)
  ```
- **Frames and Transformations:**
  - **ICRS (RA/Dec), Galactic, and Ecliptic frames:**
    ```python
    gal = c.galactic
    print("Galactic coordinates:", gal.l, gal.b)
    
    ecl = c.barycentrictrueecliptic
    print("Ecliptic coordinates:", ecl.lon, ecl.lat)
    ```

**5. Creating SkyCoords from Tables**  
- **From Astropy Table or Arrays:**
  ```python
  # Suppose you have arrays of RA and Dec
  ra_array = np.array([10, 11, 12]) * u.deg
  dec_array = np.array([-5, -5.1, -4.9]) * u.deg
  coords = SkyCoord(ra=ra_array, dec=dec_array, frame='icrs')
  coords
  ```
  
- **Vectorized Transformations:**
  ```python
  gal_coords = coords.galactic
  ```

**6. Angular Separations and Matching**  
- **Measuring Separation:**
  ```python
  c1 = SkyCoord(ra=10*u.deg, dec=-5*u.deg, frame='icrs')
  c2 = SkyCoord(ra=10.5*u.deg, dec=-5.2*u.deg, frame='icrs')
  
  sep = c1.separation(c2)
  print("Separation:", sep.to(u.arcsec))
  ```
- **Practical Use Case:**
  - Given a list of star positions in RA/Dec, find the closest star to a reference point.
  ```python
  # coords defined as above
  reference = SkyCoord(ra=10*u.deg, dec=-5*u.deg, frame='icrs')
  separations = reference.separation(coords)
  closest_idx = np.argmin(separations)
  print("Closest star coordinates:", coords[closest_idx], "Separation:", separations[closest_idx])
  ```

**7. Converting Between Distance and Angles**  
- Using distances with SkyCoord to represent full 3D coordinates (if parallax or distance is known):
  ```python
  c3d = SkyCoord(ra=10*u.deg, dec=-5*u.deg, distance=100*u.pc, frame='icrs')
  print(c3d.cartesian)
  ```

**8. Exercises**  
- **Exercise 1:**  
  Create a `SkyCoord` object for Vega (RA ~ 279.2347°, Dec ~ +38.7837°) and print its Galactic coordinates.
  
- **Exercise 2:**  
  Take two sets of coordinates (e.g., from a small catalog) and compute the angular separation between each pair. Identify the pair with the smallest separation.
  
- **Exercise 3:**  
  Convert a given angle in degrees to arcseconds and print the result. Then convert it to radians.
  
- **Exercise 4 (Challenge):**  
  Suppose you have RA/Dec values for several objects in a cluster. Convert their positions to Galactic coordinates and see if they form a tighter distribution in one frame than another (just by visually inspecting a scatter plot).

**9. Summary and Next Steps**  
- **Recap:**
  - Learned how to use Astropy units to attach and convert units.
  - Learned how to work with sky coordinates, transform between frames, and measure separations.
- **Next Step:**
  Move on to using Astropy’s cosmology module or data modeling tools. Understanding units and coordinates sets the stage for more advanced computations and interpretations.
