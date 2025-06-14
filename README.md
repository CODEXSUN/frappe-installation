# frappe-installation
Frappe install
# Frappe-ERPNext Version-15 in Ubuntu 24.04 LTS
A complete Guide to Install Frappe/ERPNext version 15  in Ubuntu 24.04 LTS


### Pre-requisites

      Python 3.11+                                  (python 3.12 is inbuilt in 24.04 LTS)
      Node.js 18+

      Redis 5                                       (caching and real time updates)
      MariaDB 10.3.x / Postgres 9.5.x               (to run database driven apps)
      yarn 1.12+                                    (js dependency manager)
      pip 20+                                       (py dependency manager)
      wkhtmltopdf (version 0.12.5 with patched qt)  (for pdf generation)
      cron                                          (bench's scheduled jobs: automated certificate renewal, scheduled backups)
      NGINX                                         (proxying multitenant sites in production)


> ## Note:
> ubuntu 24.04 default python version is python3.12
>
> ubuntu 24.04 default mariadb version is 10.11

### STEP 1 update linux

```sh
sudo apt-get update -y 
```
```sh
sudo apt-get upgrade -y
```

### STEP 1 Install git
```sh
sudo apt-get install git -y
```    

### STEP 2 install python-dev
```sh
sudo apt-get install python3-dev -y
```

### STEP 3 Install setuptools and pip (Python's Package Manager).
```sh
sudo apt-get install python3-setuptools python3-pip -y 
```    

### STEP 4 Install virtualenv
```sh
sudo apt install python3.12-venv -y
``` 


### STEP 5 Software Properties
```sh
sudo apt-get install software-properties-common -y
```    
### STEP 5 Install MariaDB
```sh
sudo apt install mariadb-server -y
```

```
sudo systemctl status mariadb
```
```
sudo mysql_secure_installation
```

### STEP 7
```
      In order to log into MariaDB to secure it, we'll need the current
      password for the root user. If you've just installed MariaDB, and
      haven't set the root password yet, you should just press enter here.

      Enter current password for root (enter for none): # PRESS ENTER
      OK, successfully used password, moving on...


      Switch to unix_socket authentication [Y/n] Y
      Enabled successfully!
      Reloading privilege tables..
       ... Success!

      Change the root password? [Y/n] Y
      New password:
      Re-enter new password:
      Password updated successfully!
      Reloading privilege tables..
       ... Success!

      Remove anonymous users? [Y/n] Y
       ... Success!

       Disallow root login remotely? [Y/n] Y
       ... Success!

       Remove test database and access to it? [Y/n] Y
       - Dropping test database...
       ... Success!
       - Removing privileges on test database...
       ... Success!

       Reload privilege tables now? [Y/n] Y
       ... Success!
```




### STEP 6  MySQL database development files
```sh
sudo apt-get install libmysqlclient-dev -y
```

### STEP 7 Edit the mariadb configuration ( unicode character encoding )
```sh
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
## add this to the 50-server.cnf file
```sh
[server]
user = mysql
pid-file = /run/mysqld/mysqld.pid
socket = /run/mysqld/mysqld.sock
basedir = /usr
datadir = /var/lib/mysql
tmpdir = /tmp
lc-messages-dir = /usr/share/mysql
bind-address = 127.0.0.1
query_cache_size = 16M
log_error = /var/log/mysql/error.log

[mysqld]
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
```
Now press (Ctrl-X) to exit
```sh
    sudo service mysql restart
```

### STEP 8 install Redis
```sh
sudo apt-get install redis-server -y
```

### STEP 9 install Node.js 18.X package
```sh
sudo apt install curl
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 18
```

### STEP 10  install Yarn
```sh
sudo apt-get install npm
```

```sh
sudo npm install -g yarn
```

### STEP 11 install wkhtmltopdf
```sh
sudo apt-get install xvfb libfontconfig wkhtmltopdf -y
```

### STEP 12 install frappe-bench
```sh
    sudo -H pip3 install frappe-bench --break-system-packages
```

```sh
bench --version
```

### STEP 13 initilise the frappe bench & install frappe latest version
```sh
    bench init frappe-bench --frappe-branch version-15
```
```sh
cd frappe-bench/
```

```sh
bench start
```

### STEP 14 create a site in frappe bench

>### Note
>Warning: MariaDB version ['10.11', '7'] is more than 10.8 which is not yet tested with Frappe Framework.
```sh
    bench new-site dcode.com
```
```sh
bench --site dcode.com add-to-hosts
```

Open url http://dcode.com:8000 to login


### STEP 15 install ERPNext latest version in bench & site
```sh
bench get-app erpnext --branch version-15
```
        ###OR
```sh
bench get-app https://github.com/frappe/erpnext --branch version-15
```
```sh
bench --site dcode.com install-app erpnext
```
```sh
    bench start
```




