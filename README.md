![No longer maintained](https://img.shields.io/badge/Maintenance-OFF-red.svg)
### [DEPRECATED] This repository is no longer maintained
> While this project is fully functional, the dependencies are no longer up to date. You are still welcome to explore, learn, and use the code provided here.
>
> Modus is dedicated to supporting the community with innovative ideas, best-practice patterns, and inspiring open source solutions. Check out the latest [Modus Labs](https://labs.moduscreate.com?utm_source=github&utm_medium=readme&utm_campaign=deprecated) projects.

[![Modus Labs](https://res.cloudinary.com/modus-labs/image/upload/h_80/v1531492623/labs/logo-black.png)](https://labs.moduscreate.com?utm_source=github&utm_medium=readme&utm_campaign=deprecated)

---

# Polygon Experiment

Experiments with placing a marker around the center of a polygon.  This repo has an accompanying [blog post](http://moduscreate.com/placing-markers-inside-polygons-with-google-maps/).

![demo](screenshot.png)

Find approximate center point of an arbitrary polygon on Google Maps.  Process:

* Add a `getBoundingBox` method to `google.maps.Polygon.prototype` which returns a LatLngBounds object (rectangle) that entirely contains an arbitrarily complex polygon
* Get the center of that bounding box
* If the center of the bounding box is within the area of the polygon, put the marker there
* If the center of the bounding box is not within the area of the polygon then:
	* Work out the height of the bounding box
	* Look at points North, East, South and West of the center at 5% increments of the total height and width of the bounding box
	* If any of those points is within the area of the polygon, place the marker there and stop looking

This may not be foolproof but should get a point within the polygon that's good enough.  As this moves up and down the bounding box 
looking for points within the polygon at 5% height increments, it could miss a very thin slice of the polygon that crosses the 
center line and never find a point... could fix this by using 1% increments and a 50 loop count for higher search "resolution" 
but lower performance.

## Examples

The following show two sample polygons where the centroid (blue) of the bounding box (shown as black outline) falls outside the polygon.  
The algotithm found the nearest point North, South, East or West and decided on a final placement where the red marker lies.

![demo1](demo1.png)

This example selected the point due East of the centroid because the polygon is much wider than it is high, so the East / West search 
increments were bigger than the North / South ones, so the Eastern border was discovered first.

![demo2](demo2.png)

## Notes

* You need to supply a Google Maps API key and set this in the link to the Google Maps API JavaScript in `index.html`
* Because we are using `google.maps.geometry` functions, the link to get the Google Maps API JavaSCript needs to include `&libraries=geometry` (see `index.html`)

## Other Ways of Doing This

There are other possible algorithms for getting similar results:

* MapBox has one [here](https://github.com/mapbox/polylabel/blob/master/index.js) with a supporting [blog post]		(https://www.mapbox.com/blog/polygon-center/)
* [Discussion on calculating centroid](http://mathcentral.uregina.ca/qq/database/qq.09.07/h/david7.html)
