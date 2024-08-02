# elk-server
## Installation de Elastic dans ubuntu-22.04

### A faire avant toute manipulation dès la connexion à la VM
```sh
sudo -i
apt update
apt upgrade -y
```

### Installations diverses
```sh
sudo apt install -y git curl wget ca-certificates vim apt-transport-https
```

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
- Security related information
```txt
Authentication and authorization are enabled.
TLS for the transport and HTTP layers is enabled and configured.

The generated password for the elastic built-in superuser is : nXgh*8qYCD9JbolwcJlo

If this node should join an existing cluster, you can reconfigure this with
'/usr/share/elasticsearch/bin/elasticsearch-reconfigure-node --enrollment-token <token-here>'
after creating an enrollment token on your existing cluster.

You can complete the following actions at any time:

Reset the password of the elastic built-in superuser with
'/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic'.

Generate an enrollment token for Kibana instances with
 '/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana'.

Generate an enrollment token for Elasticsearch nodes with
'/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node'.

-------------------------------------------------------------------------------------------------
### NOT starting on installation, please execute the following statements to configure elasticsearch service to start automatically using systemd
 sudo systemctl daemon-reload
 sudo systemctl enable elasticsearch.service
### You can start elasticsearch service by executing
 sudo systemctl start elasticsearch.service
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.
```

Change Elasticsearch configuration

```sh
vim /etc/elasticsearch/elasticsearch.yml
```

- Remove comment marks from the following lines
```yaml
cluster.name: indira-cluster-elk
network.host: 0.0.0.0
http.port: 9200
```

Start and enable Elasticsearch:
```sh
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

Open a browser to display Elasticsearch (First log request will use superuser credentials => check security related information)

If required (for work on localhost), remove security constraints
```sh
sudo vim /etc/elasticsearch/elasticsearch.yml
```
```yml

# Enable security features
xpack.security.enabled: false

xpack.security.enrollment.enabled: false

# Enable encryption for HTTP API client connections, such as Kibana, Logstash, and Agents
xpack.security.http.ssl:
  enabled: false
  keystore.path: certs/http.p12

# Enable encryption and mutual authentication between cluster nodes
xpack.security.transport.ssl:
  enabled: false
  verification_mode: certificate
  keystore.path: certs/transport.p12
  truststore.path: certs/transport.p12

```

### 4. Install Kibana
- Data visualisation (KQL : to transform data from logstash/filebeat to json payloads)
```sh
sudo apt update
sudo apt install kibana -y
```

- Miscellaneous information
```txt
Kibana is currently running with legacy OpenSSL providers enabled! For details and instructions on how to disable see https://www.elastic.co/guide/en/kibana/8.14/production.html#openssl-legacy-provider
Created Kibana keystore in /etc/kibana/kibana.keystore
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.

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
- Data scraping
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


### Modify host file in Windows
Modifying the hosts file in Windows can be useful for redirecting domain names to specific IP addresses. Here are the steps to edit the hosts file on a Windows machine:

#### 1. Open Notepad as an Administrator
To modify the hosts file, you need administrative privileges.

1. Press the `Windows` key.
2. Type `Notepad`.
3. Right-click on `Notepad` and select `Run as administrator`.

#### 2. Open the Hosts File
1. In Notepad, go to `File` > `Open`.
2. Navigate to `C:\Windows\System32\drivers\etc`.
3. In the file type dropdown, select `All Files` (so you can see files without a `.txt` extension).
4. Select the `hosts` file and click `Open`.

#### 3. Modify the Hosts File
Add the desired mappings at the end of the file. The format is:
```
<IP address> <hostname>
```
For example, to redirect `example.com` to `127.0.0.1`, you would add:
```
127.0.0.1 example.com
```
You can also add comments by starting a line with `#`.

#### 4. Save the Hosts File
1. Save the file by clicking `File` > `Save` or pressing `Ctrl + S`.
2. Close Notepad.

#### 5. Flush DNS Cache (if necessary)
To ensure the changes take effect immediately, you might need to flush the DNS cache:

1. Press `Windows + R` to open the Run dialog.
2. Type `cmd` and press `Enter`.
3. In the Command Prompt window, type the following command and press `Enter`:
   ```sh
   ipconfig /flushdns
   ```

#### Example Hosts File Entry
Here’s an example of what the hosts file might look like after modification:
```
# localhost name resolution is handled within DNS itself.
#    127.0.0.1       localhost
#    ::1             localhost

127.0.0.1 example.com
192.168.1.1 myrouter.local
```

This file redirects `example.com` to `127.0.0.1` and `myrouter.local` to `192.168.1.1`.

By following these steps, you can successfully edit the hosts file in Windows to manage domain name resolutions on your local machine.


### Commandes Vagrant
#### Destruction de la machine
`vagrant destroy -f`

#### Démarrage de la machine
`vagrant up`

#### Etat de la machine
`vagrant status`

### Ins