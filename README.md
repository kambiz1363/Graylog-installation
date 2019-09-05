# Graylog-installation
## How to install graylog server on ubuntu 18.04.
Graylog is an open-source log management tool that collect, index and analyze any machine logs.
### Components
MongoDB
Elasticsearch
Graylog Server
### Prerequisites
We need this additional packages:
apt-transport-https openjdk-8-jre-headless uuid-runtime pwgen
```
$ sudo apt-get update && sudo apt-get upgrade
$ sudo apt-get install apt-transport-https openjdk-8-jre-headless uuid-runtime pwgen
```
