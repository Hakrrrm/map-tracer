# map-tracer

A single-file web app for visualizing GeoJSON tracks and measuring along-path distance.

## Usage

Open `track_viewer.html` in a browser. Load a GeoJSON file (or paste GeoJSON), then choose a measurement mode:

- **Quick (2-point)** — click two points on the track to measure the along-path distance between them.
- **Path Trace** — click "Start Path Trace", then click as many points along the track as you like, even across separate stitched segments. Click near your starting point to snap the loop closed, then click "Done" for the total distance. "Undo Last Point" and "Clear All" are available while tracing.

You can also load raw GPS points to view them superimposed on the map and the path, for comparing noisy GPS fixes against the drawn track. Accepts GeoJSON (`Point`/`MultiPoint`), NDJSON (one point object or GeoJSON Point per line), GPX (`trkpt`/`wpt`/`rtept`), or CSV with `lat`/`lon` columns — as a file or pasted text. GPS points are independent of the loaded path, so loading a new path doesn't clear them.

The sidebar is organized into two tabs — **View Map Trace** (loading a path and raw GPS points) and **Import and Process Map Trace** (stitching, measuring, and the segment/export tool) — so only one group of controls shows at a time, and can be collapsed entirely (✕ button in its header) to give the map more room, reopened with the ☰ button that appears in the top-left corner.

Stations get a permanent pin marker with an always-visible name label on the map — no hover needed — so they stand out clearly against the track and any waypoints, from two sources:
- **Uploading an already-processed `track.geojson`** (one whose segment features carry `from`/`to` properties, matching this tool's own export format) shows its stations as pins immediately in the View tab, no re-tracing required.
- **Building a new station/segment table** from a finished Path Trace, in the Import and Process tab's "Segment & export" panel — mark which placed waypoints are actual stations (the rest stay in as shape detail within whichever segment they fall in), name each one, and export a `track.geojson` + `loop_lengths.csv` pair matching the `track_pipeline.py` contract.

Features:
- Renders GeoJSON `LineString`/`MultiLineString` tracks on a Leaflet map
- Tabbed, collapsible sidebar
- Stations show as permanent labeled pin markers on the map, whether detected from an uploaded file's from/to properties or built via a Path Trace
- Import raw GPS points (GeoJSON, NDJSON, GPX, or CSV) and view them superimposed on the map and path, with a toggle to show/hide them and a way to clear them independently of the loaded path
- Auto-stitches disjointed line segments (e.g. a track split into multiple features) into continuous paths whose endpoints are within a configurable tolerance, so measurements can cross between them; re-run stitching with a different tolerance at any time via "Re-stitch Segments"; bridged discontinuities — including a loop whose own recorded start and end nearly touch — are highlighted on the map in purple; each resulting continuous line is drawn in its own color, and every remaining loose end gets a small colored dot, so a discontinuity is always visible even when it's wider than the auto tolerance or hidden in a cluster of nearby lines
- Manual stitch mode for gaps too large for the auto tolerance: click "Start Manual Stitch", then click two loose line endpoints to force-join them (including a single line's own two ends, to close a loop); manual joins persist through re-stitching and can be removed individually. Clicks always resolve to the specific line actually clicked, not just whichever line happens to be nearest, so a busy junction with several close, near-parallel lines won't snap to the wrong one
- Path Trace and Quick measure correctly measure the short way around when closing or spanning a loop, instead of picking the long way around the far side
- Quick 2-point along-path and straight-line (chord) distance measurement
- Multi-point path trace across stitched segments, with loop-closing snap and a running/total distance
- Falls back to straight-line distance for any trace segment whose points aren't on a connected/stitched line, and flags it in the result
- Build a station/segment table from a finished trace and export it as `track.geojson` + `loop_lengths.csv`
