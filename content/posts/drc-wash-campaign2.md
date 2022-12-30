---
title: "Cholera Response DRC2"
date: 2021-02-16T11:40:14+02:00
publishdate: 2021-02-16T11:40:14+02:00
imagethumb: "/images/cholera_drc/cholera_frontpage_adj.jpg"
image: "/images/cholera_drc/village_river_min.jpg"
description: "Extracting villages location and population estimates to support WASH activity in Democratic Republic of Congo"
comments: false
imgcaption: "Village along the river, Bor, South Sudan."
lens: "false"
zoom: "true"
---


<div class='container'>
<h1>Fighting Cholera in Democratic Republic of Congo</h1>
<p></p>
<p>This project started after a call from a friend of mine deployed in DRC, the Democratic Republic of Congo.
She said that <b>cholera outbreaks</b> were ongoing in many villages along the rivers Kasai and Sankuru and her organisation wanted to roll out a vast <b>Water Sanitation and Hygiene</b> (WASH) campaign, targeting all settlements within 20 km and monitor those between 30 and 50 km from these two rivers. The problem was that they did not have a reliable list of villages with <b>population estimates</b>, and she asked me if I could support looking into available datasets. </p>

<p> Let's see what I can do, I said... and after a a few days of intense work, I shared back a list of <b>over 5000</b> villages within 20 and 50 km from the two rivers, with population estimates extracted from two different datasets and an interactive <b>R Shiny Web Application</b> to navigate the results.</p>   

<h2>Area of interest</h2>
<p> The area of interest is quite large, stretching over more than 3000 km of river length and an area of approximately 100 thousand km<sup>2</sup> for the 20 km buffer and 250 thousand km<sup>2</sup> for the 50 km buffer. </p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:85%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Rivers Kasai and Sankuru and buffer around the rivers."  class="medium" src="/images/cholera_drc/drc_layout_min.jpg">
<figcaption>Figure 1 - Rivers Kasai and Sankuru (Left). Buffer around the rivers (Right).</figcaption>
</figure>
</div>

<div style="text-align: center">
<figure class="figcap">
<figcaption style="padding: 1rem 0rem 1rem 0rem">Table 1 - Length of the rivers Kasai, Sankuru and total length of both combined.</figcaption>
<table class='table2 table-hover'><thead>
    <tr>
        <th></th>
        <th>River Length</th>
        <th>Unit</th>
    </tr>
</thead><tbody>
    <tr>
        <td>River Kasai</td>
        <td>1926</td>
        <td>km</td>
    </tr>
    <tr>
        <td>River Sankuru</td>
        <td>1280</td>
        <td>km</td>
    </tr>
    <tr>
        <td>Total length</td>
        <td>3206</td>
        <td>km</td>
    </tr>
</tbody></table>
</figure>
</div>


<h2>Village dataset</h2>
<p>After comparing datasets from different sources, and testing them against satellite images I chose to use the dataset compiled by the <b> National Geospatial Intelligence Agency </b>(NGA) for its completeness and accuracy, with 5277 villages reported in the 50 km buffer area.</p> 
<p><a target="_blank" href="http://geonames.nga.mil/gns/html/namefiles.html">Download the data</a></p>


<h2>Population dataset</h2>

<p>
To estimate the population dataset I chose to use both a well-known dataset but also a new product - at the time of the analysis. These two datasets are the <b>Global Human Settlement Layer</b> (GHSL) published by the <b>EU Commission, Joint Research Center</b> and the <b>Facebook -</b> <b>High-Resolution Population Density Maps</a></b> published on the Humanitarian Data Exchange platform.
<p>
<a target="_blank" href="https://data.humdata.org/dataset/highresolutionpopulationdensitymaps"> Facebook- Download the data</a></br>
<a target="_blank" href="https://ghsl.jrc.ec.europa.eu/datasets.php"> GHSL - Download the data</a>
</p>


<h2>Methods and results</h2>
<p>The data processing and analysis was carried out using <b>R Studio</b> and <b>QGIS</b>, below a list of the libraries used:</p>

