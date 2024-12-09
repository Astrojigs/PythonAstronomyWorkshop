
# *Basic Python for Astronomy*

**1. Introduction**  
- **Motivation:** Explain why Python is widely used in astronomy (large datasets, robust libraries, community support).  
- **What You’ll Learn Today:** Variables, data types, operators, basic input/output, control structures (if/else, loops), and functions.

**2. Getting Started with Python**  
- **The Python Interpreter and Jupyter Notebooks:**  
  - How to run cells.  
  - Difference between code cells and Markdown cells.  
- **Print Statements and Comments:**  
  ```python
  # This is a comment
  print("Hello, Astronomy!")
  ```

**3. Variables and Data Types**  
- **Variables:**  
  - Assigning variables, variable naming rules.  
  ```python
  star_name = "Sirius"
  star_distance = 2.64  # parsecs
  ```
- **Common Data Types:**  
  - Integers, floats, strings, and booleans.
  - Introduce simple type checking.
  ```python
  type(star_name), type(star_distance)
  ```
- **Basic Arithmetic:**  
  - Addition, subtraction, multiplication, division, and exponents.
  ```python
  # Convert distance in parsecs to light years (1 parsec ~ 3.26 light years)
  star_distance_ly = star_distance * 3.26
  print("Sirius is approximately", star_distance_ly, "light years away.")
  ```

**4. Strings and Simple Manipulations**  
- **String Methods:**  
  - `.lower()`, `.upper()`, `.replace()`, `.format()`, and f-strings.
  ```python
  print(f"{star_name} is {star_distance_ly:.2f} light years away.")
  ```
- **Why important?**  
  - Astronomers often parse filenames, object names, or logs of observations.

**5. Lists and Basic Collections**  
- **Lists:**  
  - Creating, indexing, slicing.
  ```python
  magnitudes = [14.2, 15.6, 13.9, 14.7]
  print("First star magnitude:", magnitudes[0])
  ```
- **Modifying Lists:**  
  - Append, insert, remove elements.
  ```python
  magnitudes.append(14.5)
  ```
- **Iterating over lists:**  
  ```python
  for mag in magnitudes:
      print(mag)
  ```

**6. Control Flow: Conditionals and Loops**  
- **Conditionals (if/else):**  
  ```python
  if star_distance < 3:
      print(f"{star_name} is relatively close.")
  else:
      print(f"{star_name} is a bit farther away.")
  ```
- **For Loops:**  
  - Iterating over lists of star names, magnitudes, etc.
- **While Loops:**  
  - Use sparingly, but show a simple example.

**7. Basic Functions**  
- **Why Functions?:**  
  - Reusability and clarity.
- **Defining a Function:**  
  ```python
  def parsec_to_ly(distance_parsec):
      return distance_parsec * 3.26

  distance_ly = parsec_to_ly(2.64)
  print(distance_ly)
  ```
- **Docstrings and Comments:**
  - Explain the importance of documenting what the function does.

**8. Simple Astronomy-Driven Examples**  
- **Example 1: Converting Between Units**  
  - A function to convert parsecs to light years or AU, or to convert brightness from flux to magnitude:
    ```python
    import math

    def flux_to_magnitude(flux, flux_zero_point=3.631e-20):  # arbitrary zero-point
        return -2.5 * math.log10(flux / flux_zero_point)

    test_flux = 1e-19
    print("Magnitude:", flux_to_magnitude(test_flux))
    ```
- **Example 2: Compute the Angular Size of an Object**  
  - Given a physical size and a distance:
    ```python
    # Angular size in radians ~ size / distance (for small angles)
    # Convert to arcseconds: 1 rad ~ 206265 arcsec
    def angular_size_physical(size_m, distance_m):
        # If size and distance are in the same units (e.g., meters)
        angular_radians = size_m / distance_m
        return angular_radians * 206265  # arcseconds

    # Example: A planet with diameter ~1.4e7 m seen from 1e11 m away
    size_arcsec = angular_size_physical(1.4e7, 1e11)
    print(f"Angular size: {size_arcsec:.2f} arcseconds")
    ```

**9. Common Errors and Debugging Tips**  
- **Syntax Errors vs. Runtime Errors:**  
  - Show an example and how to interpret error messages.
- **Print Statements for Debugging:**
  ```python
  # Example: print intermediate variables to ensure code logic is correct
  ```

**10. Exercises**  
- **Exercise 1:**  
  Create a list of star distances in parsecs: `[1.3, 4.2, 0.5, 2.7]`.  
  Use a `for` loop to convert each distance to light years and print a message for each star:
  ```python
  distances_pc = [1.3, 4.2, 0.5, 2.7]
  # Your code here
  ```

- **Exercise 2:**  
  Write a function that takes a star’s distance in parsecs and returns a string stating whether it’s “very close” (< 1 pc), “close” (1–5 pc), or “far” (> 5 pc). Test this function on a few values.
  ```python
  def classify_distance(distance_pc):
      # Your code here
      pass
  ```

- **Exercise 3:**  
  Given a list of star magnitudes `[14.2, 13.7, 15.1, 16.0]`, use an `if`/`else` statement in a loop to print whether each star is “Bright” if `mag < 14.5` or “Dim” otherwise.

**(Optional) Advanced Challenge:**  
- Write a function that given a star’s apparent magnitude and distance in parsecs, estimates its absolute magnitude using the distance modulus formula:
  \[
  M = m - 5 \log_{10}(d) + 5
  \]
  Test this function on a few star magnitudes and distances.
