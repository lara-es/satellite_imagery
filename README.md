# Satellite Imagery Analysis
## Lara Strachan, October 17, 2022

### **Introduction**

Seeing something familiar from a new perspective is a fantastic way to reveal previously difficult-to-see insights. This is very much the case for what satellite imagery can teach us about our home, planet Earth. Many people are leveraging these opportunities, and in fact, there are “4,852 active artificial satellites orbiting the Earth as of January 1, 2022”. [(1)](https://www.statista.com/statistics/264472/number-of-satellites-in-orbit-by-operating-country/) I hoped to join the ranks of those that are able to distill answers and solutions from the wealth of satellite imagery available and find some answers to the question “Which areas of Colorado would be most valuable to protect based on biodiversity and pressure from human impact?”. 

I chose those two factors as my criteria because highly biodiverse areas (habitats for vertebrates, freshwater invertebrates, pollinators, and vascular plants) could have the most benefit to a wide range of flora and fauna, and human activities such as urbanization, transportation, agriculture, etc could threaten these biodiverse habitats. Protecting these areas before irreversible harm such as species extinction or habitat destruction happens could help maintain ecological balance and keep Colorado the beautiful, wild place that so many residents and visitors cherish.

In the course of doing literature review and background research, I found a great deal of helpful information about the types of satellite imagery available, the potential applications of satellite imagery analysis, and tools for conducting analysis. In fact, it is quite a dense and sometimes confusing world to navigate. I encountered many obstacles in attempting to collect the right types of data needed for the specific analysis I wanted to do but also discovered unexpected tools and information to support my queries. Through this trial and error, I was able to build an unsupervised machine learning model, k-Means clustering, to identify similarities in land types based on Sentinel-2 satellite images, as well as create an ArcGIS web map for visualizing areas of imperiled biodiversity, park areas, and areas vulnerable to land change in Colorado.

### **Table of Contents** 

#### Python Notebooks

[1 - Xarray Notebook](../1_xarray.ipynb)

Data sources used: 

- Machine Learning Human Footprint Index  http://dx.doi.org/10.25675/10217/216207

[2 - Rasterio Notebook](../2_rasterio.ipynb)

Data sources used:

- Human Footprint Index https://datadryad.org/stash/dataset/doi:10.5061/dryad.3tx95x6d9
- Global Forest Change https://earthenginepartners.appspot.com/science-2013-global-forest/download_v1.7.html

[3 - k-Means Notebook](../3_unsup_learning.ipynb)

Data sources used:

- Sentinel-2  https://www.sentinel-hub.com/explore/eobrowser/


#### ArcGIS content

[Web map](https://learngis2.maps.arcgis.com/apps/instant/imageryviewer/index.html?appid=1eefb6a8b40c4f53be3affd947e1c1da&center=-105.427;39.0524&level=7&selectedFeature=USA_Parks_4323;15459&hiddenLayers=Land_Cover_Vulnerability_2050_8288;mapNotes_9112)


### **Methodology**

To start, I familiarized myself with some of the more popular Python Libraries for working with Geospatial data, including Xarray and Rasterio. Additional libraries are necessary for working with geospatial data file types such as netCDF, geoTIFF, and geoJSON. The first two notebooks, “xarray” and “rasterio” contain code for reading in different types of geospatial data, visualizing it, and creating subsets. These two notebooks are not necessary to run before running the k-Means notebook, but xarray should be run before rasterio if they will be executed. 

The two most beginner-friendly and valuable tools I found, out of the many that I tried, are sentinelhub’s EO Browser (https://www.sentinel-hub.com/explore/eobrowser/) and Esri’s ArcGIS. EO Browser is absolutely phenomenal for locating and downloading a wide variety of different satellite images in multiple different formats, and is completely free. There are robust documentation and tutorials, but it is also very intuitive to use. ArgGIS offers a free 21-day trial and has a wealth of tutorials and an interesting gallery of examples to browse through as well. This program can easily be used to implement different types of projects, visualizations, and analyses. 

With an abundance of satellite imagery available, and a shortage of pixel-wise labelled semantic classes, unsupervised learning is an awesome way to leverage recent advances in remote sensing. Of these unsupervised learning techniques, k-means can be a simple yet effective place to start with semantic segmentation. I decided to model on imagery of an area of Colorado that contains a diverse mix of urban, foothills, and mountainous territory (an area west of, and including some of Denver) so that there would be a variety of landscapes to classify. Using EO Browser, I downloaded 11 bands of Sentinel-2 images of my area of interest (AOI). I then stacked the data for these bands, standardized them, and performed Principle Component Analysis (PCA) to reduce the dimensionality before feeding them into the k-Means model.


Taking advantage of the features of ArgGIS, I also created a web map of Colorado with layers depicting “Areas of Unprotected Biodiversity Importance of Imperiled Species in the United States”, designated Parks (Nat’l & State park or forest, regional, county, or local parks), and “Land Cover Vulnerability Change by 2050”. These map layers were available in ArcGIS’s Living Atlas, which currently has 6,433 available layers. It is then possible to visualize the intersection and overlapping of areas that are unprotected areas of imperiled species, and that are vulnerable to land cover change.


### **Results**

Working with an unsupervised machine learning model such as k-Means, means that to truly validate the accuracy of my clusters, I would need to have some ground truth to compare to. While that option was not available to me for this project, there are tools where it is possible to manually go through images and identify either specific objects or land classes, then train a supervised model on that. I did not find any free or accessible options for doing that, but I was able to visually compare the segmentation of my k-Means predictions to the same area that had undergone SENTINEL-2 Scene classification (which is a more advanced/complex classification system using neural nets and threshold algorithms). 

While the SENTINEL-2 classification map was more detailed and contained class titles, the k-Means model performed fairly impressively and was able to identify many of the same landscape features. Additionally, the SENTINEL-2 map has 10 different classes, and I determined that 4 clusters were appropriate for my map area by visualizing a plot of inertia for clusters 1-6. The “elbow” was not abundantly obvious, but 4 clusters appeared to work well. Being able to classify land types/uses from satellite imagery is valuable in attempting to figure out what areas could be most beneficial to protect in Colorado because that can be useful in identifying habitats and observing patterns of human development.

Inspecting the layers of the web map, it appears that much of the land that is vulnerable to change by 2050 is along the front-range area, which makes sense because a great deal of the major cities in Colorado are along the front-range I-25 corridor. However, I do notice that near Gunnison, there is a large area with indications towards having unprotected biodiversity along with a pocket of predicted land cover change by 2050. Grand Junction is another example of this overlap. 

Visual analysis is not sufficient to make concrete recommendations, however, so performing a deeper analysis on both the species habitats presently existing as well as 
take these insights to the relevant parties involved in conservation and land planning would be wise. However, having resources such as this map is a good initial step in narrowing down the field of where to look.


### **Conclusions** 

While I don’t believe I was able to fully answer the question I set out with of what areas in Colorado are most valuable to protect, I gained a much greater appreciation and awareness for satellite imagery analysis and the tools used in the field. Robustly labeled satellite imagery is hard to come by, so using unsupervised learning methods such as k-means is a good way to gain initial findings about the area of interest. Moving forward, my recommendations for continued insight into this problem would be honing in on areas of overlap with imperiled biodiversity and vulnerability to land change, while also finding away to perform more intricate analysis of the features used to create these map layers. Creating a resource of updated beginner-friendly data science GIS tools is also something I’d like to create to share with my fellow datascientiests. Humans must find a way to live in harmony with our fellow flora and fauna if we want to maintain the integrity of our environment, and using the unique perspectives and information provided by satellite imagery could be an essential tool for achieving that.
