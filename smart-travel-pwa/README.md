# Smart Travel Companion

**Offline City Guide & Travel Journal – Progressive Web Application**

Smart Travel Companion is a Progressive Web Application (PWA) that helps users explore their surroundings by discovering nearby points of interest, viewing detailed information, and creating personal travel notes with photos.
The application is installable, works offline, and uses native device features such as geolocation and camera access.

The app works anywhere in the world using real-time geolocation and OpenStreetMap data.

---

## Project Criteria Overview

This project was developed to meet the following requirements:

* Technologies: **HTML, CSS, JavaScript**
* Optional framework allowed **only for the presentation layer**
* PWA features implemented in **vanilla JavaScript**
* Installable application
* Use of native device features
* Offline functionality with Service Workers and caching
* At least three logically connected views
* Hosted online over HTTPS
* Responsive design
* Performance-conscious implementation
* Clear caching strategy
* Well-documented, readable code

All criteria are met and documented below.

---

## Technologies Used

* **React** (presentation layer / UI only)
* **Vanilla JavaScript** (Service Worker, PWA logic, IndexedDB)
* HTML5 & CSS3
* Service Workers & Cache API
* IndexedDB
* OpenStreetMap (Overpass API)
* Wikipedia REST API (for place descriptions)
* GitHub Pages (HTTPS hosting)

---

## Installability

The application is installable on supported browsers.

### Implementation

* A `manifest.json` file defines:

    * Application name and short name
    * Icons for multiple resolutions
    * Theme color and background color
    * `start_url` and `scope`
    * Display mode set to `standalone`
* A Service Worker is registered in production.
* A custom install prompt is handled using the `beforeinstallprompt` event.

This allows users to add the application to their home screen and launch it like a native app.

---

## Native Device Features Used

### 1. Geolocation API

The Geolocation API is used to determine the user’s current position.

**Implementation**

* `navigator.geolocation.getCurrentPosition()` is wrapped in a Promise.
* Latitude and longitude are passed to the Overpass API to retrieve nearby points of interest.
* Error handling is implemented for denied or unavailable location access.

### 2. Camera Access

The camera is used to attach photos to journal notes.

**Implementation**

* `<input type="file" accept="image/*" capture="environment">` opens the device camera on mobile.
* Photos are handled as `Blob` objects and stored locally in IndexedDB.
* Images are previewed using `URL.createObjectURL()` to avoid memory issues.

These two features are clearly documented in the code and README as required.

---

## Fetching Nearby Places (OpenStreetMap)

Nearby attractions are retrieved using **OpenStreetMap data via the Overpass API**.

### Process

1. User location is obtained using the Geolocation API.
2. A POST request is sent to the Overpass API querying:

    * Tourist attractions
    * Museums
    * Parks
3. Results are normalized on the client side.
4. Distances from the user are calculated.
5. Retrieved places are stored in IndexedDB for offline access.

This solution:

* Uses real geographic data
* Requires no API key
* Works globally
* Is suitable for academic use

---

## Offline Functionality

The application works offline using a combination of Service Workers and IndexedDB.

### Behavior

* When offline:

    * An offline banner is shown to the user.
    * Cached places and saved notes remain accessible.
* When online:

    * Fresh data is fetched and stored for future offline use.

---

## Service Worker & Caching Strategy

### App Shell

* **Cache First**
* HTML, CSS, JS, icons, and manifest are cached during the Service Worker install phase.

### Static Assets

* **Cache First with runtime caching**
* Scripts, styles, and images are cached on first request.

### Dynamic Data

* **Network First with IndexedDB fallback**
* Place data is fetched from the network when available.
* Cached IndexedDB data is used when offline or when the API is unavailable.

This hybrid strategy ensures good performance and offline reliability.

---

## IndexedDB Usage

IndexedDB is used for persistent offline storage.

### Object Stores

* **places**

    * Stores nearby places fetched from OpenStreetMap
* **notes**

    * Stores user-created journal notes, text content, timestamps, and photo blobs

Photos are stored as **Blobs**, not Base64 strings, to prevent memory issues on mobile devices.

---

## Views & Navigation

The application contains four main views with a consistent and intuitive flow:

1. **Home**

    * Requests location permission
    * Displays nearby places
    * Shows online/offline status

2. **Details**

    * Shows detailed information about a selected place
    * Displays a description (Wikipedia when available)
    * Provides a link to external map applications
    * Allows creating a note for the place

3. **Note**

    * Create or edit a journal note
    * Add text and photo attachments
    * Save or delete the note

4. **Journal**

    * Displays a list of saved notes
    * Shows photo previews
    * Allows opening existing notes

Navigation is handled through a tab-based interface and internal state transitions.

---

## Responsiveness & Performance

* Mobile-first responsive design
* Flexbox and CSS Grid used for layout
* Minimal dependencies
* Optimized asset loading
* Designed to score well in Lighthouse PWA and performance audits

---

### Location

```
src/utils/image.js
```

### Purpose

* Compress photos before storing them in IndexedDB
* Reduce file size on mobile devices

### Usage

This feature can be enabled in `Note.jsx` before saving photos, but it is optional and not required for the core functionality.

---

## Hosting & Security

The application is hosted over **HTTPS** using GitHub Pages.

HTTPS is required for:

* Service Workers
* Geolocation API
* Installability

---

## How to Run the Project

### Install dependencies

```bash
npm install
```

### Development server

```bash
npm run dev
```

### Production build

```bash
npm run build
npm run preview
```

### Deployment

The project is deployed using GitHub Pages and served from the `gh-pages` branch.

---

## Conclusion

Smart Travel Companion fulfills all project requirements by combining:

* Modern PWA capabilities
* Native device feature integration
* Offline-first design
* Clean, readable, and well-documented code

The application is scalable, location-agnostic, and suitable for real-world use as well as academic evaluation.
