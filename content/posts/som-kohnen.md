---
title: "SOM - Self Organising Maps"
date: 2018-03-10T11:40:14+02:00
publishdate: 2018-03-10T11:40:14+02:00
imagethumb: "/images/som_konhen/small_image_som.jpg"
image: "/images/som_konhen/edinburg_east_min.jpg"
description: "Unsupervised machine learning to analyse the Scottish Index of Multiple Deprivation"
comments: false
imgcaption: "View of the East side of Edinburgh from Calton Hill"
lens: "false"
zoom: "true"
---


<div class='container'>
<h1>A different look into the Scottish Index of Multiple Deprivation</h1>
<p></p>
<p>The Scottish Government calculates and publish the <b>Scottish Index of Multiple Deprivation</b> (SIMD) which measures the access and availability of opportunities and resources in the different Scottish Data Zones. 
The SIMD has <b>26 indicators</b>, aggregated in <b>seven domains</b>: income, employment, health, education, mobility, crime and housing. Each domain has a single index where low values represent high levels of deprivation and all seven indexes combined build the overall deprivation index.</p>

<p> The outcomes of the SIMD are meant to be used by the Scottish Government and other organizations to better target their interventions. However, the <b>structure of the index</b> poses a <b>challenge</b> making difficult the selection of the geographical units to target with social interventions. This because a user can either look at the overall ranking, which lacks in fine details about deprivation, or look at the single domain ranking or the single indicator losing a holistic view. </p>   

<p> In between these two levels of interpretations, overall ranking and single domain ranking, this work looks at using the <b>Self Organising Maps</b> (SOM), an <b>unsupervised machine learning technique</b>, to profile the <b>527 Data Zones</b> inS the City of Edinburgh and group them by similarity, making differences and affinities immediately visible.</p>   



<h2>The Self Organising Maps</h2>
<p>The SOM is an algorithm belonging to the broad family of the <b>Artificial Neural Networks</b>, which given a dataset with multiple variables and observations performs both clustering and dimensional reduction <i>(Skupin and Agarwal, 2008)</i>. <b>Clustering</b> means that the SOM algorithm evaluates similarities and differences between the observations, grouping the more similar ones. The <b>dimensional reduction</b> takes place because the SOM allocates multi-dimensional data (many variables) in a 2-dimensional space, consisting of a grid of hexagonal cells.</p>

<h2>Model Calibration</h2>

<p>The model calibration is necessary to adjust the parameters to obtain the best model performances and testing the variables for collinearity.

<h3>Grid</h3>
<p>
The first parameter to set is the size of the 2-dimensional grid that will host the observations. <i>Vesanto and Alhoniemi, 2000</i> propose the following formula [1] to calculate the grid size, where <b>N</b> is the number of grid cells and <b>obs</b> the number of observations. 
</p>

<!-- https://code-boxx.com/math-equations-html/ -->
<div style="text-align: center; ">
<i>N = 5 &#215; &#8730; obs  &nbsp;&nbsp;&nbsp; [1]</i> 
</div>

<h3>Neighborhood function</h3>
<p>
This function determines the allocation of new observations, following <i>Skupin and Agarwal, 2008</i>, it was chosen a Gaussian function.
</p>

<h3>Cycles</h3>
<p>
The learning rate line reaching a plateau determines the ideal number of cycles, after some testing it was chosen a learning rate of 800 cycles.
</p>

<h3>Collinearity</h3>
<p>
The variables need to be tested for collinearity, selecting only those that are relevant. The collinearity test was done qualitatively, observing the distribution of each individual variable in the 2-dimensional space after the training, where variables that are correlated will have a similar distribution pattern <i>(Koua and Kraak, 2004)</i>.
Figure 1 shows the distribution patterns while Table 1 the variables chosen for the analysis.
</p>


<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:85%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 1 - Distribution of the different variables in the grid space, obtained using the Gaussian function."  class="medium" src="/images/som_konhen/collinearity_check.jpg">
<figcaption>Figure 1 - Distribution of the different variables in the grid space, obtained using the Gaussian function.</figcaption>
</figure>
</div>

<div style="text-align: center">
<figure class="figcap">
<figcaption style="padding: 1rem 0rem 1rem 0rem">Table 1 - Selected variables for the final analysis after the collinearity check.</figcaption>
<table class='table2 table-hover'><thead>
    <tr style="text-align: left">
        <th>Domain</th>
        <th>Variable</th>
    </tr>
