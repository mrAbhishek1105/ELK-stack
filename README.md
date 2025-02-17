# ELK Stack 

___


The ELK stack is a collection of open-source tools that work together to search, analyze, and visualize data. The acronym ELK stands for Elasticsearch, Logstash, and Kibana. 
The ELK Stack (now often called the Elastic Stack) is a powerful log management and data analysis solution. It consists of three main components:

- Elasticsearch – A distributed search and analytics engine. It stores and indexes log data and allows fast searching.
- Logstash – A data processing pipeline that ingests logs from various sources, processes them, and sends them to Elasticsearch.
- Kibana – A visualization and dashboard tool that helps users analyze and monitor data stored in Elasticsearch.

### Use Cases:
- Log Monitoring – Collect logs from applications, servers, or containers.
- Security Analytics – Detect and analyze security threats.
- Performance Monitoring – Track server and application performance in real time.


## Implementation
-----

###  Prerequisites
- A Linux server (Ubuntu/Debian/CentOS)
- Java 8 or higher (needed for Logstash)


### Start Working

#### Installation of Ubantu
- Go to AWS and make Ec2
- ec2 with  OS ubantu and Instance type t$.medium for 2 vCPY and 4 gigMemory.
- ON Network Setting Allow SSH, HTTP, HTTPS & (include all traffic in inbound rule)

----
## Installation of ELK on Ubantu
> before starting the actuall code, first update the os 

 ```bash 
 sudo apt-get update
 ```

#### Install java (required for Elasticsearch & Logstash)

```bash
sudo apt-get install -y openjdk-8-jdk
java -version
```

#### Instal Nginx

```bash
sudo apt-get -y install nginx
systemctl status nginx
nginx -v
```

### Install and Configure Elasticsearch

#### Add Elasticsearch Repository
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apt-transport-https ca-certificates curl -y
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update
```
#### Install Elasticsearch
```bash
sudo apt install elasticsearch -y
```

#### Enable and Start Elasticsearch
```bash
sudo systemctl enable --now elasticsearch
systemctl status elasticsearch
```

#### Configure Elasticsearch
Edit the configuration file:
>Uncomment and set inside the file.
> - my-application
> - network.host: localhost

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```
### Install and Configure Kibana

#### Install Kibana

```bash
sudo apt install kibana
```

#### Configure Kibana
Edit the configuration file:
>Uncomment and set inside the file.
> - Server.port: 5601
> - Server.host: localhost
```bash
sudo nano /etc/kibana/kibana.yml
```
#### Start Kibana

```bash
sudo systemctl start kibana
sudo systemctl enable kibana
sudo systemctl status kibana
```

### Install and Configure Logstash
#### Install Logstash
```bash
sudo apt install logstash
```
#### Start Logstash
```bash
sudo systemctl start logstash
sudo systemctl enable logstash
sudo systemctl status logstash
```

### Install and Configure Filebeat

#### Install Filebeat

```bash
sudo apt install filebeat
```
#### Start Filebeat

```bash
sudo systemctl start filebeat
sudo systemctl enable filebeat
sudo systemctl status filebeat
```


## Set Up Nginx Reverse Proxy for kibana
#### Install Apache Utils for Authentication
```bash
sudo apt install apache2-utils -y
```

#### Set Up Basic Authentication for Kibana
> Enter password when prompted
```bash
sudo htpasswd -c /etc/nginx/.htpasswd youruser
```
#### Configure Nginx for Kibana

```bash
sudo mv /etc/nginx/sites-available/default /etc/nginx/sites-available/new-default
sudo nano /etc/nginx/sites-available/new-default
```
> Add the Following Configuration
```bash
server {
    listen 80;
    server_name <your-server-ip>; #replace with your ec2 IP

    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/.htpasswd;

    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

#### Restart Nginx
```bash
sudo nginx -t
sudo systemctl restart nginx
```

### Verify Installation
> Access Kibana in Browser

`http://your-server-ip:5601/`
- Enter the username and password set in .htpasswd file.

> You should now see the Kibana dashboard.



<ins> This guide provides a structured way to install and configure the ELK Stack (Elasticsearch, Logstash, Kibana, and Filebeat) with Nginx as a reverse proxy.</ins>
