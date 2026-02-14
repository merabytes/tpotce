# Attack Map Frontend Architecture

## Overview

The T-Pot Attack Map frontend is a **real-time cybersecurity visualization dashboard** that displays honeypot attacks as they occur. It is rendered using modern web technologies with an emphasis on performance, real-time data streaming, and rich interactive visualizations.

## Technology Stack

### Backend Server
- **Python 3** with **aiohttp** web framework
- **Asynchronous WebSocket** communication using `asyncio`
- **Redis** for pub/sub messaging and data distribution
- **Elasticsearch** integration for data retrieval (via DataServer component)

### Frontend Technologies

#### Core JavaScript Libraries

1. **Leaflet.js** (v7.x)
   - Primary mapping library for rendering the interactive world map
   - Displays attack lines, markers, and circles
   - Supports dark/light themes using CARTO basemaps
   - Includes fullscreen plugin for enhanced viewing

2. **D3.js** (v7.x)
   - Data visualization library
   - Used for advanced data transformations and visualizations
   - Powers dynamic attack line animations

3. **jQuery** (v3.7.1)
   - DOM manipulation and AJAX operations
   - Event handling and UI interactions

4. **Chart.js** (v4.x)
   - Creates enhanced data visualizations
   - Displays statistics charts and graphs

5. **Luxon**
   - Modern date/time handling library
   - Timezone management and timestamp formatting

6. **Bootstrap** (v5.x)
   - UI framework for responsive layout
   - Provides consistent styling and components

7. **Font Awesome**
   - Icon library for UI elements

### Custom JavaScript Modules

The frontend consists of three main JavaScript files:

1. **`map.js`** (55,991 bytes)
   - WebSocket connection management
   - Map initialization and rendering
   - Attack visualization logic
   - Real-time data processing
   - Connection health monitoring

2. **`dashboard.js`** (139,284 bytes)
   - Attack caching system using IndexedDB/LocalStorage
   - Statistics tracking (1m, 1h, 24h attack counts)
   - Top attacker/country tracking
   - Data aggregation and processing
   - Cache restoration on page load

3. **`cache-bridge.js`** (1,781 bytes)
   - Service worker bridge for offline functionality
   - Cache management coordination

### Data Storage

1. **IndexedDB** (Primary)
   - Persistent storage for attack events
   - Retains up to 24 hours of attack data (10,000 events max)
   - Indexed by timestamp, source IP, and honeypot type
   - Automatic cleanup every 5 minutes

2. **LocalStorage** (Fallback)
   - Used when IndexedDB is unavailable
   - Same retention policies as IndexedDB

## Architecture Flow

```
┌─────────────────┐
│  Data Sources   │
│  (Honeypots)    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Elasticsearch   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐      ┌─────────────────┐
│ DataServer_v2.py│─────▶│  Redis PubSub   │
│  (Python)       │      │    Channel      │
└─────────────────┘      └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │AttackMapServer  │
                         │  (Python)       │
                         │  - aiohttp      │
                         │  - WebSocket    │
                         └────────┬────────┘
                                  │
                                  ▼ WebSocket
                         ┌─────────────────┐
                         │  Web Browser    │
                         │                 │
                         │  Frontend Stack:│
                         │  - Leaflet.js   │
                         │  - D3.js        │
                         │  - Chart.js     │
                         │  - Custom JS    │
                         │  - IndexedDB    │
                         └─────────────────┘
```

## Key Features

### Real-Time Communication
- **WebSocket** connection to `AttackMapServer.py` (port 64299)
- Receives JSON attack events in real-time from Redis pub/sub
- Auto-reconnect with 60-second delay on connection loss
- Connection health monitoring and idle state detection

### Interactive Map
- **Leaflet.js** map with CARTO basemaps
- Dark/Light theme support
- Attack lines animated between source and destination IPs
- Markers and circles for attack visualization
- Fullscreen mode support
- Zoom levels 2-8 with fractional zoom support

### Data Persistence
- **24-hour attack cache** using IndexedDB
- Survives page refreshes and browser restarts
- Automatic cleanup of expired events
- Fallback to LocalStorage if IndexedDB unavailable

### Statistics Dashboard
- Real-time attack counters (1 minute, 1 hour, 24 hours)
- Top attacker IPs and countries
- Protocol-based color coding
- Connection status indicators

### Performance Optimizations
- Async/await pattern for non-blocking operations
- Event batching and throttling
- Efficient data structures for fast lookups
- Sub-pixel rendering fix for Leaflet grid lines
- Page visibility detection to reduce processing when tab is inactive

## HTML Structure

The main HTML file (`index.html`) includes:

1. **Meta Tags**
   - Viewport configuration for mobile devices
   - Content Security Policy for enhanced security

2. **Loading Screen**
   - Spinner animation during initialization
   - "Initializing T-Pot Attack Map" message

3. **Navigation Bar**
   - Branding and title
   - Real-time statistics (1m, 1h, 24h)
   - Connection status indicator
   - Cache status indicator
   - Theme toggle button

4. **Main Content Area**
   - Interactive Leaflet map container
   - Dashboard panels for statistics
   - Attack lists and charts

## Styling

- **Custom CSS** (`index.css` - 59,182 bytes)
- **Bootstrap CSS** for base styling
- **Leaflet CSS** for map components
- **Font Awesome** for icons
- **Custom fonts**: Inter and JetBrains Mono (loaded locally)

## Security

- **Content Security Policy** restricting resource loading
- All dependencies served locally (no external CDNs)
- Subresource Integrity (SRI) hashes for all static files
- WebSocket authentication using T-Pot web credentials

## Deployment

The Attack Map is deployed as Docker containers:

1. **map_web** - Frontend server (AttackMapServer.py)
   - Serves static files
   - Manages WebSocket connections
   - Port 127.0.0.1:64299

2. **map_data** - Data processor (DataServer_v2.py)
   - Queries Elasticsearch
   - Publishes to Redis

3. **map_redis** - Message broker
   - Redis pub/sub channel: "attack-map-production"

## Source Repository

The Attack Map is maintained as a separate project:
- **Repository**: https://github.com/t3chn0m4g3/t-pot-attack-map
- **Version**: 3.0.0
- **Based on**: Original GeoIP Attack Map by Matthew Clark May
- **Forked and enhanced by**: t3chn0m4g3 and Eddie4

## Summary

The Attack Map frontend is a **vanilla JavaScript application** that does NOT use modern frameworks like React, Vue, or Angular. Instead, it leverages:

1. **Leaflet.js** for map rendering and geospatial visualization
2. **D3.js** for data transformations and animations
3. **WebSocket** for real-time bidirectional communication
4. **IndexedDB** for client-side data persistence
5. **aiohttp** on the backend for asynchronous Python web serving

This architecture provides excellent performance, minimal dependencies, and the ability to display thousands of real-time attack events with smooth animations and responsive interactions.
