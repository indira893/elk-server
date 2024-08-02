# elk-server
## Installation de Elastic dans ubuntu-22.04

### A faire avant toute manipulation dès la connexion à la VM
`sudo -i`
`apt update`
`apt upgrade -y`

### Installations diverses
`sudo apt install -y curl wget ca-certificates vim apt-transport-https`

To install the Elastic Stack 8.x (Elasticsearch, Logstash, Kibana, and optionally Beats) on Ubuntu 22.04 with the dearmor tool, follow these steps:

### 1. Install Java
Elasticsearch requires Java. Install OpenJDK:
```sh
sudo apt update
sudo apt install openjdk-17-jdk -y
```

### 2. Install the Elastic Stack APT Repository
Add the Elastic APT repository to your system and install the Elastic Stack packages.

#### a. Install the dearmor tool
```sh
sudo apt install debsigs
```

#### b. Add the Elastic GPG key and repository
Download and add the Elastic GPG key:
```sh
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | gpg --dearmor | sudo tee /usr/share/keyrings/elastic-archive-keyring.gpg > /dev/null
```

Add the Elastic APT repository:
```sh
echo "deb [signed-by=/usr/share/keyrings/elastic-archive-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```

### 3. Install Elasticsearch
```sh
sudo apt update
sudo apt install elasticsearch -y
```

Start and enable Elasticsearch:
```sh
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

### 4. Install Kibana
```sh
sudo apt update
sudo apt install kibana -y
```

Start and enable Kibana:
```sh
sudo systemctl enable kibana
sudo systemctl start kibana
```

### 5. Install Logstash
```sh
sudo apt update
sudo apt install logstash -y
```

Start and enable Logstash:
```sh
sudo systemctl enable logstash
sudo systemctl start logstash
```

### 6. Install Beats (optional)
To install Filebeat as an example:
```sh
sudo apt update
sudo apt install filebeat -y
```

Start and enable Filebeat:
```sh
sudo systemctl enable filebeat
sudo systemctl start filebeat
```

### 7. Configuration
#### a. Elasticsearch
Edit the Elasticsearch configuration file:
```sh
sudo nano /etc/elasticsearch/elasticsearch.yml
```
Set the `network.host` to allow connections from your local network:
```yaml
network.host: 0.0.0.0
```

#### b. Kibana
Edit the Kibana configuration file:
```sh
sudo nano /etc/kibana/kibana.yml
```
Set the server host:
```yaml
server.host: "0.0.0.0"
```

#### c. Logstash
Edit the Logstash configuration file:
```sh
sudo nano /etc/logstash/logstash.yml
```
Define the pipeline configurations.

### 8. Access Kibana
Open your browser and navigate to:
```
http://<your-server-ip>:5601
```

### 9. Test the Installation
You can use the following commands to ensure that each service is running properly:
```sh
sudo systemctl status elasticsearch
sudo systemctl status kibana
sudo systemctl status logstash
sudo systemctl status filebeat
```

### 10. Secure the Installation
Secure your Elasticsearch and Kibana instances by setting up user authentication and HTTPS.

### 11. Use Elasticsearch, Kibana, and Logstash
Start adding data to Elasticsearch and visualize it using Kibana. Configure Logstash to process and forward log data.

By following these steps, you should have a functional Elastic Stack 8.x installation on Ubuntu 22.04.

### Commandes Vagrant
#### Destruction de la machine
`vagrant destroy -f`

#### Démarrage de la machine
`vagrant up`

#### Etat de la machine
`vagrant status`

### Ins