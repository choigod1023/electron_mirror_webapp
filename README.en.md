# electron_mirror_webapp

[한국어](README.md) · [日本語](README.ja.md) · **English**

> UI for a Raspberry Pi smart mirror — an Electron.js clock and weather dashboard

## About

**electron_mirror_webapp** implements, in Electron.js, the UI shown on a Raspberry Pi-based smart mirror. It is a mirror-oriented dashboard — white text on black — showing the current time and date, current weather, and an hourly forecast on a single screen. The window is fixed at 1024×600 to match the mirror display.

## ✨ Features (as implemented)

- **Clock & date**: 12-hour time (AM/PM) and "M월 D일 O요일" (Korean weekday) shown large on the right.
- **Current weather**: fetches OpenWeatherMap data and displays the current weather icon, temperature (°), and humidity (%).
- **Hourly forecast**: lists the next five time slots with time (AM/PM), weather icon, temperature, and humidity.
- **Context-aware icon mapping**: combines weather condition (Rain/Clouds/Snow/Thunderstorm/Mist·Haze·Fog/Clear) with cloudiness and sunrise/sunset times to pick the right icon from `img/` (clear, cloudy, night, snow, storm, fog, …). See `return_img()`.
- **Mirror-friendly UI**: black background, white text, Nanum Gothic — designed to read well reflected in a mirror.

## 🛠 Tech stack

![Electron](https://img.shields.io/badge/Electron-12-47848F?logo=electron&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![OpenWeatherMap](https://img.shields.io/badge/OpenWeatherMap-API-EB6E4B?logo=openweathermap&logoColor=white)

- **Electron.js** (`^12.0.5`) — desktop shell / browser window
- **JavaScript / HTML / CSS** (vanilla)
- **request** — calls the OpenWeatherMap One Call API
- **nodemon** (devDependency)

## 🏗 How it works / architecture

- **Main process** (`main.js`): after `app.whenReady()`, creates a `BrowserWindow` (1024×600, `nodeIntegration: true`, `contextIsolation: false`) and loads `index.html`. Includes the usual macOS `activate` / `window-all-closed` handling.
- **Renderer process** (`index.html` + `renderer.js`): on load, computes the current time/date and renders it into `#clock`, then calls the OpenWeatherMap One Call API (`api.openweathermap.org/data/2.5/onecall`) to fill in current weather (`#current`) and the hourly forecast (`#hours`).
- **Assets** (`img/`): per-condition icon PNGs (`sun_clear`, `sun_cloud`, `cloud`, `night_clear`, `night_cloud`, `rainy`, `snow`, `storm`, `fog`).

## 🚀 Getting started

### Prerequisites

- [Node.js](https://nodejs.org) (with npm)
- An [OpenWeatherMap](https://openweathermap.org/api) API key

### Install

```bash
git clone https://github.com/choigod1023/electron_mirror_webapp.git
cd electron_mirror_webapp
npm install
```

### Configuration (weather API)

Weather data comes from the OpenWeatherMap One Call API call in `renderer.js`. The API key (`appid`) and coordinates (`lat`, `lon`) are currently hardcoded, so **replace them with your own key and the coordinates you want** before using it.

```js
// renderer.js
request('https://api.openweathermap.org/data/2.5/onecall?lat=<lat>&lon=<lon>&appid=<YOUR_API_KEY>&units=metric', ...)
```

> Security note: the example API key committed to this repository should be rotated immediately.

### Run

```bash
npx electron .
```

> The `start` script in `package.json` is defined as `electromon .`. The standard way to run is to launch Electron directly as above; use `nodemon` if you want auto-restart during development.

## 📁 Structure

```
electron_mirror_webapp/
├── main.js          # Electron main process (BrowserWindow 1024x600)
├── preload.js       # preload script (mostly commented out today)
├── index.html       # mirror UI layout and styles
├── renderer.js      # clock and weather logic (OpenWeatherMap call, icon mapping)
├── img/             # per-condition weather icon PNGs
└── package.json
```

## 📝 License

[CC0 1.0 (Public Domain)](LICENSE.md)

---

## 👤 Contribution & development environment

| Item | Detail |
|---|---|
| **Contribution share** | **4 commits** of my own — the repo was started by importing the `electron-quick-start` template with its history, so the commit ratio (4/97 = 4.1%) is not meaningful |
| **Contributors** | 1 (the remaining commits belong to the template's authors) |

<sub>Counting basis (snapshot as of 2026-08-12): commits reachable from **every branch** on origin (merge commits and empty commits excluded), counted by commit author email with one person’s multiple addresses merged; bot and automation commits (93 from dependabot) are excluded.</sub>
