# Extra Info and Data SMASH 2018

## Contents
1. [Current state of the project](#current-state-of-the-project)
2. [Aims of the project](#aims-of-the-project) 
3. [The representations](#the-representations)  
  a. [Coulomb Matrix](#coulomb-matrix)  
  b. [FCHL](#fchl)  
  c. [SLATM](#slatm)  
  d. [JACSF](#jacsf)  
4. [Kernel Ridge Regression](#kernel-ridge-regression)  
5. [The Datasets](#the-datasets)  
 a. [Dataset 1 (pre-initial test set)](#dataset-1)  
 b. [Dataset 2 (First test set)](#dataset-2)  
 c. [Dataset 3 (recreated based on ShML)](#dataset-3)  
 d. [Dataset 4 (better FPS selection, being created)](#dataset-4)  
 e. [Future Datasets](#future-datasets)  

## Current state of the project
The following table gives the best prediction accuracy for each of the main NMR parameters being investigated.

| NMR Parameter | Mean Absolute Error | Standard Deviation |
|:----:|:----:|:----:|
| $^1H$ $\delta$ | 0.22 | 0.66 |
| $^{13}C$ $\delta$ | 0.22 | 0.66 |
| $^1J_{HC}$ Coupling | 0.22 | 0.66 |
| $^3J_{HH}$ Coupling | 0.22 | 0.66 |

The corresponding methods are provided in the table below:

| NMR Parameter | Representation | Method | Dataset | 
|:----:|:----:|:----:|:---:|
| $^1H$ $\delta$ | SLATM | KRR | 3 |
| $^{13}C$ $\delta$ | SLATM | KRR | 3 |
| $^1J_{HC}$ Coupling | SLATM | KRR | 3 |
| $^3J_{HH}$ Coupling | JACSF | KRR | 3 |

## Aims of the project
Ultimately the goal of the project is to predict experimental scalar coupling constants and chemical shift for small molecules to a comparative accuracy to modern DFT methods. . . .

## The Representations
The data used for the training of the machine learning algorithms is derived from DFT calculated NMR parameters and DFT optimised xyz coordinates. The input data to the Kernel Ridge Regression algorithm needs to satisfy two main criteria for efficient and successful training. 

 - Unique structures must be represented by unique data structures
 - Identical structures must be represented by identical data structures
 
There are hundreds of formats which satisfy these criteria, the issue then becomes which will best encode the chemical information necessary to predict the parameters being trained.

Several different representations have been tested so far in the project, each with different benefits and drawbacks.

### Coulomb Matrix
The coulomb matrix is by far the most simple of the representations tested in the project. It is based on just two terms, the distance between pairs of atoms and the product of their atomic numbers. The matrix is comprised atom-by-atom via the following formula for diagonal and off-diagonal elements. 

![](https://image.ibb.co/johNZe/coulomb_equation.png)

The representations are generated for each individual atom of interest. The generated coulomb matrix is then centered on that atom, with terms for atoms extending out to the selected cutoff range.

| NMR Parameter | Mean Absolute Error | Standard Deviation |
|:----:|:----:|:----:|
| $^1H$ $\delta$ | 0.22 | 0.66 |
| $^{13}C$ $\delta$ | 0.22 | 0.66 |
| $^1J_{HC}$ Coupling | 0.22 | 0.66 |
| $^3J_{HH}$ Coupling | 0.22 | 0.66 |

### FCHL
The FCHL representation comes from [this paper](https://aip.scitation.org/doi/10.1063/1.5020710), the name is derived from the initials of the authors. FCHL is a three body representation which means that each term is derived from the interaction between three atoms in the molecule. 

| NMR Parameter | Mean Absolute Error | Standard Deviation |
|:----:|:----:|:----:|
| $^1H$ $\delta$ | 0.22 | 0.66 |
| $^{13}C$ $\delta$ | 0.22 | 0.66 |
| $^1J_{HC}$ Coupling | 0.22 | 0.66 |
| $^3J_{HH}$ Coupling | 0.22 | 0.66 |

### SLATM
The Spectrum of London and Axillrod-Teller-Muto potential (SLATM) is another three body representation. 

| NMR Parameter | Mean Absolute Error | Standard Deviation |
|:----:|:----:|:----:|
| $^1H$ $\delta$ | 0.22 | 0.66 |
| $^{13}C$ $\delta$ | 0.22 | 0.66 |
| $^1J_{HC}$ Coupling | 0.22 | 0.66 |
| $^3J_{HH}$ Coupling | 0.22 | 0.66 |

### JACSF
The JACSF molecular representation is based on atom-centred symmetry functions, adapted specifically for j coupling. The representation is currently being developed, however it is much more suited to application with neural networks, so hasn't been tested much yet. The neural network algorithms are being developed currently.

| NMR Parameter | Mean Absolute Error | Standard Deviation |
|:----:|:----:|:----:|
| $^1H$ $\delta$ | N/A | N/A |
| $^{13}C$ $\delta$ | N/A | N/A |
| $^1J_{HC}$ Coupling | Coming soon | Coming soon |
| $^3J_{HH}$ Coupling | 3.33 | 3.33 |

## Kernel Ridge Regression
There are much better explanations of what kernel ridge regression (KRR) is and does, notably [here](https://www.ics.uci.edu/~welling/classnotes/papers_class/Kernel-Ridge.pdf) and [here](https://www.youtube.com/watch?v=XUj5JbQihlU&t=3s&frags=pl%2Cwn). Ridge regression alone is simply mathematical regression using the squared error as the loss function (also referred to as the L2-norm). The essential concept is the kernel trick employed to simplify the mathematics. The kernel trick involves taking the (mathematical) distance between each pair of representations in the dataset and then finding the coefficient matrix to produce the desired output vector from this similarity (or kernel) matrix. This coefficient matrix is found via regression, and determining it is often what is referred to as training.

For the applications in this project we have used the laplacian kernel function

LAPLACIAN KERNEL


## The Datasets

### Dataset 1
Dataset 1 was inherited from previous work in the butts research group, it contains several hundred molecules with DFT calculated parameters, it was only used to test the initial programs whilst the first proper dataset was being generated.

### Dataset 2
Dataset 2 was created by the following process:

1. An initial set of 100,000 structures was identified from the Cambridge structural database via an initial query:
   * Organic molecules only
   * Non-polymeric
   * No Ions
   * Non disorderd
   * Single crystal structures only
   * Contain the fragment H-C-X where X: C, N, O
   * Atoms with 3D coordinates
   * R factor $\leq$ 0.05
   * No unresolved errors
2. From this initial set, 1,000 structures were randomly selected
3. All 1,000 structures were geometry optimised, although not all successfully. Due to errors in the structures many failed the DFT optimisation.
4. The 874 structures which successfully optimised were then put into DFT NMR calculations to give the NMR parameters
5. The optimised geometries and the NMR parameters were combined to produce the molecular representations used in the training of the machine learning algorithms.

### Dataset 3
Dataset 3 is based on the work from [this paper](https://arxiv.org/pdf/1805.11541.pdf), which uses kernel ridge regression to predict NMR chemical shifts in solids from DFT data. 
1. The list of 2500 structures was taken from the paper and the molecules retrieved from the CSD
2. The structures were then geometry optimised, just over 2000 of which were successful
3. The optimised geometries were used for the DFT NMR calculations
4. The optimised geometries and the NMR parameters were combined to produce the molecular representations used in the training of the machine learning algorithms.

The shiftML dataset was used because the published results were good and so present a good opportunity for comparison with our own work. It also presents a significant improvement on dataset 2 in terms of the way the structures were selected. 500 of the structures were randomly selected: this forms the test set. The remaining 2000 were selected via furthest point sampling, which is a method of identifying the least similar molecules in the dataset. This should provide a significant improvement on random selection as it should give a better coverage of the chemical space.

The dataset comprises of only HCNO atoms, and is the current working dataset for the project, all data and graphs in this document are for this dataset

DATASET STATISTICS ??

### Dataset 4
This dataset is currently being produced. We have identified a potentially better method of FPS sampling and we also want to include fluorine atoms in the dataset to expand the applicability.

### Future Datasets
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkzMTEwMjMyOSwtMjA3OTgzOTgwMiwxND
czMzEwMjUsODk4MzMxNjk4LDE4MzE4NDQwMjgsLTYxNDk2Njkz
MSwxNTk4NzQyOTc0LDk3NjQzMjc2Myw1ODY4MTc1NCwtMjAyNT
cxMDUyNCwtMTAzNzMyMzU3OCwtODE4MzMyODMzLC0xODE2MzEw
ODksLTQ1NDE4MDI0MywtODMxNjk3MTkxLC0xNDA2MzM3MTI5LC
00MzkyNzAwMzAsLTE3MDc5MDgyNTUsLTEwODY5MDIxNDNdfQ==

-->