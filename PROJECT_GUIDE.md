# Route Optimizer Web Application

A web application that helps users optimize routes when visiting multiple locations, saving time and effort by using Google Maps real-time traffic data.

## Features

- **Multiple Waypoints Support** - Add as many addresses as you need to visit
- **Real-time Traffic Data** - Uses Google Maps API with live traffic information
- **Smart Optimization** - Employs Nearest Neighbor algorithm with 2-opt optimization for efficient routing
- **Visual Map Display** - Interactive map using Google Maps JavaScript API
- **Real-time Geocoding** - Converts addresses to coordinates using Google Geocoding API
- **Route Visualization** - Shows optimized route with numbered markers
- **Detailed Route Summary** - Displays total distance, travel time, and step-by-step directions
- **Custom Start/End Points** - Optionally set starting point and destination
- **Responsive Design** - Works on desktop and mobile devices

## Technology Stack

- **Backend**: Flask (Python)
- **Frontend**: HTML5, CSS3, JavaScript (Vanilla)
- **Mapping**: Google Maps JavaScript API
- **Routing**: Google Maps Directions API
- **Distance Calculation**: Google Maps Distance Matrix API
- **Geocoding**: Google Maps Geocoding API
- **Optimization Algorithm**: Nearest Neighbor + 2-opt Local Search

## Setup

### Prerequisites

- Python 3.7 or higher
- Google Cloud Platform account with billing enabled
- Google Maps API key with the following APIs enabled:
  - Maps JavaScript API
  - Directions API
  - Distance Matrix API
  - Geocoding API

### Installation

1. Clone or navigate to the project directory:
   ```bash
   cd c:\wxp\python\timesaver
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Configure your Google Maps API key:
   - Open `templates/index.html`
   - Replace `AIzaSyBOGxIR4LbxsFDHNkVGa_U1l7kSb8iWYks` with your API key (2 occurrences)

4. Run the application:
   ```bash
   python app.py
   ```

5. Open your browser and navigate to:
   ```
   http://localhost:5000
   ```

## Usage

### Adding Locations

1. **Add Waypoints**: Enter an address in the "Waypoints" field and click "+ Add Waypoint"
2. **Optional Start Point**: Enter a starting address to begin your route
3. **Optional Destination**: Enter a destination address to end your route

### Optimizing Route

1. Add at least 2 waypoints
2. Select optimization priority:
   - **Shortest Travel Time** - Minimizes time using real-time traffic data
   - **Shortest Distance** - Minimizes physical distance
3. Click "Optimize Route"
4. View the optimized route on the map with:
   - Numbered markers showing visit order
   - Total distance and travel time
   - Step-by-step directions with live traffic info

### Managing Locations

- **Remove Individual**: Click the "×" next to any waypoint in the list
- **Clear All**: Click "Clear All" button to reset everything

## How It Works

### Optimization Algorithm

The application uses a two-phase optimization approach:

1. **Nearest Neighbor Algorithm**
   - Starts from the first waypoint
   - Greedily selects the nearest unvisited location
   - Repeats from multiple starting points to find the best initial route

2. **2-Opt Local Search**
   - Takes the Nearest Neighbor solution
   - Iteratively swaps pairs of route segments
   - Keeps swaps that improve the total travel time
   - Continues until no further improvements are possible

This combination provides good results quickly while significantly outperforming pure greedy algorithms.

### Google Maps Integration

- **Distance Matrix API**: Gets real-time travel times and distances between all location pairs
- **Directions API**: Renders the optimized route on the map
- **Geocoding API**: Converts text addresses to latitude/longitude coordinates
- **Traffic Data**: Uses `TrafficModel.BEST_GUESS` with current departure time for live traffic information

### Distance Matrix Structure

```
[
  [start_to_start, start_to_wp1, start_to_wp2, ..., start_to_end],
  [wp1_to_start,   wp1_to_wp1,   wp1_to_wp2, ..., wp1_to_end],
  [wp2_to_start,   wp2_to_wp1,   wp2_to_wp2, ..., wp2_to_end],
  ...
  [end_to_start,   end_to_wp1,   end_to_wp2, ..., end_to_end]
]
```

The optimization algorithm uses this matrix to calculate total travel time for different route orders.

## API Usage and Costs

### Quotas and Limits

- **Distance Matrix API**: 
  - Up to 1,000 elements per request (25 origins × 25 destinations)
  - 100,000 elements per day (free tier)
  
- **Directions API**: 
  - Unlimited waypoints per day (requires billing)
  
- **Geocoding API**: 
  - 40,000 requests per month (free tier)

### Cost Estimation

For 10 waypoints (11 locations including start/end):
- Distance Matrix call: 11 × 11 = 121 elements per request
- Each optimization: 1 Distance Matrix API call

See [Google Maps Platform Pricing](https://mapsplatform.google.com/pricing) for detailed pricing.

## Project Structure

```
timesaver/
├── app.py                  # Flask application server
├── requirements.txt        # Python dependencies
├── README.md              # This file
└── templates/
    └── index.html         # Main application (HTML + CSS + JS)
```

## Troubleshooting

### "Google Maps Error" Message

- Verify your API key is valid
- Ensure all required APIs are enabled in Google Cloud Console
- Check that billing is enabled for your project

### Incorrect Distances/Times

- Verify the Distance Matrix API is enabled
- Check that real-time traffic is available in your region
- Try refreshing and optimizing again

### Route Not Optimimal

- For more than 15 waypoints, consider using professional TSP solvers
- The 2-opt algorithm provides good results for 5-15 locations
- Try with different optimization priorities

## Future Improvements

- [ ] Add route export (GPX, PDF)
- [ ] Save favorite routes
- [ ] Support for multiple vehicles
- [ ] Route comparison feature
- [ ] Mobile app version
- [ ] More advanced optimization algorithms (simulated annealing, genetic algorithms)

## License

This project is provided as-is for educational and personal use.

## API Key Security

**Important**: This project currently includes an API key in the code. For production use:
- Store API keys in environment variables
- Use API key restrictions (HTTP referrer, IP address)
- Never commit API keys to version control
- Rotate API keys regularly

## Support

For issues or questions, please refer to:
- [Google Maps Platform Documentation](https://developers.google.com/maps)
- [Google Maps JavaScript API](https://developers.google.com/maps/documentation/javascript)
- [Google Maps Distance Matrix API](https://developers.google.com/maps/documentation/distance-matrix)
