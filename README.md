# Address Map Plotter

A single-file HTML tool that geocodes US addresses and plots them on an interactive map. Built for batch-plotting addresses in the `street, city, zip, name` format — especially NYC-area addresses with hyphenated house numbers.

## Quick Start

1. Open `index.html` in any modern browser.
2. The sample addresses load and geocode automatically.
3. To use your own data, paste it into the text area and click **Plot on Map**.

## Data Format

Entries are separated by colons (`:`). Each entry is comma-separated:

```
street, city, zip, name: street, city, zip, name: ...
```

**Example:**

```
140-33 Rose Ave, Flushing, 11355, John Doe: 88 Cutter Mill Rd, Great Neck, 11021, Jane Smith
```

## Map Providers

Use the dropdown in the header to switch between:

| Provider       | API Key Required? | Notes                                      |
|----------------|-------------------|---------------------------------------------|
| Google Maps    | No (basic tiles)  | Adding a key enables Google Geocoding API   |
| OpenStreetMap  | No                | Free, always available                      |
| MapQuest       | Yes               | Falls back to OSM without a key             |
| Bing Maps      | Yes               | Falls back to OSM without a key             |

Your provider selection is saved in localStorage and persists across sessions.

## API Keys

### Via Query String (recommended for sharing)

Append keys directly to the URL:

```
index.html?google_key=YOUR_GOOGLE_KEY
index.html?key=YOUR_GOOGLE_KEY
index.html?google_key=XXX&mapquest_key=YYY&bing_key=ZZZ
```

| Parameter       | Description               |
|-----------------|---------------------------|
| `key`           | Shorthand for `google_key`|
| `google_key`    | Google Maps API key       |
| `mapquest_key`  | MapQuest API key          |
| `bing_key`      | Bing Maps API key         |

Query string keys take priority over keys saved in localStorage.

### Via Settings Panel

Click the **API Keys** button in the header to open the settings panel. Enter your keys and click **Save & Apply**. Keys are stored in your browser's localStorage.

### Getting API Keys

- **Google Maps:** [Google Cloud Console](https://console.cloud.google.com/apis/credentials) — enable "Maps JavaScript API" and "Geocoding API"
- **MapQuest:** [MapQuest Developer](https://developer.mapquest.com/)
- **Bing Maps:** [Bing Maps Portal](https://www.bingmapsportal.com/)

## Geocoding

Addresses are geocoded using multiple strategies in order of accuracy:

1. **Google Geocoding API** — if a Google API key is provided (most accurate)
2. **US Census Bureau Geocoder** — free, no key needed, handles NYC hyphenated addresses well
3. **Nominatim (structured)** — OpenStreetMap's free geocoder
4. **Nominatim (freeform)** — fallback with full address string
5. **Nominatim (city + zip)** — last resort approximate placement

Entries that land on an approximate location are flagged with a warning badge.

## Editing Markers

Each address in the sidebar has two buttons:

- **Edit Location** — opens an inline form to manually enter latitude/longitude, or click **Re-geocode** to retry
- **Remove** — deletes the entry and re-numbers the remaining markers

All markers on the map are **draggable** — grab and move any marker to correct its position. The coordinates in the sidebar update automatically.

## Features Summary

- Paste and plot batch addresses in one click
- Switch between Google Maps, OpenStreetMap, MapQuest, and Bing Maps
- API keys via URL query string or settings panel
- Multi-strategy geocoding (Google → Census Bureau → Nominatim)
- Draggable markers for manual position correction
- Inline lat/lng editor per address
- Click sidebar entries to pan to that marker (without zoom change)
- Progress bar during geocoding
- Numbered markers with name/address popups
