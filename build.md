ubuntu
======

rc.local
--------

  sudo ln -s /lib/systemd/system/rc-local.service /etc/systemd/system/rc-local.service

  sudo vi /etc/systemd/system/rc-local.service

    [Unit]  
    Description=/etc/rc.local Compatibility  
    Documentation=man:systemd-rc-local-generator(8)  
    ConditionFileIsExecutable=/etc/rc.local  
    After=network.target  

    [Service]  
    Type=forking  
    ExecStart=/etc/rc.local start  
    TimeoutSec=0  
    RemainAfterExit=yes  
    GuessMainPID=no  

    [Install]  
    WantedBy=multi-user.target  
    Alias=rc-local.service

  sudo vi /etc/rc.local

    #!/bin/sh -e
    #
    # rc.local
    #
    # This script is executed at the end of each multiuser runlevel.
    # Make sure that the script will "exit 0" on success or any other
    # value on error.
    #
    # In order to enable or disable this script just change the execution
    # bits.
    #
    # By default this script does nothing.

    exit 0

  sudo chmod 0755 /etc/rc.local  
  sudo systemctl daemon-reload

  sudo systemctl enable rc-local  
  sudo systemctl restart rc-local


centos
======


python
======

ubuntu
------

  sudo apt install -y libbz2-dev libdb-dev libexpat1-dev libffi-dev libgdbm-dev liblzma-dev libncurses5-dev libpcap-dev libreadline-dev libsqlite3-dev libssl-dev  
  sudo apt install -y tk-dev uuid-dev xz-utils zlib1g-dev

  ./configure --prefix=/opt/python --enable-optimizations --enable-shared  
  make install

  sudo cp /opt/python/lib/libpython*.so.* /usr/lib

centos
------

  yum install -y libdb-devel libffi-devel libpcap-devel libuuid-devel  
  yum install -y bzip2-devel expat-devel gdbm-devel ncurses-devel readline-devel sqlite-devel openssl-devel tk-devel xz xz-devel zlib-devel


apache
======

ubuntu
------

  sudo apt install -y apache2 apache2-dev  
  sudo apt install -y php php-mysql php-xml libapache2-mod-php

  sudo a2enmod proxy  
  sudo a2enmod rewrite  
  sudo a2enmod ssl

  sudo systemctl restart apache2

  ----------------------------------------

  sudo vi /etc/apache2/sites-available/php.conf

    <VirtualHost *>
        ServerName 127.0.0.1
        DocumentRoot /build/www/myapp

        <Directory /build/www/myapp>
            AllowOverride All
            Require all granted
        </Directory>
    </VirtualHost>

  sudo a2ensite php

  sudo systemctl restart apache2

  ----------------------------------------

  pip install mod_wsgi

  mod_wsgi-express module-config

    LoadModule wsgi_module "/opt/python/lib/python3.7/site-packages/mod_wsgi/server/mod_wsgi-py37.cpython-37m-x86_64-linux-gnu.so"
    WSGIPythonHome "/opt/python"

  sudo vi /etc/apache2/mods-available/wsgi.load

    LoadModule wsgi_module "/opt/python/lib/python3.7/site-packages/mod_wsgi/server/mod_wsgi-py37.cpython-37m-x86_64-linux-gnu.so"
    WSGIPythonHome "/opt/python"

  sudo vi /etc/apache2/sites-available/wsgi.conf

    <VirtualHost *>
        ServerName 127.0.0.1

        WSGIDaemonProcess myapp user=user group=user home=/build/www/myapp python-path=/build/www/myapp
        WSGIScriptAlias / /build/www/myapp/app.wsgi

        <Directory /build/www/myapp>
            WSGIProcessGroup myapp
            WSGIApplicationGroup %{GLOBAL}

            AllowOverride All
            Require all granted
        </Directory>
    </VirtualHost>

  sudo vi /etc/apache2/sites-available/default-ssl.conf

    <VirtualHost _default_:443>
        ServerName 127.0.0.1

        WSGIDaemonProcess myapp443 user=user group=user home=/build/www/myapp python-path=/build/www/myapp
        WSGIScriptAlias / /build/www/myapp/app.wsgi

        <Directory /build/www/myapp>
            WSGIProcessGroup myapp443
            WSGIApplicationGroup %{GLOBAL}

            AllowOverride All
            Require all granted
        </Directory>
    </VirtualHost>

  vi /build/www/myapp/app.wsgi

    import sys

    sys.path.insert(0, '/build/venv/lib/python3.7/site-packages')

    from app import app as application

  sudo a2enmod wsgi  
  sudo a2ensite wsgi  
  sudo a2ensite default-ssl

  sudo systemctl restart apache2

centos
------

  yum install -y httpd


samba
=====

ubuntu
------

  sudo apt install -y samba

  sudo vi /etc/selinux/config

    SELINUX=disabled

  sudo vi /etc/samba/smb.conf

    # ----------------------------------------------------------

    [build]
    comment = build
    path = /build
    writeable = yes
    browseable = yes
    valid users = user

  systemctl enable smb.service  
  systemctl restart smb.service

centos
---------

  yum install -y samba

  systemctl disable firewalld.service  
  systemctl stop firewalld.service


mariadb
=======

ubuntu
------

  sudo apt install -y mariadb-server

  sudo systemctl enable mariadb  
  sudo systemctl restart mariadb

  sudo mysql -uroot

    grant all privileges on *.* to 'root'@'localhost' identified by '123456' with grant option;
    grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;

  sudo vi /etc/mysql/mariadb.conf.d/50-server.cnf

    # bind-address    = 127.0.0.1

  sudo systemctl restart mariadb

centos
---------
