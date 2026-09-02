# 🚇 MetroNavX — Delhi Metro

**MetroNavX** is a feature-rich Delhi Metro navigation application designed to calculate optimized routes across the metro network. It follows an **offline-first architecture**, aggressively caching application assets and map tiles so that core functionality remains available without an active internet connection after the initial load.

## 🌟 Features

* **🧭 Smart Route Planning**
  Modified Dijkstra's algorithm with four routing modes:

  * Balanced
  * Fastest
  * Least Crowd
  * Women's Safety

* **📴 Offline-First Architecture**
  Custom service worker dynamically caches application assets and CartoDB / OpenStreetMap map tiles for offline use.

* **🚪 Smart Exit Recommendations**
  Recommends convenient and safer station exits based on lighting, accessibility, and available facilities.

* **📡 Facility Status**
  Tracks escalator and elevator availability through periodically updated status data, with architecture designed for integration with a Cloudflare Worker scraper.

---

## 🛠️ Tech Stack

### Web

* **React 18**
* **Vite**
* **Tailwind CSS v3**
* **React-Leaflet / Leaflet.js**
* **Zustand**
* **LocalStorage**
* **Service Workers**
* **Cloudflare Workers**

### Mobile

* **React Native**
* **Expo Router**
* **react-native-maps**
* Shared offline routing logic with the web application

---

## 🧠 Routing Engine

MetroNavX uses a modified **Dijkstra's shortest-path algorithm** that dynamically calculates route costs according to the user's selected preferences.

### Cost Function

$$
\text{Cost} =
(W_{\text{time}} \times \text{time}) +
(W_{\text{crowd}} \times \text{crowd}) +
(W_{\text{comfort}} \times \text{comfort}) +
(W_{\text{safety}} \times (10-\text{safety}))
$$

### Routing Modes

| Mode               | Time | Crowd | Comfort | Safety |
| ------------------ | ---: | ----: | ------: | -----: |
| **Balanced**       | 0.40 |  0.30 |    0.20 |   0.10 |
| **Fastest**        | 0.80 |  0.06 |    0.06 |   0.06 |
| **Least Crowd**    | 0.06 |  0.80 |    0.06 |   0.06 |
| **Women's Safety** | 0.06 |  0.06 |    0.06 |   0.80 |

### 🔄 Interchange Handling

When changing metro lines at an interchange station, the routing engine applies an additional penalty to better represent real-world transfers:

* **+5 minutes** estimated transfer time
* **+2 comfort penalty**

This helps prevent routes with multiple interchanges from being unrealistically favored based solely on travel time.

---

## 📁 Project Structure

```text
├── public/
│   ├── manifest.json
│   └── sw.js
│
├── src/
│   ├── components/
│   │   ├── Navbar.jsx
│   │   ├── SearchPanel.jsx
│   │   ├── MapView.jsx
│   │   └── RouteDetails.jsx
│   │
│   ├── data/
│   │   └── metroData.js
│   │
│   ├── store/
│   │   └── useMetroStore.js
│   │
│   ├── utils/
│   │   └── router.js
│   │
│   ├── App.jsx
│   ├── index.css
│   └── main.jsx
│
├── mobile/
│   └── ...
│
├── wrangler.toml
├── worker.js
├── tailwind.config.js
├── postcss.config.js
├── vite.config.js
├── package.json
└── README.md
```

---

# 📱 Mobile Application

The repository also contains a native mobile application inside the `mobile/` directory.

Built with **React Native + Expo Router**, the mobile application shares the core offline routing logic with the web version while providing a native, mobile-optimized experience.

### Mobile Highlights

* 🗺️ **Native Routing Map**

  * `react-native-maps`
  * High-contrast route rendering
  * Automatic camera fitting for calculated routes

* 🧭 **Tabbed Journey Planner**

  * Route timeline
  * Metro line transitions
  * Platform information
  * Transfer details
  * Fare comparison
  * Exit recommendations
  * Facility status
  * Crowd reporting

* 🚉 **Station Explorer**

  * Station directory
  * Exit information
  * Accessibility indicators
  * Elevators
  * Escalators
  * Wheelchair ramps
  * Tactile paths

---

## ⚙️ Running the Mobile App

```bash
cd mobile
npm install
npx expo start
```

### Build Android APK

```bash
cd android
chmod +x gradlew
./gradlew assembleRelease
```

The release APK will be generated at:

```text
mobile/android/app/build/outputs/apk/release/app-release.apk
```

---

## 📴 Offline Architecture

MetroNavX is designed around an **offline-first approach**.

The application uses:

* Local routing data
* LocalStorage persistence
* Service-worker caching
* Dynamically cached map tiles
* Client-side route calculation

Once required assets and map tiles have been cached, the application can continue to provide core routing and mapping functionality without an active connection.

---

## 🔐 Zero-Key Architecture

MetroNavX does **not require premium API keys** such as Mapbox or Google Maps credentials.

The project relies on:

* Leaflet
* OpenStreetMap-based infrastructure
* CartoDB map tiles
* Local routing data
* Client-side caching

This keeps the application lightweight and avoids mandatory paid API dependencies.

> **Note:** Public map services remain subject to their respective usage policies and availability.

---

## 📜 License

**Copyright © 2026 MetroNavX (Aamir). All Rights Reserved.**

This project is **proprietary and confidential**.

Unauthorized copying, distribution, modification, reverse engineering, republishing, commercial usage, or redistribution of the source code or compiled binaries is strictly prohibited without prior written permission from the copyright holder.
