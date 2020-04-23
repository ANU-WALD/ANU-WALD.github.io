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
        "b73e72c14bc24f12b914da794a3fb0df": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1699746878ea4994ada1b73d06394e31": {
            "model_name": "VBoxModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "_dom_classes": [
                    "widget-interact"
                ],
                "children": [
                    "IPY_MODEL_731a4d80871e42a58b89a3c7a8e6e429",
                    "IPY_MODEL_065dda93bf74412695b67ef0c460ff03"
                ],
                "layout": "IPY_MODEL_b73e72c14bc24f12b914da794a3fb0df"
            }
        },
        "75e4b46c23d14c8b88b44bf5b2f613d6": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5200e36ff486484589a29ae5c93354cb": {
            "model_name": "SliderStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "731a4d80871e42a58b89a3c7a8e6e429": {
            "model_name": "IntSliderModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "x",
                "layout": "IPY_MODEL_75e4b46c23d14c8b88b44bf5b2f613d6",
                "max": 30,
                "min": -10,
                "style": "IPY_MODEL_5200e36ff486484589a29ae5c93354cb",
                "value": 10
            }
        },
        "26c899ce1e0744339006dfaeb4d97636": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "065dda93bf74412695b67ef0c460ff03": {
            "model_name": "OutputModel",
            "model_module": "@jupyter-widgets/output",
            "model_module_version": "1.0.0",
            "state": {
                "layout": "IPY_MODEL_26c899ce1e0744339006dfaeb4d97636",
                "outputs": [
                    {
                        "output_type": "display_data",
                        "data": {
                            "text/plain": "100"
                        },
                        "metadata": {}
                    }
                ]
            }
        },
        "0bee97195a3b469ebd941fc2f2beb42a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5ded732b72d34d7a9b7f37f9cd5a7e32": {
            "model_name": "VBoxModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "_dom_classes": [
                    "widget-interact"
                ],
                "children": [
                    "IPY_MODEL_f360e33da41247bfb91599f60e1a3ab1",
                    "IPY_MODEL_835738f45d97443f83a015ed3fa1bd4d"
                ],
                "layout": "IPY_MODEL_0bee97195a3b469ebd941fc2f2beb42a"
            }
        },
        "122d938bfef3405ab6fe2f6d9e8a55f6": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "059f3cde596344c6821878c1bcf5fdfe": {
            "model_name": "SliderStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "f360e33da41247bfb91599f60e1a3ab1": {
            "model_name": "IntSliderModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "x",
                "layout": "IPY_MODEL_122d938bfef3405ab6fe2f6d9e8a55f6",
                "max": 30,
                "min": -10,
                "style": "IPY_MODEL_059f3cde596344c6821878c1bcf5fdfe",
                "value": -10
            }
        },
        "52a0853e85fb443a95b80d0d97225183": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "835738f45d97443f83a015ed3fa1bd4d": {
            "model_name": "OutputModel",
            "model_module": "@jupyter-widgets/output",
            "model_module_version": "1.0.0",
            "state": {
                "layout": "IPY_MODEL_52a0853e85fb443a95b80d0d97225183",
                "outputs": [
                    {
                        "output_type": "display_data",
                        "data": {
                            "text/plain": "100"
                        },
                        "metadata": {}
                    }
                ]
            }
        },
        "3f56517c5bf048df89020fa08bba384d": {
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
        "1fca28dff8ff401c9ecf3faa0153f4fd": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "4f647bd023c248ddba4cad0233411bb9": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "9219a273a384421da4315a9e7424cd33": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f3b6b87b4aa9497ba55b7e5149795940": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "2e05b37e99a1419181e7fb6cc8ce7c8c": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_24c41d913ef146ff901ac71fc1786202",
                    "IPY_MODEL_3d9891304034443e83adf277a7c4c610",
                    "IPY_MODEL_275fe24861ba42bcaef9744e21d85f48"
                ],
                "default_style": "IPY_MODEL_1fca28dff8ff401c9ecf3faa0153f4fd",
                "dragging_style": "IPY_MODEL_4f647bd023c248ddba4cad0233411bb9",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_3f56517c5bf048df89020fa08bba384d"
                ],
                "layout": "IPY_MODEL_9219a273a384421da4315a9e7424cd33",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_1fca28dff8ff401c9ecf3faa0153f4fd",
                "west": 180,
                "zoom": 5
            }
        },
        "24c41d913ef146ff901ac71fc1786202": {
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
        "3d9891304034443e83adf277a7c4c610": {
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
        "275fe24861ba42bcaef9744e21d85f48": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "fillColor": "#fca45d",
                        "color": "#fca45d",
                        "fillOpacity": 1
                    }
                }
            }
        },
        "83945737427546179a62bb83719efc6e": {
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
        "c9552a865cb649458090cb4cedd822e8": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "c4bb343c996441688a1ffd9d88c8dee6": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "2112c81cda804a47a5a09c96a8697056": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "317140ce40d24c85b9a4af32c117c1a8": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "e8f3b9a82de34f25b346845c12f802cb": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_a6f90b60d9d74c0aaa19e9bf218469b9",
                    "IPY_MODEL_53631f783b2b49a88e1d74b3ad4705d7",
                    "IPY_MODEL_a99ff20c75ef442cbd35de15b5448ccc"
                ],
                "default_style": "IPY_MODEL_c9552a865cb649458090cb4cedd822e8",
                "dragging_style": "IPY_MODEL_c4bb343c996441688a1ffd9d88c8dee6",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_83945737427546179a62bb83719efc6e"
                ],
                "layout": "IPY_MODEL_2112c81cda804a47a5a09c96a8697056",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_c9552a865cb649458090cb4cedd822e8",
                "west": 180,
                "zoom": 5
            }
        },
        "a6f90b60d9d74c0aaa19e9bf218469b9": {
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
        "53631f783b2b49a88e1d74b3ad4705d7": {
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
        "a99ff20c75ef442cbd35de15b5448ccc": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#fca45d",
                        "fillOpacity": 1
                    }
                }
            }
        },
        "0b495c72301642a6a59ace1ef52a6704": {
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
        "27e12c8a8db745efab5e07bcb9fb4c70": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "0f7dce797cc84f7c8c2bf78872082aa8": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "b4cf81b786ee4108825fe9906d97c948": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "c97434bf05864c99b30e2bf2050b6fc2": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "d07707960ba54a668b24b476584dd9d5": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_93e681799d7947b29f864baa245748d6",
                    "IPY_MODEL_60fdc40ee62243d38c2d4d38110f1448",
                    "IPY_MODEL_797420ac25b9451f86a70838978f9f0d"
                ],
                "default_style": "IPY_MODEL_27e12c8a8db745efab5e07bcb9fb4c70",
                "dragging_style": "IPY_MODEL_0f7dce797cc84f7c8c2bf78872082aa8",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_0b495c72301642a6a59ace1ef52a6704"
                ],
                "layout": "IPY_MODEL_b4cf81b786ee4108825fe9906d97c948",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_27e12c8a8db745efab5e07bcb9fb4c70",
                "west": 180,
                "zoom": 5
            }
        },
        "93e681799d7947b29f864baa245748d6": {
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
        "60fdc40ee62243d38c2d4d38110f1448": {
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
        "797420ac25b9451f86a70838978f9f0d": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#fca45d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "39f0fc426b9c4297838a88d57ea8c927": {
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
        "2a5e538148f54fd5ad242a9b777ce822": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "5f1d856ad3284b66b29987f775516206": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "26cdcaca4bc54866ba0d01c9da8054d2": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "32d9f7f2983546bfa0f9f8d25fa8ad47": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "836c1a274c4b40848bdd6e5363a9a335": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    46.49839225859763,
                    363.86718750000006
                ],
                "controls": [
                    "IPY_MODEL_61aad090c17a45a898adac38484b5de0",
                    "IPY_MODEL_e2fd8de542df4b09809f0ef587384130",
                    "IPY_MODEL_af4df3821b704b408762fad79fbb6e9f"
                ],
                "default_style": "IPY_MODEL_2a5e538148f54fd5ad242a9b777ce822",
                "dragging_style": "IPY_MODEL_5f1d856ad3284b66b29987f775516206",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_39f0fc426b9c4297838a88d57ea8c927"
                ],
                "layout": "IPY_MODEL_26cdcaca4bc54866ba0d01c9da8054d2",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_2a5e538148f54fd5ad242a9b777ce822",
                "west": 180,
                "zoom": 4
            }
        },
        "61aad090c17a45a898adac38484b5de0": {
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
        "e2fd8de542df4b09809f0ef587384130": {
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
        "af4df3821b704b408762fad79fbb6e9f": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "d7746f88243d4c459f26ce93119ffc9b": {
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
        "f2d0a7b3b06b422f9a50a73f4fbfa544": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "9a45a0328cfc4effa1874ebbf6689c44": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "216799f81cff430890992ffc09b7cdd2": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "009b7946c3c349bdafb59a82e9a5bf01": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "1c60bd72939c48d49253af6a1150839f": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_06d64a1210914a3fb8d27ecbb51900da",
                    "IPY_MODEL_815ad1c0264b4d9fb5b563b5ea5917f0",
                    "IPY_MODEL_a3e8242b35f84342827aaab4d96e1e3f"
                ],
                "default_style": "IPY_MODEL_f2d0a7b3b06b422f9a50a73f4fbfa544",
                "dragging_style": "IPY_MODEL_9a45a0328cfc4effa1874ebbf6689c44",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_d7746f88243d4c459f26ce93119ffc9b"
                ],
                "layout": "IPY_MODEL_216799f81cff430890992ffc09b7cdd2",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_f2d0a7b3b06b422f9a50a73f4fbfa544",
                "west": 180,
                "zoom": 5
            }
        },
        "06d64a1210914a3fb8d27ecbb51900da": {
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
        "815ad1c0264b4d9fb5b563b5ea5917f0": {
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
        "a3e8242b35f84342827aaab4d96e1e3f": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "37de1e5d93d64ab492ce7b9cefb707db": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "ac17736db77641108fe4adcef2e7d001": {
            "model_name": "VBoxModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "_dom_classes": [
                    "widget-interact"
                ],
                "children": [
                    "IPY_MODEL_e61f8136e04b46a2b5fb3b6f9dd599ea",
                    "IPY_MODEL_ec2d1628f60a4679982bbce1b63841f6"
                ],
                "layout": "IPY_MODEL_37de1e5d93d64ab492ce7b9cefb707db"
            }
        },
        "7a26924556e4471c9111eac79a0030ba": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "b6e80843f59e4002993b125a38e42f72": {
            "model_name": "SliderStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "e61f8136e04b46a2b5fb3b6f9dd599ea": {
            "model_name": "IntSliderModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "x",
                "layout": "IPY_MODEL_7a26924556e4471c9111eac79a0030ba",
                "max": 30,
                "min": -10,
                "style": "IPY_MODEL_b6e80843f59e4002993b125a38e42f72",
                "value": 10
            }
        },
        "e5dcffbe562d4c67b78177726f1cd95c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "ec2d1628f60a4679982bbce1b63841f6": {
            "model_name": "OutputModel",
            "model_module": "@jupyter-widgets/output",
            "model_module_version": "1.0.0",
            "state": {
                "layout": "IPY_MODEL_e5dcffbe562d4c67b78177726f1cd95c",
                "outputs": [
                    {
                        "output_type": "display_data",
                        "data": {
                            "text/plain": "100"
                        },
                        "metadata": {}
                    }
                ]
            }
        },
        "032ee9d35e6d435fa88cc03e57e8ea3a": {
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
        "f4a936cd765247a49c59fc82c584bf6c": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "2fddb181ef644708870f44c3e53f1adf": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "7edf090973d842ea8b9d1c0a35dfa7c2": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2b8055d8f0ff497d9ac425f139ff815c": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "62ba8ad6c8144a94a7797115fe01531f": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_5c22c953463f4c92a48ba4fc3c1a4456",
                    "IPY_MODEL_03318001037741d893478a0aadfdd1e9",
                    "IPY_MODEL_0c0795c1af3f4511a744b6ee31c5a4f2"
                ],
                "default_style": "IPY_MODEL_f4a936cd765247a49c59fc82c584bf6c",
                "dragging_style": "IPY_MODEL_2fddb181ef644708870f44c3e53f1adf",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_032ee9d35e6d435fa88cc03e57e8ea3a"
                ],
                "layout": "IPY_MODEL_7edf090973d842ea8b9d1c0a35dfa7c2",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_f4a936cd765247a49c59fc82c584bf6c",
                "west": 180,
                "zoom": 5
            }
        },
        "5c22c953463f4c92a48ba4fc3c1a4456": {
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
        "03318001037741d893478a0aadfdd1e9": {
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
        "0c0795c1af3f4511a744b6ee31c5a4f2": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "38e7fc7950c841868d311030471c733d": {
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
        "a87bfc896e7c4830b00dda3b2b9e89c7": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "33ef2527b6214897ae9d3e39c88725aa": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "32bdbb9a26854fa990fb255a96fb3a04": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "c2adffc25d9e4a61abe080eb05286b37": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "dbdd03347c744f648c9c4cc46a420e77": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_5b7aed076b3448228b832c9fe0469e0b",
                    "IPY_MODEL_819c7ae3e2e04651a8e74fdb3333536d",
                    "IPY_MODEL_2516feabb92d4d329a2a1771ea8fe6c6"
                ],
                "default_style": "IPY_MODEL_a87bfc896e7c4830b00dda3b2b9e89c7",
                "dragging_style": "IPY_MODEL_33ef2527b6214897ae9d3e39c88725aa",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_38e7fc7950c841868d311030471c733d"
                ],
                "layout": "IPY_MODEL_32bdbb9a26854fa990fb255a96fb3a04",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_a87bfc896e7c4830b00dda3b2b9e89c7",
                "west": 180,
                "zoom": 5
            }
        },
        "5b7aed076b3448228b832c9fe0469e0b": {
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
        "819c7ae3e2e04651a8e74fdb3333536d": {
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
        "2516feabb92d4d329a2a1771ea8fe6c6": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "7c7aad093e5b48568b1d96228a979cbe": {
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
        "6b9573eca79342e29d48034cb1699301": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "b27302b7a7d64a7b84afb21ba224df21": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "e5a768893fdb423d81b983734e064188": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d2ed436b8558412cb69bb7385cfcbde3": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "a5285ad634524e3e93b0e89767a69970": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_3ab872ce8b144367a14d71cc9c66a492",
                    "IPY_MODEL_58032eb878cb4092a1939a91733cb372",
                    "IPY_MODEL_e95c44b388884b8cbe1a0765d87667e4"
                ],
                "default_style": "IPY_MODEL_6b9573eca79342e29d48034cb1699301",
                "dragging_style": "IPY_MODEL_b27302b7a7d64a7b84afb21ba224df21",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_7c7aad093e5b48568b1d96228a979cbe"
                ],
                "layout": "IPY_MODEL_e5a768893fdb423d81b983734e064188",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_6b9573eca79342e29d48034cb1699301",
                "west": 180,
                "zoom": 5
            }
        },
        "3ab872ce8b144367a14d71cc9c66a492": {
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
        "58032eb878cb4092a1939a91733cb372": {
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
        "e95c44b388884b8cbe1a0765d87667e4": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "1af913143fc64b969a4123adb16d8d90": {
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
        "2ae35897d4754d60b01ecb70b3a3007b": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "7c8898aae9a648829da9873033568a1d": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "326eeed77f74474f9881415c9a2240e7": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e84edb49d1574d389acb1456381cd0db": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "cb12cfbbb98a4e36b9232ae551d297c4": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50,
                    354
                ],
                "controls": [
                    "IPY_MODEL_6d97199a9f6e42138c3713378e185c74",
                    "IPY_MODEL_dd28426e60d74ae4be2ef88adcc8a025",
                    "IPY_MODEL_a7c0077244eb40c9800d76da88011086"
                ],
                "default_style": "IPY_MODEL_2ae35897d4754d60b01ecb70b3a3007b",
                "dragging_style": "IPY_MODEL_7c8898aae9a648829da9873033568a1d",
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_1af913143fc64b969a4123adb16d8d90"
                ],
                "layout": "IPY_MODEL_326eeed77f74474f9881415c9a2240e7",
                "modisdate": "yesterday",
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
                "style": "IPY_MODEL_e84edb49d1574d389acb1456381cd0db",
                "zoom": 5
            }
        },
        "6d97199a9f6e42138c3713378e185c74": {
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
        "dd28426e60d74ae4be2ef88adcc8a025": {
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
        "a7c0077244eb40c9800d76da88011086": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "d0a15783c7f54ab29a7881853d90b265": {
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
        "492fa82aea77495b94472a2490ee861f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "f68f2ab0f7c045a9a3da50000fabb84e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "f5930fafcb294ab194bc563905554557": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2cac4767d2e44a148a99d686b3cb7533": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "848ec307d2b74880935bcdc57d69843e": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_12f5a8fb61a64ea0ba3c8232d276c5b1",
                    "IPY_MODEL_cd4f343cc36a4635a58c9caf42ae17fd",
                    "IPY_MODEL_9c62f5b1875d488198e1d98ae1edbf07"
                ],
                "default_style": "IPY_MODEL_492fa82aea77495b94472a2490ee861f",
                "dragging_style": "IPY_MODEL_f68f2ab0f7c045a9a3da50000fabb84e",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_d0a15783c7f54ab29a7881853d90b265"
                ],
                "layout": "IPY_MODEL_f5930fafcb294ab194bc563905554557",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_492fa82aea77495b94472a2490ee861f",
                "west": 180,
                "zoom": 5
            }
        },
        "12f5a8fb61a64ea0ba3c8232d276c5b1": {
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
        "cd4f343cc36a4635a58c9caf42ae17fd": {
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
        "9c62f5b1875d488198e1d98ae1edbf07": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "e38de60f2ce743b2b2468ff7f38a56f8": {
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
        "85f6bff6623344c3840d8f4e8ae3c0e7": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "b1defe0afd9b49baab2623a366d638ca": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "17912998afed444cac170797fd3d63da": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "64e61f06a9b54546bd7259437b2582b6": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "fef33ab96bb14ce7af6d7346da5dc3aa": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_4a6ee792006f4c52860af002a1b2b3ef",
                    "IPY_MODEL_d8557b1fdcf84ff0ab3194709b48839f",
                    "IPY_MODEL_c4f41bae3b67473195a8655fcdd209df"
                ],
                "default_style": "IPY_MODEL_85f6bff6623344c3840d8f4e8ae3c0e7",
                "dragging_style": "IPY_MODEL_b1defe0afd9b49baab2623a366d638ca",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_e38de60f2ce743b2b2468ff7f38a56f8"
                ],
                "layout": "IPY_MODEL_17912998afed444cac170797fd3d63da",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_85f6bff6623344c3840d8f4e8ae3c0e7",
                "west": 180,
                "zoom": 5
            }
        },
        "4a6ee792006f4c52860af002a1b2b3ef": {
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
        "d8557b1fdcf84ff0ab3194709b48839f": {
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
        "c4f41bae3b67473195a8655fcdd209df": {
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
        },
        "f107bc31830c4f2e877607e6e4308855": {
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
        "8bf250efe5864445a14e65ca259d7ec3": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "28ff54fade2147eaac45d3624ad84dd9": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "f630f9e35eaf4866985f0d4028928bed": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5971ce3301a24a498ca01d5bf8b6ea74": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "dcad6c8162d54c70b7d4373a60374bfb": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_096fb244ba924e908f77a0e9c216caa0",
                    "IPY_MODEL_31d442ed61f44dd1b7406351b38dbaa5",
                    "IPY_MODEL_41ee398b80764e838874ac447640f297"
                ],
                "default_style": "IPY_MODEL_8bf250efe5864445a14e65ca259d7ec3",
                "dragging_style": "IPY_MODEL_28ff54fade2147eaac45d3624ad84dd9",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_f107bc31830c4f2e877607e6e4308855"
                ],
                "layout": "IPY_MODEL_f630f9e35eaf4866985f0d4028928bed",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_8bf250efe5864445a14e65ca259d7ec3",
                "west": 180,
                "zoom": 5
            }
        },
        "096fb244ba924e908f77a0e9c216caa0": {
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
        "31d442ed61f44dd1b7406351b38dbaa5": {
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
        "41ee398b80764e838874ac447640f297": {
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
        },
        "01f5525b78d84dfeac88453a688aa4a5": {
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
        "62906d7646c549489cceab93ed24588e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "5277fb443c874b6cb0538b1278829709": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "721886dc6cf54cf79d87ce38da036e38": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "eb73d2ece9904833bc8396e8f3100bb7": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "71d36257a1e342abb951013f86816c7a": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    49.41097319969587,
                    370.63476562500006
                ],
                "controls": [
                    "IPY_MODEL_e57083b765584d7e956157fc368af9f2",
                    "IPY_MODEL_6f6f7d28121e439b8cc530b5fbc3d8ec",
                    "IPY_MODEL_b81cf00751674431b6463e56f671fe9a"
                ],
                "default_style": "IPY_MODEL_62906d7646c549489cceab93ed24588e",
                "dragging_style": "IPY_MODEL_5277fb443c874b6cb0538b1278829709",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_01f5525b78d84dfeac88453a688aa4a5"
                ],
                "layout": "IPY_MODEL_721886dc6cf54cf79d87ce38da036e38",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_62906d7646c549489cceab93ed24588e",
                "west": 180,
                "zoom": 5
            }
        },
        "e57083b765584d7e956157fc368af9f2": {
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
        "6f6f7d28121e439b8cc530b5fbc3d8ec": {
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
        "b81cf00751674431b6463e56f671fe9a": {
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
        },
        "3c00f48b3dc64bd484f02ec9d8e31812": {
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
        "8892f08428574a7fb947d136e195144e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "6cca69f867cd4be6b94dee019f4a319a": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "b8303fc0c52240a394811546c3b31ceb": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "76d61b5313a8403883fe6b4afc52b411": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "12300cecb1044a3f97d852bd3c52a2a8": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_af932b29de7b488b81f394eaf140b17d",
                    "IPY_MODEL_6fb08a9a05a545efa107f9f1dc4d9a6b",
                    "IPY_MODEL_8b80d5aed1094d8dbd64c50d3df3ba31"
                ],
                "default_style": "IPY_MODEL_8892f08428574a7fb947d136e195144e",
                "dragging_style": "IPY_MODEL_6cca69f867cd4be6b94dee019f4a319a",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_3c00f48b3dc64bd484f02ec9d8e31812"
                ],
                "layout": "IPY_MODEL_b8303fc0c52240a394811546c3b31ceb",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_8892f08428574a7fb947d136e195144e",
                "west": 180,
                "zoom": 5
            }
        },
        "af932b29de7b488b81f394eaf140b17d": {
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
        "6fb08a9a05a545efa107f9f1dc4d9a6b": {
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
        "8b80d5aed1094d8dbd64c50d3df3ba31": {
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
        },
        "808b2910b88b4d8891204e721c8ff4e7": {
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
        "021b360921834389bf8bf597b0e8d80f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "3a81c4015ef84b08bcca1d33bbf34dc2": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "98dc7636d491486aa6517f19e9736f84": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e71c28082a7f4635ab64b573271e7cfc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "0cbd0e69006f484f9cac6876d21a6675": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_b7c5aef690a84b0db1b25529566e4be7",
                    "IPY_MODEL_cff50147b8c34e79a6ec884532442e02",
                    "IPY_MODEL_d89c415e388e46249c6a38e9a2fcdde0"
                ],
                "default_style": "IPY_MODEL_021b360921834389bf8bf597b0e8d80f",
                "dragging_style": "IPY_MODEL_3a81c4015ef84b08bcca1d33bbf34dc2",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_808b2910b88b4d8891204e721c8ff4e7"
                ],
                "layout": "IPY_MODEL_98dc7636d491486aa6517f19e9736f84",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_021b360921834389bf8bf597b0e8d80f",
                "west": 180,
                "zoom": 5
            }
        },
        "b7c5aef690a84b0db1b25529566e4be7": {
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
        "cff50147b8c34e79a6ec884532442e02": {
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
        "d89c415e388e46249c6a38e9a2fcdde0": {
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
        },
        "fb04bc4cf6fb4d1bb1a4bb444c8275ac": {
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
        "4636e4b117d3458bb657ca599eabbc38": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "e6738af4c22f4ff081aa12851f9b110d": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "6360a10de3e941b681a5dce56d47ddb8": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "96f8a65400d245c392bf3a5648b5e50e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "94fc2d80afd2431a8bc9203762682bde": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_974d435660094f50bc0d355036928b1d",
                    "IPY_MODEL_cce26a27b54b46a3a85badcfbd40d8dc",
                    "IPY_MODEL_c0f1d485e4f346c0b833899119ddaf79"
                ],
                "default_style": "IPY_MODEL_4636e4b117d3458bb657ca599eabbc38",
                "dragging_style": "IPY_MODEL_e6738af4c22f4ff081aa12851f9b110d",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_fb04bc4cf6fb4d1bb1a4bb444c8275ac"
                ],
                "layout": "IPY_MODEL_6360a10de3e941b681a5dce56d47ddb8",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_4636e4b117d3458bb657ca599eabbc38",
                "west": 180,
                "zoom": 5
            }
        },
        "974d435660094f50bc0d355036928b1d": {
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
        "cce26a27b54b46a3a85badcfbd40d8dc": {
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
        "c0f1d485e4f346c0b833899119ddaf79": {
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
        },
        "aa98dadf4f5e479cbb00edbadf30b46d": {
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
        "431283bd3a934caabad25f975d655efe": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "1479dd91462a45d8bc5d16be24cf903c": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "973f0e2058c443e89630b8c433f2b938": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "43b6fb811fee4f19b082de5c5ff4f5bc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "78aef8ecf9af40c9a3be8bc554993b85": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_c4e9290236814b2a85364ec91a83306e",
                    "IPY_MODEL_1617f4b84a514493971809a7aae25164",
                    "IPY_MODEL_d07ef52136bc413eaa5e237621b51674"
                ],
                "default_style": "IPY_MODEL_431283bd3a934caabad25f975d655efe",
                "dragging_style": "IPY_MODEL_1479dd91462a45d8bc5d16be24cf903c",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_aa98dadf4f5e479cbb00edbadf30b46d"
                ],
                "layout": "IPY_MODEL_973f0e2058c443e89630b8c433f2b938",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_431283bd3a934caabad25f975d655efe",
                "west": 180,
                "zoom": 5
            }
        },
        "c4e9290236814b2a85364ec91a83306e": {
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
        "1617f4b84a514493971809a7aae25164": {
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
        "d07ef52136bc413eaa5e237621b51674": {
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
        },
        "5657c7b0c9d343eca620095cf7829996": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "8851a289d56241e8b2694386f2a5819b": {
            "model_name": "SliderStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "44bd883ae8424d51a496b3e43d23696c": {
            "model_name": "IntSliderModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "layout": "IPY_MODEL_5657c7b0c9d343eca620095cf7829996",
                "style": "IPY_MODEL_8851a289d56241e8b2694386f2a5819b",
                "value": 100
            }
        },
        "dffc0a738b9f46209ff4ab3582023305": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1cefe3f8fb01454599602f5998090c5f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "e45c325318b746f3a76579d8341e3ad7": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "layout": "IPY_MODEL_dffc0a738b9f46209ff4ab3582023305",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_1cefe3f8fb01454599602f5998090c5f"
            }
        },
        "9e032c2e98524f78b76189272ed6d69b": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "4203de9bd50e4050a13ef564aa957897": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "3662e2bc87ea415ab7583d0b514fbb5d": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "layout": "IPY_MODEL_9e032c2e98524f78b76189272ed6d69b",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_4203de9bd50e4050a13ef564aa957897"
            }
        },
        "46ba14ed6cad4743b01aae8e2e85f4d7": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "6500697f4c124f5785f05fb7cd65ac8b": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "88e5ae25887b40268504aae30523d585": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Text:",
                "layout": "IPY_MODEL_46ba14ed6cad4743b01aae8e2e85f4d7",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_6500697f4c124f5785f05fb7cd65ac8b"
            }
        },
        "3f09c3df8650421eb406c42f0381cd6e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "cede58650d9c427887239e4aaa8a55ef": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "7f2bda649e3c43f3a2793bc711d30001": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Text:",
                "layout": "IPY_MODEL_3f09c3df8650421eb406c42f0381cd6e",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_cede58650d9c427887239e4aaa8a55ef"
            }
        },
        "c89264599e9547d892759770917dda86": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "cc459c987c6c49aa9ef9fd568b5f8d0a": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "c6aa72aef68c48f9a2b947f3f4e5416f": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_c89264599e9547d892759770917dda86",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_cc459c987c6c49aa9ef9fd568b5f8d0a"
            }
        },
        "e11ce3ffd5a843429be857a61aeb6afe": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "4f0cb61035534ad8a8ae362c20b2e326": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "2ce4ad18dc6a462393241f63204e8f95": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_e11ce3ffd5a843429be857a61aeb6afe",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_4f0cb61035534ad8a8ae362c20b2e326"
            }
        },
        "46bfeb55472c448bb56034b36d0eb7cb": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "0fc9dadb7c66472e85eec14ad67fc941": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "6daa72f9f417486db11fbb913776e471": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_46bfeb55472c448bb56034b36d0eb7cb",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_0fc9dadb7c66472e85eec14ad67fc941"
            }
        },
        "d0198a94e5d6488cabd5a932f37414ac": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "9ddf7519ab124bce89da60da1d51053d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "a3865615f15c48f2909137b3ee1b3d0c": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_d0198a94e5d6488cabd5a932f37414ac",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_9ddf7519ab124bce89da60da1d51053d"
            }
        },
        "a46337e6362a41f9a4c89010e025bfbe": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "24ffdcefd44b4c8f80db2138632070f0": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "125073f4caf946cf9c85019850c4be63": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_a46337e6362a41f9a4c89010e025bfbe",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_24ffdcefd44b4c8f80db2138632070f0",
                "value": 48.216438
            }
        },
        "409bfdc70b484160bcb42e04420d6292": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "8556410db64e4218a873ec32b678a822": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "672bf5df3c8c4ad59bf6e9b97997d535": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_409bfdc70b484160bcb42e04420d6292",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_8556410db64e4218a873ec32b678a822",
                "value": 53.948812
            }
        },
        "881cf53d767b419d997f14f94cf32da5": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2a6ceb0abd4e44fa936324bada750ff4": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "59871567ffce4af88b7f2c165064d5bb": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_881cf53d767b419d997f14f94cf32da5",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_2a6ceb0abd4e44fa936324bada750ff4",
                "value": -10.410882000000015
            }
        },
        "7b167374466545399c1e901bd48c72ec": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "01398fcb1f4040d5ac7290ea558657ea": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "0436720905d3408ab9a37e7f86af5f9f": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_7b167374466545399c1e901bd48c72ec",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_01398fcb1f4040d5ac7290ea558657ea",
                "value": 4.357767000000024
            }
        },
        "48452a2724f14d09a92a85caaabd91bd": {
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
        "166c35d193bf45028d525f9547d34b02": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "8e7870d4deb346f5b50a083747d47538": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "e26716501ed74f20967b0f5a8aaccf2e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "7e655529509b4db3856cb1877f1bcadc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "be80cd172a784a0186b7cd472ad610eb": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_ab92b88cd6174de48e0936c91f682db3",
                    "IPY_MODEL_19a2bbb3919a4c4e8a9147a15fe4835c",
                    "IPY_MODEL_819ded1cc22b404ebc3d812a7614b6be"
                ],
                "default_style": "IPY_MODEL_166c35d193bf45028d525f9547d34b02",
                "dragging_style": "IPY_MODEL_8e7870d4deb346f5b50a083747d47538",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_48452a2724f14d09a92a85caaabd91bd"
                ],
                "layout": "IPY_MODEL_e26716501ed74f20967b0f5a8aaccf2e",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_166c35d193bf45028d525f9547d34b02",
                "west": 180,
                "zoom": 5
            }
        },
        "ab92b88cd6174de48e0936c91f682db3": {
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
        "19a2bbb3919a4c4e8a9147a15fe4835c": {
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
        "819ded1cc22b404ebc3d812a7614b6be": {
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
        },
        "3fde860a07294bc19bcea6e391d0f9b4": {
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
        "3f8c9279b54440c88b3a9b5d843e110f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "36704e30ef7d4be494a6e2d364be28b4": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "c862ca305a8941b8b3e32f896a78af4a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f7aa1ba948294d59a74a3b200dd2573c": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "8f0a35f808774b879a524657f18c3233": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_aed9602c9a414cb9b697c6727e1791a1",
                    "IPY_MODEL_af52e28e96c94a3cb229123664af288f",
                    "IPY_MODEL_8df2ee89f1e54f7ab0672f9197fdf697"
                ],
                "default_style": "IPY_MODEL_3f8c9279b54440c88b3a9b5d843e110f",
                "dragging_style": "IPY_MODEL_36704e30ef7d4be494a6e2d364be28b4",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_3fde860a07294bc19bcea6e391d0f9b4"
                ],
                "layout": "IPY_MODEL_c862ca305a8941b8b3e32f896a78af4a",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_3f8c9279b54440c88b3a9b5d843e110f",
                "west": 180,
                "zoom": 5
            }
        },
        "aed9602c9a414cb9b697c6727e1791a1": {
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
        "af52e28e96c94a3cb229123664af288f": {
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
        "8df2ee89f1e54f7ab0672f9197fdf697": {
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
        },
        "ee47164b3c434a849c39fe7ea3665246": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f237836fa5e645e9a9805b7c743fac8f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "1d288eb9fa51484b894e81d0118d5d58": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "layout": "IPY_MODEL_ee47164b3c434a849c39fe7ea3665246",
                "step": 1,
                "style": "IPY_MODEL_f237836fa5e645e9a9805b7c743fac8f",
                "value": 4326
            }
        },
        "2a701099d38348dfac4981a35d17dbef": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e272f43c1eb24a36b86e717aa60765d7": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "2b161606391f4a8980ced936769ea70a": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_2a701099d38348dfac4981a35d17dbef",
                "step": 1,
                "style": "IPY_MODEL_e272f43c1eb24a36b86e717aa60765d7",
                "value": 3577
            }
        },
        "4b6ececc93b04d92b419added340196d": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "b566f82739284b3e8719da8037aaa411": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "46e0e905c03e44b48e23d00775e0d96a": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_4b6ececc93b04d92b419added340196d",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_b566f82739284b3e8719da8037aaa411"
            }
        },
        "220e2bc2d03d476883fafbc4f5e208d3": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "3dd8d0ee555340c1a9b48863b361947e": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "651970ad47f541b4af1b504f84265143": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_220e2bc2d03d476883fafbc4f5e208d3",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_3dd8d0ee555340c1a9b48863b361947e"
            }
        },
        "9e6d4092e7bd412cb38faa0c53eecb64": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d2409c4a3d234d1d9c69bd553a705457": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "10e36083654c4f71bca4fe099a5c20fc": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_9e6d4092e7bd412cb38faa0c53eecb64",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_d2409c4a3d234d1d9c69bd553a705457"
            }
        },
        "b8bc62e1f8c54232ac0be1a6d75698cd": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "600b370f52cb46aa83b85a8a0e4d1832": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "05579ed7dc2b4ec998ce97c7c789f334": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_b8bc62e1f8c54232ac0be1a6d75698cd",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_600b370f52cb46aa83b85a8a0e4d1832"
            }
        },
        "050250b958cf45b6b58e83881c9d3444": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "37208ecf55644e21bedb4388f7e98d86": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "7bc6fb3398994e758180b3347f3e72be": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_050250b958cf45b6b58e83881c9d3444",
                "step": null,
                "style": "IPY_MODEL_37208ecf55644e21bedb4388f7e98d86"
            }
        },
        "87f90fcd7f7d4a21877d149628c5e32a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "ddc82c8680494effae514aabda4d700d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "db19ed345baa46f985cacf7639c5a8a1": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_87f90fcd7f7d4a21877d149628c5e32a",
                "step": null,
                "style": "IPY_MODEL_ddc82c8680494effae514aabda4d700d"
            }
        },
        "d0555037400e4051ac17d47c7b3e7e8e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "7f13d1b1f64b46b59cf0c86fc1b57348": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "209e2692e5c0496ea6ef0b4cab0bf2f3": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_d0555037400e4051ac17d47c7b3e7e8e",
                "step": null,
                "style": "IPY_MODEL_7f13d1b1f64b46b59cf0c86fc1b57348"
            }
        },
        "cbfe294d7b1e40a29513b9d1a7f77044": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d36b1ddd977541ce8f4b248a2f9df521": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "ae810560c65b4532a19826b6ade646cc": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_cbfe294d7b1e40a29513b9d1a7f77044",
                "step": null,
                "style": "IPY_MODEL_d36b1ddd977541ce8f4b248a2f9df521"
            }
        },
        "e88c8beca1c94d18b019cb114e3babf1": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "19960ebaac1c46e6a76823f407b8014d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "08399fc8e869441a82fca06c91b73910": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "layout": "IPY_MODEL_e88c8beca1c94d18b019cb114e3babf1",
                "step": 1,
                "style": "IPY_MODEL_19960ebaac1c46e6a76823f407b8014d",
                "value": 4326
            }
        },
        "d9a38a4d40fe47c889cb4336e3d72a36": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "be70c2910f5f445290a68a877533de20": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "df6e3aaac22a4f468f101380e0fa3a80": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_d9a38a4d40fe47c889cb4336e3d72a36",
                "step": 1,
                "style": "IPY_MODEL_be70c2910f5f445290a68a877533de20",
                "value": 3577
            }
        },
        "b6fd657f679b46efa5f5738f24a879b8": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "92e7117268ac41b9970f0e0e68979d05": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "cd013e6bda57401bb1599c101f529adf": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_b6fd657f679b46efa5f5738f24a879b8",
                "step": null,
                "style": "IPY_MODEL_92e7117268ac41b9970f0e0e68979d05",
                "value": 27775.674821281424
            }
        },
        "25e81d185b6d42609d4fa3734696d640": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "234dca2a8ead4c7fb80cafc65d1b8fd2": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "0c6c34fb7fe94fa39ea087f2226fb751": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_25e81d185b6d42609d4fa3734696d640",
                "step": null,
                "style": "IPY_MODEL_234dca2a8ead4c7fb80cafc65d1b8fd2",
                "value": -2566853.945113243
            }
        },
        "f95000dedfee4e3182499d986e5e371c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "897388468d654bc08b7ac4a9aaacf79c": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "bd6e0446d2d84301a5d21fb850fa70c9": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_f95000dedfee4e3182499d986e5e371c",
                "step": null,
                "style": "IPY_MODEL_897388468d654bc08b7ac4a9aaacf79c",
                "value": 1389570.1046725435
            }
        },
        "388c5693bb134995826db837caeee147": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "a67f857a2b284da3a3fcf0248e0d1650": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "4ba55c9fb3bf41a6bad11191b63c065c": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_388c5693bb134995826db837caeee147",
                "step": null,
                "style": "IPY_MODEL_a67f857a2b284da3a3fcf0248e0d1650",
                "value": -1299293.844582205
            }
        },
        "ec545c916701490f8d2b2b7a3d0ed865": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "c6af5abbe769491998399d444926a577": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "963ea571d1844865a8d3ed09a198007f": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_ec545c916701490f8d2b2b7a3d0ed865",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_c6af5abbe769491998399d444926a577",
                "value": 132.275391
            }
        },
        "ebd337a1fb5644508b575397fc83abd7": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "95dccdb8fed44c56a2b0893243d502ba": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "fa6d95b4c2f740618382fcc1a8a85569": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_ebd337a1fb5644508b575397fc83abd7",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_95dccdb8fed44c56a2b0893243d502ba",
                "value": -23.787858
            }
        },
        "0895cf872b534fe5a0223993bd975a6c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "92103010bd8c49daae82ee56d6186cc0": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "cc94d3c3ad454b3ca437f6bff4b7dbeb": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_0895cf872b534fe5a0223993bd975a6c",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_92103010bd8c49daae82ee56d6186cc0",
                "value": 144.50336399999998
            }
        },
        "55e1eab079fd47e090d9702b27355049": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5cd17dee862f4a0d984b09a3c5a19cac": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "68c75dddf0324ba0b4ee211cca2d9dc6": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_55e1eab079fd47e090d9702b27355049",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_5cd17dee862f4a0d984b09a3c5a19cac",
                "value": -11.695273
            }
        },
        "5c851c1e172c4863a2326ab12e2b25f5": {
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
        "81d9c2e9e6e147478c7918307d6733ef": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "b6dc38440af74b7fa96b6a0f0f4af5ed": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "11c5497f7d534cb9be6e7d9f1af21c7c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5ceef636d92344e3ae571c5f8606f733": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "ee3293f8e2444cdb8c475665120b41fb": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -24.766784522874442,
                    500.09765625000006
                ],
                "controls": [
                    "IPY_MODEL_81599faf121a4f46bd54b7bb96178605",
                    "IPY_MODEL_40805f26af55438988e20d57e7f908fb",
                    "IPY_MODEL_314b29a04c194565a24a7e0aa2b1135d"
                ],
                "default_style": "IPY_MODEL_81d9c2e9e6e147478c7918307d6733ef",
                "dragging_style": "IPY_MODEL_b6dc38440af74b7fa96b6a0f0f4af5ed",
                "east": 500.09765625000006,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_5c851c1e172c4863a2326ab12e2b25f5"
                ],
                "layout": "IPY_MODEL_11c5497f7d534cb9be6e7d9f1af21c7c",
                "modisdate": "yesterday",
                "north": -24.766784522874442,
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
                "south": -24.766784522874442,
                "style": "IPY_MODEL_81d9c2e9e6e147478c7918307d6733ef",
                "west": 180,
                "zoom": 4
            }
        },
        "81599faf121a4f46bd54b7bb96178605": {
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
        "40805f26af55438988e20d57e7f908fb": {
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
        "314b29a04c194565a24a7e0aa2b1135d": {
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
        },
        "261923f191a342d38cca8c467ba2ce6a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5ee19975b4c341659885a957544a8904": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "fee3d903cda845379463448e41b5589e": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_261923f191a342d38cca8c467ba2ce6a",
                "step": 1,
                "style": "IPY_MODEL_5ee19975b4c341659885a957544a8904",
                "value": 4326
            }
        },
        "f6ab17e03273448eab2696a410ed4f3a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "0dce51e502e1488d8b26ef0a5bbbc8aa": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "d8c45605093b49c3903fb741bc2ca7bf": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_f6ab17e03273448eab2696a410ed4f3a",
                "step": 1,
                "style": "IPY_MODEL_0dce51e502e1488d8b26ef0a5bbbc8aa",
                "value": 3577
            }
        },
        "f066f3d93b6f4ab0b371382db5b8bd25": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2911ea35cdb14b0c8269f152684b30ee": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "1f225ccc8a4d4680934312d7ddfa31cb": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_f066f3d93b6f4ab0b371382db5b8bd25",
                "step": 1,
                "style": "IPY_MODEL_2911ea35cdb14b0c8269f152684b30ee",
                "value": 4326
            }
        },
        "f160933d66744598ba8df9f7500e3d9b": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "68c3f3f33c654647a57e55b78609c962": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "5f5f7fe9f7144553ac61e02a7b777e50": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_f160933d66744598ba8df9f7500e3d9b",
                "step": 1,
                "style": "IPY_MODEL_68c3f3f33c654647a57e55b78609c962",
                "value": 3577
            }
        },
        "0a3d2f1e55d6443c84b2e90769be8cb1": {
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
        "aee4865f696c45998691677e1a0f257f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "1dec1b965e9d45ffb679245a81e83057": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "38ff8dcea9684f0fad2c7e95291353d5": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f4c6ad4d4a674e128e94e3f31ffec248": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "84aad67908df44849aab4150a4502b1e": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_3e1e801917f844b587f4c55848d6fd64",
                    "IPY_MODEL_33651349c89c489facbf8d65cccbc66f",
                    "IPY_MODEL_b2f973f795a34b26a7546754d0715f04"
                ],
                "default_style": "IPY_MODEL_aee4865f696c45998691677e1a0f257f",
                "dragging_style": "IPY_MODEL_1dec1b965e9d45ffb679245a81e83057",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_0a3d2f1e55d6443c84b2e90769be8cb1"
                ],
                "layout": "IPY_MODEL_38ff8dcea9684f0fad2c7e95291353d5",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_aee4865f696c45998691677e1a0f257f",
                "west": 180,
                "zoom": 5
            }
        },
        "3e1e801917f844b587f4c55848d6fd64": {
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
        "33651349c89c489facbf8d65cccbc66f": {
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
        "b2f973f795a34b26a7546754d0715f04": {
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
        },
        "7bb1b8639baf499d82510d8a8bc661e4": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e41eed47397e4e249d2e97c5e6343e7d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "472a455b9e09423a9a12610bd38766e5": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_7bb1b8639baf499d82510d8a8bc661e4",
                "step": null,
                "style": "IPY_MODEL_e41eed47397e4e249d2e97c5e6343e7d"
            }
        },
        "01725535bbc8456cbc7311af8c6865ed": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2630e0e52934457097930809bf8edcfc": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "6626f740a0404eb7b6f7e324c90ebfa0": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_01725535bbc8456cbc7311af8c6865ed",
                "step": null,
                "style": "IPY_MODEL_2630e0e52934457097930809bf8edcfc"
            }
        },
        "1cd42e8c519840a487f0e5308988cc36": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d65e6c8487c544df89752af794774310": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "d7ed4f00a8ef4519b8f532e03043b7d6": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_1cd42e8c519840a487f0e5308988cc36",
                "step": null,
                "style": "IPY_MODEL_d65e6c8487c544df89752af794774310"
            }
        },
        "88dd5e04502048acb3963851dcdb10e4": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "142f79c9e3cc4aa0ae2f4bfa0a9d435f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "e21a2976fd664d9dad13e152fe3312d9": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_88dd5e04502048acb3963851dcdb10e4",
                "step": null,
                "style": "IPY_MODEL_142f79c9e3cc4aa0ae2f4bfa0a9d435f"
            }
        },
        "c8d9ff0b1fc849bd8eaeca5a2fa33edd": {
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
        "f996bfb677cc4b6c80083a5e2cce7128": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "a79afd6af4a04ee494d4686449899486": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "43b05b55f58847e59efaf88803598659": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "265182d26ade4d59818cdfeaa782487f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "7dd94bce12f14cd8a09f1f37433af852": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_6a140df9da964d198f5169bf51011d82",
                    "IPY_MODEL_633fe9e092d74faebc91b087c56bd761",
                    "IPY_MODEL_366458ed58b243b884a44c3ad55802ac"
                ],
                "default_style": "IPY_MODEL_f996bfb677cc4b6c80083a5e2cce7128",
                "dragging_style": "IPY_MODEL_a79afd6af4a04ee494d4686449899486",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_c8d9ff0b1fc849bd8eaeca5a2fa33edd"
                ],
                "layout": "IPY_MODEL_43b05b55f58847e59efaf88803598659",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_f996bfb677cc4b6c80083a5e2cce7128",
                "west": 180,
                "zoom": 5
            }
        },
        "6a140df9da964d198f5169bf51011d82": {
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
        "633fe9e092d74faebc91b087c56bd761": {
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
        "366458ed58b243b884a44c3ad55802ac": {
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
        },
        "7aab53b501074744bfa87381a84ade82": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d718a4d3186944b5b4bdda741407ec68": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "a1b074d2723f4ffbbd2c579e38b4dd5d": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_7aab53b501074744bfa87381a84ade82",
                "step": 1,
                "style": "IPY_MODEL_d718a4d3186944b5b4bdda741407ec68",
                "value": 4326
            }
        },
        "45707e0a32fc472cac244754adc11b6a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "7ed98257403c47d2ba1465acb4675522": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "ad1e203c1ce64ae181f350e86ae89ccf": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_45707e0a32fc472cac244754adc11b6a",
                "step": 1,
                "style": "IPY_MODEL_7ed98257403c47d2ba1465acb4675522",
                "value": 3577
            }
        },
        "155f8489cd184145a2d68b4d644c557a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1ed5507d549845b991183ba0b2f2b85d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "4f80367a43ef429d96be3af9c520a91d": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_155f8489cd184145a2d68b4d644c557a",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_1ed5507d549845b991183ba0b2f2b85d",
                "value": -10.542806999999982
            }
        },
        "260d6d1126b54e9da02bdb1f610e0bb0": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "eae04647ca3e491aac0be5beffc9f9c1": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "3430839cd059432f9aa36e350dc65b2b": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_260d6d1126b54e9da02bdb1f610e0bb0",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_eae04647ca3e491aac0be5beffc9f9c1",
                "value": 46.397147
            }
        },
        "4ec01ee760ef40cdb84419411fcdc83d": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "539cd1cb94764163b236a9208aca6823": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "bdbd4ef7c1204a4dbe15570e28a64058": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_4ec01ee760ef40cdb84419411fcdc83d",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_539cd1cb94764163b236a9208aca6823",
                "value": 5.148829999999975
            }
        },
        "c948e32fd3684e729e4eb4281db464c4": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1f1681d142f34269a59f7b2794351839": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "5d04ee59a05445b2b911a4ab1daa004a": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_c948e32fd3684e729e4eb4281db464c4",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_1f1681d142f34269a59f7b2794351839",
                "value": 52.873274
            }
        },
        "472e7200df79447bae4fcebb3501d9dd": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "09f710312ae84f669d36d9237224d06f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "4e9e60f0574443a4a3452789ea1c5638": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_472e7200df79447bae4fcebb3501d9dd",
                "step": null,
                "style": "IPY_MODEL_09f710312ae84f669d36d9237224d06f",
                "value": -17267828.476042427
            }
        },
        "60c50afa8cf74d0ea9222f756074e2aa": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "30d36233f6c842a6a4e5d7362173024c": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "53b482b250db44f0b996caab604c7da0": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_60c50afa8cf74d0ea9222f756074e2aa",
                "step": null,
                "style": "IPY_MODEL_30d36233f6c842a6a4e5d7362173024c",
                "value": -7002995.638395221
            }
        },
        "6d8f9cd6072f45fe952bf2f1a7d7bede": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "717b2d22d9284b13aa2072280b55c5e2": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "2942984f1bf642b4ae3991f0eeab5bd3": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_6d8f9cd6072f45fe952bf2f1a7d7bede",
                "step": null,
                "style": "IPY_MODEL_717b2d22d9284b13aa2072280b55c5e2",
                "value": -16389494.2723409
            }
        },
        "cd16c7c8cad94e4db8572f27f43a3c74": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "3fcfc40f997a455fbe69f1165e5b2020": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "e793962200b7486b866653f6523e0cc1": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_cd16c7c8cad94e4db8572f27f43a3c74",
                "step": null,
                "style": "IPY_MODEL_3fcfc40f997a455fbe69f1165e5b2020",
                "value": -4763941.026321793
            }
        },
        "97e4f990c24b424285a9d878992a61a6": {
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
        "975fef8220294327b50f7bc43c108dcb": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "ebcda031178b4d629c03447a15f312bc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "58e8015fc8394d94acf4e31061e9ad46": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "0a1201cc28394576bad1015156937228": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "2030d83147684f118fdaea084a7cce42": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    140.00976562500003
                ],
                "controls": [
                    "IPY_MODEL_424e0a7abb794651be909289cb974ccd",
                    "IPY_MODEL_17a312fcbe8e4a97ae1b4d5dda7c2195",
                    "IPY_MODEL_4bfb0e6da919487796c9c9542d65dbc4"
                ],
                "default_style": "IPY_MODEL_975fef8220294327b50f7bc43c108dcb",
                "dragging_style": "IPY_MODEL_ebcda031178b4d629c03447a15f312bc",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_97e4f990c24b424285a9d878992a61a6"
                ],
                "layout": "IPY_MODEL_58e8015fc8394d94acf4e31061e9ad46",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_975fef8220294327b50f7bc43c108dcb",
                "west": 180,
                "zoom": 5
            }
        },
        "424e0a7abb794651be909289cb974ccd": {
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
        "17a312fcbe8e4a97ae1b4d5dda7c2195": {
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
        "4bfb0e6da919487796c9c9542d65dbc4": {
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
        },
        "d0c6db7013d74110b56e88f53e5058cc": {
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
        "fce03a9c426248ad9c7251f89f0b16c8": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "b74373166450478f86fbb483a7e3c031": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "a41d4ca0fac641a5b689bc21097d5d37": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "dc11f1e3396e4153954ab8cfdbffaed5": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "a235895b3679473abeab838978d42ea2": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -40.01078714046552,
                    140.00976562500003
                ],
                "controls": [
                    "IPY_MODEL_6f4732ac842f45c1bfef0b4862923b55",
                    "IPY_MODEL_28fefee7f6444a048487684acdbe5266",
                    "IPY_MODEL_4b06a64c4c5b415da3e219c62bb52506"
                ],
                "default_style": "IPY_MODEL_fce03a9c426248ad9c7251f89f0b16c8",
                "dragging_style": "IPY_MODEL_b74373166450478f86fbb483a7e3c031",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_d0c6db7013d74110b56e88f53e5058cc"
                ],
                "layout": "IPY_MODEL_a41d4ca0fac641a5b689bc21097d5d37",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_fce03a9c426248ad9c7251f89f0b16c8",
                "west": 180,
                "zoom": 5
            }
        },
        "6f4732ac842f45c1bfef0b4862923b55": {
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
        "28fefee7f6444a048487684acdbe5266": {
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
        "4b06a64c4c5b415da3e219c62bb52506": {
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
        },
        "f7f973d7231a469692c8d440a5649b97": {
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
        "0a0c0f0b39a84ca9829e8579ad9544bc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "0d3dda9acf7248538adbb1f6ee7e2f7d": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "df5aa905dffe47a484fa508d6e018e19": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "a9d605c8d2474dc7b024ba725345b06b": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "61a3464f086d4de1b108a6d8b81490c1": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -39.97712009843963,
                    140.00976562500003
                ],
                "controls": [
                    "IPY_MODEL_ecd66bfbc617498e8d5329bbe608fe6f",
                    "IPY_MODEL_af68a8c15512483bb9bd13668707f868",
                    "IPY_MODEL_98f11d40a7164a2c823b945ab23feea2"
                ],
                "default_style": "IPY_MODEL_0a0c0f0b39a84ca9829e8579ad9544bc",
                "dragging_style": "IPY_MODEL_0d3dda9acf7248538adbb1f6ee7e2f7d",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_f7f973d7231a469692c8d440a5649b97"
                ],
                "layout": "IPY_MODEL_df5aa905dffe47a484fa508d6e018e19",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_0a0c0f0b39a84ca9829e8579ad9544bc",
                "west": 180,
                "zoom": 4
            }
        },
        "ecd66bfbc617498e8d5329bbe608fe6f": {
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
        "af68a8c15512483bb9bd13668707f868": {
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
        "98f11d40a7164a2c823b945ab23feea2": {
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
        },
        "8c11e49242fa46dc9cbad26ea3ab5f2e": {
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
        "fe1994a585714555936aa294db6e306e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "9dc23811ce484c15acc9a1434fc78bcc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "816af7fab4c7449dbb451ce97e421540": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d9da2a56a7e64ae6af7ba4d1467b5f34": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "ea615c3c1acb4b8d82f4ba331f356b34": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -9.96885060854611,
                    140.00976562500003
                ],
                "controls": [
                    "IPY_MODEL_7b4d8552971b421baccd4bd32dfce3fc",
                    "IPY_MODEL_19b8eae4c4fa404e93c76e6ed40c55c5",
                    "IPY_MODEL_61d53b4a27dc482d8533062a4b33cb50"
                ],
                "default_style": "IPY_MODEL_fe1994a585714555936aa294db6e306e",
                "dragging_style": "IPY_MODEL_9dc23811ce484c15acc9a1434fc78bcc",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_8c11e49242fa46dc9cbad26ea3ab5f2e"
                ],
                "layout": "IPY_MODEL_816af7fab4c7449dbb451ce97e421540",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_fe1994a585714555936aa294db6e306e",
                "west": 180,
                "zoom": 4
            }
        },
        "7b4d8552971b421baccd4bd32dfce3fc": {
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
        "19b8eae4c4fa404e93c76e6ed40c55c5": {
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
        "61d53b4a27dc482d8533062a4b33cb50": {
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
        },
        "c4740848d0254a3099b5ab6b46e999ce": {
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
        "19aeb5f1946d4a8f91c455139d6cef12": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "d3551c8c0e5243a5b2f745bd1673d894": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "a830f2a89cdd468f93bb406232afdda9": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "b1b53f56afa140578bb63c0da6130736": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "7d10b7585bc246189d9592267c9aaf05": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -19.973348786110602,
                    140.00976562500003
                ],
                "controls": [
                    "IPY_MODEL_fe17c7d3032444078dad33e0fdb2d412",
                    "IPY_MODEL_0f11d26c29e349e8b58a821db6a78b8f",
                    "IPY_MODEL_9132ac1e4e194784adf09ed3ff390d38"
                ],
                "default_style": "IPY_MODEL_19aeb5f1946d4a8f91c455139d6cef12",
                "dragging_style": "IPY_MODEL_d3551c8c0e5243a5b2f745bd1673d894",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_c4740848d0254a3099b5ab6b46e999ce"
                ],
                "layout": "IPY_MODEL_a830f2a89cdd468f93bb406232afdda9",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_19aeb5f1946d4a8f91c455139d6cef12",
                "west": 180,
                "zoom": 4
            }
        },
        "fe17c7d3032444078dad33e0fdb2d412": {
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
        "0f11d26c29e349e8b58a821db6a78b8f": {
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
        "9132ac1e4e194784adf09ed3ff390d38": {
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
        },
        "38dc8e59c6c548a19e7337f2313cdddd": {
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
        "b5778702e3474ab8a05f370e4ea9ef26": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "ec67abfe8c1a441693c8d389b73b071e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "440c97a4f5b84dcdac0a88575356da19": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "edd90281f40b46a5b8933726d81b3425": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "b33a5d8982be4e7a8947d1b9642ded02": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -26.194876675795218,
                    132.45117187500003
                ],
                "controls": [
                    "IPY_MODEL_d2fdfd034b93418cba77a44ad3611395",
                    "IPY_MODEL_3d9570f0a44543e6bf195730a258a11e",
                    "IPY_MODEL_2c2d81a72e8e4daaa7ab8ffb998ffc13"
                ],
                "default_style": "IPY_MODEL_b5778702e3474ab8a05f370e4ea9ef26",
                "dragging_style": "IPY_MODEL_ec67abfe8c1a441693c8d389b73b071e",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_38dc8e59c6c548a19e7337f2313cdddd"
                ],
                "layout": "IPY_MODEL_440c97a4f5b84dcdac0a88575356da19",
                "modisdate": "yesterday",
                "north": -90,
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
                "south": 90,
                "style": "IPY_MODEL_b5778702e3474ab8a05f370e4ea9ef26",
                "west": 180,
                "zoom": 4
            }
        },
        "d2fdfd034b93418cba77a44ad3611395": {
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
        "3d9570f0a44543e6bf195730a258a11e": {
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
        "2c2d81a72e8e4daaa7ab8ffb998ffc13": {
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
        },
        "84ce240b6e984f17948df96606f58014": {
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
        "d4fb2c63a0d94154b56dbe4085858888": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "59d05510a23c4b3681c817f98ad11ea1": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "62101a8774b3402c8078e3d422108608": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1ea47434fe984f07a847d309748d3f57": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "9eab3ea26c084f3ab28bf2d04830d45f": {
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
                    "IPY_MODEL_71dbce1598da47968385b42f8e59bc87",
                    "IPY_MODEL_1c37e465bf0641b68b6c66aef3040a69",
                    "IPY_MODEL_ef6aff72275c4623aad3c4352444b9c3"
                ],
                "default_style": "IPY_MODEL_d4fb2c63a0d94154b56dbe4085858888",
                "dragging_style": "IPY_MODEL_59d05510a23c4b3681c817f98ad11ea1",
                "east": 180.87890625000003,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_84ce240b6e984f17948df96606f58014"
                ],
                "layout": "IPY_MODEL_62101a8774b3402c8078e3d422108608",
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
                "style": "IPY_MODEL_1ea47434fe984f07a847d309748d3f57",
                "west": 99.05273437500001,
                "zoom": 4
            }
        },
        "71dbce1598da47968385b42f8e59bc87": {
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
        "1c37e465bf0641b68b6c66aef3040a69": {
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
        "ef6aff72275c4623aad3c4352444b9c3": {
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
        },
        "9064c60c7f34432db7165ec4bbffa5c1": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f5c804a2088348b398b3f4d022fcb02f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "f7dce4c53abf43c29ae1762837e1935c": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_9064c60c7f34432db7165ec4bbffa5c1",
                "step": 1,
                "style": "IPY_MODEL_f5c804a2088348b398b3f4d022fcb02f",
                "value": 4326
            }
        },
        "f3f13d49a37a4b5a96e74789a5fbade0": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1b346266bfe740d4bedf76ebc8c6adae": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "d8a94d929c7c4bdb915de569d8213699": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_f3f13d49a37a4b5a96e74789a5fbade0",
                "step": 1,
                "style": "IPY_MODEL_1b346266bfe740d4bedf76ebc8c6adae",
                "value": 3577
            }
        },
        "6a0161c6bc59417195afdedb9126c7c9": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "6059ba0f86754b59baeb2af698ad26ea": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "a1d00adc8e084293bde5fd3681144e62": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_6a0161c6bc59417195afdedb9126c7c9",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_6059ba0f86754b59baeb2af698ad26ea"
            }
        },
        "364f2e952b304ac38779cc9b0885d1f4": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "bacdbc5129f04c189abd2293058b0e50": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "edd5153f9ff74a478eb8658f54694620": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_364f2e952b304ac38779cc9b0885d1f4",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_bacdbc5129f04c189abd2293058b0e50"
            }
        },
        "551c37609d704af9940708857e1db15e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f717c3e04043432eb68da96fbe800320": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "953b2ccabdd4476599a8f19e926597d2": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_551c37609d704af9940708857e1db15e",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_f717c3e04043432eb68da96fbe800320"
            }
        },
        "e91391e0fb9241988b578cb28a5fa823": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "10be216bb43f405a839cf3f9c8d23f25": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "aafd3293645145e395ad52b897735393": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_e91391e0fb9241988b578cb28a5fa823",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_10be216bb43f405a839cf3f9c8d23f25"
            }
        },
        "3044e24c9afa4addbca68ba477dfa356": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "455378e126134c33aa4b86dd7365ba56": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "c3eb1c074eb6480cafb4cf658eb15fc3": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_3044e24c9afa4addbca68ba477dfa356",
                "step": null,
                "style": "IPY_MODEL_455378e126134c33aa4b86dd7365ba56"
            }
        },
        "8df8e886ec7b421e92158776e843016f": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "223c65b352e3446fab59c9081e9b7a0c": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "be3be6d8c18e45209c46f24d899d95c4": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_8df8e886ec7b421e92158776e843016f",
                "step": null,
                "style": "IPY_MODEL_223c65b352e3446fab59c9081e9b7a0c"
            }
        },
        "020ae4ee2c544693ab3c6db024476844": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f44441e79ad94c8884975dfb2c875fe0": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "bc32bee6364c4044a35a28120201472d": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_020ae4ee2c544693ab3c6db024476844",
                "step": null,
                "style": "IPY_MODEL_f44441e79ad94c8884975dfb2c875fe0"
            }
        },
        "e9262c2309e94d1aa8f06c6a730031c7": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "c249a3a1fed7416a821917d7438f7a0d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "efa4be2e950c41c8a4aa36a887451e73": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_e9262c2309e94d1aa8f06c6a730031c7",
                "step": null,
                "style": "IPY_MODEL_c249a3a1fed7416a821917d7438f7a0d"
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
    "model_id": "9eab3ea26c084f3ab28bf2d04830d45f"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "f7dce4c53abf43c29ae1762837e1935c"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "d8a94d929c7c4bdb915de569d8213699"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "a1d00adc8e084293bde5fd3681144e62"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "edd5153f9ff74a478eb8658f54694620"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "953b2ccabdd4476599a8f19e926597d2"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "aafd3293645145e395ad52b897735393"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "c3eb1c074eb6480cafb4cf658eb15fc3"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "be3be6d8c18e45209c46f24d899d95c4"
}
</script>

<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "bc32bee6364c4044a35a28120201472d"
}
</script>
AAAAAAAAAAAAAAAAAAAAAA
<script type="application/vnd.jupyter.widget-view+json">
{
    "version_major": 2,
    "version_minor": 0,
    "model_id": "efa4be2e950c41c8a4aa36a887451e73"
}
</script>

</body>
</html>

