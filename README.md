# Wheat Kernel Categorization Using a Neural Network

## Introduction and Credits

In this project, we will attempt to classify three wheat varieties based on
geometric attributes of their kernels. The dataset we use for training and
testing was prepared by M. Charytanowicz et al. at the Institute of Mathematics
and Computer Science of the John Paul II Catholic University in Lublin, Poland.
It is available at the [UCI Machine Learning
Repository](https://archive.ics.uci.edu/ml/datasets/seeds).

## Dataset Description

Using a soft X-ray technique, Charytanowicz et al. collected the following
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

## Training and accuracy measures

We use a randomly selected 80/20 training/testing split. Thus 168 entries are
used for training. Given the relatively small size of the training set, we run
the training 5 times on different training/testing selections, and take the
median loss and accuracy scores across runs. We found this step necessary as
the loss and accuracy scores varied by more than 5% at times, making accessing
both the absolute and relative performance of different training parameters
difficult.

## Feature Selection: First Round

### Correlations

We use the following Pearson correlation matrix to aid with feature selection.
Among highly correlated features, we are interested in selecting features whose
kernel density plots (seen below) most clearly suggest multiple distributions
exist (the hope being that each of these distributions is associated with a
category).

|               | Area   | Perimeter | Compact. | Length | Width  | Asymmetry | Groove L | Variety |
| ------------- | ------ | --------- | ----------- | ------ | ------ | --------- | ------------- | ------- |
| Area          | 1      | 0.994     | 0.608       | 0.95   | 0.971  | -0.23     | 0.864         | -0.346  |
| Perimeter     | 0.994  | 1         | 0.529       | 0.972  | 0.945  | -0.217    | 0.891         | -0.328  |
| Compact.   | 0.608  | 0.529     | 1           | 0.368  | 0.762  | -0.331    | 0.227         | -0.531  |
| Length        | 0.95   | 0.972     | 0.368       | 1      | 0.86   | -0.172    | 0.933         | -0.257  |
| Width         | 0.971  | 0.945     | 0.762       | 0.86   | 1      | -0.258    | 0.749         | -0.423  |
| Asymmetry     | -0.23  | -0.217    | -0.331      | -0.172 | -0.258 | 1         | -0.011        | 0.577   |
| Groove L | 0.864  | 0.891     | 0.227       | 0.933  | 0.749  | -0.011    | 1             | 0.024   |
| Variety       | -0.346 | -0.328    | -0.531      | -0.257 | -0.423 | 0.577     | 0.024         | 1       |

### Distributions

<p align="middle">
    <img src="illustrations/area.png?raw=true)" width="350"/> 
    <img src="illustrations/asymmetry.png?raw=true)" width="350"/>
    <img src="illustrations/compactness.png?raw=true)" width="350"/> 
    <img src="illustrations/groove_length.png?raw=true)" width="350"/>
    <img src="illustrations/length.png?raw=true)" width="350"/> 
    <img src="illustrations/perimeter.png?raw=true)" width="350"/>
    <img src="illustrations/width.png?raw=true)" width="350"/> 
</p>

## Experiment Table Example

| Train Size | Nodes/ Layer| M Loss               | Opt. | M Accu. | Epoch | Input Shape | Feature Select.             | Test Loss | Test Accu. | Comment                  |
| ----------- | ----------- | ------------------------ | --------- | -------- | ------ | ----------- | ------------------------------ | --------- | ------------- | ------------------------- |
| 168         | 128         | cat. c. entrp            | adam      | accu. | 120    | 3           | Area, Asymm., Compact.      | 0.757   | 0.857        | None                |
| 168         | 128         | cat. c. entrp            | adam      | accu. | 75     | 7           | 1, 2, 3, 4, 5, 6, 7            | 0.992    | 0.881         | Regular. on layer 2 |
| 168         | 128         | cat. c. entrp            | adam      | accu. | 75     | 7           | 1, 2, 3, 4, 5, 6, 7            | 1.05    | 0.881         | Const. to 10    |
| 168         | 128         | cat. c. entrp            | adam      | accu. | 75     | 7           | 1, 2, 3, 4, 5, 6, 7            | 1.05    | 0.881         | Rand. data sel      |
| 160         | 128         | cat. c. entrp | adam      | accu. | 120    | 3           | Width, Compact., Groove L | 0.273    | 0.92          | One hidd. layer          |
| 160         | 128         | cat. c. entrp | adam      | accu. | 68     | 3           | Width, Compact., Groove L | 0.266    | 0.94          | Two hidd. layers         |
| 160 | 128 | cat. c. entrp | adam | accu. | 120 | 3   | Area, Compact., Groove L | 0.161 | 0.94 | Two hidd. layers |
| 160 | 128 | cat. c. entrp | adam | accu. | 120 | 3   | Area, Compact., Groove L | 0.143 | 0.94 | Three hidd. layers |
| 160 | 128 | cat. c. entrp | adam | accu. | 120 | 4   | Area, Compact., Groove L, Perim. | 0.153 | 0.96 | Sigmoid act. last hidd. layer |
| 150 | 64  | cat. c. entrp | adam | accu.| 500 | 3   | Length, Compact., Groove L | 0.176 | 0.967 | Five hidd. layers, reg. last hidd lay |

### Conclusions

Do not underestimate feature selection.
Evaluate single features, and combinations. Using two features that are not highly correlated may seem attractive, but if the one of the features is not very helpful in training for your objectives, combining it with the other might not help as much as you expect. Kernel length and the asymmetry coefficient, for example, were significantly less correlated than kernel length and groove length. Nevertheless, the kernel length and groove length pair proved to be more fruitful in training. 
If your model doesn’t converge in training, or does so sporadically, a slow learning rate and high number epochs, combined with a high capacity may help significantly. Once the model starts overfitting, introduce regularization. Once testing performance is satisfactory, experiment with lower capacities, epoch numbers, and learning and learning decay rates.
