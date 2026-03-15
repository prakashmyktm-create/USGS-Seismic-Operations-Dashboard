# USGS-Seismic-Operations-Dashboard
An automated Crisis Command Center built in Power BI. Transforms a rolling 30-day live USGS API feed into actionable threat intelligence using custom M-code and DAX risk modeling.

Markdown
# Global Seismic Threat Intelligence & Operations Monitor

## Executive Summary
In emergency management, raw data without context is just noise. This project is a fully automated, enterprise-grade **Crisis Command Center Dashboard** built in Power BI. It ingests a **rolling 30-day feed** of live seismic telemetry from the United States Geological Survey (USGS) via a direct REST API connection. 

By engineering advanced ETL pipelines and conditional risk-modeling DAX algorithms, this tool filters out non-actionable micro-tremors and prioritizes high-magnitude, shallow-depth events that pose the highest risk to critical infrastructure and human life.

## Technical Architecture & Automation
This dashboard is "Always-On." Because it connects directly to the USGS Web API, the data refreshes automatically without manual file uploads.

### 1. Data Extraction (Live API Integration)
* **Source:** USGS GeoJSON API (All Earthquakes, Past 30 Days).
* **Automation:** The dashboard is hard-wired to the live URL. Every refresh drops the oldest day of data and adds the most recent hour of seismic activity.

### 2. Data Transformation (ETL via Power Query / M-Code)
* **JSON Flattening:** Unpacked deep nested arrays (`features.geometry.coordinates`) to extract Longitude, Latitude, and Subterranean Depth.
* **UNIX Epoch Conversion:** Developed custom M-code to transform 13-digit millisecond timestamps into operational Date/Time formats:
  ```powerquery
  #datetime(1970, 1, 1, 0, 0, 0) + #duration(0, 0, 0, [time]/1000)
Data Integrity: Implemented logic to remove duplicate entries and categorize geographic coordinates.

3. Data Modeling (DAX)
Engineered custom calculated columns to categorize hazard severity based on logarithmic magnitude scales:

Code snippet
Risk Level = 
IF('EarthquakeData'[mag] >= 6.0, "Critical Hazard",
    IF('EarthquakeData'[mag] >= 4.5, "Moderate Hazard",
        "Low Risk"))

Operational Intelligence Features
Automated Threat Distribution: A custom GIS map where bubble sizes scale dynamically by magnitude, colored by Risk Level for immediate cognitive recognition.

Seismic Activity Pulse: A daily frequency tracker that identifies "seismic swarms," allowing crisis managers to see spikes in activity that static monthly reports often miss.

Subterranean Risk Matrix: A scatter plot analyzing Depth vs. Magnitude. This helps isolate "Shallow Threats"—events close to the surface that cause the most infrastructure destruction.

Tactical Incident Log: A real-time, auto-sorting ledger that pushes the highest magnitude threats to the top for immediate review.
