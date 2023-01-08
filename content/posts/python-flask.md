---
title: "Raster exploration with Python and Flask"
date: 2022-10-22T11:40:14+02:00
publishdate: 2022-10-22T11:40:14+02:00
imagethumb: "/images/raster_python_flask/small_raster_py_flask.jpg"
image: "/images/raster_python_flask/raster_py_flask.jpg"
description: "Explore remotely sensed images through an interactive tool built with Python and Flask"
comments: false
imgcaption: ""
lens: "false"
zoom: "true"
---




<div class='container'>
<h1>1. Project aim</h1>

<p>
With this project, I wanted to create an interactive tool with Python and Flask to allow both experienced users and those with no previous GIS experience to easily retrieve information on raster files with a few simple clicks.<br>
<p></p>
Often we download <b>remotely sensed data</b> in raster format from different sources, which could be for example the Digital Elevation Model (DEM) from the
<a target="_blank" href="https://earthexplorer.usgs.gov/">U.S. Geological Survey (USGS)</a>
website, or the human presence dataset from the 
<a target="_blank" href="https://ghsl.jrc.ec.europa.eu/datasets.php">Global Human Settlement Layer (GHSL)</a>
 page, or maybe a colleague sends through some raster data files. In all these cases if we forget to take note of the raster metadata, when we revisit the files we have to inspect them again, either using GIS software or by writing lines of code. A lot of overhead if we just want to retrieve basic information quickly! <br> 
Using Python and Flask I built an <b>interactive tool</b> called <b>“Bird’s eye viewer”</b> that can help users have a glance at the raster metadata and visualise a raster preview quickly with a few clicks. 

</p>

<h2>2. What is a raster?</h2>

<p>
Remotely sensed images such as aerial photographs, imagery from satellites, or imagery from sensors are saved as raster. In its simplest form, a raster is a matrix of cells (or pixels), organized into a grid of rows and columns, with each cell containing a value representing information <i>(ESRI)</i>. <br>
Figure 1 represents a simple raster data structure, highlighting columns and rows directions from their point of origin, which by convention for the raster is the top left corner. This differs from the geographical coordinates point of origin, which is the bottom left corner.  
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:70%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 1 – Simple raster data structure."  class="medium" src="/images/raster_python_flask/raster_image_example.png">
<figcaption>Figure 1 – Simple raster data structure represented in both: the image space (left) and the coordinate space (right).</figcaption>
</figure>
</div>
<br>

<p>
Raster files not only contain cell value but also metadata information, necessary to correctly display and allocate the raster grid in the coordinate space, which in other words means showing the raster in the correct geographical location. The main metadata information is listed below and can be retrieved using GIS software such as QGIS or ArcGIS or directly using specific libraries in languages such as R or Python.  
</p>
<p>The main raster metadata: </p>
<ul>
    <li>
    Width – the width of the raster data is the number of cells in the X direction;
    </li>
    <li>
    Height – the height of raster data is the number of pixels in the Y direction;
    </li>
    <li>
    Resolution – represents the cell size; all cells have the same dimensions and shape, higher resolution means more pixels, while lower resolution means fewer pixels;
    </li>
    <li>
    Coordinate reference systems (CRS) - projection and spatial reference information;
    </li>
    <li>
    Driver – in simple words a driver allows the information saved in a file to be translated into something a programming language or software can understand, read and display. There are different drivers for raster files, for more information see the <a target="_blank" href="https://gdal.org/drivers/raster/index.html">GDAL</a> documentation;
    </li>
    <li>
    Spatial extent – The spatial extent (bounding box) is represented by the "X” and “Y" coordinates of the raster corners in the geographic space. To get the full set of coordinates are only necessary four numbers (X min, X max, Y min, Y max);
    </li>
    <li>
    Band – Rasters can be single-band or multi-band. A multi-band raster consists of many rasters stacked on top of each other. A simple example of a multi-band raster can be an RGB (Red-Blue-Green) image, in which each layer represents a colour.
    </li>
