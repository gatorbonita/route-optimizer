# 🗺️ Route Optimizer

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-2.0+-green.svg)](https://flask.palletsprojects.com)
[![Google Maps](https://img.shields.io/badge/Google%20Maps-API-red.svg)](https://developers.google.com/maps)
[![Responsive](https://img.shields.io/badge/Design-Responsive-orange.svg)]()
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> 🚀 Save time by finding optimal route for visiting multiple locations with real-time traffic data
>
> 📱 **NEW**: Fully responsive design optimized for mobile, tablet, and desktop

A smart route optimization web application that uses advanced algorithms and Google Maps traffic data to minimize travel time when visiting multiple destinations. Features an intuitive tabbed interface for mobile devices and a powerful multi-column layout for desktop users.

---

## ✨ Features

### 🎯 Smart Optimization
- **Nearest Neighbor Algorithm** with 2-opt local search optimization
- Real-time traffic-aware routing using Google Maps Distance Matrix API
- Flexible routing: start only, end only, both, or neither
- Automatic waypoint reordering for maximum efficiency

### 📍 Location Management
- Add unlimited waypoints (addresses/locations)
- Optional starting point and destination
- Visual map with numbered markers
- Real-time geocoding with address validation

### 🎨 User Experience
- **Fully Responsive Design** - Optimized for desktop, tablet, and mobile
- **Desktop Features**:
  - 3-column layout with collapsible sidebar
  - Toggle sidebar with ◀ button for more map space
  - Side-by-side results panel
- **Mobile Features** (NEW ✨):
  - Intuitive tabbed interface (Locations | Map | Route)
  - Full-screen map view
  - Bottom sheet for route details
  - Large touch targets (44x44px minimum)
  - Auto-switch to map after optimization
- Instant route visualization on Google Maps
- Detailed route summary with:
  - Total travel distance
  - Total travel time (with live traffic)
  - Step-by-step directions with leg distances and times

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|------------|
| **Backend** | Flask 2.0+ (Python 3.9+) |
| **Frontend** | Vanilla JavaScript + CSS3 (Responsive) |
| **Design** | Mobile-first responsive (breakpoints: 480px, 768px, 1200px) |
| **Maps** | Google Maps JavaScript API |
| **Traffic** | Google Distance Matrix API (TrafficModel.BEST_GUESS) |
| **Geocoding** | Google Geocoding API |
| **Deployment** | Render (Free tier) |

---

## 🚀 Quick Start

### Prerequisites

- Python 3.9 or higher
- Google Maps API key with enabled services:
  - Maps JavaScript API
  - Directions API
  - Distance Matrix API
  - Geocoding API

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/gatorbonita/route-optimizer.git
   cd route-optimizer
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure Google Maps API**
   
   Open `templates/index.html` and update the API key (line ~281):
   ```javascript
   const API_KEY = 'YOUR_GOOGLE_MAPS_API_KEY_HERE';
   ```

4. **Run the application**
   ```bash
   python app.py
   ```

5. **Open in browser**
   Navigate to: http://localhost:5000

### Testing on Mobile Devices

To test the mobile experience on your phone:

1. **Find your computer's IP address**
   ```bash
   ipconfig | findstr "IPv4"
   # Look for something like: 192.168.1.XXX
   ```

2. **Make sure your phone is on the same WiFi network**

3. **Update Google Maps API Key restrictions**
   - Go to [Google Cloud Console](https://console.cloud.google.com/apis/credentials)
   - Add your local IP to HTTP referrers:
     ```
     http://192.168.1.*:5000/*
     ```

4. **Open on your phone**
   - Navigate to: `http://YOUR-IP:5000` (e.g., `http://192.168.1.157:5000`)
   - You'll see the mobile tabbed interface!

**Alternative**: Deploy to Render (free) for permanent public access - see [Deployment section](#-deployment)

---

## 📖 Usage

### Desktop Experience 🖥️

1. **Add Locations**
   - Enter addresses in the "Waypoints" field
   - Click "+ Add Waypoint" to add each location
   - Optionally specify a starting point and/or destination

2. **Choose Optimization Priority**
   - **⏱️ Fastest**: Routes based on real-time traffic
   - **📏 Shortest**: Routes based on geographic distance

3. **Optimize**
   - Click "🚀 Optimize Route"
   - Wait for route calculation (uses real-time traffic data)
   - View the optimized route on the map

4. **Follow the Route**
   - Numbered markers show optimal visitation order
   - Route summary displays total distance and time
   - Each leg shows distance, time, and live traffic status

5. **Maximize Map Space** (Optional)
   - Click the **◀** button on the sidebar edge to collapse it
   - Click **▶** to expand it back

### Mobile Experience 📱

The mobile interface uses a **tabbed layout** for optimal screen usage:

1. **📍 Locations Tab** - Add and manage your waypoints
   - Tap on the Locations tab
   - Add addresses using the input field
   - Set optional start/end points

2. **🗺️ Map Tab** - View your route
   - After optimization, automatically switches to map
   - Full-screen map display
   - Pinch to zoom, drag to pan

3. **🚀 Route Tab** - View route details
   - Tap to see detailed route information
   - Bottom sheet slides up with results
   - Swipe or tap × to close

### Pro Tips

- Use specific addresses for better geocoding accuracy
- Try different optimization priorities for different scenarios
- Peak hours may show longer travel times due to traffic
- Route optimization works best with 5-15 locations
- On mobile, landscape orientation provides more map space

---

## 🌐 Deployment

### Deploy to Render (Free)

1. **Push to GitHub**
   ```bash
   git add .
   git commit -m "Ready for deployment"
   git push origin main
   ```

2. **Create Render Web Service**
   - Go to [render.com](https://render.com)
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Configure:
     - **Runtime**: Python 3
     - **Build Command**: `pip install -r requirements.txt`
     - **Start Command**: `python app.py`
   - Click "Create Web Service"

3. **Set Environment Variables** (Optional but recommended)
   ```
   FLASK_ENV=production
   FLASK_DEBUG=0
   PORT=5000
   ```

4. **Configure Google Maps API Key**
   - Add your Render URL to the API key HTTP referrers
   - Format: `https://your-app-name.onrender.com/*`

Your app will be available at: `https://your-app-name.onrender.com`

### Alternative Deployments

- **Railway**: [railway.app](https://railway.app)
- **PythonAnywhere**: [pythonanywhere.com](https://pythonanywhere.com)

---

## 🔧 Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|----------|
| `FLASK_ENV` | Flask environment | `development` |
| `FLASK_DEBUG` | Enable debug mode | `True` |
| `PORT` | Server port | `5000` |

### Google Maps API Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project or select existing
3. Enable required APIs:
   - Maps JavaScript API
   - Directions API
   - Distance Matrix API
   - Geocoding API
4. Create credentials (API Key)
5. Add application referrers for security:
   - Development: `http://localhost:5000/*`
   - Production: `https://your-app-name.onrender.com/*`

---

## 📊 Algorithm Details

### Optimization Approach

The app uses a two-phase optimization algorithm:

1. **Nearest Neighbor Greedy Algorithm**
   - Starts from a point and repeatedly visits the nearest unvisited location
   - Tries multiple starting points to find the best initial route
   - Handles four scenarios:
     - Fixed start + fixed end
     - Start only
     - End only
     - No constraints (circular route)

2. **2-opt Local Search**
   - Takes the greedy solution and iteratively improves it
   - Swaps route segments if it reduces total travel time
   - Continues until no improvements can be found

### Distance Matrix

The app builds an N×N matrix where N = total locations:
```
matrix[i][j] = travel time from location i to location j
```

This matrix enables O(1) lookups during optimization.

### Complexity

- **Distance Matrix**: O(N²) API calls
- **Nearest Neighbor**: O(N²)
- **2-opt Optimization**: O(N³) in worst case
- **Practical**: Works well for 5-15 locations

---

## 📝 API Quotas & Limits

| Service | Free Tier Limit | Unit |
|----------|----------------|-------|
| Distance Matrix API | 100,000 | elements/day |
| Directions API | 10,000 | requests/day |
| Geocoding API | 40,000 | requests/day |

> 💡 **Note**: 1 Distance Matrix request with 4 locations = 16 elements (4×4 matrix)

---

## 🐛 Troubleshooting

### Common Issues

**App shows "Google Maps Error"**
- Check the API key is correct and not restricted
- Ensure all required APIs are enabled in Google Cloud Console
- Verify your domain is added to API key HTTP referrers

**Route optimization is slow**
- First API call may take 10-30 seconds
- Ensure billing is enabled on your Google Cloud project
- Try fewer locations (<10 for faster performance)

**Map markers don't display**
- Check browser console for JavaScript errors
- Verify the API key has Maps JavaScript API enabled
- Clear browser cache and reload

**Mobile device can't connect (RefererNotAllowedMapError)**
- Ensure phone is on the same WiFi as your computer
- Add your local IP to API key HTTP referrers (e.g., `http://192.168.1.*:5000/*`)
- Wait 1-2 minutes after updating API key restrictions
- For production, deploy to Render and add that URL to referrers

**Tabs not switching on mobile**
- Hard refresh the page (Ctrl+Shift+R or Cmd+Shift+R)
- Clear browser cache
- Check if JavaScript is enabled

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

### Development Setup

```bash
git clone https://github.com/gatorbonita/route-optimizer.git
cd route-optimizer
pip install -r requirements.txt
python app.py
```

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**Route Optimizer** - Built with ❤️ to help save time on the road

---

## 🙏 Acknowledgments

- [Google Maps Platform](https://developers.google.com/maps) - Maps, Directions, and Distance Matrix APIs
- [Flask](https://flask.palletsprojects.com/) - Web framework
- [Render](https://render.com) - Free hosting platform

---

<div align="center">

**Made with ❤️ by Route Optimizer Team**

[⬆ Back to Top](#-route-optimizer)

</div>
