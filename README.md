Installations and Pre-requisites

1) Docker
sudo apt update
sudo apt install -y docker.io
sudo usermod -aG docker $USER

2) Nginx
sudo apt update
sudo apt install -y nginx

3) Grafana
sudo apt  install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt update
sudo apt install grafana

4) Loki
sudo apt install -y unzip wget
mkdir -p /home/ubuntu/loki
cd /home/ubuntu/loki
wget https://github.com/grafana/loki/releases/download/v3.4.2/loki-linux-amd64.zip
unzip loki-linux-amd64.zip
sudo cp loki-linux-amd64 /usr/local/bin/loki
loki --version

5) Promtail
sudo apt install -y unzip wget
mkdir /home/ubuntu/promtail
cd /home/ubuntu/promtail
wget https://github.com/grafana/loki/releases/download/v3.4.2/promtail-linux-amd64.zip
unzip promtail-linux-amd64
sudo cp promtail-linux-amd64 /usr/local/bin/promtail
promtail --version

SETUP:
1) Loki
cd /home/ubuntu/loki
vim config.yml
"ADD THE CONTENTS OF config.yml FROM THE FILE STORED IN THE REPOSITORY"
vim loki.service
"ADD THE CONTENTS OF loki.service FROM THE FILE STORED IN THE REPOSITORY"
mkdir -p /etc/loki
cp config.yml /etc/loki
cp loki.service /etc/systemd/system/loki.service
sudo systemctl daemon-reload
sudo systemctl start loki

2) Promtail
cd /home/ubuntu/promtail
vim config.yml
"ADD THE CONTENTS OF config.yml FROM THE FILE STORED IN THE REPOSITORY"
vim promtail.service
"ADD THE CONTENTS OF promtail.service FROM THE FILE STORED IN THE REPOSITORY"
mkdir -p /etc/promtail
cp config.yml /etc/promtail
cp promtail.service /etc/systemd/system/promtail.service
sudo systemctl daemon-reload
sudo systemctl start promtail
