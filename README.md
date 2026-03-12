## Laboratory 3: 
3D Computational Modeling: DEM–Vector Integration and GeoJSON Service Delivery 

## Overview: 
Laboratory 3 focuses on transitioning from planar spatial analysis to three-dimensional (3D) computational modeling by integrating raster and vector data. Unlike simple 2.5D visualization, this exercise requires to construct LineString geometries where each vertex includes z value derived from sampling elevation data from a Digital Elevation Model (DEM). The technical workflow involves road vectors from a PostGIS database using GeoPandas, while the DEM is loaded directly from a GeoTIFF file using Rasterio. To ensure the 3D model accurately captures terrain variation, students must perform densification which is a process of inserting new vertices along the road lines at fixed intervals,wherein in this exercise is 10meters.

## How to run analysis.py
1.  Activate the virtual environment 
2. Run analysis.py to unify various data sources into a cohesive 3D spatial model and to handle the transformation of these analytical results into a 3D GeoJSON format for export and service delivery, acting as the link between raw storage and interactive 3D visualization.

## Outputs expected in output
 Elevation values from the DEM is expected to be injected into the coordinate tuples to create (x, y, z) geometries. The final expected output is a 3D GeoJSON file where the z values are preserved within the coordinate structure, allowing for realistic visualization in a QGIS 3D environment.

## Milestones and reflections
1. Hybrid IO Milestone Refection 
----------------------------------------------------------------------------------------------------------
Roads are retrieved from PostGIS because the vector data was structured, relational, and simplified to be managed in a database (pgAdmin). With the use of a spatial database like PostGIS, it allows for centralized storage, multi-user access, and the ability to perform relational queries.

For the raster file, the DEM is loaded directly because raster data is grided hence it is likely easier for data to be managed in a singular file. Unlike vector data, rasters consist of massive grids of pixels, and loading directly via libraries enable for independent processing and elevation extraction without the overhead of database storage.

Hybrid IO reflects real-world architecture by demonstrating different types of spatial data that require different storage logics. For example, vector data is stored in relational databases for management and querying while raster data is kept in optimized file formats for high-speed numeric processing. The use of python serves as bridge consists of commands, unifying these different storage models into a single representation where algorithms can operate on both simultaneously.

At this stage, spatial analysis do not occur at the Hybrid IO stage. This part of the laboratory is focused on the Input component of the GIS workflow. It is to establish the connection to the data sources and verify that they represent the data correctly in Python before any 3D modeling or elevation sampling begins.

2. 3D Geometry Construction Milestone
---------------------------------------------------------------------------------------------------------
Densification id importance because it allows to insert new points at a fixed interval as it was done in SAMPLE_STEP, ensuring the road ovelaid as part of the terrain or elevation model. However, CRS alignment is a must before processing due to the structures of the vector data (roads) and the raster data (DEM) where these must share the same spatial coordinate system for the sampling to be accurate. If the coordinates do not match, the sample_dem_z function will attempt to look up coordinates that do not exist or point to the wrong geographic location, resulting incorrect elevation values.

Unlike symbolic extrusion where a 2D spatial structure is simply stretched upward by a single height value, spatial data containing true Z values means each individual vertex of the LineString is now a 3D coordinate (x, y, z). In a 3D viewer od QGIS, the road follows the actual rise and fall of the land surface rather than appearing as a flat lines separately.

3. Creation of 3D Output Layer
--------------------------------------------------------------------------------------------------------------
The coordinate structure is preserved when 3D geometry is being exported to GeoJSON. Each vertex is stored as a list of three numbers [longitude, latitude, altitude] (or [x, y, z]). However, the 3D layer's structure features/characteristics not formally expressed. The GeoJSON standard (RFC 7946) only officially defines Point, LineString, and Polygon. It doesn't have a specific "3DLineString" type hence it just allows the coordinate array to have a third element.

Nonetheless, GeoJSON is still label it as "LineString". This shows the difference between the actual values and the schema. It is designed for 2D web mapping, so it uses familiar 2D labels even when the content is 3D. In QGIS, it treats this as True 3D geometry because it reads the third coordinate of each vertex. Unlike 2.5D where a flat shape is extruded by one value, each point on the road spatial data has its own specific height.

To preserve 3D semantics more explicitly, I would prioritize 3D Tiles or glTF because they allow for efficient streaming of hierarchical spatial data and complex textures while maintaining detailed object attributes. Anotehr is the use of PostGIS. It provides spatial indexing and supports large quantities of geometries directly within a relational database. For high-fidelity portability, the GeoPackage format could also used because its serves as a tool to store 3D primitives or point clouds, ensuring that spatial relationships and metadata remain tightly coupled in a single, standardized file. 