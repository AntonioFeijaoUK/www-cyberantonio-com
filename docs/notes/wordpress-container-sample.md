wordpress container sample

## docker wordpress

start a docker container with wordpress with port mapping to access from local machine

```bash

docker pull wordpress

# multiple options to start the container, more details here [https://hub.docker.com/_/wordpress/](https://hub.docker.com/_/wordpress/)

# docker run --name some-wordpress --network some-network -d wordpress`
# docker run --name antonio-wordpress --network bridge -d wordpress`

docker run --name antonio-wordpress --network wordpress-network -p 8080:80 -d wordpress

# http://127.0.0.1:8080/

```

Database name

Database username

Database password

Database host

Table prefix (if you want to run more than one WordPress in a single database)

This information is being used to create a wp-config.php file.

If for any reason this automatic file creation does not work, do not worry.

All this does is fill in the database information to a configuration file.

You may also simply open wp-config-sample.php in a text editor, fill in your information, and save it as wp-config.php.

Need more help?

Read the support article on wp-config.php.

In all likelihood, these items were supplied to you by your web host.

If you do not have this information, then you will need to contact them before you can continue.

If you are ready…

---

##  database

starting the database container with MariaDB for wordpress

<https://hub.docker.com/_/mariadb>

<https://www.freecodecamp.org/news/tag/docker/>

<https://resources.infosecinstitute.com/topics/hacking/>

This will will start the database with port mapping, so you can access directly from your machine.

added the `--network wordpress-network` to specify the network that will share with the `wordpress` and `mariadb` container

```bash

docker run --detach --name antonio-mariadb -p 3306:3306 --network wordpress-network --env MARIADB_USER=antonio-user --env MARIADB_PASSWORD=antonio-super-cool-secret --env MARIADB_ROOT_PASSWORD=rootantonio-super-cool-secret  mariadb:latest

```

without port mapping (default)

```bash

docker run --detach --name antonio-mariadb --env MARIADB_USER=antonio-user --env MARIADB_PASSWORD=antonio-super-cool-secret --env MARIADB_ROOT_PASSWORD=rootantonio-super-cool-secret  mariadb:latest

docker run --detach --name antonio-mariadb --env MARIADB_USER=antonio-user --env MARIADB_PASSWORD=antonio-super-cool-secret --env MARIADB_ROOT_PASSWORD=rootantonio-super-cool-secret  mariadb:latest

```

The following command starts another MariaDB container instance and runs the mysql command line client against your original mariadb container, allowing you to execute SQL statements against your database instance

With this, we can create the database and user for the wordpress website

`docker run -it --rm mariadb mysql -h172.17.0.2 -uantonio-user -p`

`docker run -it --rm --network wordpress-network mariadb mysql -h127.0.0.1 -uroot -p`

CREATE DATABASE wordpress;

CREATE USER 'wpdb_user'@'localhost' IDENTIFIED BY 'SQHicxbcZ4wp7QSA5!RQZad$&V5UWVL2vFJW';

GRANT ALL PRIVILEGES ON wordpress.* TO 'wpdb_user'@'localhost';

SHOW DATABASES;

# USE wordpress

USE mysql;

SHOW TABLES;

SHOW COLUMNS FROM user;

SELECT Host,User,Password,Select_priv,Grant_priv FROM user WHERE User="wpdb_user";

---

## docker network

with the `docker network ls` we can list the docker network interfaces

with the `docker network inspect NET-WORK-ID` we can see what container are linked to that network

```bash

## list docker images

docker images

docker image ls


## list docker containers

docker container ls

docker container ls -a
```

example

```bash
docker container ls

    CONTAINER ID   IMAGE            COMMAND                  CREATED        STATUS        PORTS                    NAMES
    d3b96bb0fcc2   mariadb:latest   "docker-entrypoint.s…"   22 hours ago   Up 22 hours   0.0.0.0:3306->3306/tcp   antonio-mariadb
    8e7e8181ed85   wordpress        "docker-entrypoint.s…"   22 hours ago   Up 22 hours   0.0.0.0:8080->80/tcp     antonio-wordpress
