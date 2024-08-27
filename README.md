# Newton Interpolation Numerical Method Implementation in Python

This repository contains a Python implementation of Newton Interpolation for estimating the value of a function at a given interpolating point based on a set of data points. The code reads the data points from an Excel file (`datai.xls`), computes the divided differences, performs the interpolation, and plots the results.

## Table of Contents
- [Newton Interpolation Theory](#newton-interpolation-theory)
- [Dependencies](#dependencies)
- [Installation](#installation)
- [Usage](#usage)
- [Code Explanation](#code-explanation)
- [Example](#example)
- [Files in the Repository](#files-in-the-repository)
- [Input Parameters](#input-parameters)
- [Troubleshooting](#troubleshooting)
- [Author](#author)

## Newton Interpolation Theory
Newton's Interpolation is a method of polynomial interpolation where the interpolating polynomial is expressed in the Newton form. It is particularly useful for data sets where the points are not evenly spaced. The method relies on the computation of divided differences.

**Steps:**
1. Compute the divided differences based on the given data points.
2. Formulate the Newton interpolating polynomial using the computed divided differences.
3. Evaluate the polynomial at the desired interpolating point.

## Dependencies
To run this code, you need the following libraries:
- `numpy`
- `xlrd`
- `matplotlib`

## Installation
To install the required libraries, you can use `pip`:
```sh
pip install numpy xlrd matplotlib
```

## Usage
1. Clone the repository.
2. Ensure the script and the Excel file (`datai.xls`) are in the same directory.
3. Run the script using Python:
    ```sh
    python newton_interpolation.py
    ```
4. Provide the required input when prompted:
    - Enter the interpolating point.

## Code Explanation
The code begins by importing the necessary libraries and taking the interpolating point as input. It reads the data points from the Excel file and computes the divided differences to construct the Newton interpolating polynomial. The polynomial is then evaluated at the interpolating point, and the results are plotted.

Below is a snippet from the code illustrating the main logic:

```python
import numpy as np
import xlrd
from matplotlib import pyplot as plt

# Taking necessary input values from keyboard
X = float(input('Enter the interpolating point: '))

# Reading data from excel file
loc = ('datai.xls')
wb = xlrd.open_workbook(loc)
sheet = wb.sheet_by_index(0)

n = sheet.ncols - 1
x = np.zeros([n])
y = np.zeros([n])
F = np.zeros([n-1, n])

# Creating vectors from the data 
for i in range(1, sheet.ncols):
    x[i-1] = sheet.cell_value(0, i)
    y[i-1] = sheet.cell_value(1, i)

# Computing divided difference
y1 = y    
for j in range(1, n):
    for i in range(j+1, n+1):
        F[j-1, i-1] = (y1[i-1] - y1[i-2]) / (x[i-1] - x[i-1-j])
    y1 = F[j-1, :]

# Initializing variables    
Y = 0
summation = 0
b = np.zeros([n])
b[0] = 1

# Computing the function value at the interpolating point
for j in range(n):
    if j == 0:
        summation = y1[j]
    
    if j > 0:
        a = F[j-1, j]
        for i in range(j):
            b[i+1] = (X - x[i]) * b[i]
            
        summation = a * b[i + 1]
    
    Y = summation + Y
    
print('The interpolating result at x = ' + str(Y))

plt.figure(1)
plt.plot(x, y) 
plt.plot(X, Y, 'o')
plt.xlabel('Values of x')
plt.ylabel('Values of y')
plt.title('Graphical verification of the interpolation result')
plt.legend(['Measured', 'Estimated / Interpolated'], loc='best')
plt.show()
```

The code completes by plotting the original data points and the interpolated point using `matplotlib`.

## Example
Below is an example of how to use the script:

1. Prepare the `datai.xls` file with the data points. The first row should contain the \( x \)-values and the second row should contain the corresponding \( y \)-values.
2. **Run the script**:
    ```sh
    python newton_interpolation.py
    ```

3. **Enter the input value**:
    ```
    Enter the interpolating point: 2.5
    ```

4. **Output**:
    - The script will compute the interpolated value at the specified point and plot the original data points along with the interpolated point.

## Files in the Repository
- `newton_interpolation.py`: The main script for performing Newton Interpolation.
- `datai.xls`: Excel file from which the data points are read.

## Input Parameters
The initial input data is expected to be in the form of two rows within the `datai.xls` file:
- First row: \( x \)-values
- Second row: \( y \)-values

## Troubleshooting
1. **Excel File**: Ensure that the input data is correctly formatted and placed in the `datai.xls` file.
2. **Interpolating Point**: Ensure the interpolating point falls within the range of the input \( x \)-values.
3. **Python Version**: This script is compatible with Python 3. Ensure you have Python 3 installed.

## Author
Script created by sudipto3331.

---

This documentation should guide you through understanding, installing, and using the Newton Interpolation script. For further issues or feature requests, please open an issue in the repository on GitHub. Feel free to contribute by creating issues and submitting pull requests. Happy coding!
