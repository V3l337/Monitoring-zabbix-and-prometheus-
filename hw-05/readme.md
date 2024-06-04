# Задание 2
```
sudo wget https://github.com/prometheus/alertmanager/releases/download/v0.27.0/alertmanager-0.27.0.linux-amd64.tar.gz
sudo tar xfvz alertmanager-0.27.0.linux-amd64.tar.gz
sudo cp alertmanager /usr/local/bin/
sudo cp amtool /usr/local/bin/
sudo cp alertmanager.yml /etc/prometheus/
sudo chown prometheus:prometheus /etc/prometheus/alertmanager.yml 
sudo vim /etc/systemd/system/alertmanager.service
```
```python
[Unit]
Description=Alertmanager Service MastveevVB
After=network.target

[Service]
EnvironmentFile=-/etc/default/alertmanager
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/alertmanager --config.file=/etc/prometheus/alertmanager.yml --storage.path=/var/lib/prometheus/alertmanager $ARGS
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
```
sudo systemctl daemon-reload
sudo systemctl start alertmanger.service
sudo systemctl enable alertmanger.service
sudo systemctl status alertmanger.service
```
![alt text](https://github.com/V3l337/Monitoring-zabbix-and-prometheus-/blob/main/hw-05/screenshot/21.png)
![alt text](https://github.com/V3l337/Monitoring-zabbix-and-prometheus-/blob/main/hw-05/screenshot/22.png)
![alt text](https://github.com/V3l337/Monitoring-zabbix-and-prometheus-/blob/main/hw-05/screenshot/23.png)

# Задание 3

sudo vim /etc/prometheus/prometheus.yml 

```yaml
# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - localhost:9093
```
```
sudo systemctl restart prometheus
sudo systemctl status prometheus
```

sudo vim /etc/prometheus/alertTEST.yml

```yml
groups: # Список групп
  - name: alertTEST # Имя группы
    rules: # Список правил текущей группы
    - alert: InstanceDown # Название текущего правила
      expr: up == 0 # Логическое выражение
      for: 1m # Сколько ждать отбоя сработки перед отправкой оповещения
      labels:
        severity: critical # Критичность события
      annotations: # Описание
        description: '{{ $labels.instance }} of job {{ $labels.job }} has been down for more than 1 minute.' # Полное описание алерта
        summary: Instance {{ $labels.instance }} down # Краткое описание алерта
```

sudo vim /etc/prometheus/prometheus.yml 

```yaml
rule_files:
  - "alertTEST.yml"
```

sudo vim /etc/prometheus/alertmanager.yml

```yaml
global:
route:
  group_by: ['alertname'] # Параметр группировки оповещений — по имени
  group_wait: 30s # Сколько ждать восстановления, перед тем как отправить первое оповещение
  group_interval: 10m # Сколько ждать, перед тем как дослать оповещение о новых сработках по текущему алерту
  repeat_interval: 60m # Сколько ждать, перед тем как отправить повторное оповещение
  receiver: 'email' # Способ, которым будет доставляться текущее оповещение
receivers: # Настройка способов оповещения
  - name: 'email'
    email_configs:
    - to: 'yourmailto@todomain.com'
      from: 'yourmailfrom@fromdomain.com'
      smarthost: 'mailserver:25'
      auth_username: 'user'
      auth_identity: 'user'
      auth_password: 'paS$w0rd'
```
```
sudo systemctl restart prometheus.service
sudo systemctl status prometheus.service
```
![alt text](https://github.com/V3l337/Monitoring-zabbix-and-prometheus-/blob/main/hw-05/screenshot/3.png)

# Задание 4

sudo vim /etc/docker/daemon.json

```json
{
	"metrics-addr" : "0.0.0.0:9323",
	"experimental" : true
}
```
```
sudo systemctl restart docker
sudo systemctl status  docker
```
```
vim /etc/prometheus/prometheus.yml
```

![alt text](https://github.com/V3l337/Monitoring-zabbix-and-prometheus-/blob/main/hw-05/screenshot/3.png)
