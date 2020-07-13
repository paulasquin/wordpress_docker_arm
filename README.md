# Wordpress on Raspberry Pi with Docker
Use this repo to run your own wordpress site on a Raspberry Pi.
The wordpress site will be embedded into docker containers for easy and portable installation.

This project is [a fork from Rothgar](https://github.com/rothgar/rpi-wordpress) which has been upgraded and enhanced by myself, [Paul Asquin](https://www.linkedin.com/in/paulasquin).
You can find [here](https://medium.com/@rothgar/wordpress-in-docker-on-a-raspberry-pi-149b4301195) a tutorial made by Rothgar. The motivations for this project fork are security upgrades made by the Caddy project which we are going to use, and the proposition of easier configuration steps.

## Install Docker and Docker Compose
Prior to the project installation, you'll need to get the two only dependencies of this project: Docker and its augmentation Docker Compose.
For this, you can find many tutorials on internet. For instance you can follow [this one from Bobby Iliev](https://devdojo.com/bobbyiliev/how-to-install-docker-and-docker-compose-on-raspberry-pi)


## Clone this project
You'll need to download the Github repository in your raspberry pi. For this
```bash
git clone https://github.com/paulasquin/wordpress_docker_arm.git
```
Then move inside the locally downloaded repository with
```bash
cd wordpress_docker_arm
```

## Download wordpress template
You'll need to download wordpress basic file prior to booting the project. To get the latest wordpress files, run
```bash
curl -sL http://wordpress.org/latest.tar.gz | tar --strip 1 -xz -C .data/wp/www
```

## Parametrize the project
Open and edit the .env file.
Update the lines with
```
DOMAIN_NAME=
MYSQL_ROOT_PASSWORD=
```
and
```
DOMAIN_NAME=
```

## Configure SSL/HTTPS
This project enable you to use HTTPS protocol quite easily.
Caddy is proposing automated certificates gathering, but we suggest that you gather your own certificates in order to master their renewal and their storage.  

### Get SSL certificates
To get SSL certificates, follow the instructions from [Certbot](https://certbot.eff.org/) that'll let you get Let's Encrypt keys. 
We suggest that you do activate the automated keys renewal.
The keys should be hosted under `/etc/letsencrypt/live/DOMAIN.COM` and `/etc/letsencrypt/archive/DOMAIN.COM`. Check the existence of those files before proceeding.

### Configure project for SSL certificates
You'll need to indicate to the project were you certificates are stored. If you already edited the `.env` file with you domain name, you can now directly move to the edition
of `.data/wp/Caddyfile`.
- Uncomment the line with `{$DOMAIN_NAME}` 
- Comment the line starting with `:80`
- Uncomment the line with `tls`


## Manage the project
To run the wordpress services, just type
```bash
docker-compose up -d
```
-d stands for --detach, meaning you'd like the services to run without an open shell.

To check the logs
```bash
docker-compose logs
```

To stop the services
```bash
docker-compose down
```

## Wordpress configuration
You should now be able to configure and work on your wordpress website.
For this, get the IP of your raspberry pi, or configure your DNS routing and get 
the raspberry url, and open it in your browser.
You should be able to configure the wordpress server.  

Provide the same information as presented in the .env file, and indicate `mysql` as 
database host (name of the container hosting the mysql service)

![](https://i.ibb.co/njrFZkX/Wordpress-config.png)


## Fix SQL database issues
You may encounter SQL Database issues, especially if you changed the SQL password 
after the first project run. 
For easy fixing, you can choose to reset the SQL Database. 
Of course, keep in mind that this could lead to data loss if you already 
started working on your website
```bash
rm -r .data/db/*
```

## Use previously set Wordpress website
If you need to backup an existing Wordpress site you can copy your 
files into .data/wp/www and put a database .sql backup file in .data/backup
