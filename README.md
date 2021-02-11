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

   