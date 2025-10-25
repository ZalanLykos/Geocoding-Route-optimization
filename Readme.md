
# 🚀 Smart Logistics Automation Demo (n8n + OpenRouteService + Google Sheets + Slack)

## 📦 Overview
This demo showcases an automated logistics workflow built in **n8n**, designed to streamline delivery operations by integrating multiple services for address handling, route optimization, and driver/client communication.

---

## ⚙️ Workflow Summary

### 🔹 Step 1: Input
Addresses are imported from a **Google Sheet** named `Orders`.  

Example:
| id | address | country |
|----|----------|----------|
| 1 | 22 avenue des Champs Élysées, Paris | FR |
| 2 | Pariser Platz, Berlin | DE |

---

### 🔹 Step 2: Geocoding
Each address is automatically geocoded using the **OpenRouteService Geocoding API**, converting it into latitude and longitude.  
Results are saved back into:
- `Orders` sheet (for destinations)
- `Dept` sheet (for departure points)

---

### 🔹 Step 3: Distance & Duration
Using **OpenRouteService Directions API**, the workflow calculates:
- Distance (in meters)  
- Duration (in seconds)  
- Steps (optional route details)

The data is appended to the corresponding rows in both sheets.

---

### 🔹 Step 4: Route Generation
Each delivery route is converted into a **Google Maps link** for easy navigation:

```js
const coords = [
  [latitude_departure, longitude_departure], // Start
  [latitude_destination, longitude_destination] // Stop 1
];

const mapsUrl = "https://www.google.com/maps/dir/" + 
  coords.map(c => `${c[0]},${c[1]}`).join("/") + "/?dirflg=d";
````

The generated URLs are driver-ready (motorcycle/car route flag `?dirflg=d`).

---

### 🔹 Step 5: Driver Notification (Slack)

Each driver receives an automatic message in a Slack channel (`#drivers`):

```
Your Route is ready: Driver#2: Petyr
Your route is https://www.google.com/maps/dir/-18.85109,-48.27743/-22.95526,-45.45591/?dirflg=d
```

---

### 🔹 Step 6: Client Notification (Email)

Clients receive a personalized email with delivery details:

```
Hello client,
Your order is out for delivery.
Estimated time: 45 min
Distance: 12.4 km
```

---

## 🧠 Features

* Fully automated multi-service workflow
* Real-time data updates between Sheets and Slack
* Converts raw data into human-readable results (`minutes`, `km`)
* Modular design — each step can be reused or expanded
* Ideal for logistics, dispatch, or delivery management systems

---

## 🛠️ Technologies Used

* [n8n](https://n8n.io/)
* [OpenRouteService API](https://openrouteservice.org/)
* [Google Sheets](https://www.google.com/sheets/about/)
* [Slack API](https://api.slack.com/)
* [Gmail or SMTP integration](https://n8n.io/integrations/email/)

---

## 🔑 API Endpoints

**Geocoding:**

```
https://api.openrouteservice.org/geocode/search
```

**Routing (car):**

```
https://api.openrouteservice.org/v2/directions/driving-car
```

**Routing (motorcycle/truck variants):**

```
https://api.openrouteservice.org/v2/directions/driving-hgv
https://api.openrouteservice.org/v2/directions/cycling-regular
```

---

## 🧩 Workflow Logic (Simplified)

```
Start → Google Sheets (Orders)
     → Geocode Addresses (OpenRouteService)
     → Update Sheets (Orders + Dept)
     → Calculate Distance & Duration (OpenRouteService)
     → Convert to Human-Readable
     → Generate Google Maps Route URLs
     → Notify Drivers (Slack)
     → Notify Clients (Email)
```

---

## 🖼️ Example Output

| Driver | From               | To                   | ETA    | Distance | Route                                                                                 |
| ------ | ------------------ | -------------------- | ------ | -------- | ------------------------------------------------------------------------------------- |
| Juan   | Av. Res. dos Lagos | R. Francisco Pachêco | 15 min | 8.3 km   | [Open in Maps](https://www.google.com/maps/dir/-23.17,-47.06/-23.11,-47.20/?dirflg=d) |

---

## 📅 Next Steps

* Build a frontend dashboard for live route tracking
* Add real-time ETA recalculation
* Implement route optimization for multiple stops per driver

---

## 👤 Author

**Dktr Dee**
Automation | Cybersecurity | Self-Improvement
📍 [LinkedIn](https://www.linkedin.com/in/zalanlykos/) | 💻 [GitHub](https://github.com/ZalanLykos/)

