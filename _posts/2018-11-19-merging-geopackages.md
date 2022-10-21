---
title: Merging GeoPackages
date: 2018-11-19 09:35:00
# last_modified_at: 2018-00-00 00:00:00
header: 
 overlay_image: /assets/images/blog/2018/geopackage-layers.png
 teaser: /assets/images/blog/2018/geopackage-layers.png
 caption: GeoPackage layers representation. [Source](http://www.opengeospatial.org/blog/2769). 
image: /assets/images/blog/2018/geopackage-layers.png
categories: 
 - GIS
 - Technology
 - Professional
 - Forestry
tags: [how to, QGIS, data science, Finland, forest inventory]
toc: true
# toc_label: "Unique Title"
# toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: true
---

The other day I had to deal with a problem I haven't dealt before. I have to merge a bunch of [GeoPackages](https://www.geopackage.org) (.gpkg) containing Finnish Forest data —downloaded from [here](https://www.metsaan.fi/paikkatietoaineistot) :finland: — because they had more sense if they ended up together. The reality is, GeoPackage format is something quite new to me —well, it was approved in [2014](http://www.opengeospatial.org/pressroom/pressreleases/1964)— since I've been using ESRI products, where [shapefiles](https://en.wikipedia.org/wiki/Shapefile) (.shp) are more or less the standard —they are the King 👑— and if you want to go a little bit further you have to use [file geodatabases](https://gisgeography.com/geodatabase-personal-file/). From there on you need to step up your game and have a real database engine/server like Oracle :money_mouth_face::money_mouth_face::money_mouth_face:, MS SQL :money_mouth_face:, [PostGIS](https://postgis.net) :elephant: —free and open source :+1:—​ and some [others](https://en.wikipedia.org/wiki/Spatial_database). 

## What is? and why the GeoPackage?

Well, as you can read in this [article](https://carto.com/blog/inside/fgdb-gpkg/) and I've stated above, shapefiles are the de facto standard in the geographic world. However, it's true that could be really problematic to work with them. For starters, a shapefile isn't just one file but at least 3 and usually they are 4 or more, which make things difficult when you want to deal with them at operating system level and outside any geographic viewer or file manager. Not to mention the other characteristics that make them a poor choice nowadays:

- Small storage capacity: 2GB
- Only one type of spacial features per file: points, lines, polygons, etc. 
- Poor data storage features as columns name, timestamps or small capacity of storage in fields. 
- Etc. 

ESRI came out with two substitute formats, [Personal Geodatabase and File Geodatabase](https://gisgeography.com/geodatabase-personal-file/), with problems galore. Former one was basically a Microsoft Access MDB file tuned up to store geographical data, which pose many many problems —first and most important Windows only :-1:. Latter one was an improvement, but was proprietary :lock: and ESRI never really open it up. In other words, if you don't pay, you can access de data in read only mode.

For all these reasons [OGC](http://www.opengeospatial.org) (Open Geospatial Consortium) came forward and try to create a real standard to store spacial data. GeoPackage was born and with a series of [improvements](http://www.geopackage.org/spec/#_introduction). 

- It's [SQLite](https://en.wikipedia.org/wiki/SQLite) 3 database file so it's more reliable and and language independent. 
- Multiplatform. Windows, macOS, Linux and more. 
- It's just a file, not a series of files or a directory. 
- Can store more than one kind of data in a file, even raster files. 
- Can be extended over time if it's needed an evolution. 
- And .... **IT'S [OPEN](https://en.wikipedia.org/wiki/Open_data)** :unlock:. So you never ever are going to need to pay to access your data.

You can read a little bit more about the formats and why you should use GeoPackages :package: [here](http://switchfromshapefile.org). 

## How to merge?

Now the **big question**, how do you merge several GeoPackages in only one? The answer didn't seem easy, and after a lot of rummaging  here and there I came to the conclusion that with QGIS I couldn't perform the task, at least in the graphical interface, and GDAL and the terminal was the [answer](https://gis.stackexchange.com/questions/244263/how-to-merge-multiple-geopackage-files-into-one-file-with-a-single-layer-using-o). 

However, merge one by one on the terminal was a *pain-in-the-tree* :deciduous_tree: so I decided to merge all of them using the a `for` loop. The command for one file is: 

```shell
ogr2ogr -f "format" --append destination-file origin-file 
```

Where: 

* `-f` is for format in our case `"gpkg"`. 
* `—append` is to append information fo the destination file instead of overwrite it. 

The syntax for `for` is: 

```shell
for whatever-variable in somewhere; do whatever-command with whatever-variable; done
```

So the final command should be: 

```shell
for filename in your/folder/*.gpkg; do ogr2ogr -f "gpkg" -append your/other/folder/destination-file.gpkg "$filename"; done
```

Voila... you now should have a `destination-file.gpkg` on `your/other/folder/` containing all the info of of the GeoPackages in `your/folder/`. In my case ~250mb GeoPackage file. 

## Downsides?

Yeah... one in this case. Since the info in from those grid GeoPackages was overlapping on the borders, we now have some info duplicated. I'll explain how you can clean that info in a post in the following days. 

------

PS/ The problem I faced [the other day](/blog/2018/11/05/cutting-down-the-size-of-your-repo/) related to the git repo being too *fat* and with *too-big-for-github* files was related to this GeoPackage file :smiley:. 



