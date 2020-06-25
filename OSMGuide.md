---
title: OSM Guide
---
Other Guides:
- [GEMV2 Guide](GEMV2Guide.md)
- [FCD Guide](FCDGuide.md)

# The process for creating your own OSM files:

NB: All commands were run on a Linux terminal. I believe all the required terminal commands were installed automatically when I installed SUMO.

## Step 1: pick a location.
More specifically, pick a stretch of highway that is included in the [PEMS database](https://pems.dot.ca.gov) [if you don’t have an account on that website signing up should be pretty quick]. The website interface isn’t the most friendly, here’s a link to the [user guide](http://pems.dot.ca.gov/Papers/PeMS_Intro_User_Guide_v6.pdf) in case you need to look something up. Here’s a [link](http://pems.dot.ca.gov/?dnode=State&content=fwy_list) to the list of accessible freeways in case you’re having trouble navigating the site.
When picking which highway stretch to use make sure to pick one that has enough detector coverage (checking the # of detectors is usually enough) because we’re gonna be taking the data from the detectors to create the FCD files.
It’s also important to pick the exact portion/stretch of the highway you want to simulate, we only want a couple miles at most because any more and our simulations will take too long to complete.
## Step 2: Download the .osm files for the chosen location.
The osm files are an open source collection of roadway and building definitions for any given area on the globe. We download them from [Open Street Map](https://www.openstreetmap.org). Once you’ve found the area on OpenStreetMap you click “Export” on the top of the screen, then “Manually select a different area”, and drag the newly produced rectangle to highlight the area you want, then click the blue “Export” button to download the .osm file for that area. If you’re doing an area too large for the website then the export won’t download, I ran into this issue myself, the easy fix is just to download (export) multiple smaller .osm files and then merge them together afterwards. If downloading multiple .osm files to be merged, make sure that each .osm file overlaps with its neighboring .osm file or else the final merged result might not have the complete area defined.
### Step 2.5: (If downloading multiple .osm files) Merge the .osm files.
This command also removes author info for the file to save some space.
```bash
$ osmconvert <input .osm files> -v --drop-author -o=<output.osm file>
```
Example:
```bash
$ osmconvert map1.osm map2.osm map3.osm map4.osm -v --drop-author -o=mergedMap.osm
```
## Step 3: Clean up the .osm file.
We only want the roadway and surrounding building definitions, the .osm file contains many other things along with it (like sidewalks and railways) which we can remove to reduce any interferences that those might incite.
Cuts out some unnecessary stuff from .osm file:
```bash
$ osmfilter mergedMap.osm --drop-tags="railway= highway=service =footway =path =residential =pedestrian =unclassified =tertiary =tertiary_link =cycleway =steps" -o=trimmedMap.osm
```
The .osm file is now ready for use in GEMV2!!! Next step is to [create the FCD files](FCDGuide.md).