</ul>

<br>

<h1>3. The “Bird’s eye viewer” Web Application</h1>
<h2>3.1 Technology overview</h2>
<p>
The project was developed using Python to handle the two macro functionalities needed: 
(1) the raster analysis and (2) the front-end construction and rendering, to display the results. <br>
The first functionality is built using the rasterio and numpy libraries, while the latter is built leveraging the Flask web application framework. 
Below is the list of the libraries needed for this project, also listed in the requirement.txt file in <a target="_blank" href="https://github.com/marcopizzolato-mp/flask-python">GitHub</a>. 

</p>

<div style="text-align: left">
<figure class="figcap framelight"><blockquote><code style = "line-height:0.2">
flask~=2.2.2 <br>
matplotlib~=3.6.2  <br>
numpy~=1.23.5  <br>
Werkzeug~=2.2.2  <br>
rasterio~=1.3.4  <br>
</blockquote></code></figure>

<a target="_blank" href="https://github.com/marcopizzolato-mp/flask-python"><i class="fab fa-github">&nbsp;&nbsp;</i>Visit Git Repo</a></figcaption>
</div>

<p>
The project develops in three python files: the app.py and the rasterhandler.py, which contain respectively the code for the first and the second macro functionalities and the test_rasterhandler.py, which contains unit tests. The paragraphs below describe the content of these files.
</p>


<h2>3.2 Python object-oriented structure - rasterhandler.py</h2>
<p>
As mentioned the rasterhandler.py module contains the object, methods and functions necessary to analyse a raster file, retrieving the information that needs to be displayed in the front-end. It contains the elements listed below. 
</p> 
<h3>3.2.1 Raster object </h3>
<p>
The purpose of this object is to store efficiently raster metadata as strings and raster data as an array, to facilitate the retrieval of these through specific get methods. <br>
From the code snippet in Figure 2, it is possible to see the definition of the raster Class, with its private attributes filled upon the creation of the object itself. In the last rows is also available an example of the get methods used to retrieve the private attribute values from the raster Class. The raster Class has one get method for each one of the attributes. 
</p> 

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:60%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 2 – raster class and private attributes"  class="medium" src="/images/raster_python_flask/code_snippet_raster_object.png">
<figcaption>Figure 2 – code snippet that creates the raster Class and fills the private attributes.</figcaption>
</figure>
</div>

<br>

<h3>3.2.2 read_raster function</h3>
<p>
The function read_raster takes as an input parameter a raster file Path object, opens the raster file as a rasterio dataset object and passes it to the raster object Class, creating one instance of it. This raster object instance is then passed as output with the return statement. <br>
In addition to the above, the function contains also the code to raise some exceptions if the parameter passed is not correct (Figure 3). In particular, exceptions will be raised if: (1) the input parameter is not a Path object, (2) the target file does not exist and (3) if the file does not have a .tif extension.  
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:70%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 3 read_raster function"  class="medium" src="/images/raster_python_flask/code_snippet_read_raster_function.png">
<figcaption>Figure 3 - Code snippet of the read_raster function.</figcaption>
</figure>
</div>

<br>

<h3>3.2.3 raster_metadata function</h3>
<p>
The raster_metadata function takes as input an instance of the raster Class, calling the get method of the object to retrieve all the attributes of interest, which are passed into a dictionary. This dictionary is then returned with the return statement at the end of the function.
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:65%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 4 raster_metadata"  class="medium" src="/images/raster_python_flask/code_snippet_metadata_function.png">
<figcaption>Figure 4 – Code snippet of the raster_metadata function.</figcaption>
</figure>
</div>

<br>

