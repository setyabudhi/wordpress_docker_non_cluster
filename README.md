# wordpress_docker_non_cluster

Update OS Package &Configure SELinux

Update OS package

> dnf clean all

> dnf repolis

> dnf update -y

# Configure SELinux to permissive

### set selinux temporary
> setenforce 0

### set selinux permanently
> vi /etc/selinux/config

> SELINUX​=permissive #⟵ change this value to permissive


# Install Docker & Start Docker Services
# Install Docker CE

wget https://download.docker.com/linux/centos/docker-ce.repo

cp docker-ce.repo /etc/yum.repos.d/

dnf repolist |​grep -i docker

dnf install docker-ce -y

# Verify Docker installation

rpm -qa |​grep docker
​
# Start Docker Services

systemctl enable docker

systemctl start docker

systemctl status docker


# Testing Docker Functionality
Search images

> docker search hello

Pull images

> docker pull hello-world

Run a container

> docker run hello-world

View docker images locally

> docker images

View docker container

> docker ps

> docker ps -a

> docker container ls
​
# Prepare MariaDB & Wordpress Image from Registry
Search images

> docker search mariadb

Download mariadb image

> docker pull mariadb

Search wordpress image

> docker search wordpress

Download wordpress image

> docker pull wordpress

Verify downloaded images

> docker images

REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
wordpress     latest    67c9c1ed40ff   4 days ago    550MB
mariadb       latest    1138596852f3   5 days ago    401MB
hello-world   latest    d1165f221234   3 weeks ago   13.3kB


# Prepare Persistent Volume for MariaDB & Wordpress

# Create directory for MariaDB and for Wordpress source code

> mkdir -p /root/var/lib/mysql

> mkdir -p /root/var/www/html


# Prepare Virtual Network
# View existing virtual network

> docker network ls

# Create network for MariaDB & Wordpress

> docker network create --attachable mariadb-wp-privnet

# Verify created private network

> docker network ls

NETWORK ID     NAME                 DRIVER    SCOPE
d4ef406a2b56   bridge               bridge    local
82862fb5a762   host                 host      local
941494cfbcb3   mariadb-wp-privnet   bridge    local
2d61f1b10543   none                 null      local

# Run the Containers
Create shell script to simplify running that container, you can write manually shell script from PDF in gdrive, or you can use my script that already hosted on github.

# Download the shell scripts

> wget https://raw.githubusercontent.com/rifqim/dts/main/run_mariadb_with_persistent_volume_and_private_network.sh
>
> wget https://raw.githubusercontent.com/rifqim/dts/main/run_wordpress_with_persistent_volume_and_published_port.sh

Give they executable mode

> chmod +x run_*.sh

Finally, execute script one by one to start the containers

> ./run_mariadb_with_persistent_volume_and_private_network.sh
>
> ./run_wordpress_with_persistent_volume_and_published_port.sh

Verify the container list, there should be two container is running

> docker ps

CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                NAMES

e7bf21d91d8b   wordpress   "docker-entrypoint.s…"   53 minutes ago   Up 53 minutes   0.0.0.0:80->​80/tcp   wordpress

e421caaebe6f   mariadb     "docker-entrypoint.s…"   53 minutes ago   Up 53 minutes   3306/tcp             wordpressdb

# Final Step

Verify that wordpress can be accessed, with curl or browser

With curl :

> curl http://localhost -sL |​grep -i "wordpress\|installation\|welcome"

With browser :
Access http://localhost 
