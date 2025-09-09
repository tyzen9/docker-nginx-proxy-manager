# <img src="docs/images/t9Logo.png" height="25"> Tyzen9 - docker-ngnix-proxy-manager
This is an all-in-one Docker stack for running NGINX and common supporting services. It provides a lightweight, high-performance web server and reverse proxy, with built-in support for managing SSL certificates, hosting multiple sites, and serving static or dynamic content. The stack also includes options for log monitoring, performance metrics, and configuration management, making it easy to deploy, secure, and maintain modern web applications and services.

<p align="center">
<img src="docs/images/npm_logo.png" height="50">&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; <img src="docs/images/mysql.png" height="60">
</p> 

## Prerequisites
In production it's generally best to use [Docker Engine](https://docs.docker.com/get-docker/) on a Linux host operating system, and a lightweight service delivery platform designed for managing containerized applications such as [Portainer](https://www.portainer.io/). This documentation assumes you have a working knowledge of [Docker](https://www.docker.com/).

## Container Configuration
This `docker-compose` implementation is configured using the `environment` section(s) of the `compose.yaml` file.  The values between the `${}` characters are substituted from the `.env` file.  If these values are not configured, any default values preceded by the hyphen will be used.

# What's Inside?
The docker-ngnix-proxy-manager stack contains everything you need for a stable Nginx proxy. Here is what this project has to offer:

1. [Nginx Proxy Manager](https://hub.docker.com/r/jc21/nginx-proxy-manager)
1. [MySql](https://hub.docker.com/_/mysql)

## Getting Started
Deploy the stack into your Docker environment. This can be done any number of ways, to get started quickly log onto your Docker host system, and clone this repository: `git clone https://github.com/tyzen9/docker-nginx-proxy-manager.git`

1. Make a copy of `sample.env`, and name it `.env`
1. Set the configuration options as desired in the `compose.yaml` and `.env` files.
1. Navigate to the project's root directory and run the following command:

```
docker compose up
```

If everything works as expected, you should be able to access Nginx at http://hostname:81
