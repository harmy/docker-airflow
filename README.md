# docker-airflow for python3
[![CircleCI branch](https://img.shields.io/circleci/project/harmy/docker-airflow/master.svg?maxAge=2592000)](https://circleci.com/gh/harmy/docker-airflow/tree/master)
[![Docker Hub](https://img.shields.io/badge/docker-ready-blue.svg)](https://hub.docker.com/r/harmy/docker-airflow/)
[![Docker Pulls](https://img.shields.io/docker/pulls/harmy/docker-airflow.svg?maxAge=2592000)]()
[![Docker Stars](https://img.shields.io/docker/stars/harmy/docker-airflow.svg?maxAge=2592000)]()

This repository contains **Dockerfile** of [airflow](https://github.com/apache/incubator-airflow) for [Docker](https://www.docker.com/)'s [automated build](https://registry.hub.docker.com/u/harmy/docker-airflow/) published to the public [Docker Hub Registry](https://registry.hub.docker.com/).

## Informations

* Based on Debian Jessie official Image [debian:jessie](https://registry.hub.docker.com/_/debian/) and uses the official [Postgres](https://hub.docker.com/_/postgres/) as backend and [RabbitMQ](https://hub.docker.com/_/rabbitmq/) as queue
* Install [Docker](https://www.docker.com/)
* Install [Docker Compose](https://docs.docker.com/compose/install/)
* Following the Airflow release from [Python Package Index](https://pypi.python.org/pypi/airflow)

## Installation

Pull the image from the Docker repository.

        docker pull harmy/docker-airflow

## Build

For example, if you need to install [Extra Packages](http://pythonhosted.org/airflow/installation.html#extra-package), edit the Dockerfile and than build-it.

        docker build --rm -t harmy/docker-airflow .

## Usage

By default, docker-airflow run Airflow with **SequentialExecutor** :

        docker run -d -p 8080:8080 harmy/docker-airflow

If you want to run other executor, you've to use the docker-compose.yml files provided in this repository.

For **LocalExecutor** :

        docker-compose -f docker-compose-LocalExecutor.yml up -d

For **CeleryExecutor** :

        docker-compose -f docker-compose-CeleryExecutor.yml up -d

NB : If you don't want to have DAGs example loaded (default=True), you've to set the following environment variable :

`LOAD_EX=n`

        docker run -d -p 8080:8080 -e LOAD_EX=n harmy/docker-airflow

If you want to use Ad hoc query, make sure you've configured connections:
Go to Admin -> Connections and Edit "mysql_default" set this values (equivalent to values in airflow.cfg/docker-compose.yml) :
- Host : mysql
- Schema : airflow
- Login : airflow
- Password : airflow

For encrypted connection passwords (in Local or Celery Executor), you must have the same fernet_key. By default docker-airflow generates the fernet_key at startup, you have to set an environment variable in the docker-compose (ie: docker-compose-LocalExecutor.yml) file to set the same key accross containers. To generate a fernet_key :

        python -c "from cryptography.fernet import Fernet; FERNET_KEY = Fernet.generate_key().decode(); print FERNET_KEY"

Check [Airflow Documentation](http://pythonhosted.org/airflow/)


## Install custom python package

- Create a file "requirements.txt" with the dedired python modules
- Mount this file as a volume `-v $(pwd)/requirements.txt:/requirements.txt`
- The entrypoint.sh script execute the pip install command (with --user option)

## UI Links

- Airflow: [localhost:8080](http://localhost:8080/)
- Flower: [localhost:5555](http://localhost:5555/)
- RabbitMQ: [localhost:15672](http://localhost:15672/)

When using OSX with boot2docker, use: open http://$(boot2docker ip):8080

## Scale the number of workers

Easy scaling using docker-compose:

        docker-compose scale worker=5

This can be used to scale to a multi node setup using docker swarm.

## Links

 - Airflow on Kubernetes [kube-airflow](https://github.com/mumoshu/kube-airflow)

# Wanna help?

Fork, improve and PR. ;-)
