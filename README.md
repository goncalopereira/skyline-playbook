## New vanilla box

```
root@monitoring:~# lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 13.10
Release:	13.10
Codename:	saucy
```

## local box

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


