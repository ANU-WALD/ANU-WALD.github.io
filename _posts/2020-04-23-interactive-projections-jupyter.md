---
layout: post
title:  Interactive Jupyter notebook for coordinates transformation
categories: [python]
---

# Graphical interactive coordinate transformation app

### This first cell defines the map where will draw the boxes with the coordinates we want to reproject.


```python
from ipyleaflet import Map, basemaps, basemap_to_tiles, DrawControl
import geojson

def bbox(coord_list):
     box = []
     for i in (0,1):
         res = sorted(coord_list, key=lambda x:x[i])
         box.append((res[0][i],res[-1][i]))
 
     return box[0][0]-360, box[1][0], box[0][1]-360, box[1][1]
    

watercolor = basemap_to_tiles(basemaps.Stamen.Watercolor)

m = Map(layers=(watercolor, ), center=(-25, 140), zoom=4)

draw_control = DrawControl(circle={}, circleMarker={}, circlemarker={}, 
                           CircleMarker={}, polyline={}, marker={}, polygon={},
                          rectangle = {"shapeOptions": {"color": "#00005d","fillOpacity": 0.0}})


def handle_draw(self, action, geo_json):
    #self.clear()
    lon_min.value,lat_min.value,lon_max.value,lat_max.value=bbox(list(geojson.utils.coords(geo_json['geometry'])))
    x_min.value,y_min.value=transform(lon_min.value,lat_min.value)
    x_max.value,y_max.value=transform(lon_max.value,lat_max.value)
    
draw_control.on_draw(handle_draw)

m.add_control(draw_control)

m
```


    Map(center=[-25, 140], controls=(ZoomControl(options=['position', 'zoom_in_text', 'zoom_in_title', 'zoom_out_t…


### Let's define the output CRS using its EPSG code


```python
from __future__ import print_function
from ipywidgets import interact, interactive, fixed, interact_manual
import ipywidgets as widgets

in_crs = widgets.IntText(value=4326,description='Input EPSG:',disabled=True)
display(in_crs)
out_crs = widgets.IntText(value=3577,description='Output EPSG:',disabled=False)
display(out_crs)
```


    IntText(value=4326, description='Input EPSG:', disabled=True)



    IntText(value=3577, description='Output EPSG:')


### These boxes will contain the corners of the drawn in geographical coordinates


```python
lon_min = widgets.BoundedFloatText(value=0.0,min=-180.0,max=180.0,description='Min Longitude:')
display(lon_min)
lat_min = widgets.BoundedFloatText(value=0.0,min=-90.0,max=90.0,description='Min Latitude:')
display(lat_min)
lon_max = widgets.BoundedFloatText(value=0.0,min=-180.0,max=180.0,description='Max Longitude:')
display(lon_max)
lat_max = widgets.BoundedFloatText(value=0.0,min=-90.0,max=90.0,description='Max Latitude:')
display(lat_max)
```


    BoundedFloatText(value=0.0, description='Min Longitude:', max=180.0, min=-180.0)



    BoundedFloatText(value=0.0, description='Min Latitude:', max=90.0, min=-90.0)



    BoundedFloatText(value=0.0, description='Max Longitude:', max=180.0, min=-180.0)



    BoundedFloatText(value=0.0, description='Max Latitude:', max=90.0, min=-90.0)


### Finally we declare the boxes that will contain the projected coordinates in the chosen projection


```python
from pyproj import Proj, transform
from pyproj import Transformer

transformer = Transformer.from_crs(f"EPSG:{in_crs.value}", f"EPSG:{out_crs.value}", always_xy=True)

def transform(x,y):
    return transformer.transform(x,y)

x_min = widgets.FloatText(value=0.0,description='Min X:')
display(x_min)
y_min = widgets.FloatText(value=0.0,description='Min Y:')
display(y_min)
x_max = widgets.FloatText(value=0.0,description='Max X:')
display(x_max)
y_max = widgets.FloatText(value=0.0,description='Max Y:')
display(y_max)
```


    FloatText(value=0.0, description='Min X:')



    FloatText(value=0.0, description='Min Y:')



    FloatText(value=0.0, description='Max X:')



    FloatText(value=0.0, description='Max Y:')


### Now go back to the interactive map and draw a rectangle to see the values in these cells updated.


```python

```
