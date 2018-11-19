# Drupal Local environment setup using Docker4Drupal

## Introduction

This is a set of intructions and sample files to setup a local environment using Docker4Drupal.

Includes single traefik file for setting up multiple projects and secure connection locally.

Docker4Drupal is a set of docker images optimized for Drupal. Use `docker-compose.yml` file from the [latest stable release](https://github.com/wodby/docker4drupal/releases) to spin up local environment on Linux, Mac OS X and Windows.

* Read the docs on [**how to use**](https://wodby.com/docs/stacks/drupal/local#usage)

## Setup

For single projects is OK to use Docker4Drupal as it is. However, for running multiple projects, setup HTTPS locally and debugging properly is required to have a global traefik file.

1. Create a certs folder for holding self-signed certificates.

    `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /certs/key.pem -out /certs/cert.pem`

2. Create a Sites folder
3. Create a `traefik.yml` file to be able to manage multiple projects and setup a secure connection HTTPS
4. For existing projects:
    - For DVA for example:
      - Clone the site
      - Copy `docker-compose file` and `.env` file
      -	Create folder **mariadb-init** folder and copy db backup(.sql) inside that folder

5. For new projects:
    -	For brand new projects(D7):
        -	Create folder project
        -	Copy `docker-compose.yml` and `.env` file
        -	Create web folder and copy all drupal files inside

    -	For brand new projects(D8):
        - Create site with composer -
        [**Composer Drupal Project**](https://github.com/drupal-composer/drupal-project)

          `composer create-project drupal-composer/drupal-project:8.x-dev some-dir --stability dev --no-interaction`

        - Copy `docker-compose.yml` and `.env` file
        - When installing a new site UI will ask to create config folder with right permissions

6.	In all cases you will need to modify docker-compose file and traefik.yml accordingly
7.	Update `/etc/hosts` file
8. For debugging add **_VSCODE_** into docker-compose file

## How to run the containers

After all files are in place:

1.	On project folder just run

    `docker-compose up -d`

2.	To run traefik file

    `docker-compose –f traefik.yml up –d`

## DB Export

Exporting all databases:

`docker-compose exec mariadb sh -c 'exec mysqldump --all-databases -uroot -p"root-password"' > databases.sql`


Exporting a specific database:

`docker-compose exec mariadb sh -c 'exec mysqldump -uroot -p"root-password" my-db' > my-db.sql`

## Get new modules for D8

`composer require drupal/<module_name>`

## DRUSH & Alias

`drush upwd USERNAME --password="SOMEPASSWORD"`

`drush upwd dvamaster --password="dvamaster`

`drush @dev status`

`drush @dev upwd dvamaster --password="dvamaster"`

`ssh @dev`

## Modified permissions

`find /opt/lampp/htdocs -type d -exec chmod 755 {} \;`

`find /opt/lampp/htdocs -type f -exec chmod 644 {} \;`
