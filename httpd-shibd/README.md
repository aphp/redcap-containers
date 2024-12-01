# `aphp/httpd-shibd` container image

## Description
This image aims to build a custom container made to host an instance of Apache HTTPd and Shibboleth. Since Shibboleth is provided as a HTTPd module that spawns its own process, it cannot be easily isolated in a dedicated container. This container is then spawning and managing both of those processes with a `supervisord` rootless installation.
You can then map a `supervisord` configuration file that will handle the spawning of HTTPd and/or Shibboleth, according to your usecase.

## Content
This image use [the reccomended way to install Shibboleth](https://shibboleth.atlassian.net/wiki/spaces/SP3/pages/2065335566/RPMInstall) on a RHEL/CentOS/RockyLinux 8+ base OS, alogside httpd.
A dedicated non-root user is then created in order to manage the processes in a secure way.

## Configuration

### supervisord

Usually, a `supervisord` configuration file is used alongside this image, in order to efficienly manage and logs the processes activity.

Here's an example :

```ini
[supervisord]
#user=supervisor
pidfile=/var/run/supervisor/supervisord.pid
logfile=/var/log/supervisor/supervisord.log
logfile_backups=10          ; (num of main logfile rotation backups;default 10)
loglevel=info               ; (log level;default info; others: debug,warn,trace)
nodaemon=true              ; (start in foreground if true;default false)
minfds=1024                 ; (min. avail startup file descriptors;default 1024)
minprocs=200                ; (min. avail process descriptors;default 200)

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[eventlistener:processes]
command=bash -c "printf 'READY\n' && while read line; do kill -SIGQUIT $PPID; done < /dev/stdin"
events=PROCESS_STATE_STOPPED,PROCESS_STATE_EXITED,PROCESS_STATE_FATAL

[program:httpd]
command=httpd -c "CustomLog /dev/stdout common" -c "ErrorLog /dev/stderr" -DFOREGROUND
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0
killasgroup=true
stopasgroup=true

# If you wish to spawn Shibboleth process
[program:shibd]
command=shibd -F
stderr_logfile=/dev/fd/1
stderr_logfile_maxbytes=0
stdout_logfile=/dev/fd/1
stdout_logfile_maxbytes=0
```

### HTTPd

Depending of your usecase, the REDcap Communite Site can provide you with the best ways to configure your HTTPd server.

## How to use

The container image build from this project's Github Workflow is hosted on the GHCR of the [`aphp/redcap-containers` project](https://github.com/aphp/redcap-containers/pkgs/container/redcap-httpd-shibd). You can pull it using that command : 

```sh
docker pull ghcr.io/aphp/redcap-httpd-shibd:latest
```

If you wish to run it alongside the `supervisord` config file, juste use the following command : 

```sh
docker run ghcr.io/aphp/redcap-httpd-shibd:latest -v ${your-config-file}:/etc/supervisor/supervisord-redcap-front.conf
```

## How to build locally

From the root of this repository, simply build the image (example with Docker) : 

```sh
docker build -t localhost/redcap-httpd-shibd:latest ./httpd-shibd
```