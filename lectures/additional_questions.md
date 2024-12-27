

**1. Galactic Time Zones**  
The Milky Way rotates around its center such that the Sun takes approximately 225 million years to complete one orbit, known as the "Galactic Year."  

- Using the `astropy.units` module, define a new time unit called `galactic_year`.  
- Calculate how many Galactic Years have passed since the Solar System's formation (~4.6 billion years ago).  
- Bonus: If one Galactic Year is divided into 12 Galactic Months, how many Galactic Months has the Solar System existed?

---

**2. Kepler's Orbital Puzzle**  
Kepler’s Third Law states that the square of a planet's orbital period is proportional to the cube of the semi-major axis of its orbit. Assume Earth's orbital period is 1 year and semi-major axis is 1 AU.  

- Write a Python function using `scipy.optimize.curve_fit` to fit Kepler’s law to the following data for a fictional planetary system:  
  - Planet A: Orbital period = 0.5 years, Semi-major axis = 0.63 AU  
  - Planet B: Orbital period = 2 years, Semi-major axis = 1.59 AU  
  - Planet C: Orbital period = 8 years, Semi-major axis = 3.98 AU  
- Predict the semi-major axis for a planet with an orbital period of 16 years.

---

**3. Supernova Timing**  
Type Ia supernovae are standard candles used to measure cosmic distances. Their luminosity fades over time according to the formula \( L(t) = L_0 e^{-kt} \), where \( L_0 \) is the initial luminosity and \( k \) is the decay constant. Assume \( L_0 = 10^9 \) \(L_\odot\) and \( k = 0.693 \) (time in days).  

- Using `numpy` and `matplotlib`, plot the luminosity decay over 100 days.  
- Calculate how long it will take for the luminosity to drop to \( 10^6 \) \(L_\odot\).

---

**4. Exoplanetary Climate Cycles**  
An exoplanet's axis is tilted by 45°, causing extreme seasonal changes. Assume its year is 365 Earth days, and it follows a sinusoidal pattern for temperature variation:  
\[ T(t) = T_0 + A \sin\left(\frac{2\pi t}{P}\right), \]  
where \( T_0 = 15^\circ C \), \( A = 30^\circ C \), and \( P = 365 \).  

- Use Python to calculate and plot the temperature over one year.  
- What is the maximum temperature? On what day does it occur?  
- If the planet's rotation speed doubled, how would that affect the seasonal cycle?

---

**5. Star Mapping with Coordinates**  
A telescope captures data of three stars with the following equatorial coordinates:  
- Star X: RA = 12h 30m, Dec = -5°  
- Star Y: RA = 13h 45m, Dec = 10°  
- Star Z: RA = 15h 10m, Dec = 25°  

- Convert these equatorial coordinates to horizontal coordinates (altitude and azimuth) for an observer in Mumbai (latitude = 19° N) at 9 PM local time on 25th December. Use the `astropy.coordinates` module.  
- Plot the stars' positions on a polar plot to visualize their location in the sky.


# Solutions:

Here are detailed Python solutions for the questions provided:

---

**1. Galactic Time Zones**

```python
from astropy import units as u

# Define galactic_year
galactic_year = u.def_unit("galactic_year", 225 * u.Myr)

# Calculate Galactic Years since the Solar System's formation
solar_system_age = 4.6 * u.Gyr
galactic_years_passed = (solar_system_age.to(u.Myr) / (225 * u.Myr)).value
print(f"Galactic Years Passed: {galactic_years_passed:.2f}")

# Calculate Galactic Months (1 Galactic Year = 12 Galactic Months)
galactic_months_passed = galactic_years_passed * 12
print(f"Galactic Months Passed: {galactic_months_passed:.2f}")
```

---

**2. Kepler's Orbital Puzzle**

