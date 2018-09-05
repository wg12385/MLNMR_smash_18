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


## The Datasets

### Dataset 1

### Dataset 2

### Dataset 3

### Dataset 4

### Future Datasets
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ3MzMxMDI1LDg5ODMzMTY5OCwxODMxOD
Q0MDI4LC02MTQ5NjY5MzEsMTU5ODc0Mjk3NCw5NzY0MzI3NjMs
NTg2ODE3NTQsLTIwMjU3MTA1MjQsLTEwMzczMjM1NzgsLTgxOD
MzMjgzMywtMTgxNjMxMDg5LC00NTQxODAyNDMsLTgzMTY5NzE5
MSwtMTQwNjMzNzEyOSwtNDM5MjcwMDMwLC0xNzA3OTA4MjU1LC
0xMDg2OTAyMTQzXX0=
-->