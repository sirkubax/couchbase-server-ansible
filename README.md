# Couchbase Server with Ansible


## 301 MOVED PERMANENTLY

PLEASE NOTE: All of the current work for Couchbase Server with Ansible has
moved to this repository:

https://github.com/couchbaselabs/ansible-couchbase-server


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
automate the deployment of Couchbase Server Enterprise Edition using the
[Ansible](http://www.ansibleworks.com/) orchestration engine on Linux and
for Mac OS X with Vagrant managed VirtualBox environments.

This project is comprised of 2 distinct sub-projects structured as follows:

* **linux**: Ansible project and [documentation](https://github.com/couchbaselabs/couchbase-server-ansible/tree/master/linux#couchbase-server-with-ansible) for Linux
 * group_vars: Global group variables for the project
 * library: The couchbase-server Ansible module
 * roles: Roles for bootstrapping and initializing a Couchbase Server cluster
 * centos: Hosts inventory file for CentOS based clusters
 * cluster_init.yml: Playbook for initializing a Couchbase Server cluster
 * cluster_install.yml: Playbook for installing OS and Couchbase Server
 * create_bucket.yml: Ansible playbook for creating a Couchbase Server bucket
 * site.yml: Top level Ansible playbook installs and builds the cluster
 * ubuntu: Hosts inventory file for Ubuntu based clusters

* **macosx**: Ansible project and [documentation](https://github.com/couchbaselabs/couchbase-server-ansible/tree/master/macosx#couchbase-server-with-ansible) for Mac OS X with Vagrant
  * bin: Convenience scripts for Vagrant host preparation
  * group_vars: Global group variables for the project
  * library: The couchbase-server Ansible module
  * roles: Roles for bootstrapping and initializing a Couchbase Server cluster
  * centos: Hosts inventory file for CentOS based clusters
  * cluster_init.yml: Playbook for initializing a Couchbase Server cluster
  * cluster_install.yml: Playbook for installing OS and Couchbase Server
  * create_bucket.yml: Ansible playbook for creating a Couchbase Server bucket
  * site.yml: Top level Ansible playbook installs and builds the cluster
  * ubuntu: Hosts inventory file for Ubuntu based clusters
  * Vagrantfile: The Vagrant configuration file

## Thank You

Without the following people, this project would not have been possible, so
a big thank you to:

* Tugdual Grall for his excellent [Create a Couchbase Cluster with Ansible](http://blog.couchbase.com/create-couchbase-cluster-with-ansible)
blog post, which served as inspiration for this project
* Brent Woodruff for providing several helpful tips

