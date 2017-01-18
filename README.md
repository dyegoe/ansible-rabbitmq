# Ansible RabbitMQ
This ansible-playbook is used to deploy a standalone or a cluster of RabbitMQ service, and you can also use to deploy how many servers you want.
If you want just standalone, specify only masters at inventory or if you want a cluster you need to specify either master (only 1) and slaves (how many you want).

## Requirements
* CentOS7 minimal install
* Check if you have access with key pairs
* Check sudoers to don't prompt password
* If you will use a different storage device to RabbitMQ, verify if the server has a parition mounted as /var/lib/rabbitmq

## Coverage
* role: check_os 
  * check if OS is compatible
* role: firewall
  * open some tcp ports used by RabbitMQ ('amqp': '5672', 'mgmt': '15672', 'amqp_cli': '25672', 'empd': '4369')
  * open tcp ports wich will be used by HAproxy
* role: hosts
  * configure /etc/hosts based onde IP and hostnames fetched by Ansible Setup
* role: common
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
  * stop_app and reset all nodes
  * start_app at master
  * connect slaves to master
  * start_app at slaves.
  * set policy to default vhost to sync messages through the cluster
* role: haproxy (as the same condition above)
  * install haproxy
  * copy conf
  * start service
## Configuration
* Some vars can be changed at inventory/production
* at site.yml you can uncomment environment vars to use proxy. This is an example, you need change it;

## Consider
**Advice:** This project was created just to maintain my ansible-playbooks. You can use, distribute, change or whatever you want, and don't need to mention myself. But use at your own risk. I can't guarantee that will be functional in your environment.

**Contact:** if you have any question or sugestion you can open an issue and I will response as soon as possible.
