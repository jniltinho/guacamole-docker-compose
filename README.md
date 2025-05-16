
# guacamole-docker-compose
Build Apache Guacamole using MariaDB and Nginx with docker compose. Utilizes Docker secrets and a persistent database volume.

I have an upstream WAF and Reverse Proxy that uses LE Certificates. That proxy connects to this instance and some other services I self host. This project was created so I could quickly stand up guacamole in my environment. It can work for you as well if u want to leverage self signed certificates. You are welcome to incorporate a LE version that auto updates.

## Tested On
`Ubuntu 22.04`  
 

## Prerequisites

`docker-ce`  
`docker-compose >= 1.23.0 || docker-compose-plugin`  
`git`

```
guacamole
├── docker-compose.yml
├── .env
├── guacamole-user
├── mysql-root
└── nginx
    ├── nginx.conf
    ├── sites
    │   └── gproxy
    └── ssl
        ├── nginx-priv.key
        ├── nginx-pub.crt
        └── nginx-ssl.conf

```
| File | Description |
| --- | --- |
| .env | Docker compose will pickup environment vars from this file |
| guacamole-user | Put your guacamole user password here. |
| mysql-root | Put your MySQL root password here. |
| nginx.conf |nginx site wide configuration. |
| gproxy | vhost configuration file for nginx. |
| nginx-pub.crt | TLS public certificate for nginx. (needs to be generated) |
| nginx-prv.key | TLS private key for nginx. (needs to be generated) |
| nginx-ssl.conf | TLS settings for nginx |


## Usage

```
apt update
apt install -y apt-transport-https ca-certificates curl software-properties-common docker.io
wget https://github.com/docker/compose/releases/download/v2.36.0/docker-compose-linux-x86_64 -O /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
git clone https://github.com/tankmek/guacamole-docker-compose.git
cd guacamole-docker-compose/guacamole
openssl req -nodes -days 3650 -newkey rsa:2048 -new -x509 -keyout ssl/nginx-priv.key -out ssl/nginx-pub.crt -subj '/C=DE/ST=BY/L=Hintertupfing/O=Dorfwirt/OU=Theke/CN=www.createyourown.domain/emailAddress=docker@createyourown.domain'
mkdir ./init
docker run --rm guacamole/guacamole:1.5.5 /opt/guacamole/bin/initdb.sh --mysql > ./init/initdb.sql
```

## Links

- https://github.com/boschkundendienst/guacamole-docker-compose
- https://github.com/DmitryZagr/guacamole-docker-compose
