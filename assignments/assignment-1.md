
# Assignment 1: Basic Python for Astronomy

**Instructions:**  
Complete the following exercises to reinforce the Python basics you've learned today. For each question, write your code and results in a separate Jupyter notebook or script. Submit your final code and any outputs as instructed by your course organizers.

If any of the instructions are unclear, feel free to ask questions during the next session or post them on the course forum.

---

## Part 1: Python Fundamentals

1. **Variables and Arithmetic**  
   - Create a variable `star_distance_pc` that stores the distance to a star in parsecs. Assign it a value of 2.64 (the approximate distance to Sirius in parsecs).
   - Convert this distance to light years, given that 1 parsec ≈ 3.26 light years. Store this result in a new variable `star_distance_ly`.
   - Print both distances along with a brief description. For example:  
     *"Sirius is approximately X parsecs or Y light years away."*

2. **Strings and Formatting**  
   - Create a variable `star_name = "Sirius"`.
   - Print a message that includes the star’s name and distance in light years, formatted to 2 decimal places. Use an f-string for clean formatting. For example:  
     `print(f"{star_name} is {star_distance_ly:.2f} light years away.")`

3. **Lists and Loops**  
   - Create a list of star magnitudes: `magnitudes = [14.2, 13.7, 15.1, 16.0, 12.9]`.
   - Use a `for` loop to print each magnitude.  
   - Add an `if` statement inside the loop to print a message next to each magnitude indicating if the star is "Bright" if `mag < 14.5` or "Dim" otherwise.

4. **Functions**  
   - Write a function `parsec_to_ly(distance_pc)` that returns the distance in light years. Test it by passing in `star_distance_pc` and printing the result.
   - Write another function `flux_to_mag(flux, flux_zero_point=3.631e-20)` that converts a flux value to a magnitude using the formula:

$$m = -2.5 \times \log_{10}\left(\frac{\text{flux}}{\text{fluxzeropoint}}\right)$$

Test this function with a sample flux of `1e-19` and print the resulting magnitude.


---

## Part 2: Astronomically Relevant Calculations

1. **Creating a Synthetic Catalog**  
   - Suppose you have a small catalog of three stars. Create three lists:  
     - `names = ["StarA", "StarB", "StarC"]`  
     - `distances_pc = [1.3, 4.2, 0.5]` (distances in parsecs)  
     - `magnitudes = [14.2, 15.6, 13.9]`
   - Print each star’s name, distance in parsecs, converted distance in light years (use your function from above), and magnitude in a single, well-formatted print statement per star.

2. **Classifying Stars by Distance**  
   - Write a function `classify_distance(distance_pc)` that returns:
     - `"Very close"` if `distance_pc < 1`
     - `"Close"` if `1 <= distance_pc <= 5`
     - `"Far"` if `distance_pc > 5`
   - Use this function on each star in `distances_pc` and print the result.

---

## Part 3: Short Exercises and Reflection

1. **Exploring Error Handling**  
   - Intentionally try to access `magnitudes[10]` (an index that doesn't exist). What error do you get? Add a comment explaining what went wrong and how to fix it.

2. **Reflections**  
   - In a Markdown cell (no code), write a brief paragraph (3-4 sentences) on what you found most challenging or interesting in today’s exercises. Mention one thing you learned about Python that you think will be particularly useful for astronomy.

---

**Submission Guidelines:**  
- Organize your solutions in a Jupyter notebook called `your_name_solution_1.ipynb`.
- Include comments and Markdown cells where helpful to explain your approach.
- Make sure your code runs without errors.
- Submit your notebook to the course platform by the due date.

---

Good luck! If you encounter difficulties, remember to consult your notes, the provided lecture materials, or the course discussion forum.
