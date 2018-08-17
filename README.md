Ansible Role for Nginx in Docker
================================

[![Build Status](https://travis-ci.org/derekmerck/ansible-nginx-docker.svg?branch=master)](https://travis-ci.org/derekmerck/ansible-nginx-docker)

Derek Merck  
<derek_merck@brown.edu>  
Rhode Island Hospital and Brown University  
Providence, RI  

Configure and run a [Nginx](https://https://www.nginx.com) web server with reverse proxying in a Docker container.


Dependencies
------------

### Galaxy Roles

- [geerlingguy.docker](https://github.com/geerlingguy/ansible-role-docker) to setup the docker environment
- [geerlingguy.pip](https://github.com/geerlingguy/ansible-role-pip) to install Python reqs


### Remote Node

- [Docker][]
- [docker-py][]

[Docker]: https://www.docker.com
[docker-py]: https://docker-py.readthedocs.io


Role Variables
--------------

### Docker Image and Tag

Always uses the official [Nginx][] image.

[nginx]: https://hub.docker.com/_/nginx/

Set the Nginx version tag.

```yaml
nginx_docker_image_tag:   "latest"
```

### Docker Container Configuration

Always runs on ports 80 and 443 (if secured)

```yaml
nginx_container_name:    "nginx"
nginx_config_dir:        "/opt/{{ nginx_container_name }}"
```

### Service Configuration

See examples for format of upstream reverse proxies and security.

```yaml
nginx_upstreams:         []
nginx_security:          {}
```


Example Playbook
----------------

```yaml
- hosts: web_server
  roles:
    - role: derekmerck.nginx_docker
      nginx_upstreams:
        - name: upstream
          path: /my_path    # No trailing slash, rewrite host -> host/my_path
          pool:
            - port: 5000
              # host defaults to "localhost"
            - host: another_host
              port: 80
      nginx_security:
        cert_dir:    "/opt/certs"
        common_name: "www.example.com"
```


License
-------

MIT