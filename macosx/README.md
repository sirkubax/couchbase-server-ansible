# Couchbase Server with Ansible


               .CCCCC                       CCCCC.
              .CCCCCC                       CCCCCC.
              .CCCCCC                       CCCCCC.
              .CCCCCC                       CCCCCC.
              .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.
              .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.
              .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.
               .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.


[Couchbase Server](http://www.couchbase.com/couchbase-server/overview) is a
high performance NoSQL document database available in Community and Enterprise
editions for several operating system environments.

This project provides documentation and a collection of scripts to help you
automate the deployment of Couchbase Server Enterprise Edition using the [Ansible](http://www.ansibleworks.com/)
orchestration engine. These are the instructions for deploying a development
cluster on Mac OS X with Vagrant and VirtualBox.

The documentation and scripts are merely a starting point designed to both
help familiarize you with the processes and quickly bootstrap an environment
for development. You may wish to expand on them and customize
them with additional features specific to your needs later.

## Mac OS X Development Cluster

In some situations deploying a small cluster on your local development
machine under Mac OS X can be handy. This section will describe such a
scenario using the following technologies:

* [Couchbase Server](http://www.couchbase.com/couchbase-server/overview)
* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](http://www.vagrantup.com/) with Ansible provisioner and
  supporting plugin
* [Ansible](http://www.ansibleworks.com/)

Each of the virtual machines are configured with 1.5GB RAM, 2 CPU cores, and
2 network interfaces. The first interface uses NAT and has connection via the
host to the outside world. The second interface is a private network and is
used for intra-cluster communication in addition to access from the host
machine to Couchbase Server's administration and API ports.

The Vagrant configuration file (`Vagrantfile`) is responsible for
configuring the virtual machines and a baseline OS installation.

The Ansible playbooks then further refine OS configuration, Couchbase Server
package download and installation, and the initialization of the 3 nodes
into a ready to use cluster.

## Quick Start

Begin from the top level directory of this project and use the following
5 steps to get up and running:

1. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads), [Vagrant](http://downloads.vagrantup.com/), [vagrant-hosts](https://github.com/adrienthebo/vagrant-hosts), and [Ansible](http://www.ansibleworks.com/docs/intro_installation.html#latest-releases-via-pip).
2. Edit `/etc/hosts` or use the included `bin/preinstall` script to add
   the following entries:
 * 10.1.42.10 node1.local node1
 * 10.1.42.20 node2.local node2
 * 10.1.42.30 node3.local node3
3. `cd macosx`
4. `vagrant up`
5. Access the cluster at http://node1:8091 with username **Administrator**
   and password **couchbase**.

By default, this project will install CentOS 6.4 based cluster nodes. If you
prefer, it can also install Ubuntu 12.04 based nodes by changing the command
in step 4 to the following:

```
BOX_NAME="precise64" BOX_URI="http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-12.04_chef-provisionerless.box" CLUSTER_HOSTS="ubuntu" vagrant up
```

If you'd like to follow a more detailed installation process with additional
explanation of the steps and technologies, see the following sections of
this guide.

## Prerequisites

Before you begin with the steps in this guide, please ensure that you have
the following prerequisites configured and installed.

### Networking

Ensure that your host machine can access the internet so that the required
software can be downloaded as needed. This includes both downloading the
prerequisite software, the VirtualBox operating system image, and Couchbase
Server itself.

You'll also need to resolve the node VM hostnames to their default IP
addresses. This can be most easily done by adding the following lines to
the `/etc/hosts` file on the host machine:

```
10.1.42.10 node1.local node1
10.1.42.20 node2.local node2
10.1.42.30 node3.local node3
```

A convenience script, `bin/preinstall` is provided to automate this step
and some other steps for you.

### VirtualBox

VirtualBox is a freely available virtualization product that supports various
operating system guests running in virtual machines on Mac OS X. Download and
install the latest version from the [VirtualBox website](https://www.virtualbox.org/wiki/Downloads) before proceeding with
the rest of the steps in this section.

### Vagrant and Plugins

Vagrant helps automate building complete development environments on Mac OS X
with the notion of *providers* for different deployment targets like
VirtualBox and *provisioners* like Ansible for handling the actual deployment
and provisioning of the virtual machines.

This guide uses Vagrant version 1.3.5 along with some supporting plugins to
automate the creation of the VirtualBox machines which we will be using for
the Couchbase Server nodes.

The baseboxes for operating systems are fetched from the Opscode
[Bento](https://github.com/opscode/bento) project.

The plugin used in this project is [Vagrant hosts](https://github.com/adrienthebo/vagrant-hosts), which is used for
adding hostname information to the `/etc/hosts` file on each virtual machine.
This is required prior to the execution of Ansible playbooks so that Ansible
will function properly over SSH.

Visit the Vagrant [downloads page](http://downloads.vagrantup.com/) to get the
appropriate version, and proceed with installation of the package. After you
have installed Vagrant, open a terminal and execute the following shell
commands to install the supporting plugins:

```
vagrant plugin install vagrant-hosts
```

These commands should return success messages; provided that the commands are
successful, you can continue with the rest of the steps in this guide.

### Ansible

Install Ansible for Mac OS X using `pip` based on the
[installation documentation](http://www.ansibleworks.com/docs/intro_installation.html#latest-releases-via-pip). The steps are simple and involve just 2 steps:

```
sudo easy_install pip
sudo pip install ansible
```

## Bootstrap the Cluster

Now it's time for bootstrapping the Couchbase Server cluster.

Open a terminal, change into the `macosx` subdirectory of this project's
top level directory and execute the following:

```
vagrant up
```

Yep, that's all there is to it. One command and a bit of patience while
Vagrant and Ansible work their magic. If bootstrapping a cluster for the
first time, you'll be prompted to accept the host key information for each
node VM by ssh; be sure to answer **yes** to these prompts.

By default, this project will install CentOS 6.4 based cluster nodes. If you
prefer, it can also install Ubuntu 13.10 based nodes by changing the
`vagrant up` command to the following:

```
BOX_NAME="precise64" BOX_URI="http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-12.04_chef-provisionerless.box" CLUSTER_HOSTS="ubuntu" vagrant up
```

## Give it a Try

Once the Ansible playbooks complete without error, you can open a browser and
access the primary Couchbase Server cluster node at the following URL:

```
http://node1:8091
```

The administrator username and password as configured by this project
are:

* Username: **Administrator**
* Password: **couchbase**

Once you've logged in and verified that the cluster is available, you're now
ready to create buckets and begin using your new Couchbase Server development
cluster.

## Notes

1. The project uses CentOS 6.4 and Ubuntu 12.04 as these are the supported
   platforms which are listed on the Couchbase Server package downloads page
   (CentOS 6 and Ubuntu 12.04 to be specific)
2. The `bin/preinstall` shell script performs the following actions for you:
 * Adds each node's host information to the host machine's `/etc/hosts`
 * Ensures the correct permissions on Vagrant SSH private key
 * Optionally installs the Vagrant hosts plugin
3. Review the different operating system tuning changes in the following
  files to adjust or add your own:
  * `roles/bootstrap/common/templates/etc_rc.local.j2`
  * `roles/bootstrap/common/templates/iptables.j2`
  * `roles/couchbase-server/centos/templates/iptables.j2`
  * `roles/couchbase-server/ubuntu/templates/iptables.j2`

## References

1. http://www.couchbase.com/couchbase-server/overview
2. http://www.ansibleworks.com/
3. http://www.vagrantup.com/
4. https://www.virtualbox.org/
5. https://github.com/adrienthebo/vagrant-hosts
6. https://github.com/opscode/bento
