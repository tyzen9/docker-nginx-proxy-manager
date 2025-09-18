# <img src="docs/images/t9Logo.png" height="25"> Tyzen9 - docker-ngnix-proxy-manager
This is an all-in-one Docker stack for running NGINX and common supporting services. It provides a lightweight, high-performance web server and reverse proxy, with built-in support for managing SSL certificates, hosting multiple sites, and serving static or dynamic content. The stack also includes options for log monitoring, performance metrics, and configuration management, making it easy to deploy, secure, and maintain modern web applications and services.

In addition, this stack contains `Cloudflare DDNS` which keeps your Cloudflare DNS records updated with your current public IP address. This is especially useful if your ISP provides a dynamic IP, ensuring that your domain name always points to the right address. By automatically updating DNS records, it allows you to host websites, applications, or remote services at home without worrying about manual DNS changes when your IP changes.

<p align="center">
<img src="docs/images/npm_logo.png" height="50">&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; <img src="docs/images/mysql.png" height="60">&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; <img src="docs/images/cloudflare_ddns.png" height="50">
</p> 

## Prerequisites
In production it's generally best to use [Docker Engine](https://docs.docker.com/get-docker/) on a Linux host operating system, and a lightweight service delivery platform designed for managing containerized applications such as [Portainer](https://www.portainer.io/). This documentation assumes you have a working knowledge of [Docker](https://www.docker.com/).

## Container Configuration
This `docker-compose` implementation is configured using the `environment` section(s) of the `compose.yaml` file.  The values between the `${}` characters are substituted from the `.env` file.  If these values are not configured, any default values preceded by the hyphen will be used.

# What's Inside?
The docker-ngnix-proxy-manager stack contains everything you need for a stable Nginx proxy. Here is what this project has to offer:

1. [Nginx Proxy Manager](https://hub.docker.com/r/jc21/nginx-proxy-manager)
1. [MySql](https://hub.docker.com/_/mysql)
1. [Cloudflare DDNS](https://hub.docker.com/r/favonia/cloudflare-ddns)

> [!Note]
> `Cloudflare DDNS` requires the hosted domains' DNS records to be managed at Cloudflare.  If this is not needed, then just comment the service out from the compose.yml file

## Getting Started
Deploy the stack into your Docker environment. This can be done by cloning this repository, by downloading the most recent release, or just by simply copying the content of the `compose.yml` file and `sample.env` above. 

1. Make a copy of `sample.env`, and name it `.env`
1. Set the configuration options as desired in the `compose.yaml` and `.env` files.
1. Navigate to the project's root directory and run the following command:

```
docker compose up
```

If everything works as expected, you should be able to access Nginx at http://hostname:81

# Nginx Proxy Server
Getting started with Nginx Proxy Server it very straight forward. The following are just the basics to get started.

### Log in with the default credentials:
1. Open browser: http://hostname:81
   - **Email:** `admin@example.com`
   - **Password:** `changeme`
1. Immediately change the default login credentials for security.

### Add a Proxy Host
1. Go to **Hosts → Proxy Hosts → Add Proxy Host** in the Nginx Proxy Manager dashboard.
2. Fill in the required fields:
   - **Domain Names:** e.g., `service.mydomain.com`
   - **Scheme:** `http` or `https` (depending on your backend service)
   - **Forward Hostname / IP:** IP address or host name of your service (e.g., `myapp.local` or `192.168.1.100`)
   - **Forward Port:** e.g., `8080`
3. Optional settings:
   - Enable **Block Common Exploits**
   - Enable **Websockets Support** if needed
   - Enable **Cache Assets** if appropriate

### Configure SSL
1. While adding or editing a proxy host, go to the **SSL** tab.
2. Choose **Request a new SSL Certificate**.
3. Select **Let’s Encrypt** and provide your email.
4. Optional but recommended:
   - Enable **Force SSL**
   - Enable **HTTP/2 Support**
5. Save the configuration.

These are the basics of using Nginx Proxy Manager. Much more detail can be found in the [Nginx Proxy Manager user guide](https://nginxproxymanager.com/guide/).

# Cloudflare DDNS

In Cloudflare there isn’t a direct “DDNS” settings page — instead, you configure **DNS records** (A/AAAA) and then use an **API token** so the `Cloudflare DDNS` updater can update those records automatically.
Here’s how to set it up step by step:

---

### 1. Log in to Cloudflare

* Go to [https://dash.cloudflare.com](https://dash.cloudflare.com).
* Select your domain.

---

### 2. Create a DNS record for your DDNS hostname

* In the **DNS** tab of your domain:

  * Add an **A record** (for IPv4) or **AAAA record** (for IPv6).
  * Example:

    * **Type:** A
    * **Name:** `home` (this makes `home.yourdomain.com`)
    * **IPv4 address:** put in any placeholder IP (your updater will overwrite it).
    * **Proxy status:** either *Proxied* (orange cloud) or *DNS only* (gray cloud), depending on if you want Cloudflare proxy/CDN.

---

### 3. Create an API token for DDNS updates

* Go to your **Profile** (top-right → "My Profile").
* Select **API Tokens** → **Create Token**.
* Use the **Edit zone DNS** template.
* Permissions needed:

  * Zone → DNS → Edit
  * Zone → Zone → Read
* Set the token to apply only to your domain (recommended).
* Save the token — this is the value you put in your container’s `API_KEY`.

---

### 4. Use those values in your container

In the `.env` file update the following values:

```yaml
CLOUDFLARE_API_TOKEN=<token>
CF_DDNS_DOMAIN_LIST=home.yourdomain.com,home.yourdomain2.com
```

That way, the container will log into Cloudflare with your token, find the `home.yourdomain.com` and `home.yourdomain2.com` records, and keep it updated to your current WAN IP.
