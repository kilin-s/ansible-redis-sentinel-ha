https://redis.io/docs/management/admin/

### DEPLOY

set group vars:
```yaml
redis_server_master_ip: 192.168.122.175 #ip of redis master
redis_server_auth_pass: 123 #redis password
redis_sentinel_auth_pass: 1234 #redis sentinel password
keepalived_virtual_ip: 192.168.122.99 #keepalived virtual ip
keepalived_instance_interface: enp1s0 # keepalive interface 
keepalived_virtual_router_id: 1 # unic for vrrp cluster

# backends for haproxy
haproxy_redis_backend:
- host: host1 
  ip: 192.168.122.175
  maxconn: 50000
  interval: 1s
- host: host2
  ip: 192.168.122.49
  maxconn: 50000
  interval: 1s
- host: host3
  ip: 192.168.122.46
  maxconn: 50000
  interval: 1s
```

add host vars

host1
```yaml
keepalived_instance_priority=100 
redis_server_bind_ip: 192.168.122.175
redis_master: true
keepalived_vrrp_state: MASTER
```
host2
```yaml
keepalived_instance_priority=90
redis_server_bind_ip: 192.168.122.49
keepalived_vrrp_state: BACKUP
```
host3
```yaml
keepalived_instance_priority=80 
redis_server_bind_ip: 192.168.122.46
keepalived_vrrp_state: BACKUP
```

run playbook
```sh
ansible-playbook ./redis.yml -i hosts -u root
```

add var to group var
```yaml
redis_bootstrap: false
```
