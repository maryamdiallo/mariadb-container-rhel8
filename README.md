# MariaDB Container Deployment using Podman on RHEL 9

 Overview

This project demonstrates how to install container tools on a RHEL 9 server, pull a Red Hat certified MariaDB container image from the Red Hat Container Catalog, and run it using Podman. The MariaDB container is set up with persistent storage, port mapping, and environmental configuration. The container is configured to start automatically on boot using systemd.

---
Working with Red Hat container registry using skopeo and podman

Setting up containerized MariaDB with Podman

Managing container lifecycle with systemd

Handling SELinux context using :Z 
--- 

## Step-by-Step Deployment

### 1. Install Container Tools on RHEL 9

```bash
$ yum install container-tools -y
$ ssh maryam@localhost
$ podman login registry.redhat.io
$ skopeo inspect docker://registry.redhat.io/rhel8/mariadb-103
$ podman search registry.redhat.io/mariadb
$ podman pull registry.redhat.io/rhel8/mariadb-103:1-86
$ mkdir /home/maryam/db_data
$ chmod 777 /home/maryam/db_data
$ podman run -d --name inventorydb -p 13306:3306 
-v /home/maryam/db_data:/var/lib/mysql/data:Z 
-e MYSQL_USER=operator1 
-e MYSQL_PASSWORD=redhat 
-e MYSQL_DATABASE=inventory 
-e MYSQL_ROOT_PASSWORD=redhat 
registry.redhat.io/rhel8/mariadb-103:1-86 /bin/bash
$ podman images         # List images
$ podman ps             # Running containers
$ podman ps -a          # All containers
$ mkdir -p ~/.config/systemd/user/
$ cd ~/.config/systemd/user/
$ podman generate systemd --name inventorydb --files --new
$ podman stop inventorydb
$ podman rm inventorydb
$ systemctl --user daemon-reload
$ systemctl --user enable --now container-inventorydb.service