```python
import numpy as np
from scipy.optimize import curve_fit
import matplotlib.pyplot as plt

# Kepler's Third Law function
def kepler_law(a, k):
    return k * a**(3 / 2)

# Given data
semi_major_axes = np.array([0.63, 1.59, 3.98])  # AU
orbital_periods = np.array([0.5, 2, 8])  # years

# Fit the data
popt, pcov = curve_fit(kepler_law, semi_major_axes, orbital_periods)
k_opt = popt[0]
print(f"Fitted constant (k): {k_opt:.2f}")

# Predict semi-major axis for orbital period of 16 years
predicted_semi_major_axis = (16 / k_opt)**(2 / 3)
print(f"Predicted Semi-Major Axis: {predicted_semi_major_axis:.2f} AU")

# Plot
a_range = np.linspace(0.5, 5, 100)
plt.plot(a_range, kepler_law(a_range, k_opt), label="Fitted Kepler Law")
plt.scatter(semi_major_axes, orbital_periods, color="red", label="Data Points")
plt.xlabel("Semi-Major Axis (AU)")
plt.ylabel("Orbital Period (Years)")
plt.legend()
plt.show()
```

---

**3. Supernova Timing**

```python
import numpy as np
import matplotlib.pyplot as plt

# Parameters
L0 = 1e9  # Luminosity in solar luminosities
k = 0.693  # Decay constant per day
t = np.linspace(0, 100, 1000)  # Time in days

# Luminosity decay function
L = L0 * np.exp(-k * t)

# Calculate time to reach 10^6 solar luminosities
threshold_luminosity = 1e6
time_to_threshold = np.log(L0 / threshold_luminosity) / k
print(f"Time to drop to {threshold_luminosity} L☉: {time_to_threshold:.2f} days")

# Plot
plt.plot(t, L, label="Luminosity Decay")
plt.axhline(threshold_luminosity, color="red", linestyle="--", label="Threshold")
plt.xlabel("Time (days)")
plt.ylabel("Luminosity (L☉)")
plt.yscale("log")
plt.legend()
plt.show()
```

---

**4. Exoplanetary Climate Cycles**

```python
import numpy as np
import matplotlib.pyplot as plt

# Parameters
T0 = 15  # Base temperature in °C
A = 30   # Amplitude in °C
P = 365  # Period in days
t = np.linspace(0, 365, 1000)  # Time in days

# Temperature function
T = T0 + A * np.sin(2 * np.pi * t / P)

# Find maximum temperature and its day
max_temp = T0 + A
max_temp_day = np.argmax(T) * (P / len(t))
print(f"Maximum Temperature: {max_temp}°C on Day {max_temp_day:.2f}")

# Plot
plt.plot(t, T)
plt.xlabel("Day")
plt.ylabel("Temperature (°C)")
plt.title("Exoplanetary Seasonal Temperature Variation")
plt.show()
```

---

**5. Star Mapping with Coordinates**

```python
from astropy.coordinates import EarthLocation, AltAz, SkyCoord
from astropy.time import Time
import matplotlib.pyplot as plt
import numpy as np

# Observer's location: Mumbai
mumbai = EarthLocation(lat=19*u.deg, lon=72.8*u.deg, height=0*u.m)

# Observation time
time = Time('2024-12-25 15:30:00')  # Convert 9 PM local to UTC

# Star coordinates
star_coords = [
    SkyCoord(ra="12h30m00s", dec="-5d", frame="icrs"),
    SkyCoord(ra="13h45m00s", dec="10d", frame="icrs"),
    SkyCoord(ra="15h10m00s", dec="25d", frame="icrs"),
]

# Convert to AltAz frame
altaz_frame = AltAz(obstime=time, location=mumbai)
star_altaz = [coord.transform_to(altaz_frame) for coord in star_coords]

# Extract Altitude and Azimuth
altitudes = [s.alt.deg for s in star_altaz]
azimuths = [s.az.deg for s in star_altaz]

# Plot on polar coordinates
fig, ax = plt.subplots(subplot_kw={'projection': 'polar'})
for alt, az, star in zip(altitudes, azimuths, ['Star X', 'Star Y', 'Star Z']):
    ax.plot(np.deg2rad(az), 90-alt, 'o', label=star)

ax.set_theta_zero_location('N')
ax.set_theta_direction(-1)
ax.set_rlim(90, 0)
plt.legend()
plt.show()
```
