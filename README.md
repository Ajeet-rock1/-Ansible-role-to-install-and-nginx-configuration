# -Ansible-role-to-install-and-nginx-configuration


nginx

Installs Nginx on RedHat/CentOS or Debian/Ubuntu Linux, FreeBSD or OpenBSD servers.

This role installs and configures the latest version of Nginx from the Nginx yum repository (on RedHat-based systems) or via apt (on Debian-based systems) or pkgng (on FreeBSD systems) or pkg_add (on OpenBSD systems). You will likely need to do extra setup work after this role has installed Nginx, like adding your own [virtualhost].conf file inside /etc/nginx/conf.d/, describing the location and options to use for your particular website.


Requirements

This role requires Ansible 2.0 or higher and platform requirements are listed in the metadata file. (Some older version of the role support Ansible 1.4) For FreeBSD a working pkgng setup is required (see: https://www.freebsd.org/doc/handbook/pkgng-intro.html )
