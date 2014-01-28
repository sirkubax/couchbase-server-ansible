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
orchestration engine. These are the instructions for deploying a staging or
production cluster on CentOS or Ubuntu.

The documentation and scripts are merely a starting point designed to both
help familiarize you with the processes and quickly bootstrap an environment.
You may wish to expand on them and customize the steps in this guide with
additional features specific to your needs later.

## Linux Staging or Production Deployment

This section describes deployment of a Couchbase Server cluster
using the following technologies:

* [Couchbase Server](http://www.couchbase.com/couchbase-server/overview)
* [Ansible](http://www.ansibleworks.com/)

The Ansible playbooks refine OS configuration, download and install Couchbase
Server, and initialize the nodes into a ready to use cluster.

## Quick Start

1. Install [Ansible](http://www.ansibleworks.com/docs/intro_installation.html) on the
   system you'll use to orchestrate Couchbase Sever cluster deployment.
   This  computer must have SSH access to the cluster nodes.
2. Define the cluster node hosts in either the `centos` or `ubuntu` file 
   depending on which OS you will use.
3. By default, the project will install the latest version of 
   Couchbase Server  Enterprise Edition for the OS you choose. If you wish to
   install a previous version, simply uncomment the version's entry in the
   `linux/roles/couchbase-server/centos/vars/main.yml` for CentOS or
   `linux/roles/couchbase-server/centos/vars/main.yml` for Ubuntu.
4. Define cluster node parameters, such as the cluster RAM quota
   (`server_ram`), data and index storage paths (`data_path`, `index_path`),
   physical disk information (`data_storage`, `index_storage`), network
   interface (`network`), and whether to enable OS firewall (`firewall`)
   in the `group_vars/all` file.
5. Execute the top level Ansible playbook to fully configure system,
   install Couchbase Server, and create a cluster:
   `ansible-playbook -i centos site.yml`
   (replace *centos* with *ubuntu* if you're using Ubuntu based nodes)

## Detailed Guide

If you'd like to follow a more detailed installation process with additional
explanation of the steps and technologies, see the following sections of
this guide.

### Prerequisites

Before you begin with the steps in this guide, please ensure that you have
the following prerequisites configured and installed.

#### Ansible

Install Ansible using `pip` by following the
[installation documentation](http://www.ansibleworks.com/docs/intro_installation.html#latest-releases-via-pip). Provided you are willing to install into your system
Python, the steps are simple and involve just 2 commands:

```
sudo easy_install pip
sudo pip install ansible
```

Alternatively, you can use a
[virtualenv](http://www.virtualenv.org/en/latest/) and install Ansible
into that.

### Bootstrap the Cluster

Now it's time for bootstrapping the Couchbase Server cluster.

Open a terminal, change into the *linux* subdirectory of this project's
root directory and execute the top level Ansible playbook with commands like
the following:

```
cd linux
ansible-playbook -i centos site.yml
```

Replace *centos* with *ubuntu* if you'd prefer to use Ubuntu hosts for your
cluster nodes.

After some time to configure the OS, download and install Couchbase Server,
and initialize the cluster, you'll soon be ready to try out your new cluster.

### Give it a Try

Once the Ansible playbooks complete without error, you can open a browser and
access the primary Couchbase Server cluster node with a URL like:

```
http://node1.local:8091
```

Substitute *couchbase.local* with the hostname of your Couchbase Server
primary node.

The administrator username and password as configured by this project
are:

* Username: **Administrator**
* Password: **couchbase**

Once you've logged in and verified that the cluster is available, you're now
ready to create buckets and begin using your new Couchbase Server development
cluster.

### Other Operations

While the basic instructions for getting started do a fair bit of work, you
can also run the `cluster_init.yml` playbook to only perform the cluster
initialization.

You can also run a subset of the tasks independently through Ansible's 
tag support; here is a list of available tags by role:

**bootstrap**

* *network* : Network hostname
* *system_packages* : Install any required system packages
* *system_tuning* : Set disk scheduler specified in `linux/group_vars/all`
  for the data and index volumes, disable Transparent Huge Pages, and
  other system tuning

**couchbase-server**

* *installation* : Download and install the Couchbase Server package
* *network* : Couchbase Server specific firewall settings
* *service* : Ensure that the Couchbase Server service is started

Here are some examples of tag usage:

Install required system packages, such as *libselinux-python* on CentOS:

```
ansible-playbook -i centos cluster_install.yml --tags "system_packages"
```

Set the disk scheduler specified in `linux/group_vars/all` for the data
and index volumes, disable Transparent Huge Pages, and perform other system
tuning:

```
ansible-playbook -i centos cluster_install.yml --tags "system_tuning"
```

Download and install the version of Couchbase Server specified in either
`defaults/main.yml` or `vars/main.yml` for the Linux distribution in use:

```
ansible-playbook -i centos cluster_install.yml --tags "installation"
```

You can also combine multiple tags, as in the following example, which will
perform the system tuning and installation tasks:

```
ansible-playbook -i centos cluster_install.yml \
--tags "system_tuning, installation"
```

## Notes

0. The project is confirmed to function with the following software versions:
 * Ansible version 1.4.3 
1. The project uses CentOS 6.4 as the default since this is a supported
   platform listed on the Couchbase Server package downloads page.
2. Review the different operating system tuning changes in the following
   files to adjust or add your own:
   * `roles/bootstrap/common/templates/etc_rc.local.j2`
   * `roles/bootstrap/common/templates/iptables.j2`
   * `roles/couchbase-server/centos/templates/iptables.j2`
   * `roles/couchbase-server/ubuntu/templates/iptables.j2`

## References

1. http://www.couchbase.com/couchbase-server/overview
2. http://www.ansibleworks.com/
