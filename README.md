# Python Astronomy Workshop

## Overview

This repository contains materials and exercises for a hands-on introductory workshop on using Python and Astropy for astronomical calculations, data analysis, and visualization. It is ideal for undergraduate and graduate students, as well as astronomy enthusiasts, who are eager to learn Python-based techniques for exploring and interpreting astronomical data.

---

## Repository Structure

The repository is organized as follows:

```
├─ assignments/          # Assignments for participants
│  ├─ assignment-1.md
│  ├─ assignment-2.md
│  ├─ ...
│
├─ lectures/             # Lecture notes in Markdown
│  ├─ Lecture-1 Basic Python for Astronomy.md
│  ├─ Lecture-2 Numpy and Pandas for Astronomy.md
│  ├─ ...
│
├─ solutions/            # Solutions to assignments
│  ├─ solution-1.md
│  ├─ solution-2.md
│  ├─ ...
│
├─ requirements.txt      # Python dependencies for the workshop
├─ README.md             # This file
├─ LICENSE               # License for the repository
```

---

## Getting Started

To get started with this workshop, follow these steps:

### 1. Clone the Repository
Clone this repository to your local machine using the following command:
```bash
git clone https://github.com/Astrojigs/PythonAstronomyWorkshop.git
```

### 2. Set Up Your Environment
This workshop requires Python 3.8 or later. Use `pip` to create a Python environment and install the necessary dependencies.

#### Step 1: Create a Virtual Environment
```bash
python -m venv astronomy-env
source astronomy-env/bin/activate  # On Windows: astronomy-env\Scripts\activate
```

#### Step 2: Install Dependencies
Navigate to the cloned repository and install the required Python packages:
```bash
pip install -r requirements.txt
```

### 3. Launch Jupyter Notebook
Ensure Jupyter is installed and launch it:
```bash
pip install jupyter
jupyter notebook
```
Navigate to the appropriate folder and start working on the lectures or assignments.

---

## Workshop Topics

### Lectures
The workshop covers the following topics:
1. **Basic Python for Astronomy**: Introduction to Python syntax, loops, functions, and basics of working with data.
2. **Numpy and Pandas for Astronomy**: Numerical computations, data handling, and table manipulation.
3. **Astropy Tables and FITS Files**: Reading and writing FITS files, using Astropy Tables for catalog manipulation.
4. **Visualization for Astronomy**: Creating scatter plots, histograms, and displaying FITS images with Matplotlib.
5. **Astropy Units and Coordinates**: Using units, handling celestial coordinates, and performing transformations.
6. **Cosmology with Astropy**: Working with cosmological models and computing distances, redshifts, and lookback times.
7. **Fitting and Modeling**: Curve fitting and modeling astronomical data.
8. **Time-Series and Light Curve Analysis**: Analyzing periodic signals and working with time-series data.

### Assignments
Assignments are provided for each lecture topic to reinforce the concepts taught. Each assignment has an accompanying solution for reference.

---

## Contributing

Contributions to this workshop are welcome! If you'd like to contribute, please:
1. Fork the repository.
2. Create a new branch for your changes.
3. Submit a pull request with a detailed description of your changes.

---

## License

This project is licensed under the MIT License. See the `LICENSE` file for more details.

---

## Contact

For questions or feedback, please open an issue on GitHub or contact the workshop organizer directly.

Happy coding and clear skies! 🚀✨

---

