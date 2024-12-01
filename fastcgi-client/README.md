# `aphp/fastcgi-client` container image

## Description
This image aims to build a container able to do simple FastCGI calls. This is usefull to do calls to REDCap (eg : for firing the cronjobs) "internally", without having to expose the REDCap cronjob URL to the internet. It can be used for all alike purposes, as fring REDCap's autoinstall once the software is deployed on the PHP-FPM Server

## Content
This is a simple rootless image, based on Alpine Linux 3, on top of which is installed the [`cgi-fcgi`](https://github.com/FastCGI-Archives/fcgi2) binairies.

## How to use

The container image build from this project's Github Workflow is hosted on the GHCR of the [`aphp/redcap-containers` project](https://github.com/aphp/redcap-containers/pkgs/container/redcap-fastcgi-client). You can pull it using that command : 

```sh
docker pull ghcr.io/aphp/redcap-fastcgi-client:latest
```

## How to build locally

From the root of this repository, simply build the image (example with Docker) : 

```sh
docker build -t localhost/redcap-fastcgi-client:latest ./fastcgi-client
```