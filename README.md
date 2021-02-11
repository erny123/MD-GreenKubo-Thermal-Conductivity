# MD-GreenKubo-Thermal-Conductivity
 The thermal conductivity of a molecular dynamics system is calculated using the Green-Kubo fluctuation Dissipation Theorem

 For this particular case the Tersoff potential was used for Silicon and the thermal conductivity was calculated at 1000 K. 
 The calculations using the double exponential fit are done using references: 
 Chen, J., Zhang, G., & Li, B. (2010). How to improve the accuracy of equilibrium molecular dynamics for computation of thermal conductivity?. Physics Letters A, 374(23), 2392-2396.
 
 Files: 
 - Readme.md : just description
 
 - 1000kheatflux.in
   Input file for lammps MD code
 
 - SiC.tersoff
   Tersoff potential used for lammps MD simulation

 - "Green Kubo Thermal Conductivity Calculator.ipynb"
   Jupyter Python Notebook for calculating Thermal conductivity using the heat current flux outputted by LAMMPS 


DESCRIPTION:

![\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}](https://latex.codecogs.com/svg.latex?x%3D%5Cfrac%7B-b%5Cpm%5Csqrt%7Bb%5E2-4ac%7D%7D%7B2a%7D)
   
# Green Kubo Thermal Conductivity Calculator <br>
#### By Ernesto Barraza-Valdez 
#### ernestob@uci.edu 
#### 10/30/2020 

This notebook is to calculate the thermal conductivity using the Green-Kubo Heat Flux autocorrelation method. The steps and techniques are followed from Chen et al:
Chen, J., Zhang, G., & Li, B. (2010). How to improve the accuracy of equilibrium molecular dynamics for computation of thermal conductivity?. Physics Letters A, 374(23), 2392-2396.

We have used a 15x15x15 (lattice parameter) Si system with a total of 27,000 atoms at 1,000K. 
LAMMPS is told to calculate and output the total heat flux using *compute heat/flux* in the folowing way:
compute         myKE all ke/atom <br>
compute         myPE all pe/atom <br>
compute         myStress all stress/atom NULL virial <br>
compute         flux all heat/flux myKE myPE myStress <br>
variable        Jx equal c_flux[1]/vol <br>
variable        Jy equal c_flux[2]/vol <br>
variable        Jz equal c_flux[3]/vol <br>

The heat flux outputs for *Jx*, *Jy*, *Jz* were converted to a .csv file where it is then imported into python via pandas

Note: Chen et al uses Stillinger-Weber potential for Si. In this notebook, tersoff potential is used. 

---
To do Solve for the Autocorrelation function we use the algorithm formulated in the wikipedia article and stackoverflow:
https://stackoverflow.com/questions/14297012/estimate-autocorrelation-using-python

https://en.wikipedia.org/wiki/Autocorrelation#Estimation
 
Basically the autocorrelation function <a href="https://www.codecogs.com/eqnedit.php?latex=A_{corr}(\tau)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A_{corr}(\tau)" title="A_{corr}(\tau)" /></a> for a certain correlation length <a href="https://www.codecogs.com/eqnedit.php?latex=\tau" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\tau" title="\tau" /></a> can be described as:

<a href="https://www.codecogs.com/eqnedit.php?latex=A_{corr}(\tau)&space;=&space;\frac{1}{(n&space;-&space;\tau)\sigma^2&space;}&space;\sum&space;\limits&space;_{t=1}&space;^{n-\tau}&space;(X_t&space;-&space;\mu)\bullet(X_{t&plus;\tau}&space;-&space;\mu)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A_{corr}(\tau)&space;=&space;\frac{1}{(n&space;-&space;\tau)\sigma^2&space;}&space;\sum&space;\limits&space;_{t=1}&space;^{n-\tau}&space;(X_t&space;-&space;\mu)\bullet(X_{t&plus;\tau}&space;-&space;\mu)" title="A_{corr}(\tau) = \frac{1}{(n - \tau)\sigma^2 } \sum \limits _{t=1} ^{n-\tau} (X_t - \mu)\bullet(X_{t+\tau} - \mu)" /></a>

where <a href="https://www.codecogs.com/eqnedit.php?latex=\mu&space;=&space;\bar{X}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\mu&space;=&space;\bar{X}" title="\mu = \bar{X}" /></a> is the mean of the data set and <a href="https://www.codecogs.com/eqnedit.php?latex=\sigma^2&space;=&space;var(X)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\sigma^2&space;=&space;var(X)" title="\sigma^2 = var(X)" /></a> is the variance of the data set X. The total time (or length) of the data set is n

We need to do this for all three directions of the heat flux <a href="https://www.codecogs.com/eqnedit.php?latex=\bar{J}&space;=&space;J_x\hat{x}&plus;J_y\hat{y}&plus;J_z\hat{z}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\bar{J}&space;=&space;J_x\hat{x}&plus;J_y\hat{y}&plus;J_z\hat{z}" title="\bar{J} = J_x\hat{x}+J_y\hat{y}+J_z\hat{z}" /></a>

Notice that we use the np.correlate function which is a full convolution of the data as shown in the following links:
https://numpy.org/doc/stable/reference/generated/numpy.correlate.html

https://numpy.org/doc/stable/reference/generated/numpy.convolve.html#numpy.convolve
