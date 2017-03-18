#
# Example of Vagrant using a Docker provider and an Ansible provisioner.
#
# Given that Docker is only available in GNU/Linux, this VM is also limited to
# use in GNU/Linux.  The usual platform neutral method of using Docker with
# Vagrant is to first launch a Virtualbox VM and then launch Docker containers
# inside that VM.  This achieves platform neutrality at the cost of running a
# full VM.  If your host machine is running GNU/Linux, you can avoid running a
# full VM although you then loose platform neutrality.  That is the approach we
# have taken in this Vagrantfile.  So it produces a very cheap CentOS7 Docker
# container while retaining everything you would expect from a Vagrant VM.

Vagrant.configure(2) do |config|

  ENV["VAGRANT_DEFAULT_PROVIDER"] = "docker"

  config.vm.define "centos7-docker-vagrant-ansible-build" do |c7v|
    # Due to a bug in Vagrant 1.8.5, SSH fails after Vagrant generates and
    # inserts a secure key that replaces the default insecure key.
    # @see https://github.com/mitchellh/vagrant/issues/7610
    #
    # *** So uncomment the next line if you are using Vagrant 1.8.5. ***
    # c7v.ssh.insert_key = false

    # private_network and public_network are not available when using the
    # Docker provider.  So all we are left with is port forwarding :(
    # The next line will link up localhost:8080 with the Apache instance
    # running inside the Docker container.
    c7v.vm.network "forwarded_port", guest:80, host:8080

    c7v.vm.provider "docker" do |image|
      image.build_dir = "build"
      # Shared directory mapping.  Add more as needed.
      # Syntax is [host-dir:guest-dir, ...]
      image.volumes = ["/sys/fs/cgroup:/sys/fs/cgroup:ro", '/tmp/centos7-docker-vagrant-ansible-build-run:/run']
      image.has_ssh = true
      image.remains_running = true
    end

    c7v.vm.provision "ansible" do |ansible|
      ansible.playbook = "the-ansible-files/drupal-provision.yml"
    end
  end
end
