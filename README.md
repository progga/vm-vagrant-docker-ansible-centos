# vm-vagrant-docker-ansible-centos7

Example of Docker provider for Vagrant
======================================
This is a Vagrant project that runs CentOS 7 as a Docker container.  It uses
Ansible to install Drupal8 which becomes available at http://localhost:8080/.
The CentOS 7 Docker container comes with everything needed to use it similar
to a CentOS 7 VirtualBox VM.

Why use it?
-----------
There is a [strong argument against using Docker containers similar to virtual
machines]
(http://techblog.constantcontact.com/devops/a-tale-of-three-docker-anti-patterns/)
where one machine is responsible for multiple concerns such as running a
webserver, a database server, application server, etc.  Despite this, there is a
scenario where you may still want to use a Docker container with multiple
concerns:
* Your production environment uses servers with multiple concerns and you just
want a similiar environment in your development machine.  And...
* Given that virtual machines usually consume [more memory and takes longer to
start] (https://www.slideshare.net/Flux7Labs/performance-of-docker-vs-vms), you
want to avoid the overhead of running a full-blown virtual machine such as those
provided by VirtualBox, VMWare etc.

When to use it?
---------------
This Vagrant project is able to run its Docker container in GNU/Linux hosts
only.  In any developent team today, you will inevitably find most people using
MacOS as their host platform.  So in most cases you cannot roll out a team-wide
Vagrant VM based on this project.  But there are scenarios where this project
could still be used:
* You are the only developer in the team.  So no one else will be affected by
your choice of a GNU/Linux only Vagrant project.
* All developers are required to use a GNU/Linux host.  Rare case.
* There is no team-wide Vagrant project, so you are free to roll your own.

How to use it?
--------------
1. Install Vagrant and Docker if necessary.
2. [Install Ansible] (http://docs.ansible.com/ansible/intro_installation.html).
The playbooks have been tested in version 2.1.0.0.
3. "vagrant up" should create a Docker image based on the "build/Dockerfile"
file and then launch a Docker container created out of that image.
4. "vagrant ssh" should get you inside the Docker container.  Alternatively, you
can do "docker exec -it CONTAINER-NAME-OR-ID /bin/bash".  You can get the
container ID from "docker ps".
5. Drupal should be available at http://localhost:8080/ from the host machine.

Software versions used
----------------------
* OS: CentOS 7
* Apache: 2.4.6
* Drupal: 8.x
* Drush: 8.x
* PHP (php-fpm): 5.6.x

Known issues
------------
* As mentioned already, you cannot use this Vagrant project in non-Linux
platforms.
* /etc/resolve.conf (used for DNS) is not readable by non-root users.  So any
process launched by the "vagrant" user will *not* be able to resolve any domain
names.  For example, curl -I https://drupal.org/ will fail.  Fix is
simple: sudo chmod +r /etc/resolve.conf
* "yum -y install httpd" may fail with a ["cpio: cap_set_file" error message]
(https://hub.docker.com/r/centos/httpd/builds/baybetifj2mnnq8wzszdik8/).  This
is due to [complications with the AUFS file system]
(https://github.com/docker/docker/issues/6980).  This can be ignored because the
Ansible playbook has tasks to sort this out.