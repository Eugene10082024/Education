## Установка и настройка prometheus

groupadd --system prometheus
useradd -s /sbin/nologin --system -g prometheus prometheus

wget https://github.com/prometheus/prometheus/releases/prometheus-2.31.1.linux-amd64.tar.gz

tar zxvf prometheus-2.31.1.linux-amd64.tar.gz

cd prometheus-2.31.1.linux-amd64/

cp prometheus promtool /usr/local/bin

chown prometheus:prometheus /usr/local/bin/prometheus

mkdir /etc/prometheus

cp -R consoles/ console_libraries/ prometheus.yml /etc/prometheus

mkdir -p data/prometheus

chown -R prometheus:prometheus /data/prometheus /etc/prometheus/*

cd /lib/systemd/system
touch prometheus.service

prometheus.service

[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path="/data/prometheus" \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.enable-admin-api

Restart=always

[Install]
WantedBy=multi-user.target

systemctl daemon-reload
systemctl start prometheus
systemctl enable prometheus
