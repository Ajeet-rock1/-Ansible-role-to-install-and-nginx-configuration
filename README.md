# -Ansible-role-to-install-and-nginx-configuration


nginx

Installs Nginx on RedHat/CentOS or Debian/Ubuntu Linux, FreeBSD or OpenBSD servers.

This role installs and configures the latest version of Nginx from the Nginx yum repository (on RedHat-based systems) or via apt (on Debian-based systems) or pkgng (on FreeBSD systems) or pkg_add (on OpenBSD systems). You will likely need to do extra setup work after this role has installed Nginx, like adding your own [virtualhost].conf file inside /etc/nginx/conf.d/, describing the location and options to use for your particular website.


Requirements

This role requires Ansible 2.0 or higher and platform requirements are listed in the metadata file. (Some older version of the role support Ansible 1.4) For FreeBSD a working pkgng setup is required (see: https://www.freebsd.org/doc/handbook/pkgng-intro.html )


Role Variables

Available variables are listed below, along with default values (see defaults/main.yml):

nginx_vhosts: []

A list of vhost definitions (server blocks) for Nginx virtual hosts. If left empty, you will need to supply your own virtual host configuration. See the commented example in defaults/main.yml for available server options. If you have a large number of customizations required for your server definition(s), you're likely better off managing the vhost configuration file yourself, leaving this variable set to [].

nginx_vhosts:
  - listen: "80 default_server"
    server_name: "example.com"
    root: "/var/www/example.com"
    index: "index.php index.html index.htm"
    error_page: ""
    access_log: ""
    error_log: ""
    extra_parameters: |
      location ~ \.php$ {
          fastcgi_split_path_info ^(.+\.php)(/.+)$;
          fastcgi_pass unix:/var/run/php5-fpm.sock;
          fastcgi_index index.php;
          fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
          include fastcgi_params;
      }

An example of a fully-populated nginx_vhosts entry, using a | to declare a block of syntax for the extra_parameters.

Please take note of the indentation in the above block. The first line should be a normal 2-space indent. All other lines should be indented normally relative to that line. In the generated file, the entire block will be 4-space indented. This style will ensure the config file is indented correctly.

nginx_remove_default_vhost: false

Whether to remove the 'default' virtualhost configuration supplied by Nginx. Useful if you want the base / URL to be directed at one of your own virtual hosts configured in a separate .conf file.

nginx_vhosts_filename: "vhosts.conf"



The filename to use to store vhosts configuration. If you run the role multiple times (e.g. include the role with with_items), you can change the name for each run, effectively creating a separate vhosts file per vhost configuration.

nginx_upstreams: []

If you are configuring Nginx as a load balancer, you can define one or more upstream sets using this variable. In addition to defining at least one upstream, you would need to configure one of your server blocks to proxy requests through the defined upstream (e.g. proxy_pass http://myapp1;). See the commented example in defaults/main.yml for more information.

nginx_user: "nginx"

The user under which Nginx will run. Defaults to nginx for RedHat, www-data for Debian and www on FreeBSD and OpenBSD.

nginx_worker_processes: "{{ ansible_processor_vcpus|default(ansible_processor_count) }}"
nginx_worker_connections: "1024"
nginx_multi_accept: "off"

nginx_worker_processes should be set to the number of cores present on your machine (if the default is incorrect, find this number with grep processor /proc/cpuinfo | wc -l). nginx_worker_connections is the number of connections per process. Set this higher to handle more simultaneous connections (and remember that a connection will be used for as long as the keepalive timeout duration for every client!). You can set nginx_multi_accept to on if you want Nginx to accept all connections immediately.

nginx_error_log: "/var/log/nginx/error.log warn"
nginx_access_log: "/var/log/nginx/access.log main buffer=16k"

Configuration of the default error and access logs. Set to off to disable a log entirely.

nginx_sendfile: "on"
nginx_tcp_nopush: "on"
nginx_tcp_nodelay: "on"


_____________________________XXXX____________________________
