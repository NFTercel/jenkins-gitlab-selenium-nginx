# jenkins-gitlab-selenium-nginx

## What's inside?

Docker composefile with:

* Jenkins
* GitLab
* Selenium Hub + 1 selenium node (chrome)
* Nginx with default ssl configuration


## What's Configurable?

`.env` includes:

```
NGINX_DEFAULT_CONF=./nginx/default.conf
NGINX_SSL_CERT=./certs/self_signed_cert.pem
NGINX_SSL_KEY=./certs/self_signed_key.pem
```

These and other environment variables are used in docker-compose.yaml. You can add others too. It is possible to use env variables in `.env` as well.

## Nginx routing

The default Nginx configuration as well as arguments passed to Jenkins and GitLab assume that:

* Jenkins is available at https://server.com/jenkins
* GitLab is available at https://server.com/gitlab
* Selenium Hub is available at https://server.com (no path here)

Important! Replace ciserver.ml with an actual server name in nginx/default.conf and in `docker-compose.yaml` (environment section for gitlab service)

## SSL configuration

You need to obtain valid certificate and a key for your domain. This can be a self-signed certificate. Once done, you need to ether save the two files as:

```
certs/self_signed_cert.pem
certs/self_signed_key.pem
```

or change path to crt and key in `.env`.

## Exposed ports and security

Nginx exposes 80 and 443 (automatic redirect to 443 is configured). Jenkins, GitLab and Selenium Hub ports aren't published, and communication is performed only through Nginx with SSL
which talks to servers using service names (all of them are in one network i.e. unaccessible from outside).

Selenium node talks to Hub using `selenium-hub:4444` edpoint.

## How to Start

After you have configured Nginx and figured out SSL stuff:

```
docker-compose -d
```
