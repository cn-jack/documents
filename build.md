# ubuntu

  ## rc.local

    sudo ln -s /lib/systemd/system/rc-local.service /etc/systemd/system/rc-local.service

    sudo vi /etc/systemd/system/rc-local.service

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

# centos

# python

    ## ubuntu

        sudo apt install -y libbz2-dev libdb-dev libexpat1-dev libffi-dev libgdbm-dev liblzma-dev libncurses5-dev libpcap-dev libreadline-dev libsqlite3-dev libssl-dev
        sudo apt install -y tk-dev uuid-dev xz-utils zlib1g-dev

        ./configure --prefix=/opt/python --enable-optimizations
        make install

    ## centos

        yum install -y libdb-devel libffi-devel libpcap-devel libuuid-devel
        yum install -y bzip2-devel expat-devel gdbm-devel ncurses-devel readline-devel sqlite-devel openssl-devel tk-devel xz xz-devel zlib-devel

# samba

    ## ubuntu

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

    ## centos

        yum install -y samba

        systemctl disable firewalld.service
        systemctl stop firewalld.service

# mariadb

    ## ubuntu

        sudo apt install -y mariadb-server

        sudo systemctl enable mariadb
        sudo systemctl restart mariadb

        sudo mysql -uroot

        > grant all privileges on *.* to 'root'@'localhost' identified by '123456' with grant option;
        > grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;

        sudo vi /etc/mysql/mariadb.conf.d/50-server.cnf

            # bind-address    = 127.0.0.1

        sudo systemctl restart mariadb

    ## centos
