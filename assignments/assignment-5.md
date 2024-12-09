
# Assignment 5: Astropy Units and Coordinates

**Instructions:**  
In this assignment, you will use Astropy's `units` and `coordinates` modules to perform unit conversions, compute distances, and work with celestial coordinates. These tasks mirror common workflows in astronomical data analysis.

---

## Part 1: Working with Units

### 1. Converting Distances
- Assume the distance to a star is 12.5 $\text{pc}\$.  
  - Convert this distance to light-years.  
  - Convert the same distance to kilometers.  
  - Print both converted values with proper units.

**Hint:** Use `astropy.units` to define the distance in parsecs and convert to other units.

---

### 2. Energy and Flux Conversions
- A photon has a wavelength of 500 $\text{nm}$.  
  - Compute the photon's energy in electronvolts ($eV$).  
  - Convert the energy to joules (J).
- Suppose the flux from a star is $5 \times 10^{-13} \text{W/m}^2\$.  
  - Convert this flux to $\text{erg}/\text{cm}^2/\text{s}$.  
  - Print all results with appropriate units.

**Hint:** Use the relationship $E = \frac{hc}{\lambda}$, where $h$ is Planck's constant and $c$ is the speed of light.

---

## Part 2: Celestial Coordinates

### 1. Basic Coordinate Transformations
- A star has the following celestial coordinates in the International Celestial Reference System (ICRS):  
  - $RA$ = 10.684 $\text{degrees}$  
  - $Dec$ = -41.269 $\text{degrees}$  
- Transform these coordinates to Galactic coordinates.  
- Print the Galactic longitude ($l$) and latitude ($b$) in degrees.

**Hint:** Use `SkyCoord` from `astropy.coordinates` and its `.transform_to()` method.

---

### 2. Angular Separation Between Two Stars
- Star A: RA = $10.684 \, \text{degrees}$, Dec = $-41.269 \, \text{degrees}$
- Star B: RA = $12.345 \, \text{degrees}$, Dec = $-40.123 \, \text{degrees}$  
- Compute the angular separation between these two stars in degrees.

**Hint:** Use the `.separation()` method from `SkyCoord`.

---

## Part 3: Combining Units and Coordinates

### 1. Converting Proper Motion to Tangential Velocity
- A star has the following properties:
  - Proper motion in RA = \(100 \, \text{mas/yr}\)  
  - Proper motion in Dec = \(50 \, \text{mas/yr}\)  
  - Distance = \(50 \, \text{pc}\)  
- Compute the star's tangential velocity in \(\text{km/s}\).

**Hint:** Use the formula:  
\[
v_t = 4.74 \, \mu \, d
\]  
where \(v_t\) is the tangential velocity in \(\text{km/s}\), \(\mu\) is the total proper motion in \(\text{arcsec/yr}\), and \(d\) is the distance in parsecs.

---

### 2. Simulating an Observing Field
- Simulate 5 random stars in a \(5 \, \text{degree} \times 5 \, \text{degree}\) field centered at:  
  - RA = \(150 \, \text{degrees}\), Dec = \(2.5 \, \text{degrees}\)  
- Generate random offsets (in RA and Dec) for each star, uniformly distributed within \(\pm 2.5 \, \text{degrees}\).
- Plot these stars on a scatter plot with RA on the x-axis and Dec on the y-axis.

**Hint:** Use `numpy.random.uniform()` for generating random offsets.

---

## Part 4: Exercises and Reflection

### 1. Exercises
- Write a function `angular_diameter(distance, size)` to compute the angular diameter of an object given its distance and size.  
  - Example: The Moon has a diameter of \(3474 \, \text{km}\) and is \(384400 \, \text{km}\) away. What is its angular diameter in degrees?

### 2. Reflection
- Write a short paragraph on the importance of units and coordinate transformations in astronomy. Discuss a real-world application where these tools are essential.

---

## Submission Guidelines

- Save your code and outputs in a Jupyter notebook named `your_name_solution_5.ipynb`.
- Include comments and Markdown cells to explain each step.
- Ensure all code runs without errors.
- Submit your notebook by the due date.

---

**Good Luck!**  
By completing this assignment, you’ll gain practical experience with units and coordinate transformations, essential tools for handling astronomical data.
