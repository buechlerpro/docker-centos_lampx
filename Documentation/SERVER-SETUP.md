Server setup
============

The server has been setup using the bellow commands.

Get CENTOS machine
------------------

- docker pull centos
- docker run -i -t --name centos centos /bin/bash

Install utilities
-----------------

- yum clean all
- yum -y update
- yum -y install vim
- yum -y install wget

Install Apache and MariaDB
--------------------------

- yum -y install httpd
- yum -y install mariadb-server

Configure Apache
----------------

- vim /etc/httpd/conf/httpd.conf
  (# add: ServerName localhost)
- httpd -k start

Configure MariaDB
-----------------
- mysql_install_db
- vim /etc/my.cnf
    - add in section [mysqld]: character-set-server=utf8    
- cd /var/lib/mysql
- chown mysql aria_log*
- chgrp mysql aria_log*
- chown -R mysql mysql
- chgrp -R mysql mysql
- chown -R mysql test
- chgrp -R mysql test
   
Run MariaDB:

- mysqld_safe &
- mysql_secure_installation

Install PHP
-----------

- yum -y install epel-release
- yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
- yum-config-manager --enable remi-php71
- yum update
- yum -y install php php-fpm php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo php-pear php-pecl-zip php-devel
- yum -y install gcc gcc-c++ make

Install Xdebug
--------------
- pecl install Xdebug

Configure Xdebug/TYPO3
----------------------

- vim /etc/php.ini

  - (# add: zend_extension="/usr/lib64/php/modules/xdebug.so")
  - (# add: max_execution_time=240)
  - (# add: max_input_vars=1500)
  - (# add: xdebug.remote_log="/tmp/xdebug.log") 
  - (# add: xdebug.profiler_enable = 1)
  - (# add: xdebug.remote_enable=on)
  - (# add: xdebug.remote_host=10.0.75.1)
  - (# add: xdebug.remote_port=9000)
  - (# add: xdebug.remote_autostart=0)
  - (# add: xdebug.idekey=editor-xdebug)
  - (# add: xdebug.max_nesting_level=400)
  
- httpd -k restart

Create init script
------------------
- vim /etc/initcontainer
  (# add: #!/bin/bash)
  (# add: /usr/sbin/apachectl &)
  (# add: mysqld_safe &)
  (# add: /bin/bash)

- chmod +x /etc/initcontainer

Create image
------------
- exit
- docker commit [container ID] centos_lampx
- docker run -i -t centos_lampx /bin/bash

References:
-----------
- http://k3rnelsec.es/crear-un-entorno-de-desarrollo-con-docker-windows-10-y-el-stack-de-tecnologias-apache-php-xdebug-mysql
- https://www.cyberciti.biz/faq/how-to-install-php-7-2-on-centos-7-rhel-7/