</thead><tbody style="text-align: left">
    <tr>
        <td>Income</td>
        <td>Income rate</td>
    </tr>
    <tr>
        <td>Health</td>
        <td>Hospitalization due to alcohol misuse</td>
    </tr>
    <tr>
        <td></td>
        <td>Standard mortality ratio (SMR)</td>
    </tr>
    <tr>
        <td></td>
        <td>Percent of the population using antidepressants</td>
    </tr>
    <tr>
        <td></td>
        <td>Underweight birth</td>
    </tr>
    <tr>
        <td></td>
        <td>Emergency hospitalizations</td>
    </tr>
    <tr>
        <td>Education</td>
        <td>Pupils attendance</td>
    </tr>
    <tr>
        <td></td>
        <td>People with no qualifications</td>
    </tr>
        <tr>
        <td></td>
        <td>16-19 not in education or working (NEET)</td>
    </tr>
        <tr>
        <td></td>
        <td>School finishers enrolling in higher education (HESA)</td>
    </tr>
    <tr>
        <td>Mobility</td>
        <td>Public transport GP</td>
    </tr>
        <tr>
        <td></td>
        <td>Public transport post</td>
    </tr>
        <tr>
        <td></td>
        <td>Public transport retail</td>
    </tr>
        <tr>
        <td>Crime</td>
        <td>Crime rate</td>
    </tr>
        <tr>
        <td>Housing</td>
        <td>Overcrowded houses rate</td>
    </tr>
        </tr>
        <tr>
        <td></td>
        <td>Not heated houses rate</td>
    </tr>
</tbody></table>
</figure>
</div>



<h2>Data Analysis</h2>
<p>The data processing and analysis was carried outusing <b>R Studio</b>, below a list of the libraries used:</p>
</p>
<div style="text-align: left">
<figure class="figcap framelight"><blockquote><code style = "line-height:0.2">
library(kohonen) <br>
library(libraryggplot2) <br>
library(ggmap) <br>
library(rgdal) <br>
library(gridExtra) <br>
library(grid) <br>
library(rgeos) <br>
library(maptools) <br>
library(broom) <br>
library(dplyr) <br> 
library(data.table) <br>
library(tidyr) <br>
library(MASS) <br>
</blockquote></code></figure>

<a target="_blank" href="https://github.com/marcopizzolato-mp/self-organising-maps"><i class="fab fa-github">&nbsp;&nbsp;</i>Visit Git Repo</a></figcaption>
</div>

<p>
The model was run with the <b>16 selected variables</b>: Figure 2a shows the learning rate dropping, reaching a plateau before the 800 cycle threshold; Figure 2b shows the observation density in each node of the grids, which is in line with a density of 5-10 observation per node indicated by <i>Lynn, 2014</i>; Figure 2c shows the distance between the vector values in one node against the ones in the surrounding nodes and, aside from an outlier cell, the homogeneous values indicate that across the grid there is a smooth change in the characteristics of the observations placed in each node; Figure 2d indicates and overall good quality of the grid nodes. 
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:85%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 2 – Process outputs with 16 variables - (a) Training progresses with 800 cycles; (b) Node count; (c) Neighbor distance; (d) Node quality."  class="medium" src="/images/som_konhen/analysis_results.jpg">
<figcaption>Figure 2 – Process outputs with 16 variables - (a) Training progresses with 800 cycles; (b) Node count; (c) Neighbor distance; (d) Node quality.</figcaption>
</figure>
</div>

<br>
<h3>Clustering</h3>

After the SOM distributes the <b>527 observations</b> (Data Zones) in the 2-dimensional space the next step is to divide them into clusters, making it easier to identify the main characteristics of each cluster. To perform the clustering it was used the <b>k-means within-cluster sums of squares</b>. Figure 3a displays the <b>within-cluster sum of squares graph</b> (WCSS) graph which can give a rough indication of the optimal number of clusters when applying the “elbow method” <i>(Kodinariya and Makwana, 2013)</i>. However, from the graph, it is difficult to identify the elbow, therefore after several experiments were chosen <b>5 clusters</b>.


<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:85%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 3 - (a) Within-cluster sum of squares graph (b) SOM grid divided into six clusters without division lines."  class="medium" src="/images/som_konhen/clustering.jpg">
<figcaption>Figure 3 - (a) Within-cluster sum of squares graph (b) SOM grid divided into six clusters without division lines.</figcaption>
</figure>
</div>

<br>
<h3>Cluster profiling</h3>
<p>
The cluster profiling can be done qualitatively, interpreting the output in Figure 4, however I did a quantitative analysis, looking at the <b>average z-value</b> per each indicator per each cluster. The outputs of this further step are displayed in Figure 5.
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:50%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 4 - SOM grid divided into six clusters with pie charts showing individual variables."  class="medium" src="/images/som_konhen/_cluster_details_min.jpg">
<figcaption>Figure 4 - SOM grid divided into six clusters with pie charts showing individual variables.</figcaption>
</figure>
</div>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:65%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="RFigure 5 - Profiling of the cluster using the average normalized z-value per each variable in each cluster."  class="medium" src="/images/som_konhen/cluster_profile.jpeg">
<figcaption>Figure 5 - Profiling of the cluster using the average normalized z-value per each variable in each cluster.</figcaption>
</figure>
</div>


<div style="text-align: center;">
<figure class="figcap">
<figcaption style="padding: 1rem 0rem 1rem 0rem">Table 2 - Cluster description.</figcaption>
<table class='table2 table-hover; '><thead>
    <tr style="text-align: left">
        <th>Cluster</th>
        <th>Description</th>
    </tr>
