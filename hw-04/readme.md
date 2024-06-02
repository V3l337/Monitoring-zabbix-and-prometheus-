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
![alt text](https://github.com/V3l337/homework_netology/blob/main/hw-04/screen/1.png)

# Задание 2
##### Установка Node Exporter
```
sudo wget https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-amd64.tar.gz
sudo tar xfvz node_exporter-1.8.1.linux-amd64.tar.gz
sudo mkdir /etc/prometheus/node-exporter
sudo cp node_exporter /etc/prometheus/node-exporter/
sudo chown -R prometheus:prometheus /etc/prometheus/node-exporter
```

Проверка 
```
./node_exporter
```
###### Создания сервиса для авто.работы
```
sudo /etc/systemd/system/node_exporter.service
```
```python
[Unit] 
Description=Node Exporter Lesson 9.4-MatveevVB
After=network.target 
[Service] 
User=prometheus 
Group=prometheus 
Type=simple 
ExecStart=/etc/prometheus/node-exporter/node_exporter 
[Install] 
WantedBy=multi-user.target
```
```
sudo systemctl daemon-reload
sudo systemctl start node_exporter.service
sudo systemctl enable node_exporter.service
sudo systemctl status node_exporter.service
```
![alt text](https://github.com/V3l337/homework_netology/blob/main/hw-04/screen/2.png)

# Задание 3

Добавил в prometheus.yml таргет localhost:9100

sudo systemctl restart prometheus

![alt text](https://github.com/V3l337/homework_netology/blob/main/hw-04/screen/31.png)
![alt text](https://github.com/V3l337/homework_netology/blob/main/hw-04/screen/32.png)


# Задание 4

docker run -d --name=grafana -p 3000:3000 huecker.io/grafana/grafana

![alt text](https://github.com/V3l337/homework_netology/blob/main/hw-04/screen/4.png)

# Задание 5

![alt text](https://github.com/V3l337/homework_netology/blob/main/hw-04/screen/5.png)


