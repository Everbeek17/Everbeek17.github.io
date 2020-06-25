
Other Guides:
- [GEMV2 Guide](GEMV2Guide.md)
- [OSM Guide](OSMGuide.md)

### Step 1: Convert .osm to .net.xml.
SUMO needs the .osm file in a .net.xml format to create the FCD file so we convert it using the command below
```bash
$ netconvert --osm-files trimmedMap.osm -o convertedMap.net.xml --ramps.guess --junctions.join --tls.guess-signals --tls.discard-simple --tls.join -R
```
The output of this command is the file `convertedMap.net.xml`.
### Step 2: Trim.
We open the .net.xml file in netedit, remove any unnecessary remaining features that we can (it's ok if we leave some unused stuff in the file, it just simplifies things down the line if we can remove some of it now), and then save it.
```bash
$ netedit convertedMap.net.xml
```


still to do:
- create a detector definitions file
- download detector data from PeMS
- 








---------------------Create bBoxes (for spatial analysis in GEMV2)

osmconvert completeOSMEditted.osm -b=-118.297475,34.035748,-118.286214,34.038533 -o=spatial1.osm

osmconvert completeOSMEditted.osm -b=-118.286214,34.035748,-118.276611,34.039905 -o=spatial2.osm




