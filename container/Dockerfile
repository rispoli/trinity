FROM centos:latest
MAINTAINER hans.then@clustervision.com
RUN yum -y install epel-release
RUN yum -y groupinstall "Infiniband Support"

RUN useradd munge -u 1002 -U && useradd slurm -u 1001 -U && \
    mkdir /var/run/slurm /var/spool/slurm /var/log/slurm && \
    mkdir -m 755 -p /var/run/munge && \
    chown munge:munge /var/run/munge

RUN yum -y install openssh-server openssh-clients python-pip vim-minimal \
    infiniband-diags munge rsyslog bind-utils perl perl-DBI perl-Switch \
    openldap-clients nss-pam-ldapd authconfig environment-modules \
    infinipath-psm \
    xauth xorg-x11-xauth \
    vim-enhanced \
    hostname && yum clean all

RUN mkdir -p /var/run/sshd && \
    chmod -rx /var/run/sshd && \
    ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key && \
    ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key && \
    sed -ri 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/g' /etc/ssh/sshd_config

RUN pip install supervisor
COPY rootimg /

ADD rpms /rpms
RUN yum install -y /rpms/*.rpm

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
