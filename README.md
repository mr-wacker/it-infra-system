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

DNS (Domain Name Server) manage domain names for each server with specific roles.

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

# 構成

- DNS
- データベース＆バックアップシステム
- フェイルオーバー・クラスタリング
- ロードバランシング
- 広範な可視化ログモジュール
- Webアプリケーション
- サービスのDocker化

# どうやって動くのか？

Webサービスを走らせているITインフラシステムです。
大学のプロジェクトで書きました。

DNS(Domain Name Server)は、特定の役割を持つ各サーバーのドメイン名を管理します。

データベースサービスはMySQLで、2週間ごとにcronjobを実行して増分バックアップを取ります。

フェールオーバー・クラスタリングは、「KeepAlive」というサービスによって行われます。あるサーバーに障害が発生した場合、システムはそのサーバーが生きているかどうかをチェックし、生きていなければ、そのサーバーの役割は生きている別のサーバーに委ねられます。

ロードバランシングはHaproxyによって行われます。Haproxyは汎用性の高いモジュールで、数十年前から開発されています。HaproxyはTCP/HTTPノーマライザーであり、HTTPリクエスト上の悪意のあるトラフィックを検出してブロックすることができます。
トラフィックコントロールも機能のひとつで、一度に接続できる数を制限したり、IPアドレスをフィルタリングしたりします。

膨大なロギングはgrafanaによって行われる。rafanaはあくまでも可視化のため。
Prometheusは様々なデータを含むログを処理し、それをgrafanaに送って表示します。

Docker化されたサービスは、管理り良い「セキュリティ」のためのもの。
Dockerised Applicationは、システムに直接インストールされているアプリケーションほど安全ではありません。

ただし、`CAP_SYS`モジュールがコンテナに含まれていないか、ハッカーが実際のサーバーをコントロールするために使用できるその他の認証情報が含まれていないか確認必要...

