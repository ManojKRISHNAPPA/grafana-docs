# node_exporter overview
node_exporter is a Prometheus exporter for collecting and exposing various metrics related to a Linux or UNIX system. It is part of the Prometheus monitoring and alerting ecosystem and serves as a fundamental component in gathering system-level metrics.

Here's a brief overview of what node_exporter does:

System Metrics:

Collects general system-level metrics such as CPU usage, memory usage, disk I/O, and network activity.
Provides information about the overall health and performance of the system.

# Download and extract Node Exporter
- You can download node exporter from this link.
[Download Node Exporter](https://prometheus.io/download/#node_exporter)
- Extract Node Explorer
```
tar -xzvf node_exporter*.gz
```
- Create a user which we will use to run node_exporter service
Username: nodeusr
```
sudo useradd -rs /bin/false nodeusr
```


# Configuring Node Exporter to run as a service
create a file
```
sudo touch /etc/systemd/system/node_exporter.service
```

write below content in the file.
By default node_exporter runs on port `9100` , you can change it if required and specify a different port in `--web.listen-address`

```
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=nodeusr
Group=nodeusr
Type=simple
ExecStart=/usr/local/bin/node_exporter --web.listen-address=:9100

[Install]
WantedBy=multi-user.target
```
# Move Node_exporter file to 
```
mv node_exporter /usr/local/bin/node_exporter
```
# Start node_exporter service
```
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
sudo systemctl status node_exporter
```
# Allow port on filrewall & reload firewall service
```
sudo firewall-cmd --zone=public --add-port=9100/tcp --permanent
sudo systemctl restart firewalld
```

# Stress Test
```
sudo amazon-linux-extras install epel -y
sudo yum install stress -y
```
```
nohup sudo stress --cpu 8 -v --timeout 30000 &
nohup sudo stress --cpu 8 -v --timeout 30000 &
nohup sudo stress --cpu 8 -v --timeout 30000 &
nohup sudo stress --cpu 8 -v --timeout 30000 &
nohup sudo stress --cpu 8 -v --timeout 30000 &
nohup sudo stress --cpu 8 -v --timeout 30000 &
nohup sudo stress --cpu 8 -v --timeout 30000 &
nohup sudo stress --cpu 8 -v --timeout 30000 &
```

Killing stress process:
```
for i in $(ps -ef|grep -i stress|awk '{print $2}'); do kill -9 $i; done
```
```
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head
```