<h2>3.3 Flask Structure – app.py</h2>
<p>
Flask is a very accessible micro-framework, which provides the essential components for developing web applications. Some of these essential components are those used in this project, such as request handling, sessions and routing. In a micro-framework, additional functionalities are developed by building or using additional modules or use extensions, as we did in this current project, where we built a module to handle a raster object and its metadata. 
<br>
The web application is built using the code in the app.py file, which does the following: (1) initiate an instance of Flask app object, (2) create dynamically the HTML pages through the View functions and (3) assigns to each one of those a specific URL with the decorator. 
Below there is a detailed explanation of each one of these items:

</p> 
<h3>3.3.1 Flask app object</h3>
<p>
Creating an instance of the Flask object is a straightforward action, and can be done with the line of code in Figure 5 below. In the code in the same Figure, are also set some global parameters used across the Views when rendering the HTML templates. 
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:70%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 5 Flask app object and global parameters"  class="medium" src="/images/raster_python_flask/code_snippet_initiate_flask.png">
<figcaption>Figure 5 - Code snippet to create the Flask app object and set some global parameters. </figcaption>
</figure>
</div>
 
<br>

<h3>3.3.2 Flask Views</h3>
<p>
To display something on the browser it is necessary to create Views, and in Flask this is done through functions. In our web application, each View function calls one of the HTML templates and therefore we can say that each View corresponds to a page. This is true for all the functions except for the upload_file() one, which handles the POST request to upload the file. 
Figure 6 shows the code that creates the View for the homepage, while Figure 7 shows a summary list of all the five View functions, four of which are used to create website pages, while one handles the POST request, as mentioned above.  
</p>


<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:55%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 6 function to create the website homepage"  class="medium" src="/images/raster_python_flask/code_snippet_view_homepage.png">
<figcaption>Figure 6 - Code snippet with the View function to create the website homepage. </figcaption>
</figure>
</div>

<br>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:55%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 7 Summary of all the Views"  class="medium" src="/images/raster_python_flask/code_snippet_views.png">
<figcaption>Figure 7 – Summary of all the Views created in the app.py file.</figcaption>
</figure>
</div>

<br>

<p>
Looking at the return statement in Figure 6 we can see that we are calling an HTML template (index.html), passing as additional parameters the image logo and the footer text. An HTML template is an empty framework that can be filled with dynamic content. This is a convenient system to propagate changes very quickly and efficiently, for example to change the logo across all pages the action is only on a single line of code, and the change will be reflected across all the pages. <br>
Figure 8 shows a code snippet of the HTML template file that controls the general layout of all the pages. It is possible to notice the static structure and the dynamic content, which will be enclosed in the "block content". 
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:65%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 8 Flask index.html page"  class="medium" src="/images/raster_python_flask/layout_html.png">
<figcaption>Figure 8 - Code snippet with the HTML template to create the Flask index.html page.</figcaption>
</figure>
</div>

<br>

<h3>3.3.3 Route decorator</h3>
<p>
The purpose of the route decorator is to assign a specific URL to the different Flask View functions, and therefore website pages. The association between each View function and the page URL is done by using the route() decorator (Figure 6 and Figure 7). 
</p>

<h2>3.4 Unit testing - test_rasterhandler.py</h2>
<p>
When building a script to solve a task, the first thing to do is to break down the initial task into smaller ones, that are easier to solve. Each smaller task can be further broken down into single actions, such as reading a file, performing some data conversion, saving the data in the database, etc. All these actions can be isolated and considered the building blocks that once put together will start solving the smaller tasks, until arriving to solve the initial one. 
Each one of these blocks can be considered as a unit and it is good coding practice to write some tests, to ensure that each unit always performs as designed returning the expected results. These tests are called Unit Tests and are the first level of software testing. <br> 
In essence, a Unit Test consists of some code that states the expected output for some given inputs. If the code has been changed and the output will not match the expected one the Unit Test will fail. 
To write tests for this project has been used the <a target="_blank" href="https://docs.pytest.org/en/7.2.x/">pytest</a>  framework, which makes it easy to write small, readable tests.<br>
In Figure 9 below there is a code snippet that summarises some of the tests written for this project, with the docstring describing the scope of each test. 
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:65%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 9  pytest tests"  class="medium" src="/images/raster_python_flask/code_snipet_test.png">
<figcaption>Figure 9 - Code snippet with some of the pytest tests. </figcaption>
</figure>
</div>

