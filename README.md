# APHP - REDCap Helm Chart containers

## Presentation

This repository is hosting the container images that are needed to run the REDCap Helm Chart provided by the Greater Paris University Hospitals.

Those images are as follow : 
- `httpd-shibd` : A custom container made to host an instance of Apache HTTPd and Shibboleth.
   - [see the `httpd-shibd` folder](./httpd-shibd/)
- `php-fpm` : A PHP 8.4 FPM server that contains all dependencies and configurations needed by REDCap.
   - [see the `php-fpm` folder](./php-fpm/)

Each subfolder contains its own README file.

All those containers are `rootless`, and **none of them is containing, or distributing REDCap binaries**. 
If you wish to use REDCap and are not sure where to start, you may visit the dedicated [REDCap Community Site](https://projectredcap.org/resources/community/).

## continuous Integration / continuous Delivery

This project uses two Github Workflows (presents under the .github/workflows directory), which will, for each image :
- Lint the Dockerfile using `Hadolint`
- Scan the container images using `Dockle`
- Runs critical vulnerability, secrets and license checks on the container image using `Trivy`
- Pushes the container images to this project's GHCR for it to be retrieved as a container image.

## How can I contribute?

You're welcome to read the [contribution guidelines](./CONTRIBUTING.md).

## How is this project licensed?

The information about the licensing and the dependencies of this project can be found under : 
- The [project's license file](./LICENSE)
- The [legal notice](./NOTICE)