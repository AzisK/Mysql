# Mysql on Docker

TL;DR go to [Setup Database](#setup-database) to start the database

- Mysql setup on Docker is convenient because it is easy to switch through different Mysql versions or even have different Mysql containers with different versions
- It is also good becaue it takes care of all the instalations and all we need to do is specify the version via tag, e.g. `mysql:5.7.27`
- It is easy to configure Mysql through `docker-compose.yaml`. We can specify the database, user, password, port.
- Docker also allows to have database initialization scripts that are run when the database is created. This can be very convenient when developing various applications using databases. In this case the scripts are in the `initdb` folder. They are run in alphabetical order and should not have any SQL syntax errors.

---

## Setup Database

1. Setup Docker and Docker Compose
* Docker https://docs.docker.com/get-docker/
* Docker Compose https://docs.docker.com/compose/install/
2. Start database `docker-compose up`

---

## Restart Database From Zero

Because we have a volume specified and have it bound to `/var/lib/mysql`, the Mysql data is persisted even if our containers go down or are removed. To fully restart the database and remove its current data, we need to delete the volume. We need to delete the volume also if we want for Docker to run the initialization scripts with the next run as well.

```bash
    DRIVER              VOLUME NAME
    local               mysql_mysql-data
```

1. Stop docker containers `docker-compose down`
2. Search for the correct volume `docker volume list`
3. Delete the volume `docker volume rm mysql_mysql-data`
4. Run `docker-compose up` to start the stack
5. Check if the are no SQL errors

---

## Addition

### Adminer

Adminer is a database client that we can use to connect to the database, look at the data, run commands. In this case it can be found via http://localhost:8080/. We can connect to it using

Field | Value
---|---
System | MySQL
Server | mysql
Username | root
Password | root
Database | test_db
