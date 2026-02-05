# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Route Optimizer is a Flask web application that optimizes routes for visiting multiple locations using real-time Google Maps traffic data. The app uses a Nearest Neighbor algorithm combined with 2-opt optimization to find efficient routes.

## Development Commands

### Running the Application
```bash
python app.py
```
The app runs on `http://localhost:5000` by default. Port can be configured via `PORT` environment variable.

### Installing Dependencies
```bash
pip install -r requirements.txt
```

Dependencies: Flask 2.0+, requests 2.28+

### Deployment
The app is configured for multiple deployment platforms (see DEPLOYMENT.md for full guide):
- `Procfile`: Defines the web process (for Heroku-style platforms)
- `.python-version`: Specifies Python 3.9.19 (compatible with Render, Northflank, Railway)
- Environment variables: `FLASK_ENV`, `FLASK_DEBUG`, `PORT`

## Architecture

### Backend (app.py)
Minimal Flask server with single route that serves the frontend. All routing logic and optimization algorithms run client-side in JavaScript.

### Frontend (templates/index.html)
Self-contained single-page application (HTML + CSS + JavaScript) that handles:
- User interface and input management
- Google Maps API integration (Maps JavaScript, Directions, Distance Matrix, Geocoding)
- Route optimization algorithms
- Map visualization with numbered markers

### Route Optimization Algorithm

The optimization uses a two-phase approach implemented in JavaScript:

1. **Nearest Neighbor Algorithm** (`nearestNeighborFromPoint` at line 479)
   - Greedy algorithm that starts from a point and selects nearest unvisited location
   - Tries multiple starting points to find best initial route
   - Handles three scenarios: start+end points, end only, no constraints

2. **2-opt Local Search** (`twoOptOptimization` at line 507, `twoOptSimple` at line 558)
   - Takes Nearest Neighbor solution and iteratively improves it
   - Swaps pairs of route segments if it reduces total travel time
   - Continues until no improvements possible
   - Two variants: one that preserves fixed start/end points, one that's fully flexible

### Distance Matrix Structure

The app builds an NxN matrix where N = total locations (start + waypoints + end):
```
matrix[i][j] = travel time/distance from location i to location j
```

Array indexing in optimization:
- Index 0: Start point (if provided)
- Indices 1 to N-2: Waypoints
- Index N-1: End point (if provided)

The optimization algorithms work directly with array indices to reference locations in the distance matrix.

### Google Maps API Integration

- **Distance Matrix API**: Fetches real-time travel times/distances between all location pairs using `TrafficModel.BEST_GUESS`
- **Directions API**: Renders the optimized route on the map
- **Geocoding API**: Converts text addresses to lat/lng coordinates
- **Maps JavaScript API**: Provides map display and marker rendering

API key is embedded in `templates/index.html` (search for `AIzaSy`). For production, use environment variables and restrict the API key.

## Key Functions in index.html

- `optimizeRoute()` (line ~1005): Main entry point, coordinates the optimization process
- `fetchDistanceMatrix()` (line ~428): Gets travel times/distances from Distance Matrix API
- `optimizeWaypointsOnly()` (line 598): Handles case with fixed start and end points
- `optimizeWithEndOnly()` (line 799): Handles case with only end point fixed
- `optimizeNoStartEnd()` (line 850): Handles case with no constraints
- `displayRoute()` (line 651): Renders optimized route on map using Directions API

## API Configuration

The Google Maps API key must have these APIs enabled:
- Maps JavaScript API
- Directions API
- Distance Matrix API
- Geocoding API

Billing must be enabled in Google Cloud Platform.

## Important Notes

- All optimization runs client-side; no backend computation
- The app uses metric units (kilometers, meters)
- Distance Matrix API has quota: 100k elements/day on free tier
- Route optimization is suitable for 5-15 locations; larger routes may need advanced algorithms
- Recent commit (9ea5347) simplified optimization to use array indices directly instead of id mapping
