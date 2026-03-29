# 🚖 Uber Real-Time Data Engineering Project

[![Project Overview](https://img.shields.io/badge/Project-Data_Engineering-blue.svg)](#)
[![Python Version](https://img.shields.io/badge/Python-3.8%2B-blueviolet.svg)](#)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100%2B-009688?logo=fastapi&logoColor=white)](#)
[![Azure Event Hub](https://img.shields.io/badge/Azure-Event_Hub-0078D4?logo=microsoftazure&logoColor=white)](#)

Welcome to my **Uber Real-Time Data Engineering Project**! I built this application to simulate a real-time data streaming pipeline. It generates dynamic, realistic Uber ride data, streams it to **Azure Event Hubs**, and exposes a web interface to book and visualize rides using **FastAPI**.

---

## 🏗️ Architecture

![Project Architecture](architecture.png)
*(Alternatively, you can view the [SVG Version](Uber_Project.svg))*

The core pipeline operates as follows:
1. **Passenger Application**: A FastAPI web app simulates a user booking a ride.
2. **Data Generator (`data.py`)**: Uses Python `Faker` to generate detailed ride metrics (pricing, duration, distance, passenger/driver details).
3. **Data Publisher (`connection.py`)**: Streams the generated JSON payload to an Azure Event Hub in real-time.
4. **Data Consumer**: (Target pipeline not implemented in this repo, but ready for Azure Stream Analytics/Databricks).

---

## ✨ Features

- **Real-Time Data Streaming:** Sends event-driven data directly to Azure Event Hub.
- **Realistic Data Generation:** Simulates ride prices, tip amounts, surge multipliers, cancellations, and geographic details using customized `Faker` functions.
- **RESTful API:** Exposes endpoints to render UI templates and handle ride generation.
- **Environment Configuration:** Secure credential management via `.env` files.

---

## 🛠️ Project Structure

```bash
Uber_De_project/
│
├── api.py               # FastAPI application with routing logic
├── connection.py        # Logic to send events to Azure Event Hub
├── data.py              # Schema mapping and Uber data generation functions
├── requirements.txt     # Python dependencies
├── templates/           # HTML Jinja2 templates for the API endpoints
├── architecture.png     # Architectural diagram
└── .env                 # (Ignored) Environment variables for Azure credentials
```

---

## 🚀 Getting Started

### 1. Prerequisites

- Python 3.8+
- An Active **Azure Event Hub** Namespace & Instance.

### 2. Installation

Clone this repository and install the necessary dependencies:

```bash
# Clone the repository
git clone https://github.com/yatharthdaksh27/Uber_data_engineering_project.git
cd Uber_data_engineering_project

# Create and activate a virtual environment (optional but recommended)
python -m venv .venv
source .venv/bin/activate  # On Windows use: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 3. Environment Setup

Create a `.env` file in the root directory and configure your Azure Event Hub credentials:

```env
CONNECTION_STRING="Endpoint=sb://<your-namespace>.servicebus.windows.net/;SharedAccessKeyName=<key-name>;SharedAccessKey=<your-key>"
EVENT_HUBNAME="<your-event-hub-name>"
```

### 4. Running the Application

**Option A: Run the Web App (FastAPI)**

Start the FastAPI server:

```bash
python api.py
# OR
uvicorn api:app --host 0.0.0.0 --port 8000 --reload
```
Navigate to [http://localhost:8000](http://localhost:8000) to access the booking interface!

**Option B: Test Event Hub Publisher (CLI)**

You can manually trigger a mock ride generation and Event Hub push without the web server:

```bash
python connection.py
```
This will print the generated JSON payload to your console and confirm if the transmission to Azure was successful.

---

## 📊 Data Schema

The generated data (`data.py`) outputs a complex structure containing:
- **Keys/IDs:** `ride_id`, `passenger_id`, `driver_id`
- **Measures:** `distance_miles`, `duration_minutes`, `base_fare`, `surge_multiplier`, `total_fare`
- **Location:** `pickup_address`, `dropoff_address`, latitude/longitude
- **Demographics:** `driver_name`, `vehicle_model`, `payment_method`

*(Check the JSON output after running `connection.py` for a full schema example).*
