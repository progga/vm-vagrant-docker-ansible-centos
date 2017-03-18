FROM centos:7
MAINTAINER "you" <you@example.net>

ENV container docker

RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*; \
yum -y install openssh-server httpd sudo curl; \
systemctl enable sshd.service; \
systemctl enable httpd.service; \
groupadd vagrant; \
useradd --home-dir /home/vagrant -g vagrant --shell /bin/bash vagrant; \
# Enable SSH login for non-root users.
sed -ri 's/^(account +required +pam_nologin)/#\1/g' /etc/pam.d/sshd; \
# Password-less sudo access for the vagrant group of users.
echo '%vagrant ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/vagrant; \
# Setup Vagrant's default insecure SSH public key.
mkdir /home/vagrant/.ssh; \
curl --silent --output /home/vagrant/.ssh/authorized_keys https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub; \
chown -R vagrant:vagrant /home/vagrant/.ssh; \
chmod 0700 /home/vagrant/.ssh; \
chmod 0600 /home/vagrant/.ssh/authorized_keys;

VOLUME [ "/sys/fs/cgroup" ]

EXPOSE 22 80

CMD ["/usr/sbin/init"]
