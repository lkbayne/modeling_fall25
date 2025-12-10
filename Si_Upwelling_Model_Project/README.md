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
Generate the following list of files using the notebooks indicated in paratheses:

Model Grid (notebooks/Creating the Model Grid.ipynb)

Bathymetry (notebooks/Creating the Bathymetry.ipynb)

Initial Conditions (notebooks/Creating the Initial Conditions.ipynb)

External Forcing Conditions (notebooks/Creating the External Forcing Conditions.ipynb)

Boundary Conditions (notebooks/Creating the Boundary Conditions.ipynb)

