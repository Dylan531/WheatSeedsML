# Wheat Kernel Categorization Using a Neural Network

## Introduction and Credits

In this project, we will attempt to classify three wheat varieties based on
geometric attributes of their kernels. The dataset we use for training and
testing was prepared by M. Charytanowicz et al. at the Institute of Mathematics
and Computer Science of the John Paul II Catholic University in Lublin, Poland.
It is available at the [UCI Machine Learning
Repository](https://archive.ics.uci.edu/ml/datasets/seeds).

## Dataset Description

Using a soft X-ray technique, Charytanowicz et al. measured the following
kernel features:

- Area *A*
- Perimeter *P*
- Compactness

    ![\frac{4\piA}{P^2}](https://latex.codecogs.com/svg.image?C=\frac{4\pi&space;A}{P^2})
- Length
- Width
- Asymmetry coefficient
- Length of kernel groove

The kernels belong to the Kama, Rosa, and Canadian varieties, labeled in the
dataset as categories 1, 2, and 3.

## Preliminary Processing
