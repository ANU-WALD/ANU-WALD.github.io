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

<html><head>


<!-- Load require.js. Delete this if your page already loads require.js -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js" integrity="sha256-Ae2Vz/4ePdIu6ZyI/5ZGsYnb+m0JlOmKPjt6XZ9JJkA=" crossorigin="anonymous"></script>
<script src="https://unpkg.com/@jupyter-widgets/html-manager@*/dist/embed-amd.js" crossorigin="anonymous"></script>
<script type="application/vnd.jupyter.widget-state+json">
{
    "version_major": 2,
    "version_minor": 0,
    "state": {
        "4b3ff939677d428e891dcfa833d8b35c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e15ec55ffc38435db084ee417075dbde": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "b22dead6092e45d68ad12f498b2bd31d": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_4b3ff939677d428e891dcfa833d8b35c",
                "step": 1,
                "style": "IPY_MODEL_e15ec55ffc38435db084ee417075dbde",
                "value": 4326
            }
        },
        "2e93055e05404f9380b5979209f3dc72": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "74941a11c6564f29acc3332674fc84e0": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "0f2ae13dc69d4eb7874bc313f48d0cd0": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_2e93055e05404f9380b5979209f3dc72",
                "step": 1,
                "style": "IPY_MODEL_74941a11c6564f29acc3332674fc84e0",
                "value": 3577
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
    "model_id": "b22dead6092e45d68ad12f498b2bd31d"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "0f2ae13dc69d4eb7874bc313f48d0cd0"
}
</script>

</body>
</html>

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

<html><head>


<!-- Load require.js. Delete this if your page already loads require.js -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js" integrity="sha256-Ae2Vz/4ePdIu6ZyI/5ZGsYnb+m0JlOmKPjt6XZ9JJkA=" crossorigin="anonymous"></script>
<script src="https://unpkg.com/@jupyter-widgets/html-manager@*/dist/embed-amd.js" crossorigin="anonymous"></script>
<script type="application/vnd.jupyter.widget-state+json">
{
    "version_major": 2,
    "version_minor": 0,
    "state": {
        "76af33480e2045a99cf0a46bffa76ac6": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "51bc06acd8ba438ea33a9d6eaf798065": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "01a57973ca4f424882d959c6f7e90269": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_76af33480e2045a99cf0a46bffa76ac6",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_51bc06acd8ba438ea33a9d6eaf798065"
            }
        },
        "4defbe2015604d05af9d798fb0ef2677": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "6543e4e781294dd18df2b5769806e0ab": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "b677222527934c92b5c13b1479134bb8": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_4defbe2015604d05af9d798fb0ef2677",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_6543e4e781294dd18df2b5769806e0ab"
            }
        },
        "e1971aa0206047e98b750947b45aa133": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f6e2339f26b04bba85de243b25530d42": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "d7fcb7ac9eda467f9c817a8ca0a73336": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_e1971aa0206047e98b750947b45aa133",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_f6e2339f26b04bba85de243b25530d42"
            }
        },
        "5f7aadcb928e4e629888ff96a16169e7": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d03880cfbd504404aca424093ea83044": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "e4e0ff1d918d4a8fb609338b4fb9a7c2": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_5f7aadcb928e4e629888ff96a16169e7",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_d03880cfbd504404aca424093ea83044"
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
    "model_id": "01a57973ca4f424882d959c6f7e90269"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "b677222527934c92b5c13b1479134bb8"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "d7fcb7ac9eda467f9c817a8ca0a73336"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "e4e0ff1d918d4a8fb609338b4fb9a7c2"
}
</script>

</body>
</html>


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

<html><head>


