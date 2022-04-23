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

The numerical range of different features varies significantly. We noted, for
instance, that while the area varies between 10.59 and 21.18 units, the
compactness coefficient varies between just 0.8081 and 0.9183 units.

|     | Area  | Perimeter | Compactness | Length | Width | Asymmetry | Groove Length |
| --- | ----- | --------- | ----------- | ------ | ----- | --------- | ------------- |
| Min | 10.59 | 12.41     | 0.8081      | 4.899  | 2.630 | 0.7651    | 4.519         |
| Max | 21.18 | 17.25     | 0.9183      | 6.675  | 4.033 | 8.4560    | 6.550         |

In our first attempt with preprocessing, we scaled the range of each feature to
be between the numbers 1 and 2. This prevents features with larger ranges from
being represented disproportionally.

## Feature Selection: First Round

### Correlations

### Distributions

![Area Distribution](illustrations/area.png?raw=true)
![Asymmetry Distribution](illustrations/asymmetry.png?raw=true)

## Experiment Table Example

| Sample Size | Layer Units | Model Loss               | Optimizer | Accuracy | Epochs | Input Shape | Feature Selection            | Test Loss | Test Accuracy |
| ----------- | ----------- | ------------------------ | --------- | -------- | ------ | ----------- | ---------------------------- | --------- | ------------- |
| 150         | 128         | categorical_crossentropy | adam      | accuracy | 75     | 3           | Area, Asymmetry, Compactness | 0.4799    | 0.8           |