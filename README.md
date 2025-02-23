# what is stolon?
stolon is a cloud native PostgreSQL manager for PostgreSQL high availability. It's cloud native because it'll let you keep an high available PostgreSQL inside your containers (kubernetes integration) but also on every other kind of infrastructure (cloud IaaS, old style infrastructures etc...)
[stolon official github](https://github.com/sorintlab/stolon)

# what dose this ansible-playbook do?

this ansible-playbook installs and configs stolon cluster components 
including:
* etcd
* stolon-sentinel
* stolon-keeper
* stolon-proxy

# how to use it?
first of all, you need to install ansible and ansible-playbook 

```
sudo apt update
sudo apt install ansible
```
## roles
there is three role, first one is for installing and configuring etcd and the second one, dose the same for stolon keeper and the third one is for stolon-proxy.


### etcd role
to install and configure etcd, first of all put your host ip and username of your etcd host in inventory.ini in [etcd-host] section:
```
[etcd_host]
etcd ansible_host=<etcd-ip>  ansible_user=<etcd-user>
```

then, put your enviroments variables on roles/etcd/vars/main.yml
```
etcd_version: '3.5.15'
etcd_ip_address: '<etcd-ip>'
```

### stolon keeper role
same as etcd, first you need to change inventory.ini values according to your setup.
set your stolon-keeper hosts:

```
[keeper_hosts]
keeper_1 ansible_host=<keeper-ip> ansible_user=<username>

```
then you need to put your enviroments variables in roles/stolon-keeper/vars/main.yml.


### stolon proxy role
change inventory.ini values according to your setup.
set your stolon-proxy hosts:

```
[proxy_hosts]
proxy_1 ansible_host=<proxy_host-ip>  ansible_user=<username>
```

then you need to put your enviroments variables in roles/stolon-proxy/vars/main.yml.

# run playbook
you can run this playbook after configuring variables and inventory.ini

```
ansible-playbook -i inventory.ini etcd.yml --ask-pass --ask-become-pass
ansible-playbook -i inventory.ini install_keepers.yml --ask-pass --ask-become-pass
ansible-playbook -i inventory.ini install_proxy.yml --ask-pass --ask-become-pass
```
