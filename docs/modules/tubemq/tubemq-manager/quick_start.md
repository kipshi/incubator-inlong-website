---
title: Deployment
---

All deploying files at `inlong-tubemq-manager` directory.

## Configuration
- create `tubemanager` and account in MySQL.
- Add mysql information in conf/application.properties:

```ini
# mysql configuration for manager
spring.datasource.url=jdbc:mysql://mysql_ip:mysql_port/tubemanager
spring.datasource.username=mysql_username
spring.datasource.password=mysql_password
```

## Dependencies
- If the backend database is MySQL, please download [mysql-connector-java-8.0.26.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.26/mysql-connector-java-8.0.26.jar) and put it into `lib/` directory.

- If the backend database is PostgreSQL, there's no need for additional dependencies.

## Start

``` bash
$ bin/start-manager.sh 
```

## Register TubeMQ cluster

    vim bin/init-tube-cluster.sh

replace the parameters below
```
TUBE_MANAGER_IP=  
TUBE_MANAGER_PORT=   
TUBE_MASTER_IP=   
TUBE_MASTER_PORT=
TUBE_MASTER_WEB_PORT=
TUBE_MASTER_TOKEN=
```

then run：
```
sh bin/init-tube-cluster.sh
```

this will create a cluster with id = 1, note that this operation should not be executed repeatedly.