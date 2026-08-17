# map-tracer

A single-file web app for visualizing GeoJSON tracks and measuring along-path distance.

## Usage

Open `track_viewer.html` in a browser. Load a GeoJSON file (or paste GeoJSON), then choose a measurement mode:

- **Quick (2-point)** — click two points on the track to measure the along-path distance between them.
- **Path Trace** — click "Start Path Trace", then click as many points along the track as you like, even across separate stitched segments. Click near your starting point to snap the loop closed, then click "Done" for the total distance. "Undo Last Point" and "Clear All" are available while tracing.

Features:
- Renders GeoJSON `LineString`/`MultiLineString` tracks on a Leaflet map
- Auto-stitches disjointed line segments (e.g. a track split into multiple features) into continuous paths whose endpoints are within a configurable tolerance, so measurements can cross between them; re-run stitching with a different tolerance at any time
- Quick 2-point along-path and straight-line (chord) distance measurement
- Multi-point path trace across stitched segments, with loop-closing snap and a running/total distance
- Falls back to straight-line distance for any trace segment whose points aren't on a connected/stitched line, and flags it in the result
