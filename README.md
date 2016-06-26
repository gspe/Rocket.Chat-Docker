# Rocket.Chat-Docker

Docker-compose file for Rocket.Chat in production.

This docker-compose file will show you how to incorporate Let’s Encrypt official image into a docker compose service that automatically sets up everything needed to get a signed SSL cert.

You must own or control the registered domain name that you wish to use the certificate with. If you do not already have a registered domain name, you may register one with one of the many domain name registrars out there (e.g. Namecheap, GoDaddy, etc.).

## Requirements

- docker
- docker-compose
- an A Record that points your domain to the public IP address of your server

## Installation

1. Open docker-compose.yml and edit the value of `chat.example.com` and `user@chat.example .com` from following lines:

  - `ROOT_URL=https://chat.example.com`
  - `MY_DOMAIN_NAME=chat.example.com`
  - `command:  bash -c "sleep 6 && certbot certonly --standalone -d chat.example.com --text --agree-tos --email user@chat.example.com --server https://acme-v01.api.letsencrypt.org/directory --rsa-key-size 4096 --verbose --renew-by-default --standalone-supported-challenges http-01"`
<!-- 2. run `docker-compose up -d` -->

2. Start the mongodb server by:

    ```docker-compose up -d mongo```

    Mongo supports 24 x 7 operations and live backup. You should not need to restart it too frequently. See mongodb documentations for proper operation and management of a mongo server.

3. Once you’re sure that mongodb is up and running:

  `docker-compose up -d rocketchat`

  Optionally, if you want to manage your messages and configuration information, edit the file again to uncomment the volume mounts. Make sure you have a data subdirectory to mount and store the data.

4. Optionally, if you want a bot, so you don’t have to talk to yourself, after you’ve created an admin user and also a bot user, edit the file docker-compose.yml again to change the variables `ROCKETCHAT_USER` and `ROCKETCHAT_PASSWORD` in the hubot section and then start up hubot:

  `docker-compose up -d hubot`

## Update

To update the rocketchat docker image to the latest version, you can use the following commands. Your data should not be affected by this, since it’s located in the mongo image.

```
docker pull rocketchat/rocket.chat:develop
docker-compose stop rocketchat
docker-compose rm rocketchat
docker-compose up -d rocketchat
```


## Refereces

- http://www.automationlogic.com/using-lets-encrypt-and-docker-for-automatic-ssl/
- https://bitbucket.org/automationlogic/le-docker-compose
