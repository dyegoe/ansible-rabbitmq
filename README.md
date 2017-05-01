# Ansible RabbitMQ
This ansible-playbook is used to deploy a standalone or a cluster of RabbitMQ service, and you can also use to deploy how many servers as you want.
If you want just standalone, specify only masters at inventory or if you want a cluster you need to specify either master (only 1) and slaves (how many as you want).

## Requirements
* CentOS7 minimal install
* Check if you have access with key pairs
* Check sudoers to don't prompt password
* If you will use a different storage device to RabbitMQ, verify if the server has a parition mounted as /var/lib/rabbitmq

## Coverage
* role: check\_os 
  * check if OS is compatible
* role: hosts
  * configure /etc/hosts based onde IP and hostnames fetched by Ansible Setup
* role: firewall_rmq
  * open some tcp ports used by RabbitMQ ('amqp': '5673', 'mgmt': '15672', 'amqp_cli': '25672', 'empd': '4369')
* role: firewall_hap
  * open tcp ports wich will be used by HAproxy ('amqp': '5672')
* role: rabbitmq
  * change file descriptors limits
  * install RabbitMQ package
  * install libselinux-python (ansible requirement)
  * copy conf
  * add file descriptors limits to service
  * restart service
  * enable rabbitmq\_management plugin
* role: cluster (played only if you configure one or more slaves at inventory)
  * fetch .erlang\_cookie from master
  * copy it to slaves
  * restart RabbitMQ service
  * stop\_app and reset all nodes
  * start\_app at master
  * connect slaves to master
  * start\_app at slaves.
  * set policy to default vhost to sync messages through the cluster
* role: haproxy (played only if you configure one or more haproxy at inventory)
  * install haproxy
  * copy conf
  * start service
## Configuration
* Some vars could be changed at inventory/hosts
* Tips:
  * Standalone version: Specify 1 or more hosts at master group.
  * Cluster version: Specify 1 host at master group and 1 or more hosts at slaves group.
  * Cluster version with HAproxy: Specify 1 host at master group, 1 or more hosts at slaves group and 1 or more hosts at haproxy group (this could be same as slaves).

## Consider
**Advice:** This project was created just to maintain my ansible-playbooks. You would be able to use, distribute, change or whatever you want, and don't need to mention myself. But use at your own risk. I can't guarantee that will be functional in your environment.

**Contact:** if you have any question or sugestion you can open an issue and I will response as soon as possible.
