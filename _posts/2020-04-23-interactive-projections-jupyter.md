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

<html><head>


<!-- Load require.js. Delete this if your page already loads require.js -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js" integrity="sha256-Ae2Vz/4ePdIu6ZyI/5ZGsYnb+m0JlOmKPjt6XZ9JJkA=" crossorigin="anonymous"></script>
<script src="https://unpkg.com/@jupyter-widgets/html-manager@*/dist/embed-amd.js" crossorigin="anonymous"></script>
<script type="application/vnd.jupyter.widget-state+json">
{
    "version_major": 2,
    "version_minor": 0,
    "state": {
        "79da51ec728d4dd4a17207d2f1ace228": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "124a4d2cfaf74d6b8c87d00b7f1cf664": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "24d8074b39ea467d886c096d28beb3dc": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_79da51ec728d4dd4a17207d2f1ace228",
                "step": 1,
                "style": "IPY_MODEL_124a4d2cfaf74d6b8c87d00b7f1cf664",
                "value": 4326
            }
        },
        "b575e75e5bf1494b9152b1cd20365ca1": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "ea538e5aabad4d20808c49c5f7920984": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "bc4573026bd644bdafe98392983e0e9e": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_b575e75e5bf1494b9152b1cd20365ca1",
                "step": 1,
                "style": "IPY_MODEL_ea538e5aabad4d20808c49c5f7920984",
                "value": 3577
            }
        },
        "992d472a87a54e35a8caf7f2b555244f": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "74939c12285a44c08b7073f17d7246d1": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "9a4db5d171b24f2795f995b391fbb6a2": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "b27ee6f0dc2d4efdb574fa020e18ab73": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e58e70a0f98d487686253cb764ba7707": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "4fc5b7b0c48d49fe80e9e1e4337d3289": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -25,
                    140
                ],
                "controls": [
                    "IPY_MODEL_34ab274b422c40cb85824521c5e85b6f",
                    "IPY_MODEL_acf10b328f37429da64f3edf4ed9cea3",
                    "IPY_MODEL_e86afa70d71d4b0bafb58e300b2c0149"
                ],
                "default_style": "IPY_MODEL_74939c12285a44c08b7073f17d7246d1",
                "dragging_style": "IPY_MODEL_9a4db5d171b24f2795f995b391fbb6a2",
                "east": 180.87890625000003,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_992d472a87a54e35a8caf7f2b555244f"
                ],
                "layout": "IPY_MODEL_b27ee6f0dc2d4efdb574fa020e18ab73",
                "modisdate": "yesterday",
                "north": -8.233237111274553,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": -39.77476948529546,
                "style": "IPY_MODEL_e58e70a0f98d487686253cb764ba7707",
                "west": 99.05273437500001,
                "zoom": 4
            }
        },
        "34ab274b422c40cb85824521c5e85b6f": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "acf10b328f37429da64f3edf4ed9cea3": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "e86afa70d71d4b0bafb58e300b2c0149": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        }
    }
}
</script>
</head>
<body>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "4fc5b7b0c48d49fe80e9e1e4337d3289"
}
</script>

</body>
</html>


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


