## new vanilla box

```
root@monitoring:~# lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 13.10
Release:	13.10
Codename:	saucy
```

## local ansible

```
% ansible --version                                                                                                   
ansible 1.4
```

## local repo

```
± % cat staging                                                                                                                  
[monitoring]
monitoring.goncalopereira.com

± % cat roles/skyline/graphite/vars/main.yml                                                                            graphite: { url: 'http://localhost', carbon_port: 2003 }%     

± % cat roles/skyline/oculus/vars/main.yml                                                              
oculus: { url: 'http://your_oculus_host.com' }

± % cat roles/skyline/redis/vars/main.yml                                                                               
redis_server: { name: redis-server, version: '2:2.6.13-1' }

± % cat roles/skyline/vars/main.yml                                                                                     
webapp: { port: 1500, ip: 0.0.0.0 }

horizon: { pickle_port: 2024, udp_port: 2025 }

skyline: {
  version: master,
  path: /srv/skyline,
  repo: 'https://github.com/etsy/skyline.git',
  services: [ horizon.d, analyzer.d, webapp.d ]
  }
  
```

## run

```
± % ansible-playbook -i staging playbook.yml                                                                            

PLAY [monitoring] ************************************************************* 

GATHERING FACTS *************************************************************** 
previous known host file not found
The authenticity of host 'monitoring.goncalopereira.com (XXX)' can't be established.
ECDSA key fingerprint is XXX.
Are you sure you want to continue connecting (yes/no)? yes
ok: [monitoring.goncalopereira.com]

TASK: [skyline/redis | Install redis] ***************************************** 
changed: [monitoring.goncalopereira.com]

TASK: [skyline/redis | Update redis.conf] ************************************* 
changed: [monitoring.goncalopereira.com]

TASK: [skyline | Install tools] *********************************************** 
changed: [monitoring.goncalopereira.com] => (item={'name': 'python-dev'})
changed: [monitoring.goncalopereira.com] => (item={'name': 'python-pip'})
changed: [monitoring.goncalopereira.com] => (item={'name': 'git'})

TASK: [skyline | Checkout Skyline] ******************************************** 
changed: [monitoring.goncalopereira.com]

TASK: [skyline | Create Skyline log directories] ****************************** 
changed: [monitoring.goncalopereira.com] => (item=/var/log/skyline)
changed: [monitoring.goncalopereira.com] => (item=/var/run/skyline)

TASK: [skyline | Install /srv/skyline/requirements.txt with pip] ************** 
changed: [monitoring.goncalopereira.com]

TASK: [skyline | Install remaining dependencies with pip] ********************* 
changed: [monitoring.goncalopereira.com] => (item={'version': '0.2.1', 'name': 'patsy'})
changed: [monitoring.goncalopereira.com] => (item={'version': '0.4.0', 'name': 'msgpack_python'})

TASK: [skyline | Install python dependencies] ********************************* 
changed: [monitoring.goncalopereira.com] => (item={'name': 'python-numpy'})
changed: [monitoring.goncalopereira.com] => (item={'name': 'python-scipy'})
changed: [monitoring.goncalopereira.com] => (item={'name': 'python-scikits.statsmodels'})

TASK: [skyline | Update Skyline settings.py template] ************************* 
changed: [monitoring.goncalopereira.com]

TASK: [skyline | skyline init.d templates] ************************************ 
changed: [monitoring.goncalopereira.com] => (item=horizon.d)
changed: [monitoring.goncalopereira.com] => (item=analyzer.d)
changed: [monitoring.goncalopereira.com] => (item=webapp.d)

TASK: [skyline | start skyline] *********************************************** 
changed: [monitoring.goncalopereira.com] => (item=horizon.d)
changed: [monitoring.goncalopereira.com] => (item=analyzer.d)
changed: [monitoring.goncalopereira.com] => (item=webapp.d)

NOTIFIED: [skyline/redis | restart redis] ************************************* 
changed: [monitoring.goncalopereira.com]

NOTIFIED: [skyline | restart skyline] ***************************************** 
changed: [monitoring.goncalopereira.com] => (item=horizon.d)
changed: [monitoring.goncalopereira.com] => (item=analyzer.d)
changed: [monitoring.goncalopereira.com] => (item=webapp.d)

PLAY RECAP ******************************************************************** 
monitoring.goncalopereira.com : ok=14   changed=13   unreachable=0    failed=0   
  
```