```

list available docker network interfaces

```bash
docker network ls

    NETWORK ID     NAME      DRIVER    SCOPE
    9a8abce56526   bridge    bridge    local
    b5f6f44b6a39   host      host      local
    a8508a6f7890   none      null      local
```

inspect what containers are on the docker network interfaces

`docker network inspect 9a8abce56526`

```json
[
    {
        "Name": "bridge",
        "Id": "9a8abce565267070718d72ab4d97ae8c55e3ded9f93fad3d3498e6c9c3c37dae",
        "Created": "2022-08-15T13:52:20.378313708Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "8e7e8181ed856ce9e0aa6c6e5b9f0656bc045acb9cd141c4e4bd919c991d8e41": {
                "Name": "antonio-wordpress",
                "EndpointID": "645ae47d99ed4fa85358bc8c8a9f4ab6b13b1198bfdb810f4c351b49ffae1d9c",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            },
            "d3b96bb0fcc2a7a593574ed60c65ee27adcf461b4612ca4c18120f8560d4f694": {
                "Name": "antonio-mariadb",
                "EndpointID": "7d688fb94b87f142d18c76885dcc7e23403516f76d6362a87601180726f3015f",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

---

## join both networks so they can communicate

<https://medium.com/@habibridho/how-to-easily-setup-wordpress-using-docker-a798081dc577>

```bash

docker network create --attachable wordpress-network

docker network connect wordpress-network antonio-wordpress

docker network connect wordpress-network antonio-mariadb

```

confirm

```bash
docker network ls

    NETWORK ID     NAME                DRIVER    SCOPE
    9a8abce56526   bridge              bridge    local
    b5f6f44b6a39   host                host      local
    a8508a6f7890   none                null      local
    b697c9f17b3b   wordpress-network   bridge    local
```

`docker network inspect b697c9f17b3b`

```json
[
    {
        "Name": "wordpress-network",
        "Id": "b697c9f17b3bb3bd1223ae83dd4c7657f43d836602bcdbea1cf0403c3209514a",
        "Created": "2022-08-16T13:25:09.803500221Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": true,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "bf470ea4faa6d5d1d0abc3e0e77c8914606e832354311059e1b476b95ef6d5d7": {
                "Name": "antonio-wordpress",
                "EndpointID": "3ba2ed7d6e6b184f9df42bc44e11841947c14856a8671e5d960696b61eb1fe7c",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            },
            "decd0bac64d756825152490c08179358d1f01095187daaa1ceb3663b857eea7a": {
                "Name": "antonio-mariadb",
                "EndpointID": "b83a0409112787fdc225f7d6430101c8c6274f5de78fed88fa238bc473973fac",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

connect to the container on the specific network

`docker run -it --rm --network wordpress-network mariadb mysql -h172.18.0.2 -uroot -p`

`docker run --name antonio-wordpress --network wordpress-network -p 8080:80 -d wordpress`

jump into the container to check the configurations

```bash
docker container exec -it antonio-wordpress /bin/bash

# or

docker container exec -it antonio-mariadb /bin/bash
```

update if needed, not best practices, should be updated and create image, then start from image

`apt update && apt upgrade -y`

install `net-tools` to help troubleshooting with network commands

`apt install net-tools netcat vim`

`netstat -pantl`

`alias ll='ls -alhF --group-directories-first --color=always'`

testing connection from `antonio-wordpress` to `antonio-mariadb`

```bash
nc -zv -w 2 172.18.0.2 3306
    Connection to 172.18.0.2 3306 port [tcp/*] succeeded!
```

test the mysql wp username, permissions and password

```sql
mysql -h localhost -u wpdb_user -p

SHOW DATABASES;

USE wordpress;

SHOW TABLES;

CREATE TABLE Persons (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
    );

SHOW TABLES;

DESCRIBE Persons;

DROP TABLE Persons;

SHOW TABLES;

```
