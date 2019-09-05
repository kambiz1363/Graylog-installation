# Graylog With Elasticsearch Cluster
Graylog is an open-source log management tool that collect, index and analyze any machine logs.
### Graylog With Elasticsearch Cluster(3 nodes)
#### Prerequisites
we have needed 3 server
## server1, 2 & 3:
We need this additional packages:
apt-transport-https openjdk-8-jre-headless uuid-runtime pwgen
```
$ sudo apt-get update && sudo apt-get upgrade
$ sudo apt-get install apt-transport-https openjdk-8-jre-headless uuid-runtime pwgen
```
> pwgn - just server1 
#### MongoDB
```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5
$ echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.6 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.6.list
$ sudo apt-get update
$ sudo apt-get install -y mongodb-org
```
The last step is to enable MongoDB during the operating system’s startup:
```
$ sudo systemctl daemon-reload
$ sudo systemctl enable mongod.service
$ sudo systemctl restart mongod.service
```
#### Elasticsearch
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
sudo apt update
sudo apt install -y elasticsearch
```
Browse through the file, and enter the following configurations (replace the IPs with your node IPs):
> /etc/elasticsearch/elastic.yml
```
cluster.name: graylog
node.name: ${HOSTNAME}
node.master: true
node.data: true
path.data: /opt/elasticsearch/data
path.logs: /opt/elasticsearch/log
bootstrap.memory_lock: true
network.host: 192.168.43.37 # or 0.0.0.0
http.port: 9200
discovery.zen.ping.unicast.hosts: ["192.168.251.2","192.168.251.3","192.168.251.4"]
discovery.zen.minimum_master_nodes: 2
```
Running Your Elasticsearch Cluster
```
systemctl start elasticsearch
```
query Elasticsearch from any of the cluster nodes:
```
curl -XGET 'http://localhost:9200/_cluster/state?pretty'
```
Graylog Server
### 
