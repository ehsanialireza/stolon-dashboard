# what is stolon?
stolon is a cloud native PostgreSQL manager for PostgreSQL high availability. It's cloud native because it'll let you keep an high available PostgreSQL inside your containers (kubernetes integration) but also on every other kind of infrastructure (cloud IaaS, old style infrastructures etc...)
[stolon official github](https://github.com/sorintlab/stolon)

# what dose this dashboard do?
This is a simple Grafana dashboard developed for monitoring the status of various Stolon components (Keeper, Proxy, Sentinel, etc.). It allows for analyzing the behavior of the database cluster, ensuring its proper functioning, and setting up alert rules when needed.

# how to use it?
first of all, you need to install prometheus and grafana
you can use this docker compose file

```
version: '3.7'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
    networks:
      - monitoring
    restart: always

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    ports:
      - "3000:3000"
    depends_on:
      - prometheus
    networks:
      - monitoring
    restart: always

networks:
  monitoring:
    driver: bridge

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
