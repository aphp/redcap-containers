FROM rockylinux:9

### Adding Shibboleth official repos
COPY ./config/shibboleth.repo /etc/yum.repos.d/shibboleth.repo

### Installing httpd, shibboleth & supervisor
RUN dnf install -y epel-release &&\
    dnf update -y &&\
    dnf install -y shibboleth.x86_64 httpd supervisor mod_ssl &&\
    dnf clean all

RUN ls -lah /var/run 

### Creating a non-root user to manage supervisorctl & setting rights to launch the daemon
RUN adduser supervisor &&\
    usermod -a -G apache supervisor &&\
    usermod -a -G shibd supervisor &&\
    mkdir -p /etc/supervisor &&\
    chown -v root:supervisor "/var/log" &&\
    chown -Rv supervisor:supervisor "/var/log/supervisor" &&\
    chown -Rv supervisor:supervisor "/var/log/httpd" &&\
    chown -Rv supervisor:supervisor "/var/log/shibboleth" &&\
    chown -Rv supervisor:supervisor "/etc/supervisor" &&\
    chown -Rv supervisor:supervisor "/usr/bin/supervisord" &&\
    chown -Rv supervisor:supervisor "/var/run/supervisor"

USER supervisor

WORKDIR /var/www/redcap

### Launching supervisord
ENTRYPOINT ["/usr/bin/supervisord"]