<!-- Load require.js. Delete this if your page already loads require.js -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js" integrity="sha256-Ae2Vz/4ePdIu6ZyI/5ZGsYnb+m0JlOmKPjt6XZ9JJkA=" crossorigin="anonymous"></script>
<script src="https://unpkg.com/@jupyter-widgets/html-manager@*/dist/embed-amd.js" crossorigin="anonymous"></script>
<script type="application/vnd.jupyter.widget-state+json">
{
    "version_major": 2,
    "version_minor": 0,
    "state": {
        "569235dfff8447239a6e99a07f711cce": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "7f172397d4e2400e93d7949c81aa9982": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "be2233b23dd14a97afc0e8631467f71e": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_569235dfff8447239a6e99a07f711cce",
                "step": 1,
                "style": "IPY_MODEL_7f172397d4e2400e93d7949c81aa9982",
                "value": 4326
            }
        },
        "2b526e2c79864693aacf5425e28cca49": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "94fff81933f749faa63295fc2775c128": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "360a2a2fdd1b44d588f3e6d2be1065f5": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_2b526e2c79864693aacf5425e28cca49",
                "step": 1,
                "style": "IPY_MODEL_94fff81933f749faa63295fc2775c128",
                "value": 3577
            }
        },
        "7ad9042e52334dffa1671555458c581c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "90e04edcc01b4845b8a76224bd6476f9": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "b44bc2c9b03f4f499c09c586113bfde9": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_7ad9042e52334dffa1671555458c581c",
                "step": null,
                "style": "IPY_MODEL_90e04edcc01b4845b8a76224bd6476f9"
            }
        },
        "741ed7f2637542d4825f434b97ac15ee": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "6cc98d9a87d04c2faaaaf8667f8d9874": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "89c37390a578453b8cc2f7f7e0ee217d": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_741ed7f2637542d4825f434b97ac15ee",
                "step": null,
                "style": "IPY_MODEL_6cc98d9a87d04c2faaaaf8667f8d9874"
            }
        },
        "8ff3d782d3b945a7982827eca4c365d3": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "77707ad7a35548a995b4e8775e71fc87": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "feaf3f6af04d4873a972ac636f36dcbc": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_8ff3d782d3b945a7982827eca4c365d3",
                "step": null,
                "style": "IPY_MODEL_77707ad7a35548a995b4e8775e71fc87"
            }
        },
        "21408bc9220d4cd3a3445963d1ea54a9": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "6d94c301bd5b4c87851fbbf7645d0899": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "03ec5ad531a643d394835c29a35dde62": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_21408bc9220d4cd3a3445963d1ea54a9",
                "step": null,
                "style": "IPY_MODEL_6d94c301bd5b4c87851fbbf7645d0899"
            }
        },
        "e3ea0625b4e64e7ea36884a283d08d39": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "b21a62d2875448f2af09b4075039b44b": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "c931888fc36044ba9208d05dded1f707": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_e3ea0625b4e64e7ea36884a283d08d39",
                "step": 1,
                "style": "IPY_MODEL_b21a62d2875448f2af09b4075039b44b",
                "value": 4326
            }
        },
        "4fc891dc6cdb4871a3c30f31e4c59bea": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "431a8b52ad154bb8a7dd7ba68c69eb3c": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "46381dc3871c4f42aed093c7edb08846": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_4fc891dc6cdb4871a3c30f31e4c59bea",
                "step": 1,
                "style": "IPY_MODEL_431a8b52ad154bb8a7dd7ba68c69eb3c",
                "value": 3577
            }
        },
        "89edf16ff8264e3890ac1a902d00c51b": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5e18e2cf188744ddabd56bc7af95e3ea": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "9243bb75ee8d405c9a06edadd5893706": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_89edf16ff8264e3890ac1a902d00c51b",
                "step": null,
                "style": "IPY_MODEL_5e18e2cf188744ddabd56bc7af95e3ea"
            }
        },
        "cd7734c781454150bce97cd3850912ea": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "0818d505b44a4aa4ba7a240f3608634a": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "ceaf6f5fbb87417eb355f0a04b8fc106": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_cd7734c781454150bce97cd3850912ea",
                "step": null,
                "style": "IPY_MODEL_0818d505b44a4aa4ba7a240f3608634a"
            }
        },
        "9617f9919f9b413c942771c0b2f0496e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "3977d562473f4d548611e228344fcabc": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "a38391d69eb0416fb320e1b34bf8fea5": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_9617f9919f9b413c942771c0b2f0496e",
                "step": null,
                "style": "IPY_MODEL_3977d562473f4d548611e228344fcabc"
            }
        },
        "49401003c2964662b982f36b9f3c15aa": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "6185520c1da843d199971aba209db399": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "1ac85b4e30f04a53aee817c2a45ba84e": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_49401003c2964662b982f36b9f3c15aa",
                "step": null,
                "style": "IPY_MODEL_6185520c1da843d199971aba209db399"
            }
        },
        "718d9c0322964bcdb6015718771d0bc8": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "38319545420c41a0ae935d91c26fdda7": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "8a4130835889435ea4bad9c8f5b44e9f": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_718d9c0322964bcdb6015718771d0bc8",
                "step": 1,
                "style": "IPY_MODEL_38319545420c41a0ae935d91c26fdda7",
                "value": 4326
            }
        },
        "85d9144574774418b6424c01f8fa8891": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "715352a1dd44448495f9074678644b75": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "9573699c54424bb29715a9379067ebae": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_85d9144574774418b6424c01f8fa8891",
                "step": 1,
                "style": "IPY_MODEL_715352a1dd44448495f9074678644b75",
                "value": 3577
            }
        },
        "6f6a246fca6048a1b60f829270c55d6b": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "b43fab3b042a47b48633747531243b62": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "4fe9b8fe69174555b7b91bf000601d7a": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_6f6a246fca6048a1b60f829270c55d6b",
                "step": null,
                "style": "IPY_MODEL_b43fab3b042a47b48633747531243b62"
            }
        },
        "388b8b6b158648a9affaaabe2d73a709": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "c83382146b2c405e9fe8c45cd2226ce0": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "43a45380321d4aa8bcb797af6cd25e44": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_388b8b6b158648a9affaaabe2d73a709",
                "step": null,
                "style": "IPY_MODEL_c83382146b2c405e9fe8c45cd2226ce0"
            }
        },
        "ee46016efa9749e4990f4692795c7832": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "0a2149376a034557a47ab7d74e71eda2": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "d4e70566b1fd43d7b6df189dda7874ef": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_ee46016efa9749e4990f4692795c7832",
                "step": null,
                "style": "IPY_MODEL_0a2149376a034557a47ab7d74e71eda2"
            }
        },
        "2536002de0ad43ae9061b8b6ea549e12": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e72b735b94a14409ad089de6443d39f9": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "97268cec51dc424993e8c2a3f102bd4d": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_2536002de0ad43ae9061b8b6ea549e12",
                "step": null,
                "style": "IPY_MODEL_e72b735b94a14409ad089de6443d39f9"
            }
        },
        "681042757c1446d493fe82ee247d3b72": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "154a84a7953f4ce5b07fd6cfef1982d1": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "8755fd7af2d04c5b80761381c7dbd34b": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_681042757c1446d493fe82ee247d3b72",
                "step": 1,
                "style": "IPY_MODEL_154a84a7953f4ce5b07fd6cfef1982d1",
                "value": 4326
            }
        },
        "580fecaf47ad4ff19a7fad4929cc4494": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "cf63c5804de14971ae89f6dd8291d095": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "1f62a9019a5d407b92f07ba4fc04c28e": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_580fecaf47ad4ff19a7fad4929cc4494",
                "step": 1,
                "style": "IPY_MODEL_cf63c5804de14971ae89f6dd8291d095",
                "value": 3577
            }
        },
        "82218b578385456bba0d438b4c9b3e90": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "b7f60b338a08449eab7bba0d725e2466": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "097a37d3c0f144a3a40c37ded7eb1f02": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_82218b578385456bba0d438b4c9b3e90",
                "step": null,
                "style": "IPY_MODEL_b7f60b338a08449eab7bba0d725e2466"
            }
        },
        "8b788da8f7774da8a6efa1527509663a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "ffcff5271d484a98acb4b8dea8a21f5c": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "324a6ce523a343d8953d86bc823046db": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_8b788da8f7774da8a6efa1527509663a",
                "step": null,
                "style": "IPY_MODEL_ffcff5271d484a98acb4b8dea8a21f5c"
            }
        },
        "ed83973cea074b9dbfc446297d961c10": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "0fd05286add74cf3a0b7a4b1294b6c0a": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "100adb5af7c54ba0b3ee98a654e1edff": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_ed83973cea074b9dbfc446297d961c10",
                "step": null,
                "style": "IPY_MODEL_0fd05286add74cf3a0b7a4b1294b6c0a"
            }
        },
        "cb18015f9519477e88dc4478b111456f": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "06cc7e3188a048739023995c5ec8f803": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "ae5f1b74a84749f2ab43a2e3c838b512": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_cb18015f9519477e88dc4478b111456f",
                "step": null,
                "style": "IPY_MODEL_06cc7e3188a048739023995c5ec8f803"
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
    "model_id": "097a37d3c0f144a3a40c37ded7eb1f02"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "324a6ce523a343d8953d86bc823046db"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "100adb5af7c54ba0b3ee98a654e1edff"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "ae5f1b74a84749f2ab43a2e3c838b512"
}
</script>

</body>
</html>


### Now go back to the interactive map and draw a rectangle to see the values in these cells updated.


```python

```
