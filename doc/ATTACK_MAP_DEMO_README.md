# Attack Map Demo - README

## Overview

This directory contains a standalone demonstration of the T-Pot Attack Map frontend with simulated attack data. The demo file showcases how the real Attack Map works without requiring a full T-Pot installation.

## Files

- **`attack-map-demo.html`** - Standalone HTML demo with mock data simulation
- **`ATTACK_MAP_FRONTEND.md`** - Comprehensive technical documentation

## How to Use

### Option 1: Open Directly in Browser

Simply open `attack-map-demo.html` in your web browser:

```bash
# Firefox
firefox attack-map-demo.html

# Chrome
google-chrome attack-map-demo.html

# Or just double-click the file
```

### Option 2: Serve via Local Web Server

For best results, serve the file through a local web server:

```bash
# Using Python 3
python3 -m http.server 8000

# Using Node.js http-server
npx http-server -p 8000

# Using PHP
php -S localhost:8000
```

Then open: http://localhost:8000/attack-map-demo.html

## Features Demonstrated

The demo showcases the following Attack Map capabilities:

### 🗺️ Interactive World Map
- **Leaflet.js** based interactive map
- Dark theme with CARTO basemaps
- Pan, zoom, and explore attack sources

### ⚡ Real-Time Attack Visualization
- Animated attack lines from source to honeypot
- Color-coded by protocol (SSH, HTTP, FTP, Telnet, RDP)
- Smooth D3.js powered animations

### 📊 Live Statistics
- **1-minute** attack counter
- **1-hour** attack counter  
- **24-hour** attack counter
- Auto-reset intervals

### 📝 Attack Feed
- Recent attacks list with details:
  - Source country and IP
  - Protocol and port
  - Timestamp
- Color-coded by protocol type

### 🏆 Top Attackers
- Real-time country rankings
- Attack count per country
- Auto-updating leaderboard

## Mock Data

The demo simulates attacks from common attacker locations:

- 🇨🇳 China (Beijing)
- 🇷🇺 Russia (Moscow)
- 🇺🇸 USA (San Francisco)
- 🇧🇷 Brazil (São Paulo)
- 🇮🇳 India (New Delhi)
- 🇩🇪 Germany (Berlin)
- 🇬🇧 UK (London)
- 🇯🇵 Japan (Tokyo)
- 🇰🇷 South Korea (Seoul)
- 🇳🇱 Netherlands (Amsterdam)
- And more...

## Technologies Used

### Frontend Libraries (via CDN)

1. **Leaflet.js** 1.9.4 - Interactive mapping
2. **D3.js** 7.9.0 - Data visualization and animations
3. **jQuery** 3.7.1 - DOM manipulation
4. **Bootstrap** 5.3.2 - UI framework
5. **Font Awesome** 6.5.1 - Icons

All dependencies are loaded from `cdn.jsdelivr.net` for reliability.

## How the Demo Works

The demo simulates the real T-Pot Attack Map data flow:

```
Real Flow:
Honeypots → Elasticsearch → DataServer → Redis → AttackMapServer → Browser

Demo Flow:
JavaScript Timer → Random Attack Generator → Browser (Direct)
```

### Attack Simulation

Attacks are generated at random intervals (1-5 seconds) with:
- Random source location from the predefined list
- Random protocol (SSH, HTTP, FTP, Telnet, RDP)
- Random source IP address
- Current timestamp
- Realistic port numbers

### Visual Effects

Each simulated attack triggers:
1. **Curved line animation** from source to destination using D3.js
2. **Source marker** appears on the map (colored by protocol)
3. **Attack entry** added to the sidebar feed
4. **Statistics update** across all counters
5. **Country ranking update** in top attackers

## Comparison with Real Attack Map

| Feature | Demo | Real Attack Map |
|---------|------|-----------------|
| Data Source | Mock/Simulated | Real honeypot attacks via Elasticsearch |
| Communication | Local JavaScript | WebSocket to AttackMapServer |
| Persistence | None | 24-hour IndexedDB cache |
| Attack Frequency | 1-5 seconds | Based on real attack traffic |
| Geographic Data | Hardcoded | MaxMind GeoIP database |
| Protocols | 5 types | All honeypot protocols |

## Customization

You can easily customize the demo:

### Change Honeypot Location

Edit line ~340 in the HTML:
```javascript
const honeypotLocation = { 
    lat: 50.1109,  // Your latitude
    lng: 8.6821,   // Your longitude
    city: 'Frankfurt' 
};
```

### Add More Attack Sources

Edit the `attackSources` array (~328):
```javascript
{ country: 'Country', lat: XX.XXXX, lng: YY.YYYY, city: 'City' }
```

### Adjust Attack Frequency

Edit line ~618:
```javascript
// Change these values (in milliseconds)
const nextInterval = 1000 + Math.random() * 4000; // 1-5 seconds
```

### Add More Protocols

Edit the `protocols` array (~354):
```javascript
{ name: 'SMTP', port: 25, color: '#colorcode', class: 'smtp' }
```

## Troubleshooting

### Map Doesn't Load

**Issue**: CDN resources may be blocked by ad blockers or corporate firewalls.

**Solution**: 
- Disable ad blocker for localhost
- Use a local web server instead of opening file directly
- Check browser console for errors

### Console Errors

The demo logs helpful information to the browser console. Open Developer Tools (F12) to see:
```
🎯 T-Pot Attack Map Demo Started
📊 Simulating honeypot attacks with mock data
🗺️  Using Leaflet.js for mapping
📈 Using D3.js for attack line animations
```

### Performance Issues

If the demo runs slowly:
- Reduce attack frequency (edit line 618)
- Limit attack history size (edit line 506)
- Close other browser tabs

## Browser Compatibility

The demo works best in modern browsers:

- ✅ Chrome/Edge 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Opera 76+

## Educational Use

This demo is perfect for:

- 🎓 **Education** - Teaching cybersecurity visualization concepts
- 🏢 **Presentations** - Demonstrating honeypot capabilities
- 🧪 **Testing** - Understanding the Attack Map UI before deploying T-Pot
- 📚 **Learning** - Studying how Leaflet.js and D3.js work together

## Limitations

This is a **demonstration only**:

- ❌ No real attack data
- ❌ No backend connection
- ❌ No data persistence
- ❌ No authentication
- ❌ No WebSocket communication

For the **real Attack Map** with actual honeypot data, install T-Pot:
https://github.com/telekom-security/tpotce

## Related Documentation

- **ATTACK_MAP_FRONTEND.md** - Detailed technical architecture
- **T-Pot README** - Full T-Pot installation guide
- **Attack Map Repository** - https://github.com/t3chn0m4g3/t-pot-attack-map

## License

This demo follows the same licenses as the T-Pot Attack Map:
- Leaflet.js - BSD-2-Clause
- D3.js - ISC
- jQuery - MIT
- Bootstrap - MIT
- Font Awesome - Various (Font Awesome License)

## Questions or Issues?

For questions about the real T-Pot Attack Map:
- GitHub: https://github.com/telekom-security/tpotce
- Attack Map: https://github.com/t3chn0m4g3/t-pot-attack-map

---

**Note**: This is a demonstration file created to showcase the Attack Map frontend technologies and visualization capabilities. It does not represent real attack data.
