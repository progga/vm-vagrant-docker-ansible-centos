
Example of Docker provider for Vagrant
======================================
This is a Vagrant project that runs CentOS 7 as a Docker container.  It uses
Ansible to install Drupal8 which becomes available at http://localhost:8080/.
The CentOS 7 Docker container comes with everything needed to use it similar
to a CentOS 7 VirtualBox VM.

Why use it?
-----------
There is a [strong argument against using Docker containers similar to virtual
machines](http://techblog.constantcontact.com/devops/a-tale-of-three-docker-anti-patterns/)
where one machine is responsible for multiple concerns such as running a
web server, a database server, application server, etc.  Despite this, there is a
scenario where you may still want to use a Docker container with multiple
concerns:
* Your production environment uses servers with multiple concerns and you just
want a similar environment in your development machine.  And...
* Given that virtual machines usually consume [more memory and take longer to
start](https://www.slideshare.net/Flux7Labs/performance-of-docker-vs-vms), you
want to avoid the overhead of running a full-blown virtual machine such as those
provided by VirtualBox, VMWare etc.

Compatibility
-------------
This Vagrant project has been tested on GNU/Linux using Docker 1.12 and on Mac OS
using Docker 17.

How to use it?
--------------
1. Install Vagrant and Docker if necessary.
2. [Install Ansible](http://docs.ansible.com/ansible/intro_installation.html).
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
* ~~As mentioned already, you cannot use this Vagrant project in non-Linux
-platforms.~~
* "vagrant reload" destroys the existing Docker container before creating a
new one.  This means all data stored inside the container disappears after
a "vagrant reload"!  This is different from what Vagrant does with Virtualbox.
* In older versions of Docker (e.g. 1.6) /etc/resolve.conf (used for DNS) is not
readable by non-root users.  So any process launched by the "vagrant" user will
*not* be able to resolve any domain names.  For example,
curl -I https://drupal.org/ will fail.  Fix is simple:
sudo chmod +r /etc/resolve.conf
* "yum -y install httpd" may fail with a ["cpio: cap_set_file" error message](https://hub.docker.com/r/centos/httpd/builds/baybetifj2mnnq8wzszdik8/).
This is due to [complications with the AUFS file system](https://github.com/docker/docker/issues/6980).
This can be ignored because the Ansible playbook has tasks to sort this out.
