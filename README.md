# enhanced_tilemap
Kibana ships with a functional tilemap visualization. This plugin provides an additional tilemap visualization containing the enhancments listed below.

## Enhancements

### Add Markers to map
Allows for the placement of markers on the tilemap. Markers are displayed when in the visualization panel or the dashboard panel. Markers can only be added and removed when in the visualization panel.

### Minor stuff
* Display mouse latitude and longitude location in lower left corner
* Scroll map on mouse zoom. Feature can be turned off in options.

### OR geo_bounding_box queries
Kibana's tilemap visualization has a neat feature where you can draw a rectangle on the map and create a geo_bounding_box filter. The limitation arises when multiple bounding boxes are needed. Each drawn rectangle creates a new geo_bounding_box filter that are ANDed together resulting in "No results found" messages across all visualizations. 

The enhanced tilemap visualization allows for the creation of multiple bounding box filters that will be ORed together. Each drawn rectangle will append a geo_bounding_box filter to an ORed array.

### Static quantized range bands
The existing tilemap generates quantized range bands dynamically. The enhanced_tilemap provides the ability to set static quantized range bands.

### Sync maps
Sync map movements when dashboard contains multiple map visualizations. Map syncing implemented with [Leaflet.Sync](https://github.com/turban/Leaflet.Sync)

**Performance tip** Store enhanced_tilemaps belonging to the same dashboard at identical zoom levels. When enhanced_tilemaps are stored with different zoom levels, the browser will have to make 2 requests to elasticsearch for data. The first will get all data at different zoom levels. Then the next, will fetch all data at identical zoom levels. The second request can be avoided if all maps are stored at identical zoom levels. Check zoom levels by viewing the stored visualization contents under Settings->Objects->Visualizaitons and then select your visualization. Scroll down to `visState` area and examine the contents of the geohash_grid aggregation under aggs. Look at the values of mapCenter and mapZoom.

## Planned Enhancements

### Jump to location inputs
Add leaflet control that when clicked provides lat/lon/zoom inputs and a button. The inputs will allow a user to jump the map to a specific location and zoom level.

### Load markers without blocking user interface
The existing tilemap loads all of the geohash grids at a single time. This can result in adding hundreds or even thousands of DOM elements at a single time. The browser is locked up while this process occurs.

The enhanced tilemap plugin will phase-in geohash grids (for example, load 25 every 200 milliseconds) so that the browser never locks up. A control with a spinning icon will be added to the map while grids are being phased-in. The control will be removed once everything is finished.

### backport kibana 5.x tilemap improvements to 4.x.
The kibana tilemap plugin has been updated with several pull-requests but none of these have been merged with the 4.x branch. This plugin supports 4.x so it is a way to use integrate the tilemap improvements in kibana 4.x releases.
* https://github.com/elastic/kibana/pull/6001
* https://github.com/elastic/kibana/pull/8000
* https://github.com/elastic/kibana/pull/6835

### add additional layers
Provide the ablity to add additional WFS and WMS layers to the map.

### geo aggregation filtered by geo_bounding_box collar
[Filter geohash_grid aggregation to map view box with collar](https://github.com/elastic/kibana/issues/8087)

# Compatibility
The plugin is compatible with following Versions (other not tested yet):
* kibana (=4.4+)