# Monterey Bay Upwelling Silicate Model



## Project Discription
In this project, I investigate how coastal wind-driven upwelling influences nutrient dynamics in Monterey Bay, California. 

My science question: How does wind strength impact upwelling intensity and near-surface nutrient concentrations in Monterey Bay?

I developed a regional ocean model covering Monterey Bay and the adjacent California coast. 

The model is initialized using the ECCO Version 4.4 state estimate for January 2008. 

This year was selected because it represents a period of strong upwelling, as indicated by the BEUTI Index. 

I also derived the model’s boundary conditions and atmospheric forcing fields from ECCO Version 4.4 output.


I performed two year-long simulations for 2008 to isolate the effect of wind strength:

    High-wind run: forced with the full ECCO-derived wind fields, representing strong upwelling conditions.

    Reduced-wind run: identical to the high-wind case, but with wind stress reduced to one-third of its original magnitude, 
      thereby weakening Ekman-driven upwelling.

I expect that stronger winds will enhance coastal upwelling, bringing colder, nutrient-rich deep water to the surface more effectively 
than in the reduced-wind experiment. As a result, the high-wind run should exhibit higher surface nutrient concentrations.

To visualize nutrient movement more directly, I also implemented a passive tracer (pTracer) of silicate (SiO2), from an observed silicate 
profile obtained from a CTD cast in Monterey Bay. This tracer is transported by the model flow without influencing density or biogeochemistry, 
allowing me to track the pathways and vertical movement of silicate-rich waters under different wind-forcing scenarios.

For analysis, I will:
  Construct a coastal time series of near-surface silicate for both simulations to quantify differences in nutrient concentrations through time.

  Examine the evolution of the silicate pTracer to visualize the movement, surfacing, and lateral spreading of nutrient-rich deep water.

  Create a movie of surface silicate differences (high-wind minus reduced-wind) to illustrate the spatial and temporal variability in nutrient 
  delivery driven by changes in wind strength.



# Reproducing Model Results
The following steps outline how to construct the model files, configure and run the model, and assess the model results.



## Step 1: Create the Model Files
The input files (input directory in spartan) need to be created to run the model. 
Generate the following list of files using the notebooks indicated in paratheses that are in the jupyter_notebooks directory:

1. Model Grid (jupyter_notebooks/ModelGrid.ipynb) - this notebook provides the grid that is used in the following notebooks:

   delX = 1/625

   delY = 19/10000

   xgOrigin = -122.2635

   ygOrigin = 36.5436

   n_rows = 245

   n_cols = 300

   xc = np.arange(xgOrigin+delX/2, xgOrigin+n_cols*delX, delX)

   yc = np.arange(ygOrigin+delY/2, ygOrigin+n_rows*delY+delY/2, delY)

   XC, YC = np.meshgrid(xc, yc)


2. Bathymetry (notebooks/Bathymetry.ipynb) - this notebook creates the file bathymetry.bin

   -there is an issue with the ECCO bathymetry used, so also need to make the bathymetry smooth using (jupyter_notebooks/new_bathy_smooth.ipynb)

   -this notebook resaves the bathymetry.bin file that will be used in the input directory


3. Initial Conditions (jupyter_notebooks/InitialConditions_Si.ipynb.ipynb) - this notebook creates the _IC.bin files that are in the input directory


4. External Forcing Conditions (jupyter_notebooks/External_Forcing_Conditions.ipynb) - need to make another external forcing conditions for the model with 1/3 of the wind, so using this notebook you can pull in the exf files created and 1/3 the uwind and vwind files (jupyter_notebooks/External_Forcing_Conditions-NOWIND.ipynb) - these notebooks create an exf folder and files inside within the input directory


5. Boundary Conditions (jupyter_notebooks/Boundary_Conditions_Si.ipynb) - this notebook creates the obcs folder in the input directory



## Step 2: Add files to the computing cluster
Activate the SJSU VPN, log into Spartan (ssh [username]@spartan03.sjsu.edu), make a conda environment for MITgcm, and clone a copy of MITgcm into your scratch directory. Make directories for the configuration and the model, .e.g.

mkdir MITgcm/configurations/Upwelling_Si

Send the input, namelist, and code directories with the proper files to spartan using: 

scp [FILE_NAME] [username]@spartan03.sjsu.edu:/scratch/[username]/MITgcm/configurations/[model name]/input




## Step 3: Compile the model
Make a build directory and compile the model by running the following lines in succesion:

cd build

export MPI_HOME=/scratch/home/[username]/.conda/envs/ms274

../../../tools/genmake2 -of ../../../tools/build_options/linux_amd64_gfortran -mods ../code -mpi

make depend

make



## Step 4: Run the model with full upwelling winds
Make a run directory and link the following directories by running the following code:

ln -s ../namelist/* .

ln -s ../input/* .

ln -s ../build/mitgcmuv .


Then create the data output directories (diags) which match the data.diagnostics file in the namelist directory

mkdir diags

mkdir diags/[one folder for each diag]


Once everything is ready, create a .slm file with the appropriate processors:

vim Upwell_Si.slm


It should contain:

#!/bin/bash

#SBATCH --partition=nodes

#SBATCH --nodes=1

#SBATCH --ntasks=2

#SBATCH --time=10:00:00

export MPI_HOME=/scratch/home/[username]/.conda/envs/ms274

mpirun -np 2 ./mitgcmuv


To run the model:

sbatch Upwell_Si.slm


Check if your model is running using squeue


## Step 5: Run the model with the 1/3 upwelling winds
Run the new external forcing conditions with 1/3 winds (jupyter_notebooks/External_Forcing_Conditions-NOWIND.ipynb) and upload the new exf folder into the input directory.

Create a new directory and follow the step 4 instructions, making sure to upload the new files. If the files are the same name, you don't need to change anything, but if they have different names then edit the data.exf file to point to the modified wind files.

Submit the new job script and rerun the model.



## Step 5: Analyze the Results
Use the following code on your local terminal to get the entire output file (diags) to your computer:

scp -r [username]@spartan03.sjsu.edu:/scratch/[username]/MITgcm/verification/tutorial_global_oce_latlon/run/diags .


I used the following notebooks to analyze my data:
Plots (jupyter_notebooks/)

Profiles (jupyter_notebooks/)

Answering the Science Question (jupyter_notebooks/)

