# Задание 1
### Установите Zabbix Server с веб-интерфейсом.

```
apt install postgresql
wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian12_all.deb
ls -la
dpkg -i zabbix-release_6.4-1+debian12_all.deb
ls -la
ls -lat
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
systemctl status zabbix-server.service 
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
vim /etc/zabbix/zabbix_server.conf
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2 
```


# Задание 2-3
### Установите Zabbix Agent на два хоста. 

```
1-хост linux
wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian12_all.deb
dpkg -i zabbix-release_6.4-1+debian12_all.deb
apt update
apt install zabbix-agent
sudo vim /etc/zabbix/zabbix_agentd.conf
systemctl restart zabbix-agent
systemctl enable zabbix-agent

2-хост windows
Скачал с оф.сайта .msi и установил (указал адрес сервера при установки)
```