<br>


<h2>3.5 User interface and user interactions</h2>
<p>
The web app user interface has four pages: “New upload”, “File Information”, “Metadata” and a “Thumbnail” page graphically represented in Figure 10, which correspond to the View functions illustrated in Figure 7. <br>
The child pages are not available when the user first visits the home page, but become available after a raster file is uploaded and analysed. From that point onwards, all pages are available and, at any point, the user can reach again the “New Upload” page and perform a new upload. 
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:55%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 10 – Diagram depicting the structure of the website pages"  class="medium" src="/images/raster_python_flask/diagram.png">
<figcaption>Figure 10 – Diagram depicting the structure of the website pages. </figcaption>
</figure>
</div>

<br>


<p>
Figures 11 to 14 show the layout of the different pages when the website is served through a browser. <br>
Figure 11 shows the homepage of the website with the “New Upload” page, where the user can upload a raster file using the “Choose file” button, and pressing the “Submit” button to trigger the raster analysis. 
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:70%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 11 – Web App Homepage"  class="medium" src="/images/raster_python_flask/new_upload_page.png">
<figcaption>Figure 11 – Web app Homepage which is the "New Upload" page.</figcaption>
</figure>
</div>

<br>

<p>
After the raster analysis is completed all the website pages become available in the horizontal menu bar, and the user is redirected to the “File Info” page, where some basic information on the file, such as the file name and file size, is displayed (Figure 12). From this point onwards the user can move across all the pages with no restrictions.
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:70%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 12 – File Info page"  class="medium" src="/images/raster_python_flask/raster_info_page.png">
<figcaption>Figure 12 – "File Info" page.</figcaption>
</figure>
</div>

<br>

<p>
The user can at this point access the “Metadata” or the “Thumbnail” pages (Figures 13 and 14) to have an overview of the metadata information or have a visual glance at the raster content with a thumbnail image.
</p>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:70%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 13 – Metadata page"  class="medium" src="/images/raster_python_flask/raster_metadata_page.png">
<figcaption>Figure 13 – Metadata page.</figcaption>
</figure>
</div>

<br>

<div style="text-align: center">
<figure class="figcap framelight">
<img style="border-radius: 0.15rem; max-width:70%; margin-top: 0.8em; margin-bottom: 0.3em; border:1px solid rgb(202, 202, 202); padding:3px;"  alt="Figure 14 – Thumbnail page"  class="medium" src="/images/raster_python_flask/raster_thumb_page.png">
<figcaption>Figure 14 – Thumbnail page. </figcaption>
</figure>
</div>

<br>

<h2>4. Further developments</h2>

<p>
This web application is only at its first version and there some features that will be added in the next versions. Currently the code has been restricted to deal only with single-band raster images in .tif format. The next development will be to expand to other commonly used GDAL drivers, allowing the user to submit multi-band raster files.  
</p>



<h2>5. References</h2>
<div style="text-align: left">
<figure><code style = "line-height:0.2; font-style:italic">
- ESRI, "What is raster data?" https://desktop.arcgis.com/en/arcmap/latest/manage-data/raster-and-images/what-is-raster-data.htm 
<br>
- GDAL, https://gdal.org/drivers/raster/index.html
<br>
- PyTest, https://docs.pytest.org/en/7.2.x/
<br>
Other sources:
<br>
- Rasterio, https://rasterio.readthedocs.io/en/latest/index.html
<br>
- Flask, https://flask.palletsprojects.com/en/2.2.x/
<br>
Creator Marco Pizzolato - This project is licensed under the MIT License 
<br>
</code></figure>
</div>



<!-- Div End of Page -->
</div>

    

