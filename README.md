# Predicting NMR Parameters Through Machine Learning - SMASH 2018

### Will Gerrard
### wg12385@bristol.ac.uk

*** If you would like to see how the ML prediction performs for a specific molecule, please email me with an .xyz file (or PDB, Mol2, etc) and ill reply with the chemical shifts as soon as possible, the current prediction routine only takes a few seconds ! ***

# Extra Info SMASH 2018 "Predicting NMR Parameters Through Machine Learning"

### Will Gerrard
### wg12385@bristol.ac.uk

## Contents
1. [Current state of the project](#current-state-of-the-project)
2. [Aims of the project](#aims-of-the-project) 
3. [The representations](#the-representations)  
  a. [Coulomb Matrix](#coulomb-matrix)  
  b. [FCHL](#fchl)  
  c. [SLATM](#slatm)  
  d. [JACSF](#jacsf)  
4. [Kernel Ridge Regression](#kernel-ridge-regression)  
  a. [Hyper Parameter Searching] (#hyper-parameter-searching) 
5. [The Datasets](#the-datasets)  
 a. [Dataset 1 (pre-initial test set)](#dataset-1)  
 b. [Dataset 2 (First test set)](#dataset-2)  
 c. [Dataset 3 (recreated based on ShML)](#dataset-3)  
 d. [Dataset 4 (better FPS selection, being created)](#dataset-4)  
 e. [Future Datasets](#future-datasets)  
6. [Experimental Prediction] (#experimental-prediction)

## Current state of the project
The following table gives the best prediction accuracy for each of the main NMR parameters being investigated for the current dataset (Dataset 3).

| NMR Parameter | Mean Absolute Error | Standard Deviation |
|:----:|:----:|:----:|
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1H$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1H$&space;$\delta$" title="$^1H$ $\delta$" /></a> | 0.25 | 0.23 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^{13}C$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^{13}C$&space;$\delta$" title="$^{13}C$ $\delta$" /></a> | 3.22 | 4.10 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1J_{HC}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1J_{HC}$" title="$^1J_{HC}$" /></a> Coupling | 2.04 | 2.02 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^3J_{HH}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^3J_{HH}$" title="$^3J_{HH}$" /></a> Coupling | 3.44 | 5.02 |

The corresponding methods are provided in the table below:

| NMR Parameter | Representation | Method | Dataset | 
|:----:|:----:|:----:|:---:|
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1H$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1H$&space;$\delta$" title="$^1H$ $\delta$" /></a> | SLATM | KRR | 3
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^{13}C$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^{13}C$&space;$\delta$" title="$^{13}C$ $\delta$" /></a> | SLATM | KRR | 3
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1J_{HC}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1J_{HC}$" title="$^1J_{HC}$" /></a> Coupling | SLATM | KRR | 3
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^3J_{HH}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^3J_{HH}$" title="$^3J_{HH}$" /></a> Coupling | JACSF | KRR | 3

These tables will be updated as new tests are run.

## Aims of the project
Ultimately the goal of the project is to predict experimental scalar coupling constants and chemical shifts for small molecules to a comparative accuracy to modern DFT methods. So far the focus has been primarily on chemical shift prediction, as this has been done successfully by other groups in similar areas (solid state, experimental prediction). Ideally the accuracy of the machine learning algorithms in predicting DFT parameters will reach a level where they are insignficant in relation to the error between DFT and experiment. At this point the machine learning algorithms could replace DFT in some computationally aided structure elucidation workflows.

## The Representations
The data used for the training of the machine learning algorithms is derived from DFT calculated NMR parameters and DFT optimised xyz coordinates. The input data to the Kernel Ridge Regression algorithm (and in fact mose machine learning algorithms) needs to satisfy two main criteria for efficient and successful training. 

 - Unique structures must be represented by unique data structures
 - Identical structures must be represented by identical data structures
 
There are hundreds of formats which satisfy these criteria (xyz coordinates with atom types not being one !!), the issue then becomes which will best encode the chemical information necessary to predict the parameters being trained.

Several different representations have been tested so far in the project, each with different benefits and drawbacks.

### Coulomb Matrix
The coulomb matrix is by far the most simple of the representations tested in the project. It is based on just two terms, the distance between pairs of atoms and the product of their atomic numbers. The matrix is comprised atom-by-atom via the following formula for diagonal and off-diagonal elements. 

<img src="https://image.ibb.co/johNZe/coulomb_equation.png" width="300">

The representations are generated for each individual atom of interest. The generated coulomb matrix is then centered on that atom, with terms for atoms extending out to the selected cutoff range.

| NMR Parameter | Mean Absolute Error | Standard Deviation |
|:----:|:----:|:----:|
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1H$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1H$&space;$\delta$" title="$^1H$ $\delta$" /></a> | 0.39 | 0.43 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^{13}C$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^{13}C$&space;$\delta$" title="$^{13}C$ $\delta$" /></a> | 4.40 | 4.85 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1J_{HC}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1J_{HC}$" title="$^1J_{HC}$" /></a> Coupling | N/A | N/A |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^3J_{HH}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^3J_{HH}$" title="$^3J_{HH}$" /></a> Coupling | N/A | N/A |

Initial results in hyper parameter searching (shown below) indicated that the coulomb matrix introduces too much noise once the distance cutoff extends past 3-4 angstroms.

<img src="https://image.ibb.co/gEbgjz/combined_hplot_C.png" width="500">


### FCHL
The FCHL representation comes from [this paper](https://aip.scitation.org/doi/10.1063/1.5020710), the name is derived from the initials of the authors. FCHL is a three body representation which means that each term is derived from the interaction between three atoms in the molecule. It was hoped that introducing three body terms would overcome the issue in the coulomb matrices of introducing too much noise at higher cutoff values. The FCHL representation also uses a different kernel function, detailed in the paper.

| NMR Parameter | Mean Absolute Error | Standard Deviation |
|:----:|:----:|:----:|
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1H$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1H$&space;$\delta$" title="$^1H$ $\delta$" /></a> | 0.25 | 0.23 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^{13}C$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^{13}C$&space;$\delta$" title="$^{13}C$ $\delta$" /></a> | 3.32 | 3.03 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1J_{HC}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1J_{HC}$" title="$^1J_{HC}$" /></a> Coupling | 2.17 | 1.88 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^3J_{HH}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^3J_{HH}$" title="$^3J_{HH}$" /></a> Coupling | N/A | N/A |

As hoped, there was a substantial improvement in the accuracy.

### SLATM
The Spectrum of London and Axillrod-Teller-Muto potential (SLATM) is another three body representation. The relevant theory is quite old, but the introduction of atomic SLATM representations is introduced in [this paper](https://arxiv.org/abs/1707.04146).

| NMR Parameter | Mean Absolute Error | Standard Deviation |
|:----:|:----:|:----:|
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1H$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1H$&space;$\delta$" title="$^1H$ $\delta$" /></a> | 0.25 | 0.23 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^{13}C$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^{13}C$&space;$\delta$" title="$^{13}C$ $\delta$" /></a> | 3.22 | 4.10 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1J_{HC}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1J_{HC}$" title="$^1J_{HC}$" /></a> Coupling | 2.04 | 2.02 |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^3J_{HH}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^3J_{HH}$" title="$^3J_{HH}$" /></a> Coupling | N/A | N/A |

The SLATM representation provided marginal improvement over FCHL.

### JACSF
The JACSF molecular representation is based on atom-centred symmetry functions, adapted specifically for j coupling. The representation is currently being developed, however it is much more suited to application with neural networks, so hasn't been tested much yet. The neural network algorithms are being developed currently.

| NMR Parameter | Mean Absolute Error | Standard Deviation |
|:----:|:----:|:----:|
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1H$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1H$&space;$\delta$" title="$^1H$ $\delta$" /></a> | N/A | N/A |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^{13}C$&space;$\delta$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^{13}C$&space;$\delta$" title="$^{13}C$ $\delta$" /></a> | N/A | N/A |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^1J_{HC}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^1J_{HC}$" title="$^1J_{HC}$" /></a> Coupling | N/A | N/A |
| <a href="https://www.codecogs.com/eqnedit.php?latex=$^3J_{HH}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$^3J_{HH}$" title="$^3J_{HH}$" /></a> Coupling | 3.44 | 5.02 |

The initial results on 3 bond proton proton coupling were terrible, hopefully this will get better !

## Kernel Ridge Regression
There are much better explanations of what kernel ridge regression (KRR) is and does, notably [here](https://www.ics.uci.edu/~welling/classnotes/papers_class/Kernel-Ridge.pdf) and [here](https://www.youtube.com/watch?v=XUj5JbQihlU&t=3s&frags=pl%2Cwn). Ridge regression alone is simply mathematical regression using the squared error as the loss function (also referred to as the L2-norm). The essential concept is the kernel trick employed to simplify the mathematics. The kernel trick involves taking the (mathematical) distance between each pair of representations in the dataset and then finding the coefficient matrix to produce the desired output vector from this similarity (or kernel) matrix. This coefficient matrix is found via regression, and determining it is often what is referred to as training.

For the applications in this project we have used the laplacian kernel function, which is very common in machine learning applications. 

### Hyper Parameter Searching
Three main input parameters are needed for the KRR algorithm using the SLATM descriptor; the cutoff radius for the SLATM representation, the width of the kernel function (sigma) and the regularisation factor for the kernel function. These three parameters are crucial for good training, and so optimal values need to be determined before any real results can be produced. At the moment this is done via a simple grid-search over the three values.

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

The dataset comprises of only H, C, N, O atoms, and is the current working dataset for the project.


### Dataset 4
This dataset is currently being produced. We have identified a potentially better method of FPS sampling and we also want to include fluorine atoms in the dataset to expand the applicability.

### Future Datasets
Future datasets will be developed based on conclusions drawn from the performance of the current and in-production datasets. However, there are already plans to produce a dataset with a reduced number of unique molecules (~500) but with multiple conformers per molecule, to see what proportion of the prediction error can be removed by taking into account multiple conformers and their relative conformations.

## Experimental Prediction

### Progesterone
6 low energy conformers were identified for progesterone through conformational searching using macromodel. These 6 structures were geometry optimised and then NMR calculations were performed on the resulting structures. The resulting NMR calculated values were then boltzman weighted by the relative populations of the conformers at a set temperature. In the same way, the KRR algorithm using the SLATM descriptor was used to predict the chemical shifts for the 6 optimised structures, and the resulting values boltzmann weighted. The two sets of chemical shifts were then compared to the experimental values determined by members of the Butts group. The accuracy of the ML KRR algorithm to experiment was 3.47ppm and 0.28ppm for Carbon and Proton respectively.

![](https://image.ibb.co/mBbBVU/progesterone_ML_DFT_EXP.png)



### Streptomycin
The process was repeated for streptomycin, however 72 low energy conformers were selected. The results of the comparison are shown below. The accuracy of the ML KRR algorithm to experiment was 2.82ppm and 0.48ppm for Carbon and Proton respectively.
![](https://image.ibb.co/hCBdAU/streptmomycin_ML_DFT_EXP.png)

 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcyOTcyNTczOCwtNjIzOTA0MTUzLDE5Mz
ExMDIzMjksLTIwNzk4Mzk4MDIsMTQ3MzMxMDI1LDg5ODMzMTY5
OCwxODMxODQ0MDI4LC02MTQ5NjY5MzEsMTU5ODc0Mjk3NCw5Nz
Y0MzI3NjMsNTg2ODE3NTQsLTIwMjU3MTA1MjQsLTEwMzczMjM1
NzgsLTgxODMzMjgzMywtMTgxNjMxMDg5LC00NTQxODAyNDMsLT
gzMTY5NzE5MSwtMTQwNjMzNzEyOSwtNDM5MjcwMDMwLC0xNzA3
OTA4MjU1XX0=
-->
