FROM centos:7
MAINTAINER "you" <you@example.net>

ENV container docker

RUN yum -y install openssh-server httpd sudo curl; \
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

EXPOSE 22 80

CMD ["/usr/sbin/init"]
