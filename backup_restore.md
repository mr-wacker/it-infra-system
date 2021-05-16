<h1>General provisions</h1>

All services and their configurations can be restored with Ansible playbooks (except for Ansible itself), but if restoration of data or Ansible configuration/playbooks is required then custom solutions are shown.
If restoration of a service is needed to be done through Ansible and it is corrupted or absent then firstly refer to the Ansible - configuration management section to set it up.
Restorations through Ansible playbook should be done in the directory that contains them. All commands are run through console/terminal/command line; from a user "backup" for the restoration of data and configuration without Ansible on the hosts where the service and/or its data should be restored.
If you are starting as a user "ubuntu" you can become user "backup" after running next commands:

```sudo su
su backup
/bin/bash
cd ~
```


<h2>Application related services</h2>

#### MySQL - app database
To restore MySQL and its configuration run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab04_web_app.yaml
ansible-playbook lab07_grafana.yaml
ansible-playbook lab10_backups.yaml
```

To restore MySQL databases and their data run the following commands on the host with MySQL installed:

```
duplicity --no-encryption --force restore rsync://<backup_user>@<our_domain>//home/<backup_user> /home/backup/restore/
mysql agama < /home/backup/restore/agama.sql
```

###### To check the results run the next command to show AGAMA database contents
```
mysql -e 'SELECT * FROM agama.item';
```

uWSGI - app server
To restore uWSGI run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab04_web_app.yaml
```

#### AGAMA -  application
To restore AGAMA application that uses MySQL database run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab04_web_app.yaml
```

To restore containerised AGAMA application that uses MySQL database run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab12_docker.yaml
```

#### General services

Ansible - configuration management tool
To restore Ansible and playbooks go through the following options on the management host (or temporary, if management one is unavailable):


* Install Ansible if if it does not exist, otherwise this step can be skipped. There are two options:


1. Install from the repository (escalated privileges required, for that run commands as user "ubuntu"):
```
sudo apt -y install ansible
```

Afterwards PATH="$HOME/ansible-venv/bin:$PATH" string should be added to the .profile file in the /home/<ansible_user>/. The file can be open with a simple console text editor:
```
nano .profile
```


2. Install from the repository into the Python virtual environment, run in /home/<ansible_user>/ directory (escalated privileges and Python3 are required, for that run commands as user "ubuntu"):
```
sudo apt install python3.9
sudo apt install python3-venv
python3 -m venv ~/ansible-venv
~/ansible-venv/bin/pip install ansible==2.9
~/ansible-venv/bin/ansible --version
```

Afterwards PATH="$HOME/ansible-venv/bin:$PATH" string should be added to the .profile file in the /home/<ansible_user>/. The file can be open with a simple console text editor:
```
nano .profile
```

* Download Ansible repository into its new location if it does not exist. There are two options:


1. Download from GitHub
```
curl -L -o /home/<ansible_user>/ansible --header 'Authorization: token <github_token>' --header "Accept: application/vnd.github.v3+json" https://api.github.com/repos/<username>/<repository_name>/tarball/master
tar -zxf ansible
```

###### Enter the unarchived directory until you see files with .yaml extension, run playbook commands in that directory

Hosts' users configuration
To restore juri and roman users run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab02_web_server.yaml
ansible-playbook infra.yaml
```

#### Bind9 - DNS
To restore Bind9 run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab05_dns.yaml
```

Hosts' DNS resolvers configuration

To restore hosts' DNS resolvers configuration run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab05_dns.yaml
```

#### Duplicity - backup facility

To restore Duplicity run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab10_backups.yaml
```

#### Cron - time scheduler
To restore Cron run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab10_backups.yaml
```

#### Nginx - web server
To restore Nginx run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab02_web_server.yaml
```

#### Docker - containerisation service
To restore Docker run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab12_docker.yaml
```

#### Monitoring related services

##### InfluxDB - metrics/logs database
To restore InfluxDB run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab08_logging.yaml
```

##### Grafana - monitoring visualization

To restore Grafana run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab07_grafana.yaml
```

To restore containerised Grafana run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

To restore the tables in Grafana, please run the following command from machine `elvinius3` as `backup` user.

```
duplicity --no-encryption restore rsync://Elvinius@backup.devhunt.it//home/Elvinius/ /home/backup/restore/
```

```

cp -a /home/backup/restore/grafana/grafana/* /opt/docker/grafana/

cp -a /home/backup/restore/grafana/grafana/* /var/lib/grafana/

chown -R 472:472 /opt/docker/grafana/

docker start grafana
```

Then from managed host, run the following code.

```
ansible-playbook lab12_docker.yaml
```

##### Pinger - connectivity/latency checker

To restore Pinger run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):
```
ansible-playbook lab08_logging.yaml
```

##### Prometheus - metrics collector

To restore Prometheus run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab06_prometheus.yaml
```

##### Bind9 exporter
To restore Bind9 exporter run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab07_grafana.yaml
```

##### MySQL exporter
To restore MySQL exporter run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```
ansible-playbook lab07_grafana.yaml
```

##### Nginx exporters
To restore Nginx exporters run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):
```
ansible-playbook lab07_grafana.yaml
```
##### Node exporters
To restore Node exporters run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):
```
ansible-playbook lab06_prometheus.yaml
```
##### HAProxy exporters
To restore HAProxy exporters run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):
```
ansible-playbook lab13_haproxy.yaml
```
##### Keepalived exporters
To restore keepalived exporters run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):
```
ansible-playbook lab13_haproxy.yaml
```
##### Rsyslog - syslog facility
To restore Rsyslog run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):
```
ansible-playbook lab08_logging.yaml
```
##### Telegraf - metrics collector
To restore Telegraf run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):
```
ansible-playbook lab08_logging.yaml
```
#### Load balancing related services

##### HAProxy - TCP/HTTP load balancer
To restore HAProxy run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):
```
ansible-playbook lab13_haproxy.yaml
```
##### Keepalived - Layer 4 load balancer
To restore keepalived run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):
```
ansible-playbook lab13_haproxy.yaml
```