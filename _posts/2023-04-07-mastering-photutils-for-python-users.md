---
layout: post
title: Mastering Photutils
subtitle: An In-Depth Guide for Python Users
cover-img: /assets/img/night-sky.jpg
thumbnail-img: /assets/img/photutils.png
share-img: /assets/img/night-sky.jpg
tags: [python, image processing, astronomy]
---

> "Astronomy compels the soul to look upwards and leads us from this world to another."
>
> _Plato_
  
## Table of Contents

- [Table of Contents](#table-of-contents)
- [1. Introduction to Photutils](#1-introduction-to-photutils)
- [2. Installation and Basic Configuration](#2-installation-and-basic-configuration)
- [3. Generating a FITS Image with a Point Spread Function (PSF)](#3-generating-a-fits-image-with-a-point-spread-function-psf)
- [4. Source Detection](#4-source-detection)
- [5. Aperture Photometry](#5-aperture-photometry)
- [6. PSF Photometry](#6-psf-photometry)
- [7. Solving Equations](#7-solving-equations)
- [8. Linear Algebra](#8-linear-algebra)
- [9. Discrete Mathematics](#9-discrete-mathematics)
- [10. Physics and Mechanics](#10-physics-and-mechanics)
- [11. Plotting](#11-plotting)
- [12. Code Generation and Optimization](#12-code-generation-and-optimization)
- [13. Frequently Asked Questions](#13-frequently-asked-questions)
- [14. Conclusion](#14-conclusion)
- [15. External Resources](#15-external-resources)

<a name="introduction"></a>

## 1. Introduction to Photutils

Photutils is a powerful Python library for performing photometry and image processing tasks on astronomical data. It provides a wide range of tools for background estimation, source detection, aperture photometry, point spread function (PSF) photometry, and image segmentation. With Photutils, you can extract valuable information from your astronomical images and improve your data analysis workflow.

Before diving in, let's address a common question:

- **Q: Why should I choose Photutils over other image processing libraries?**
- A: Photutils is specifically designed for astronomical data and provides specialized tools and functions tailored for this purpose. Additionally, it is part of the Astropy Project, ensuring seamless integration with other Astropy-affiliated packages.

Now, let's embark on our journey with Photutils!

<a name="installation"></a>

## 2. Installation and Basic Configuration

First, we need to install Photutils. You can do so using pip:

```bash
pip install photutils
```

Now, let's import Photutils and other necessary libraries:

```python
import numpy as np
import matplotlib.pyplot as plt
from astropy.io import fits
from astropy.modeling.functional_models import Gaussian2D
from astropy.stats import gaussian_sigma_to_fwhm
import photutils

```

<a name="generating-fits"></a>

## 3. Generating a FITS Image with a Point Spread Function (PSF)

Before we start using Photutils, we need to create a FITS image with a point spread function (PSF) that can be processed by Photutils. A point spread function is the response of an imaging system to a point source. In this section, we will generate a synthetic image with a Gaussian PSF.

First, let's create a Gaussian PSF using the astropy library:

```python
def create_gaussian_psf(x_dim, y_dim, x_mean, y_mean, x_stddev, y_stddev):
    y, x = np.mgrid[0:y_dim, 0:x_dim]
    gaussian = Gaussian2D(amplitude=1, x_mean=x_mean, y_mean=y_mean,
                          x_stddev=x_stddev, y_stddev=y_stddev)
    return gaussian(x, y)

psf = create_gaussian_psf(x_dim=51, y_dim=51, x_mean=25, y_mean=25,
                          x_stddev=3, y_stddev=3)

```

Now, let's create a synthetic image containing the Gaussian PSF and save it as a FITS file using the astropy.io.fits module:

```python
from astropy.io import fits

def create_fits_image(data, output_filename):
    hdu = fits.PrimaryHDU(data)
    hdu.writeto(output_filename, overwrite=True)

create_fits_image(psf, "gaussian_psf.fits")

```

With the synthetic image saved as a FITS file, we can now proceed to process it with Photutils.

<a name="source-detection"></a>

## 4. Source Detection

Photutils provides various source detection algorithms, such as the DAOStarFinder, IRAFStarFinder, and find_peaks functions. In this example, we will use the DAOStarFinder algorithm to detect sources in our synthetic Gaussian PSF image:

```python
from photutils.detection import DAOStarFinder

# Load the synthetic image
image_data = fits.getdata("gaussian_psf.fits")

# Initialize the DAOStarFinder with a threshold of 5 times the standard deviation
daofind = DAOStarFinder(threshold=5. * np.std(image_data), fwhm=3.)

# Detect sources
sources = daofind(image_data)

# Print the number of detected sources
print(f"Number of sources detected: {len(sources)}")


```

<a name="aperture-photometry"></a>

## 5. Aperture Photometry

Aperture photometry is a technique for measuring the brightness of a celestial object within a defined aperture. Photutils provides various aperture classes, including CircularAperture, CircularAnnulus, and RectangularAperture, to perform aperture photometry on your data. In this example, we will use the CircularAperture class to measure the total flux within a circular aperture centered on the detected sources.

First, let's import the necessary modules and classes:

```python
from photutils.aperture import CircularAperture
from photutils.aperture import aperture_photometry

```

Now, let's create a CircularAperture object with a radius of 5 pixels centered on the detected sources:

```python
positions = np.transpose((sources['xcentroid'], sources['ycentroid']))
apertures = CircularAperture(positions, r=5.)
```

Next, we will perform aperture photometry on the synthetic image using the aperture_photometry function:

```python
phot_table = aperture_photometry(image_data, apertures)
```

The phot_table variable contains the results of the aperture photometry in the form of an Astropy QTable. You can print the table to see the measured flux for each detected source:

```python
print(phot_table)
```

Finally, you can plot the detected sources and the apertures on the synthetic image:

```python
from astropy.visualization import simple_norm

plt.imshow(image_data, cmap='Greys_r', norm=simple_norm(image_data, 'linear', percent=99.5))
apertures.plot(color='red', lw=1.5)
plt.colorbar()
plt.show()
```

This will display the synthetic image with the detected sources marked by red circles.

<a name="psf-photometry"></a>

## 6. PSF Photometry

PSF photometry is a technique for measuring the brightness of a celestial object by fitting its point spread function (PSF) to the data. Photutils provides the BasicPSFPhotometry class and various PSF models, such as the GaussianPSF, MoffatPSF, and AiryDisk2D, to perform PSF photometry on your data.

In this example, we will use the BasicPSFPhotometry class with the GaussianPSF model to perform PSF photometry on our synthetic Gaussian PSF image.

First, let's import the necessary modules and classes:

```python
from photutils.psf import BasicPSFPhotometry, IntegratedGaussianPRF
from astropy.modeling.fitting import LevMarLSQFitter

```

Now, let's create a BasicPSFPhotometry object with the GaussianPSF model, an initial guess for the positions of the sources, and a fitting object:

```python
psf_model = IntegratedGaussianPRF(sigma=3.)
psf_model.x_0.fixed = False
psf_model.y_0.fixed = False

fitting = LevMarLSQFitter()

psf_photometry = BasicPSFPhotometry(finder=daofind, group_maker=None,
                                    bkg_estimator=None, psf_model=psf_model,
                                    fitter=fitting, aperture_radius=5.,
                                    fitshape=(11, 11))
```

Next, we will perform PSF photometry on the synthetic image using the psf_photometry object:

```python
result_table = psf_photometry(image_data)
```

The result_table variable contains the results of the PSF photometry in the form of an Astropy QTable. You can print the table to see the measured flux and other fitted parameters for each detected source:

```python
print(result_table)
```

Additionally, you can extract and plot the residual image after subtracting the fitted PSFs from the original image:

```python
residual_image = psf_photometry.get_residual_image()

plt.imshow(residual_image, cmap='Greys_r', norm=simple_norm(residual_image, 'log', percent=99.5))
plt.colorbar()
plt.show()
```


<a name="equations"></a>

## 7. Solving Equations

SymPy can solve a wide variety of equations, including algebraic, trigonometric, and differential equations. Here are some examples:

- `solve()`: Solves an algebraic equation.
- `solve_trig()`: Solves a trigonometric equation.
- `dsolve()`: Solves a differential equation.

```python

# Algebraic Equation

eq1 = x**2 - 4
sol1 = sp.solve(eq1, x)

# Trigonometric Equation

eq2 = sp.sin(x) + sp.cos(x)
sol2 = sp.solve(eq2, x)

# Differential Equation

y = sp.Function('y')
eq3 = sp.Eq(y(x).diff(x), y(x))
sol3 = sp.dsolve(eq3, y(x))

```

<a name="linear-algebra"></a>

## 8. Linear Algebra

SymPy supports various linear algebra operations, such as matrix operations, eigenvalues, and eigenvectors. Here are some examples:

- `Matrix()`: Creates a matrix.
- `eigenvals()`: Computes the eigenvalues of a matrix.
- `eigenvects()`: Computes the eigenvectors of a matrix.

```python
A = sp.Matrix([[1, 2], [3, 4]])
B = sp.Matrix([[5, 6], [7, 8]])

# Matrix addition

C = A + B

# Matrix multiplication

D = A * B

# Eigenvalues

eigenvalues = A.eigenvals()

# Eigenvectors

eigenvectors = A.eigenvects()
```

<a name="discrete-math"></a>

## 9. Discrete Mathematics

SymPy offers functions for working with discrete mathematics, such as combinatorics, graph theory, and number theory. Here are some examples:

- `binomial()`: Computes the binomial coefficient.
- `factorial()`: Computes the factorial of a number.
- `prime()`: Checks if a number is prime.

```python

# Binomial Coefficient

bc = sp.binomial(5, 2)

# Factorial

fact = sp.factorial(5)

# Prime Number

is_prime = sp.isprime(7)

```

<a name="physics"></a>

## 10. Physics and Mechanics

SymPy has modules for classical mechanics, quantum mechanics, optics, and thermodynamics. Here are some examples:

- `Lagrangian`: Constructs the Lagrangian for a system of particles.
- `Hamiltonian`: Constructs the Hamiltonian for a system of particles.
- `MatrixOptics`: Performs matrix optics calculations.

```python
from sympy.physics.mechanics import Lagrangian, Particle, Point, ReferenceFrame, dynamicsymbols
from sympy.physics.quantum import Commutator, Dagger, Operator
from sympy.physics.optics import ThinLens, RayTransferMatrix

# Classical Mechanics: Lagrangian

m, g = sp.symbols('m g')
x, v = dynamicsymbols('x v')
N = ReferenceFrame('N')
O = Point('O')
P_pos = O.locatenew('P_pos', x * N.x)
P_pos.set_vel(N, v * N.x)  # Set the velocity of point P_pos in ReferenceFrame N
P = Particle('P', P_pos, m)
L = Lagrangian(N, P) - m * g * x

# Quantum Mechanics: Commutator

A = Operator('A')
B = Operator('B')
commutator = Commutator(A, B)

# Optics: Thin Lens

focal_length = sp.Symbol('f')
lens = ThinLens(focal_length)
transfer_matrix = RayTransferMatrix(lens)
```

<a name="plotting"></a>

## 11. Plotting

SymPy can create plots of expressions and functions, including 2D and 3D plots. Here are some examples:

- `plot()`: Plots a 2D graph of a function.
- `plot3d()`: Plots a 3D graph of a function.

```python

x, y = sp.symbols('x y')

# 2D Plot

f = sp.sin(x)
sp.plot(f, (x, -2* sp.pi, 2 * sp.pi))

# 3D Plot

g = sp.sin(x) * sp.cos(y)
sp.plotting.plot3d(g, (x, -sp.pi, sp.pi), (y, -sp.pi, sp.pi))
```

<a name="code-generation"></a>

## 12. Code Generation and Optimization

SymPy can generate code in various languages, such as C, Fortran, and Octave, for numerical evaluation of expressions. Here are some examples:

- `lambdify()`: Converts a SymPy expression to a numerical function.
- `codegen()`: Generates code in a specified language.

```python
import numpy as np
from sympy.utilities.codegen import codegen

# Lambdify

f = sp.sin(x) * sp.exp(-x)
f_num = sp.lambdify(x, f, 'numpy')
x_vals = np.linspace(0, 10, 100)
y_vals = f_num(x_vals)

# Code Generation

f_prime = sp.diff(f, x)
[(c_name, c_code), (h_name, h_code)] = codegen(('f_prime', f_prime), language='C')

```

<a name="faq"></a>

## 13. Frequently Asked Questions

- **Q: How can I improve the performance of my SymPy code?**
- A: SymPy can be slow for certain types of calculations. To improve performance, consider using `lambdify()` to convert expressions to numerical functions, or use `codegen()` to generate code in other languages.

- **Q: Can SymPy work with other Python libraries, such as NumPy or SciPy?**
- A: Yes, SymPy is compatible with other Python libraries. You can use `lambdify()` to create a function that works with NumPy arrays, or directly use SymPy objects with SciPy functions when appropriate. However, be aware that mixing SymPy and other libraries may sometimes require additional care and attention to ensure correct behavior.

- **Q: Can SymPy handle symbolic matrices and linear algebra operations?**
- A: Yes, SymPy can handle symbolic matrices and perform various linear algebra operations, such as matrix multiplication, inversion, determinant calculation, and eigenvalue/eigenvector computation.

- **Q: Does SymPy support multivariate calculus?**
- A: Yes, SymPy supports multivariate calculus, including partial differentiation, multiple integration, gradient, divergence, and curl.

<a name="conclusion"></a>

## 14. Conclusion

In this comprehensive guide, we have covered various aspects of SymPy, from basic operations to advanced techniques. By now, you should have a solid understanding of how to use SymPy effectively in your Python projects. With practice and experimentation, you will be able to master SymPy and fully harness its power for symbolic mathematics.

<a name="resources"></a>

## 15. External Resources

To further enhance your SymPy knowledge, consider exploring these external resources:

1. [SymPy Official Documentation](https://docs.sympy.org/latest/index.html)
2. [SymPy Tutorial](https://docs.sympy.org/latest/tutorial/index.html)
3. [SymPy Live](https://live.sympy.org/): An interactive online environment to try SymPy without installing it.
4. [SymPy GitHub Repository](https://github.com/sympy/sympy): Browse the source code and contribute to the project.
5. [SymPy Examples](https://github.com/sympy/sympy/wiki/Quick-examples): A collection of short examples demonstrating various SymPy features.
6. [Python for Data Science Handbook](https://jakevdp.github.io/PythonDataScienceHandbook/): A book that covers various Python libraries, including a section on SymPy.

Remember, practice is essential for mastery. We encourage you to apply what you've learned in this guide to real-world problems and challenges. Good luck on your journey with SymPy and symbolic mathematics!