</thead><tbody style="text-align: left">
    <tr>
        <td>1</td>
        <td>High level of education – High distance time</td>
    </tr>
    <tr>
        <td>2</td>
        <td>All indicators in the average</td>
    </tr>
    <tr>
        <td>3</td>
        <td>Low income – High use of antidepressant - <br>High percent of 16-19 not in education, employment or training</td>
    </tr>
    <tr>
        <td>4</td>
        <td>Low income – High hospitalizations – High alcohol consumption – <br> High percent of working age people with no qualifications -<br> High percent of 16-19 not in education, employment or training</td>
    </tr>
    <tr>
        <td>5</td>
        <td>High level of crime – Poor housing</td>
    </tr>
</tbody></table>
</figure>
</div>


<h2>Data Interpretation</h2>
<p>
The cluster profiles in Table 2 highlight some interesting aggregation patterns. In particular, <b>cluster 3 and 4</b> show a strong relationship between income, alcohol consumption, hospitalization and low schooling. All traits that can denote Data Zones with a high level of deprivation. The map in Figure 6 shows how these two clusters are geographically close to each other, sign that most probably they share the same social dynamics, which are taking place with different magnitudes. These two clusters characterize specific groups of Data Zones in the peripheral areas of the City. 
<b>Cluster 5</b> brings to light a possible link between the incidence of crime and poor quality housing. These two aspects are probably not related to a cause-effect relationship, but they are rather signs of deprived areas, even though for this cluster the income level is close to the average. From Figure 6 appears that cluster 5 includes only a few Data Zones in the center of the city. A big number of Data Zones fall in the <b>cluster 2</b>, which collects observations with no particular tracts. This area covers most of the city center and surrounding areas. Well-defined agglomerates of Data Zones belonging to the <b>clusters 3 and 4</b> interrupt the spatial homogeneity of this cluster.
<b>Cluster 1</b> is characterized by a high level of education but also a high travel time. The latter explains why most of the Data Zones belonging to this cluster are located far from the center, in the outskirts of town.
<h3>Spatial Patterns</h3>
To see whether or not the SOM output reflects the SIMD classification, in Figure 3 are displayed tiny maps representing the lower (or higher in the case of education) quartile of the SIMD ranking normalized per each indicator. To obtain the normalized SIMD ranking the original national-wide ranking values for each data zone were <b>normalized between 0 and 1</b>. Following this, to compose the maps, only the <b>last or first quartile were selected</b>; therefore, were selected only the <b>most deprived Data Zones</b> per each indicator (least deprived for the education). 
Using these maps to qualitatively evaluate the parallelism between the SOM clusters and the SIMD, it can be seen that there are some <b>spatial patterns matching</b>. For example, high education and poor accessibility from the SIMD reflect the coverage of cluster 1, which has similar characteristics. The same it can be observed for clusters number 3 and 4 against the SIMD income and health maps. Regarding the crime, this seems to be underrepresented, with cluster 5 only including few Data Zones in the center, and the other crime-deprived Data Zones flowed into Cluster 2. This distortion might be due to an outlier value among the crime data. 
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:100%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 6 - Data Zones grouped by clusters."  class="medium" src="/images/som_konhen/overall_map_min_9k.jpg">
<figcaption>Figure 6 - Data Zones grouped by clusters.</figcaption>
</figure>
</div>

<h2>Evaluation of the SOM</h2>
<p>The SOM is a valid tool for preliminary data exploration and data mining, through its capacity to handle a high number of observation with a high number of variables, producing easy to understand visual outputs. However, the SOM does not explicitly provides in-depth insight into the characteristics of each cluster. Therefore, this limits its scope, leaving the quantification of the relations between the variables to more detailed statistical tools, such as different types of regression analysis. </p>




<h2>References</h2>
<div style="text-align: left">
<figure><code style = "line-height:0.2; font-style:italic">
- Kodinariya, T. M. and Makwana, P. R. (2013) ‘Review on determining number of Cluster in K-Means Clustering’, International Journal of Advance Research in Computer Science and Management Studies, 1(6), pp. 2321–7782.<br>
- Koua, E. L. and Kraak, M. (2004) ‘Integrating computational and visual analysis for exploratory visualization and knowledge discovery in large geospatial data Correspondence : Integrating computational and visual analysis for exploratory visualization and knowledge discovery in large geos’, International Journal of Health Geographics, 3(12), pp. 1–11.<br>
- Lynn, S. (2014) Self-Organising Maps for Customer Segmentation using R | Shane Lynn. Available at: http://shanelynn.ie/index.php/self-organising-maps-for-customer-segmentation-using-r/ (Accessed: 23 March 2018).<br>
- Scottish Government (2016) ‘The Scottish Index of Multiple Deprivation (SIMD)’, pp. 1–20. <br>
- Skupin, A. and Agarwal, P. (2008) ‘Introduction: What is a Self-Organizing Map?’, Self-Organising Maps: Applications in Geographic Information Science, pp. 1–20. <br>
- Vesanto, J. and Alhoniemi, E. (2000) ‘Clustering of the self-organizing map’, IEEE Transactions on Neural Networks, 11(3), pp. 586–600. <br>
- Creator Marco Pizzolato - This project is licensed under the MIT License <br>
</code></figure>
</div>











</br></br>
<!-- Div End of Page -->
</div>

    

