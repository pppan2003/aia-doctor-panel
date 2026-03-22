# AIA 網絡醫生搜尋系統

A client-side doctor search panel for AIA Hong Kong panel providers. Built as a single-file web app with no backend required.

## Features

- **Search & filter** — filter by doctor name, specialty, district, hospital, and insurance product
- **Doctor cards** — displays specialties, qualifications, experience, clinics, hours, and phone numbers
- **Map view** — interactive Leaflet.js map with clustered markers for all clinic locations
- **Password gate** — access-controlled landing screen before the search interface
- **Mobile-first** — fully responsive across iPhone, Android, and desktop
- **Offline-capable** — all doctor data is embedded; only map tiles require internet

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Vanilla HTML / CSS / JavaScript (no build step) |
| Map | [Leaflet.js](https://leafletjs.com/) + [MarkerCluster](https://github.com/Leaflet/Leaflet.markercluster) |
| Map tiles | OpenStreetMap |
| Geocoding | [HK GeoData API](https://geodata.gov.hk/) (HK1980 → WGS84 via pyproj) |
| Fonts | Inter (Google Fonts) |
| Icons | Inline SVG |
| Hosting | GitHub Pages |

## Usage

Open `index.html` directly in a browser, or serve it via any static host (GitHub Pages, Netlify, Cloudflare Pages, etc.).

A password is required to access the doctor search interface. Contact your AIA representative for access credentials.

## Data

Doctor data is embedded in `index.html` as a JSON constant (`DOCTORS_DATA`). Each record includes:

- Name, gender, title, specialty, qualifications, years of experience
- Supported insurance products
- Affiliated hospitals
- Clinic locations with district, address, phone, hours, and geocoded coordinates (WGS84)

## Geocoding Notes

Clinic coordinates were generated using the Hong Kong Government GeoData API and converted from HK1980 to WGS84. A small number of complex building addresses (e.g. Langham Place Office Tower) required manual coordinate correction.

## Development

No build tools or package manager required. Edit `index.html` directly.

To update doctor data, use the `aia-pdf-to-data` pipeline to process the latest AIA Merged Panel Provider List PDF and replace `DOCTORS_DATA` in `index.html`.

## License

Internal use only — AIA Hong Kong panel data © AIA Group Limited.
