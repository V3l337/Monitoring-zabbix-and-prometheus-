# Задание 1
###### Установка Prometheus
```
sudo useradd --no-create-home --shell /bin/false prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.52.0/prometheus-2.52.0.linux-amd64.tar.gz
sudo tar xfvz prometheus-2.52.0.linux-amd64.tar.gz 
```
```
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
sudo cp prometheus promtool /usr/local/bin/
sudo cp -R console_libraries/ /etc/prometheus/
sudo cp -R consoles/ /etc/prometheus/
sudo cp -R prometheus.yml /etc/prometheus/
```
```
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /usr/local/bin/prometheus 
sudo chown -R prometheus:prometheus /usr/local/bin/promtool
sudo chown -R prometheus:prometheus /var/lib/prometheus/
```
###### Проверка
```
/usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus/ --web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/console_libraries
```
###### Создания сервиса для авто.работы
```
sudo vim /etc/systemd/system/prometheus.service
```
```python
[Unit]
Description=Prometheus Service Netology Lesson 9.4 - MatveevVB
After=network.target
[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus/ \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure
[Install]
WantedBy=multi-user.target
```
```
sudo systemctl restart prometheus.service
sudo systemctl enable prometheus.service
sudo systemctl status prometheus.service
```


# Задание 2
##### Установка nodes

