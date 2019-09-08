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
#### Elasticsearch
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
sudo apt update
sudo apt install -y elasticsearch
```
Browse through the file, and enter the following configurations (replace the IPs with your node IPs):
> /etc/elasticsearch/elasticsearch.yml
```
cluster.name: graylog
node.name: ${HOSTNAME}
node.master: true
node.data: true
path.data: /opt/elasticsearch/data
path.logs: /opt/elasticsearch/log
network.host: 192.168.43.37 # or 0.0.0.0
http.port: 9200
discovery.zen.ping.unicast.hosts: ["192.168.43.37","192.168.43.38","192.168.43.39"]
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
## server1
Graylog Server
#### MongoDB
```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5
$ echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.6 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.6.list
$ sudo apt-get update
$ sudo apt-get install -y mongodb-org
```
The last step is to enable MongoDB during the operating system’s startup:
```
$ sudo systemctl enable mongod.service
$ sudo systemctl restart mongod.service
```
#### Graylog
Install the Graylog repository configuration and Graylog itself with the following commands:
```
$ wget https://packages.graylog2.org/repo/packages/graylog-3.0-repository_latest.deb
$ sudo dpkg -i graylog-3.0-repository_latest.deb
$ sudo apt-get update && sudo apt-get install graylog-server
$ pwgen -N 1 -s 96
```
Output:
```
luPmJZjN2wcfsKdE8rHJ428nzpi9C6lYxhpWbIhqDZVAdfXsz9EP8hCOvMoCFp3DxK5STx8a6kMps3P0ePdmW83VWjB0CIS4
```
```
echo -n yourpassword | sha256sum
```
Output:
```
e7cf3ef4f17c3999a94f2c6f612e8a888e5b1026878e4e19398b23bd38ec221a
```
Browse through the file, and enter the following configurations:
```
password_secret = luPmJZjN2wcfsKdE8rHJ428nzpi9C6lYxhpWbIhqDZVAdfXsz9EP8hCOvMoCFp3DxK5STx8a6kMps3P0ePdmW83VWjB0CIS4
root_password_sha2 = e7cf3ef4f17c3999a94f2c6f612e8a888e5b1026878e4e19398b23bd38ec221a
root_email = youremailaddress@yourdomainaddress.com
root_timezone = UTC
is_master = true
elasticsearch_max_docs_per_index = 20000000
elasticsearch_max_number_of_indices = 20
elasticsearch_shards = 3
elasticsearch_replicas = 3
http_bind_address = your-server-ip:9000
elasticsearch_discovery_zen_ping_multicast_enabled = false
elasticsearch_discovery_zen_ping_unicast_hosts = 192.168.43.37:9300,192.168.43.38:9300,192.168.43.39:9300
```
Restart Graylog service.
```
sudo systemctl restart graylog-server
```
.
