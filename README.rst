Docker Ansible
==============

Very simple set of tasks for deploying docker onto hosts. Mostly tested on RHEL
systems, but has hooks for ubuntu targets too.

Variables
=========

docker_socket_directory 
    the directory to create the docker.sock. Useful if you want to expose the
    socket to containers without opening up all sockets in the system.
    default: /var/run

disable_selinux
    disables selinux on the system. Useful if you're just setting up sandboxes.
    selinux has a habit of throwing curve balls at you.
    default: true

primary_group
    the primary group is used for certificate lookups. For example, if the
    primary_group is set to 'docker', the certs found in files/docker/ are
    used.
    default: docker

enable_btrfs
    Enable the btrfs docker driver.
    default: true

binary
    Install the latest binary from get.docker.io
    default: yes

binary_arch
    The arch of the binary to install. 
    default: x86_64

Deployment
==========

Including the role has the following results.

Docker
------

The docker.service unit is configured for both socket and TCP connections. TCP 
connections are served using the client/server certificates in 
files/{{ primary_group }}/. Also,  if enable_btrfs is true the docker service
will use the btrfs file system driver.

The docker socket is created in the directory specified by
docker_socket_directory.

System
-------

The docker registry package is installed on RHEL family os's and is availabe on
port 5000. If disable_selinux is true, selinux has been disabled.
