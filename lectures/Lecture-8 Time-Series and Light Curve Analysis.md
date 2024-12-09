# *Time-Series and Light Curve Analysis*

**1. Introduction**  
- **Motivation:**  
  Many astronomical objects vary with time: variable stars, exoplanets transiting their host stars, pulsars, supernova light curves. To study these phenomena, astronomers analyze time-series data—measurements taken at multiple time points.
- **Goals:**  
  By the end of this notebook, you will:
  - Understand the basics of handling time-series data in Python.
  - Use Astropy’s `Time` and `TimeSeries` objects.
  - Explore basic period-finding techniques (e.g., Lomb-Scargle periodograms).
  - Visualize and phase-fold light curves to reveal periodic signals.

**2. Getting Started with Astropy Time and TimeSeries**  
- **Imports:**
  ```python
  import numpy as np
  import matplotlib.pyplot as plt
  from astropy.time import Time
  from astropy.timeseries import TimeSeries, LombScargle
  %matplotlib inline
  ```
  
- **Creating a Time Object:**
  ```python
  # Suppose we have observation times (in JD)
  jd = np.array([2457000.0, 2457001.1, 2457002.05, ...])
  t = Time(jd, format='jd', scale='utc')
  print(t)
  ```

**3. Creating and Inspecting a TimeSeries**  
- **Making a TimeSeries from Data:**
  ```python
  # Suppose we have flux measurements for a star at these times
  flux = np.random.normal(1.0, 0.05, size=len(jd))
  ts = TimeSeries(time=t, data={'flux': flux})
  ts
  ```
  
- **Inspecting the TimeSeries:**
  ```python
  ts.info()
  ts.colnames
  ```
  
- **Basic Plot:**
  ```python
  plt.plot(ts.time.jd, ts['flux'], 'o')
  plt.xlabel("Time (JD)")
  plt.ylabel("Flux")
  plt.title("Light Curve of a Variable Star")
  ```

**4. Handling Irregularly Spaced Data**  
- In astronomy, data are often unevenly sampled.  
- Show how TimeSeries can handle this gracefully and how interpolation might help if needed.

**5. Periodic Signals and the Lomb-Scargle Periodogram**  
- **Concept:**
  The Lomb-Scargle periodogram is commonly used to detect periodicity in unevenly sampled time-series data, such as variable stars or exoplanet transit signals.
  
- **Computing a Periodogram:**
  ```python
  frequency, power = LombScargle(ts.time.jd, ts['flux']).autopower()
  plt.plot(frequency, power)
  plt.xlabel("Frequency (1/day)")
  plt.ylabel("Power")
  plt.title("Lomb-Scargle Periodogram")
  ```
  
- **Identify Peak Frequency:**
  ```python
  best_freq = frequency[np.argmax(power)]
  best_period = 1 / best_freq
  print("Best period:", best_period, "days")
  ```

**6. Phase-Folding the Light Curve**  
- **Phase-Folding:**
  Once you identify a period, you can “fold” the time-series so that all points line up in phase space, revealing the underlying periodic behavior more clearly.
  
  ```python
  phase = ((ts.time.jd - ts.time.jd[0]) / best_period) % 1
  plt.scatter(phase, ts['flux'], s=10)
  plt.xlabel("Phase")
  plt.ylabel("Flux")
  plt.title("Phase-Folded Light Curve")
  ```
  
- Discuss how this technique makes periodic patterns more evident.

**7. Example: Synthetic Transiting Exoplanet Signal**  
- **Generate Synthetic Transit Data:**
  ```python
  np.random.seed(42)
  n_points = 200
  t_jd = np.linspace(2457000, 2457000+10, n_points)
  # True period ~ 2 days, shallow dip
  true_period = 2.0
  phase = ((t_jd - t_jd[0]) / true_period) % 1
  flux_synthetic = 1.0 - 0.02*(phase < 0.03)  # Transit dip in first 3% of phase
  flux_synthetic += np.random.normal(0, 0.005, size=n_points)
  
  ts_exo = TimeSeries(time=Time(t_jd, format='jd', scale='utc'), data={'flux': flux_synthetic})
  plt.plot(ts_exo.time.jd, ts_exo['flux'], 'o')
  plt.title("Synthetic Transit Light Curve")
  plt.xlabel("JD")
  plt.ylabel("Flux")
  ```

- **Period Search:**
  ```python
  freq_exo, power_exo = LombScargle(ts_exo.time.jd, ts_exo['flux']).autopower()
  best_freq_exo = freq_exo[np.argmax(power_exo)]
  best_period_exo = 1 / best_freq_exo
  print("Estimated period:", best_period_exo, "days")
  ```

- **Phase-Fold and Plot:**
  ```python
  phase_exo = ((ts_exo.time.jd - ts_exo.time.jd[0]) / best_period_exo) % 1
  plt.scatter(phase_exo, ts_exo['flux'], s=10)
  plt.axvspan(0, 0.03, color='red', alpha=0.3, label='Transit')
  plt.xlabel("Phase")
  plt.ylabel("Flux")
  plt.legend()
  plt.title("Phase-Folded Synthetic Exoplanet Transit")
  ```

**8. Exercises**  
- **Exercise 1:**
  Load a provided time-series dataset of a known variable star. Compute its Lomb-Scargle periodogram and identify the principal period. Phase-fold the light curve and plot the result.
  
- **Exercise 2:**
  Given a time-series with two known frequencies (e.g., a star pulsating with two modes), try to identify both signals. Can you see multiple peaks in the periodogram?
  
- **Exercise 3:**
  Simulate your own periodic signal (e.g., a sine wave), add noise, and see if you can recover the period using Lomb-Scargle. Experiment with different noise levels.

- **Challenge Exercise:**  
  Implement a simple algorithm to window or bin the phase-folded light curve to reduce noise and highlight the transit or periodic signal more clearly.

**9. Summary and Next Steps**  
- **Recap:**
  - Learned how to use `Time` and `TimeSeries` for handling temporal data.
  - Computed Lomb-Scargle periodograms to detect periodic signals.
  - Performed phase-folding to clarify periodicity.
- **Next Step:**
  The final project notebook will tie together all skills: reading real data, converting coordinates, analyzing magnitudes, fitting models, and maybe even folding a time-series. Students will tackle a more realistic, end-to-end astronomical analysis.
