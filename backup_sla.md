<h4>Table of contents: </h4>


<h1>Backup services SLA </h1>

<h2>Introduction </h2>

<h3>Definition of SLA </h3>
This Service Level Agreement (this “SLA”) governs the backup Services. Operations and security teams may update, amend, modify or supplement this SLA from time to time. The terms and conditions of this SLA are applicable to the Managed Backup and Disaster Recovery Services only.

<h3>Structure of SLA </h3>
This SLA describes the backup approach for all services in the infrastructure:

<h4>Web services - Nginx </h4>

<h4>App services - Agama</h4>

<h4>Database services - MySQL, InfluxDB</h4>

<h4>DNS service - Bind9</h4>

<h4>Monitoring services - Prometheus, Grafana, Telegraf, Pinger, Bind9 exporter, MySQL exporter, Nginx exporters, Node exporters, Rsyslog, HAProxy exporter, Keepalived exporter</h4>

<h4>Backup services - Scripts, Duplicity</h4>

<h4>Containerisation services - Docker</h4>

<h4>Load balancing services - HAProxy, keepalived</h4>

<h4>Additional services - Ansible, uWSGI, Cron</h4>

<h4>Ansible repository - https://github.com/Elvinius/ica0002 (stores configuration, roles, playbooks to configure all the above-mentioned services)</h4>

Descriptions of backup approaches contain information on specific backup attributes, such as:

Backup coverage - what is backed up and what is not <br>
Backup RPO (recovery point objective) - acceptable data loss (time period)<br>
Versioning and retention - how many backup versions are stored and for how long<br>
Usability - how is the backup recovery verified (backup should be usable)<br>
Restoration criteria - when should backup be restored<br>
Backup RTO (recovery time objective) - how long will it take to restore the service<br>


<h3>Backup coverage</h3>

Backup service covers only the next services:

Database services - MySQL

Monitoring services - Grafana

Explanation:
All the services included contain user provided information (MySQL) or some log information, reflecting the health of our infrastructure (Grafana). User and the log data, and Grafana configuration cannot be restored by any other means.
All the services not included under the backup service, such as web, app, DNS and monitoring services do not store any data produced by themselves and our service users and do not need manual configuration. Not covered services can be reliably and swiftly restored using Ansible service.

Backup RPO
Recovery point objective for:

MySQL equals 4 weeks/28 days.

Grafana equals 4 weeks/28 days.

Two types of backup can be produced: full and incremental.


Full backup contains the whole backed up data and can be solely used to restore require data or service. Full backups are done for each service covered by our backup strategy according to the backup RPO' schedule.


Incremental backup stores only the difference in the data relative to the last incremental backup produced. First incremental backup stores difference from the last created full backup. These backups form a chain, if some links disappear, they cannot be used to restore the data or service, but they allow to use less storage. Incremental backups are not produced done only for any service.


Time: backups should be done automatically every day at 01:10 (around 1 AM) for each service, according to EET (UTC +2) and EEST (UTC +3) time zones.

Explanation:
01:10 time was chosen as the time with the least activity in the region when our service is provided, some backed up services may be in the read-only mode or shutdown.
Incremental backups are not produced, main services are backed up rarely and according to the versioning and retention specification, only two versions are stored. These backups do not require a lot of storage in these amounts, as a result only more reliable full backups are created.

<h3>Versioning and retention</h3>

MySQL and Grafana backups are retained for 4 weeks/28 days, only 2 versions can be stored at the same time.

Time: the oldest/3rd backup should be deleted at at 02:10 (around 2 AM) on 28th day of every month, according to EET (UTC +2) and EEST (UTC +3) time zones. The date has been chosen considering the leap year as we want the deletion happening each month at least.

Explanation:
Retention period should be longer than the period between creation of backups to not create a window of time, when there is none. To minimize storage used and still provide reliable backup service, 4 weeks was chosen as a backup retention period for MySQL, and Grafana to retain only 2 backup versions. If the last backup is not compatible with the newer version of the used software, then there is a higher chance that the older backup will cover this issue.

<h3>Usability</h3>
Usability of the last MySQL and Grafana repository backup is regularly checked every 1 week/7 days before new modifications to Ansible repository and Grafana configuration is done. The test is done on the virtual environment setup, simulating our real infrastructure.
Explanation:
Before Ansible repository and Grafana configuration are modified, new backups are produced. New modifications should be tested in the development environment, this moment could be used to to test backups as well.

<h3>Restoration criteria</h3>

Backup restoration of the MySQL and Grafana data should be done only if it was detected and confirmed that the stored data was corrupted, modified by the unauthorized 1st/3rd party, stolen or deleted.

Explanation:
Backup restoration takes time and computing resources, there is no reason to use it, if the data it covers, was not corrupted.

<h3>Backup RTO</h3>

Backup service is expected to take maximum of 1 hours to fully recover all the data.
If the whole infrastructures should be restored, excepted maximum required time equals 2 hours.

Explanation:
Our infrastructure is small and its recovery with Ansible should be swift, meanwhile, to restore the data more time might be required because of the possible bandwidth and storage media limitations.
