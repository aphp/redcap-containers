# `aphp/php-fpm` container image

## Description
This image aims to provides with a PHP 8.2 FPM server, including all the dependencies that REDCap needs to operates correctly (this includes several image processing librairies, as well as some custom PHP extensions as `imagick`).

**This image doesn't contains, nor distributes REDCap binairies**. 
If you wish to use REDCap and are not sure where to start, you may visit the dedicated [REDCap Community Site](https://projectredcap.org/resources/community/).

## Content
The image is based on the official PHP 8.2 FPM image (debian-bookworm flavor), on top of which are added a few librairies and php extensions, such as :
- `imagick`
- `libpng`
- `libcurl`
- `mysqli`
- `ghostscript`

On top of this, the `/app/redcap` (with a symlink pointing to`/var/www/redcap`) and `/edocs` dirs are created with suitable persmissions, ready to handle a REDCap installation.
Finally, the image being rootless, the user `www-data` is exposed as the one executing the process. 

## How to use

The container image build from this project's Github Workflow is hosted on the GHCR of the [`aphp/redcap-containers` project](https://github.com/aphp/redcap-containers/pkgs/container/redcap-php-fpm). You can pull it using that command : 

```sh
docker pull ghcr.io/aphp/redcap-php-fpm:latest
```

If you want to serve the REDCap application with that image, you will have to retrieve the REDCap install archive, and map the content of the `redcap` directory to `` directory inside the container, like so (example with Docker) : 

```sh
docker run ghcr.io/aphp/redcap-php-fpm:latest -v ${redcap-app-dir}:/var/www/redcap
```

## How to build locally

From the root of this repository, simply build the image (example with Docker) : 

```sh
docker build -t localhost/redcap-php-fpm:latest ./php-fpm
```