# map-tracer

A single-file web app for visualizing GeoJSON tracks and measuring along-path distance between two points.

## Usage

Open `track_viewer.html` in a browser. Load a GeoJSON file (or paste GeoJSON), then click two points on the drawn track to measure the along-path distance between them.

Features:
- Renders GeoJSON `LineString`/`MultiLineString` tracks on a Leaflet map
- Click two points on a track to snap to the nearest point on the path
- Computes along-path distance (following the actual geometry) as well as straight-line (chord) distance
