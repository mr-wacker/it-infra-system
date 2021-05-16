# What is this?

This is a part of building infrastructure project with all essential features.

- DNS
- Database & Backup System
- Failover clustering
- Load balancing
- Extensive Visualised Logging Modules
- Web Application
- Dockerised Service

# How does this work?

DNS (Domain Name Server) will control each server with specific roles.

Database Service is MySQL and every 2 weeks it runs a cronjob to take incremental backup.

Failover clustering is performed by the Service called `KeepAlive` . If one server fails, the system checks whether the server is alive, if not, the server's role will be delegated to another server that is alive.

Load balancing is performed by Haproxy. Haproxy is versatile module and it's been developed for a few decades. It is a TCP/HTTP normalizer, so it can detect malicious traffic on HTTP request to block.
Traffic control is another feature, to limit the number of connection at a time, filtering IP addresses and etc.

Extensive Logging is performed by grafana. Grafana is just for visualisation. 
Prometheus process logs with various kinds of data and send them to Grafana to show.

Dockerised Service is for better management and better `security` .
Dockerised Application can't be as insecure as the application that's directly installed on the system.

However, make sure to check `CAP_SYS` module is not included in container and other credentials information which a hacker can use to gain a control on the actual server.

## References 

https://prometheus.io/docs/introduction/overview/

https://www.ansible.com/
