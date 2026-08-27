# map-tracer

A single-file web app for visualizing GeoJSON tracks and measuring along-path distance.

## Usage

Open `track_viewer.html` in a browser. Load a GeoJSON file (or paste GeoJSON), then choose a measurement mode:

- **Quick (2-point)** — click two points on the track to measure the along-path distance between them.
- **Path Trace** — click "Start Path Trace", then click as many points along the track as you like, even across separate stitched segments. Click near your starting point to snap the loop closed, then click "Done" for the total distance. "Undo Last Point" and "Clear All" are available while tracing.

You can also load raw GPS points to view them superimposed on the map and the path, for comparing noisy GPS fixes against the drawn track. Accepts GeoJSON (`Point`/`MultiPoint`), GPX (`trkpt`/`wpt`/`rtept`), or CSV with `lat`/`lon` columns — as a file or pasted text. GPS points are independent of the loaded path, so loading a new path doesn't clear them.

The sidebar can be collapsed (✕ button in its header) to give the map more room, and reopened with the ☰ button that appears in the top-left corner.

Features:
- Renders GeoJSON `LineString`/`MultiLineString` tracks on a Leaflet map
- Collapsible sidebar
- Import raw GPS points (GeoJSON, GPX, or CSV) and view them superimposed on the map and path, with a toggle to show/hide them and a way to clear them independently of the loaded path
- Auto-stitches disjointed line segments (e.g. a track split into multiple features) into continuous paths whose endpoints are within a configurable tolerance, so measurements can cross between them; re-run stitching with a different tolerance at any time
- Quick 2-point along-path and straight-line (chord) distance measurement
- Multi-point path trace across stitched segments, with loop-closing snap and a running/total distance
- Falls back to straight-line distance for any trace segment whose points aren't on a connected/stitched line, and flags it in the result
