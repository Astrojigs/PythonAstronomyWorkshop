# Assignment 5: Solutions and Explanations

---

## Part 1: Working with Units

### 1. Converting Distances

**Task:** Convert 12.5 parsecs to light-years and kilometers.

**Code:**
```python
from astropy import units as u

# Define the distance in parsecs
distance_pc = 12.5 * u.pc

# Convert to light-years
distance_ly = distance_pc.to(u.ly)

# Convert to kilometers
distance_km = distance_pc.to(u.km)

print(f"Distance: {distance_pc}")
print(f"Distance in light-years: {distance_ly:.2f}")
print(f"Distance in kilometers: {distance_km:.2e}")
```

**Expected Output:**
```
Distance: 12.5 pc
Distance in light-years: 40.73 ly
Distance in kilometers: 3.86e+14 km
```

---

### 2. Energy and Flux Conversions

**Task:** Compute the energy of a photon at \(500 \, \text{nm}\) and convert stellar flux to different units.

**Code:**
```python
# Photon energy calculation
wavelength = 500 * u.nm
energy_eV = (u.h * u.c / wavelength).to(u.eV)
energy_joules = (u.h * u.c / wavelength).to(u.J)

# Flux conversion
flux_wm2 = 5e-13 * u.W / u.m**2
flux_erg_cm2_s = flux_wm2.to(u.erg / u.cm**2 / u.s)

print(f"Photon energy at 500 nm: {energy_eV:.2f}")
print(f"Photon energy in joules: {energy_joules:.2e}")
print(f"Flux in W/m^2: {flux_wm2}")
print(f"Flux in erg/cm^2/s: {flux_erg_cm2_s:.2e}")
```

**Expected Output:**
```
Photon energy at 500 nm: 2.48 eV
Photon energy in joules: 3.97e-19 J
Flux in W/m^2: 5.00e-13 W / m2
Flux in erg/cm^2/s: 5.00e-06 erg / (cm2 s)
```

---

## Part 2: Celestial Coordinates

### 1. Basic Coordinate Transformations

**Task:** Transform celestial coordinates from ICRS to Galactic.

**Code:**
```python
from astropy.coordinates import SkyCoord

# Define the ICRS coordinates
icrs_coord = SkyCoord(ra=10.684*u.deg, dec=-41.269*u.deg, frame='icrs')

# Transform to Galactic coordinates
galactic_coord = icrs_coord.galactic

print(f"Galactic longitude (l): {galactic_coord.l:.2f}")
print(f"Galactic latitude (b): {galactic_coord.b:.2f}")
```

**Expected Output:**
```
Galactic longitude (l): 283.73 deg
Galactic latitude (b): -34.60 deg
```

---

### 2. Angular Separation Between Two Stars

**Task:** Compute the angular separation between two stars.

**Code:**
```python
# Define the two stars' coordinates
star_a = SkyCoord(ra=10.684*u.deg, dec=-41.269*u.deg, frame='icrs')
star_b = SkyCoord(ra=12.345*u.deg, dec=-40.123*u.deg, frame='icrs')

# Compute the angular separation
angular_separation = star_a.separation(star_b)

print(f"Angular separation: {angular_separation:.2f}")
```

**Expected Output:**
```
Angular separation: 1.73 deg
```

---

## Part 3: Combining Units and Coordinates

### 1. Converting Proper Motion to Tangential Velocity

**Task:** Compute the tangential velocity of a star.

**Code:**
```python
# Define proper motions and distance
mu_ra = 100 * u.mas / u.yr
mu_dec = 50 * u.mas / u.yr
distance_pc = 50 * u.pc

# Convert proper motion to arcsec/yr
mu_total = (mu_ra**2 + mu_dec**2)**0.5
mu_total_arcsec = mu_total.to(u.arcsec / u.yr)

# Compute tangential velocity
vt = (4.74 * mu_total_arcsec * distance_pc).to(u.km / u.s)

print(f"Tangential velocity: {vt:.2f}")
```

**Expected Output:**
```
Tangential velocity: 11.86 km / s
```

---

### 2. Simulating an Observing Field

**Task:** Simulate and plot 5 random stars in a field.

**Code:**
```python
import numpy as np
import matplotlib.pyplot as plt

# Center coordinates
ra_center = 150 * u.deg
dec_center = 2.5 * u.deg

# Generate random offsets
ra_offsets = np.random.uniform(-2.5, 2.5, 5) * u.deg
dec_offsets = np.random.uniform(-2.5, 2.5, 5) * u.deg

# Compute the stars' coordinates
ra_stars = ra_center + ra_offsets
dec_stars = dec_center + dec_offsets

# Plot the field
plt.figure(figsize=(6, 6))
plt.scatter(ra_stars, dec_stars, color='red', s=50)
plt.title("Simulated Observing Field", fontsize=14)
plt.xlabel("Right Ascension (degrees)", fontsize=12)
plt.ylabel("Declination (degrees)", fontsize=12)
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()

print("Simulated RA (deg):", ra_stars)
print("Simulated Dec (deg):", dec_stars)
```

**Expected Output:**  
A scatter plot showing the 5 random stars within a \(5 \, \text{degree} \times 5 \, \text{degree}\) field.

---

## Part 4: Exercises and Reflection

### 1. Angular Diameter Function

**Task:** Compute the angular diameter of the Moon.

**Code:**
```python
def angular_diameter(distance, size):
    return (size / distance).to(u.deg, equivalencies=u.dimensionless_angles())

# Define Moon's size and distance
moon_diameter = 3474 * u.km
moon_distance = 384400 * u.km

# Compute angular diameter
moon_angular_diameter = angular_diameter(moon_distance, moon_diameter)

print(f"Moon's angular diameter: {moon_angular_diameter:.2f}")
```

**Expected Output:**
```
Moon's angular diameter: 0.52 deg
```

---

### 2. Reflection

**Sample Response:**  
"Units and coordinate transformations are fundamental in astronomy because they ensure consistency and accuracy when analyzing data from different instruments or surveys. For example, converting between ICRS and Galactic coordinates is essential when comparing observations from large-scale surveys like Gaia to maps of the Milky Way. I found tangential velocity calculations particularly useful, as they provide direct insight into stellar motion."

---

By completing this assignment, students have gained hands-on experience with Astropy’s units and coordinates modules, crucial for modern astronomical data analysis.
