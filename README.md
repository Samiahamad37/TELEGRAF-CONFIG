

 Telegraf Configuration – MQTT to InfluxDB 2.0 (AQMS)

📌 Overview

This repository contains a Telegraf configuration file used for collecting air quality monitoring data from The Things Network (TTN) via MQTT and storing it in InfluxDB 2.0.

The setup is part of an Air Quality Monitoring System (AQMS) designed to ingest real-time sensor data from IoT devices and support analysis, visualization, and decision-making.



 🛠️ Architecture Summary

**Data Flow:**

```
IoT Sensors → The Things Network (TTN)
            → MQTT (TLS)
            → Telegraf
            → InfluxDB 2.0
```



⚙️ Telegraf Role

Telegraf acts as a **data ingestion agent** that:

* Subscribes to TTN MQTT topics
* Extracts decoded sensor measurements from JSON payloads
* Sends structured time-series data to InfluxDB 2.0



 📁 Configuration File

* `telegraf.conf` – Main configuration file containing:

  * MQTT consumer input
  * InfluxDB 2.0 output
  * TLS security settings
  * JSON data parsing rules



🔌 MQTT Input Configuration (TTN)

Details

* **Protocol:** MQTT over TLS
* **Server:** The Things Network (EU cluster)
* **Port:** `8883`
* **Authentication:** TTN application credentials

### Subscribed Topic

```text
v3/ardhi-dar-es-salaam@ttn/devices/+/up
```

This allows Telegraf to receive uplink data from **all devices** registered under the TTN application.

### JSON Parsing

Telegraf extracts measurements from:

```text
uplink_message.decoded_payload.measurements
```

Each measurement name is dynamically assigned using:

```text
json_name_key = "name"
```

This makes the configuration flexible for multiple sensor types.



 📊 Example Data Collected

Depending on the connected sensors, the system can collect:

* CO (Carbon Monoxide)
* NO₂ (Nitrogen Dioxide)
* Temperature
* Humidity
* Other air quality parameters



🗄️ InfluxDB 2.0 Output Configuration

 Connection Details

* **InfluxDB Version:** 2.0
* **Protocol:** HTTP
* **Organization:** `myorg`
* **Bucket:** `mybucket`

Telegraf sends all parsed MQTT data to InfluxDB as **time-series measurements**, ready for querying and visualization.



 🔐 Security Configuration

* MQTT connection secured using **TLS**
* Root CA certificate:

  ```text
  isgrootx1.pem
  ```
* Certificate verification is skipped for compatibility:

  ```text
  insecure_skip_verify = true
  ```

⚠️ **Note:** For production systems, it is recommended to enable full certificate verification.



 🚀 How to Use

### 1️⃣ Install Telegraf

```bash
sudo apt install telegraf
```

Or download from:
[https://www.influxdata.com/telegraf/](https://www.influxdata.com/telegraf/)



2️⃣ Configure Certificates

Ensure the TLS certificate exists at:

```text
C:\Program Files\mosquitto\certs\isgrootx1.pem
```

3️⃣ Replace Telegraf Configuration

Copy `telegraf.conf` to:

```bash
/etc/telegraf/telegraf.conf
```



 4️⃣ Start Telegraf

```bash
sudo systemctl start telegraf
sudo systemctl enable telegraf
```



 5️⃣ Verify Status

```bash
sudo systemctl status telegraf
```



 📈 Data Visualization & Usage

Stored data in InfluxDB can be used for:

* Grafana dashboards
* Air quality trend analysis
* Machine learning model training
* Public awareness and environmental reporting



 ⚠️ Important Notes

* **Do not commit real tokens or passwords** to public repositories
* Use environment variables or `.env` files for sensitive credentials
* Restrict access to MQTT and InfluxDB endpoints



🧑‍💻 Author

**Samia Hamad**
Student – Information Systems Management
Ardhi University
-

📜 License

This configuration is provided for **academic and research purposes**.
You are free to modify and reuse it with proper attribution.