<div style="text-align: left">
<figure class="figcap framelight"><blockquote><code style = "line-height:0.2">
library(sf)   <br>
library(dplyr)   <br>
library(tidyr)  <br> 
library(tmap)   <br>
library(raster)   <br>
library(rgdal)   <br>
</blockquote></code></figure>
</div>
<a target="_blank" href="https://github.com/marcopizzolato-mp/drc-villages"><i class="fab fa-github">&nbsp;&nbsp;</i>Visit Git Repo</a></figcaption>

<p> From a technical perspective, the allocation of the population figures to the settlement point geometries required to re-sample the Facebook population raster to match the GHSL one, which has a resolution of 2x2 km.
The allocation of the population figures to the single settlements carries a small degree of approximation at the micro-level since in case two or more points are within a 2km x 2km square the population count will be evenly divided among them.<br>

If we want to make a comparison between the two population datasets, from the results in Table 2 is possible to see that the GHSL dataset, when compared against the Facebook one, has higher population estimates around the Kasai river and lower around the Sankuru, and the gap increases moving away from the river into the mainland. 
However, the two datasets seem to agree on the overall population figure, with a divergence of only 5.4%.
</p>


<div style="text-align: center">
<figure class="figcap">
<figcaption style="padding: 1rem 0rem 1rem 0rem">Table 2 - Population figures.</figcaption>
<table  class='table2 table-hover'><thead>
<tr>
    <th></th>
    <th>Village Count</th>
    <th>Population GHSL</th>
    <th>Population FB</th>
    <th>Diff % GHSL-FB</th>
</tr>
</thead><tbody>
    <tr>
        <td>Total villages</td>
        <td>5277</td>
        <td>4895775</td>
        <td>4631871</td>
        <td style="color: red">+5.4 %</td>
    </tr>
    <tr>
        <td>Villages 20km</td>
        <td>2475</td>
        <td>2668299</td>
        <td>2549717</td>
        <td style="color: red">+4.4 %</td>
    </tr>
    <tr>
        <td>Villages 50 km</td>
        <td>2802</td>
        <td>2227476</td>
        <td>2082154</td>
        <td style="color: red">+6.5 %</td>
    </tr>
    <tr>
        <td>Villages - 20 km Sankuru</td>
        <td>806</td>
        <td>1110766</td>
        <td>1165728</td>
        <td style="color: blue">-4.9 %</td>
    </tr>
    <tr>
        <td>Villages - 50 km Sankuru</td>
        <td>1044</td>
        <td>715225</td>
        <td>839952</td>
        <td style="color: blue">-17.4 %</td>
    </tr>
    <tr>
        <td>Villages - 20 km Kasai</td>
        <td>1669</td>
        <td>1557533</td>
        <td>1383989</td>
        <td style="color: red">+11.1 %</td>
    </tr>
    <tr>
        <td>Villages - 50 km Kasai</td>
        <td>1758</td>
        <td>1512251</td>
        <td>1242202</td>
        <td style="color: red">+17.9 %</td>
    </tr>
</tbody></table>
</figure>
</div>

</br>
<h2>R Shiny Web Application</h2>
<p>
To support the visual exploration of the dataset I created and deployed a <b>fully responsive</b> and <b>dynamic</b> R-Shiny Web Application, called <b>Shiny Afrique</b>. This application allows the user to retrieve information per each village on-click, as well as filter the villages according to distance from the river or by name.
The app also calculates and dynamically returns some <b>statistics</b> based on the villages visible in the map. These are displayed in an <b>histogram</b> showing the number of villages against the distance from the rivers and in a table reporting the <b>total population</b> figure and the <b>average population</b> per village in the current selection.</p>


<a target="_blank" href="https://marcopizzo.shinyapps.io/ShinyAfrique/">Open the Web App</a>
<div style="text-align: center">
<a target="_blank" href="https://marcopizzo.shinyapps.io/ShinyAfrique/">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:100%; margin-top: 0.1em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 2 - View of the Shiny Afrique landing page."
  class="medium" src="/images/cholera_drc/shinyapp_layout_min.jpg">
<figcaption>Figure 2 - View of the Shiny Afrique landing page.&nbsp;&nbsp;<i class="fas fa-external-link-alt"></i></figcaption>
</figure>
</div>
</a>
</br></br>
<!-- Div End of Page -->
</div>

    

