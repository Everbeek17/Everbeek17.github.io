Other Guides:
- [OSM Guide](OSMGuide.md)
- [FCD Guide](FCDGuide.md)


# How to run a simulation in GEMV2:

### Step 0: Check MAtlab working directory.
Make sure MATLAB is open with the “Current Folder” working directory being “(Modified)GEMV2PackageV1.2”.
### Step 0.5: Allocate enough RAM to MATLAB.
If running a large simulation, you might run into the issue where MATLAB says you ran out of RAM/Memory. One fix for this is to allow MATLAB to use more of your RAM during its calculations, to do this go to the “HOME” tab, then click “Preferences” (in the “ENVIRONMENT” headbar section), then under “MATLAB” click on “General” then “Java Heap Memory” and slide the bar up as much as your computer will let you. For me the default value was 384 MB which is really low, I highly recommend sliding this value up to prevent any memory issues during simulations.
## Step 1: Specify input files.
In simSettings.m you tell GEMV2 which files you are using as input, one staticFile and one vehiclesFile, corresponding to roadway/building definitions and the location of each vehicle for each time step, respectively. This can be accomplished by creating a new case in the “switch scenario” switch statement, putting your file locations in there, then setting the “scenario” variable at the top of the file to the same value as the new switch case you just created (check out case 13 for an example).
## Step 2: Run Simulation.
Type “runSimulation” in the command window and hit enter to run a single simulation with the currently defined settings from simSettings.m. GEMV2’s raw output will be saved to the folders outputKML (if generating Google Earth visualizations) and outputSim (if specified in simSettings.m). My parsed output and graphs and such will be saved to a generated folder titled “Output”.


Scenarios 1-5 are example scenarios provided by the creator of GEMV2, those should run without any problems. (1 is quite large tho)
Scenarios 6-12 are scenarios I have set up, to run them you’re going to have to open the folder “Erkin” and extract the contents of “PremadeData.zip” right there in order for GEMV2 to have access to those input files I used. (Warning: the uncompressed files are 2.2 GB)


Some Files Descriptions:
simSettings.m:
Where you tell GEMV2 which input files you want to use.
staticFile = .osm file (roadway/building definitions)
vehiclesFile = .xml file defining where each vehicle is at each time step
Where you define some other settings for the simulation like the maximum distance between vehicles before they are no longer considered a communicating pair (commRange, maxNLOSvRange, maxNLOSbRange).
Where you tell GEMV2 if you want to generate visualizations that can be viewed in Google Earth (useful to create pretty visualizations of a network but adds to data generations time). Done by setting “GEVisualize = 1” at the bottom of the file. Outputs into ‘OutputKML’ folder.
Where you decide if you want the data to be run through my parser after it is generated.
As well as if you want some metrics to be calculated and graphs to be generated/saved and stuff like that.
[Function] ErkinParseFunction.m:
If ErkinParse.parse is set to ‘true’ in simSettings.m, then this function is called upon the completion of all the GEMV2 data generation.
This function calls on the other functions I wrote to accomplish all of the post-processing of data that I required. Then it calls PlotMetrics.m (detailed below) if requested.
The main post-processing that is accomplished here is the organization of the GEMV2 output data. GEMV2 creates data for every existing pair of vehicles that are able to communicate, per time step. The way GEMV2 generates this data is to treat each time step as a completely separate entity from all the prior time steps. So what I did here is I found a way to distinguish each pair of communicating vehicles, and then link those communicating vehicles to all the data generated for that specific pair, for every given time step. Thus making it possible to track the same communicating pair of vehicles over the course of time, seeing how the communication between those two vehicles evolves as their situation adapts.
[Function] PlotMetrics.m:
I created this function to calculate, generate, and save graphs (mostly cdf graphs) for multiple different simulation metrics, more specifically: Link Duration, Link Type (bar graph), Node Degree, Number of Clusters, Size of Largest Cluster, and Clustering Coefficient.
ErkinRun.m:
I used this to run multiple simulations in sequence, so that I don’t need to change settings and hit run in between each simulation and could just setup on a computer, hit run, and come back a while later with multiple simulations complete.
I had it setup to run through 10 “spatial” variations of the same 5 minute simulation with the given start time (denoted “temporal” (temporal = 800 means 8am)). runSimulation was then called 10 times where each call had a different value for “currentSpatial” which denoted which spatial area the simulation would use for that run. Only works if simSettings.m has “scenario” variable set to 12.
