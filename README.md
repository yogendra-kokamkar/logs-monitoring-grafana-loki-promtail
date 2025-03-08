# Project Setup

This guide will walk you through the installation and setup process for the tools needed for logging and monitoring, including Docker, Nginx, Grafana, Loki, and Promtail on UBUNTU LINUX.

![Flow_Diagram](https://github.com/user-attachments/assets/e83eecd4-da58-4002-8e32-a0a1df85cb82)

## Installations and Pre-requisites

### 1) Docker

To install Docker, run the following commands:

```bash
sudo apt update
sudo apt install -y docker.io
sudo usermod -aG docker $USER
```

### 2) Nginx

To install Nginx, run the following commands:

```bash
sudo apt update
sudo apt install -y nginx
```

### 3) Grafana

To install Grafana, run the following commands:

```bash
sudo apt  install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt update
sudo apt install -y grafana
sudo systemctl start grafana-server
```

### 4) Loki

To install Loki, run the following commands:

```bash
sudo apt install -y unzip wget
mkdir -p /home/ubuntu/loki
cd /home/ubuntu/loki
wget https://github.com/grafana/loki/releases/download/v3.4.2/loki-linux-amd64.zip
unzip loki-linux-amd64.zip
sudo cp loki-linux-amd64 /usr/local/bin/loki
loki --version
```

### 5) Promtail

To install Promtail, run the following commands:

```bash
sudo apt install -y unzip wget
mkdir -p /home/ubuntu/promtail
cd /home/ubuntu/promtail
wget https://github.com/grafana/loki/releases/download/v3.4.2/promtail-linux-amd64.zip
unzip promtail-linux-amd64
sudo cp promtail-linux-amd64 /usr/local/bin/promtail
promtail --version
```

## SETUP:

### 1) Loki

```bash
cd /home/ubuntu/loki
nano config.yml
#"ADD THE CONTENTS OF config.yml FROM THE FILE STORED IN THE REPOSITORY"
nano loki.service
#"ADD THE CONTENTS OF loki.service FROM THE FILE STORED IN THE REPOSITORY"
sudo mkdir -p /etc/loki
sudo cp config.yml /etc/loki
sudo cp loki.service /etc/systemd/system/loki.service
sudo systemctl daemon-reload
sudo systemctl start loki
```

### 2) Promtail

```bash
cd /home/ubuntu/promtail
nano config.yml
"ADD THE CONTENTS OF config.yml FROM THE FILE STORED IN THE REPOSITORY"
nano promtail.service
"ADD THE CONTENTS OF promtail.service FROM THE FILE STORED IN THE REPOSITORY"
sudo mkdir -p /etc/promtail
sudo cp config.yml /etc/promtail
sudo cp promtail.service /etc/systemd/system/promtail.service
sudo systemctl daemon-reload
sudo systemctl start promtail
```

### 3) Grafana

Nginx Logs Queries

```bash
{job="nginx", filename="/var/log/nginx/access.log"}
count_over_time({job="nginx"} |= ` 404 ` [1h])
count_over_time({job="nginx"} |= ` 200 ` [1h])
```

Docker Logs Queries

```bash
{job="docker"} |~ ".*"
```

![grafana_8](https://github.com/user-attachments/assets/48340a57-9eed-4873-b9c0-6f854934adbc)
