# Fasal_Kavach# IoT Environmental Monitoring System

A full-stack IoT project that monitors real-time environmental data (temperature, humidity, and soil moisture) using an ESP32, sends it to a cloud MQTT broker, stores it in a database via a Python backend, and displays it on a React Native mobile application.

## Features

- **Real-Time Data Collection**: ESP32 with DHT22 and soil moisture sensors.
- **Secure Communication**: Uses TLS/SSL for secure MQTT communication.
- **Data Persistence**: Python (Flask) backend saves sensor data to a database.
- **Cross-Platform Mobile App**: Built with React Native (Expo) to display data.
- **Modern Styling**: Utilizes NativeWind for utility-first styling in the mobile app.

## System Architecture

The system follows a simple, scalable architecture:

```
[ESP32 Sensor Node] -> (Wi-Fi) -> [HiveMQ MQTT Broker] <- (Internet) <- [Python/Flask Backend] <-> [Database]
                                                                                ^
                                                                                |
                                                                         (REST API / TBD)
                                                                                |
                                                                                v
                                                                      [React Native Mobile App]
```

1.  **ESP32**: Reads sensor data and publishes it to MQTT topics.
2.  **MQTT Broker**: Receives data and forwards it to subscribed clients.
3.  **Backend**: Subscribes to MQTT, receives data, and stores it in a database.
4.  **Mobile App**: Fetches data from the backend to display to the user.

## Tech Stack

| Component      | Technologies Used                                       |
| -------------- | ------------------------------------------------------- |
| **IoT Device** | C++ (Arduino), ESP32, DHT22, Soil Moisture Sensor       |
| **Backend**    | Python, Flask, SQLAlchemy, Paho-MQTT                    |
| **Frontend**   | React Native, Expo, TypeScript, NativeWind, TailwindCSS |
| **Cloud**      | HiveMQ Cloud (MQTT Broker)                              |
| **Database**   | SQLite / PostgreSQL / MySQL (via SQLAlchemy)            |

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) and npm
- [Python](https://www.python.org/) and pip
- [Arduino IDE](https://www.arduino.cc/en/software) with ESP32 board support
- An account on [HiveMQ Cloud](https://www.hivemq.com/cloud/) or another MQTT broker.

### 1. Backend Setup (`server/`)

```bash
# Navigate to the server directory
cd server

# Create a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`

# Install dependencies
pip install -r requirements.txt

# Create a .env file and add your credentials
cp .env.example .env
# Now, edit the .env file with your details
```

Your `.env` file should look like this:
```
MQTT_HOST="your-mqtt-broker-url.s1.eu.hivemq.cloud"
MQTT_PORT=8883
MQTT_USERNAME="your_mqtt_username"
MQTT_PASSWORD="your_mqtt_password"
DATABASE_URL="sqlite:///database.db" # Or your PostgreSQL/MySQL URL
```

### 2. Frontend Setup (`my-expo-app/`)

```bash
# Navigate to the mobile app directory
cd my-expo-app

# Install dependencies
npm install
```

### 3. IoT Device Setup (`mqtt/`)

1.  Open `mqtt/sketch_jul09b/sketch_jul09b.ino` in the Arduino IDE.
2.  Install the required libraries: `PubSubClient`, `DHT sensor library`.
3.  Update the following variables in the code with your credentials:
    - `ssid` & `password` (your Wi-Fi details)
    - `mqtt_server`, `mqtt_user`, `mqtt_pass` (your MQTT broker details)
4.  Connect your ESP32, select the correct board and port, and upload the sketch.

## Usage

1.  **Run the Backend Server**:
    ```bash
    # In the server/ directory with virtual environment activated
    flask run
    ```
    The MQTT client will start automatically and begin listening for sensor data.

2.  **Run the Mobile App**:
    ```bash
    # In the my-expo-app/ directory
    npm start
    ```
    Scan the QR code with the Expo Go app on your phone to run the application.

3.  **Power on the ESP32**:
    Once powered, the ESP32 will connect to Wi-Fi and start publishing sensor data. You should see the data being logged by the backend server.

