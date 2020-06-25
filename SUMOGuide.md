# The process for creating your own FCD and OSM files:

NB: All commands were run on a Linux terminal.

## Step 1: pick a location.
More specifically, pick a stretch of highway that is included in the PEMS database (http://pems.dot.ca.gov/) [if you don’t have an account on that website signing should be pretty quick]. The website interface isn’t the most friendly, here’s a link to the user guide in case you need to look something up: http://pems.dot.ca.gov/Papers/PeMS_Intro_User_Guide_v6.pdf . Here’s a link to the list of accessible freeways in case you’re having trouble picking one: http://pems.dot.ca.gov/?dnode=State&content=fwy_list .
When picking which highway stretch to use make sure to pick one that has enough detector coverage (checking the # of detectors is usually enough) because we’re gonna be taking the data from the detectors to create the FCD files.
It’s also important to pick the exact stretch of highway you want to simulate, we only want a couple miles at most because any more and our simulations will take too long to complete.
## Step 2: Download the .osm files for the chosen location.
The osm files are an open source collection of roadway and building definitions for any given area on the globe (https://www.openstreetmap.org). Once you’ve found the area on OpenStreetMap you click “Export” on the top of the screen, then “Manually select a different area”, and drag the newly produced rectangle to highlight the area you want and more, then click the blue “Export” button to download the .osm file for that area. If you’re doing an area too large for the website then the export won’t work, I ran into this issue myself, the easy fix is just to download (export) multiple smaller .osm files and then merge them together afterwards. If downloading multiple .osm files to be merged, make sure that each .osm file overlaps with its neighboring .osm file or else the final merged result might not have the complete area defined.
### Step 2.5: (If downloading multiple .osm files) Merge the .osm files.
To merge them I used a software called “osmconvert”. It auto installed onto my Linux Terminal when I installed SUMO and such but I know a windows “.exe” version exists as well.
```bash
osmconvert <input .osm files> -v --drop-author -o=<output.osm file>
```
```console
osmconvert <input .osm files> -v --drop-author -o=<output.osm file>
```
example:
```shell
osmconvert map1.osm map2.osm map3.osm map4.osm -v --drop-author -o=completeMap.osm
```

