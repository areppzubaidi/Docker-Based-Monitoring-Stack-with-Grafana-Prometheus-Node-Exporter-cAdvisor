Great! Let’s build a **basic monitoring stack using Docker** that includes:

* **Prometheus** – for collecting metrics
* **Node Exporter** – to monitor your **laptop's CPU, memory, and disk**
* **cAdvisor** – to monitor **Docker containers**
* **Grafana** – to visualize everything in dashboards

---

## 🎯 Goal:

✅ Monitor:

* Laptop system (CPU, Memory, Disk)
* Docker container resource usage
* View data in Grafana Dashboard

---

## 🧱 Project Structure

```
docker-monitoring/
├── prometheus/
│   └── prometheus.yml
├── docker-compose.yml
└── README.md
```

---

## 1️⃣ `docker-compose.yml`

```yaml
version: '3'

services:
  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  node-exporter:
    image: prom/node-exporter
    ports:
      - "9100:9100"

  cadvisor:
    image: gcr.io/cadvisor/cadvisor:v0.47.2
    ports:
      - "8080:8080"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
      - /dev/disk/:/dev/disk:ro

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    volumes:
      - grafana-data:/var/lib/grafana

volumes:
  grafana-data:
```

---

## 2️⃣ `prometheus.yml`

Create this inside the `prometheus/` folder.

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['prometheus:9090']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']

  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor:8080']
```

---

## 3️⃣ 📦 Run the Stack

From the root folder:

```bash
docker-compose up -d
```

Then access:

* Grafana: [http://localhost:3000](http://localhost:3000)
  Default login: `admin` / `admin`
* Prometheus: [http://localhost:9090](http://localhost:9090)
* cAdvisor: [http://localhost:8080](http://localhost:8080)

---

## 4️⃣ 📊 Setup Grafana Dashboard (Manual Setup)

1. Login to Grafana at `localhost:3000`
2. Add Prometheus as a Data Source:

   * Name: `Prometheus`
   * URL: `http://prometheus:9090`
3. Import dashboard:

   * Click “+” > Import
   * Use the following IDs:

     * **Node Exporter Full**: `1860`
     * **Docker Monitoring**: `193`
   * Select your Prometheus data source

---

## 📄 README.md

````markdown
# 🐳 Docker Monitoring Stack (Prometheus + Grafana)

This is a basic Docker-based monitoring setup using:

- Prometheus (for scraping metrics)
- Node Exporter (for system metrics)
- cAdvisor (for Docker container metrics)
- Grafana (for dashboards)

## 🚀 Getting Started

### 1. Clone the Repo

```bash
git clone https://github.com/yourusername/docker-monitoring.git
cd docker-monitoring
````

### 2. Start the Stack

```bash
docker-compose up -d
```

### 3. Access Services

* Prometheus: [http://localhost:9090](http://localhost:9090)
* Grafana: [http://localhost:3000](http://localhost:3000)
* cAdvisor: [http://localhost:8080](http://localhost:8080)

Default Grafana login:

* Username: `admin`
* Password: `admin`

---

## 📊 Dashboards

After logging into Grafana:

1. Go to **"Add data source"**, select **Prometheus**, and set URL: `http://prometheus:9090`
2. Import dashboards:

   * Node Exporter Full → ID: `1860`
   * Docker Metrics → ID: `193`

---

## 🧠 What’s Being Monitored

| Component     | Metrics                                  |
| ------------- | ---------------------------------------- |
| Node Exporter | CPU, Memory, Disk (your host system)     |
| cAdvisor      | Docker container CPU, Mem, Network, Disk |

---

## 📦 Requirements

* Docker
* Docker Compose

---

## 📄 License

MIT License

```

---

Would you like me to:
- Generate a preconfigured **Grafana dashboard JSON** file?
- Add a GitHub Actions workflow to build and deploy this?

Let me know!
```
