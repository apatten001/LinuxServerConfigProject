# LinuxServerConfigProject

### IP address and SSH port
(This IP address is no longer active on AWS)
1. **Public IP address:** 34.238.183.5 **SSH port:** 2200
2. **URL of the hosted web app:** [Catalog Project](http://34.238.183.5) 



### Create an account for AWS lightsail and create an instance.

![](https://github.com/apatten001/LinuxServerConfigProject/blob/master/Server.png)


### Software Installed
1. python3-pip
2. apache2
3. libapache2-mod-wsgi-py3
4. Flask
5. Catalog project cloned from git
6. supporting dependicies using pip3 to install

### Server Configuration Update and Upgrade
Commands used:
1. sudo `apt-get update`
2. `sudo apt-get upgrade`


### Update timezone to UTC

Commands used:
1. `sudo timedateactl set timezone UTC`

### ADD grader as a user

Commands used:
1. `sudo adduser grader`
2. Give the grader name and password
3. `sudo usermod -aG sudo grader ` # this gives the grader sudo access

### Add an SSH key for the grader

There are two way to do this. 
1. You can use the ssh-keygen command on your local computer. 
After you will create an .ssh directory and within the directory a authorized_keys file
where all of the grader's authorized keys will be stored.

2. Add the public key to the authorized_keys file stored at `/home/grader/.ssh/authorized_keys` 

### Next configure and enable the uncomplicated firewall(ufw)

1. `sudo apt-get install ufw` # install ufw 
2. `sudo ufw allow 2200` # open 2200 port  
3. `sudo deny 22` # deny port 22
4. `sudo allow ufw 123` #open port 123
5. `sudo allow ufw http` #open port 80
6. `sudo ufw enable` # complete changes to the port
7. `sudo ufw status` # to check the status of the firewall


### Modify the sshd_config file to only allow ssh into the 2200 port

* `cd /etc/ssh`
* ` sudo nano sshd_config` # edit line 5 so that it reads Port 2200 instead of 22
* Also change PermitRootLogin to 'no' instead of 'prohibit-password' 

### Install stack for the project Apache2 WSGI Python3 Postgresql

1. `sudo apt-get install apache2`
2. `sudo apt-get install libapache2-mod-wsgi-py3`
3. `sudo apt-get install python3`
4. `sudo apt-get install postgresql`
5. `sudo apt-get install git` # to clone project from repository

### Create WSGI conf file

1. `sudo mkdir /var/www/catalog` # create directory for the project files to live to live
2. `cd /var/www/catalog`
3. `sudo nano catalog.wsgi` # create wsgi file and add the following:
```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/catalog/")
sys.path.insert(1, "/var/www/catalog/")

from catalog import app as application
application.secret_key = "secretKey"
```


### Next step is to create calalog.conf file  for WSGI
1. `cd /etc/apache2/sites-available`
2. `sudo nano catalog.conf` # create configuration file
3. add the following
```
<VirtualHost *:80>
   ServerName 34.238.183.5
   ServerAdmin <name>@email.com
   WSGIDaemonProcess catalog user=ubuntu group=ubuntu threads=2
   WSGIScriptAlias / /var/www/catalog_project/catalog.wsgi
   DocumentRoot /var/www/catalog_project/catalog
   <Directory /var/www/catalog_project/>
     WSGIProcessGroup catalog_project
     WSGIApplicationGroup %{GLOBAL}
     <IfVersion < 2.4>
        Order allow,deny
        Allow from all
     </IfVersion>
     <IfVersion >= 2.4>
        Require all granted
      </IfVersion>
   </Directory>
   Alias "/static/" "/var/www/catalog_project/static/"
   <Directory /var/www/catalog_project/static/>
     <IfVersion < 2.4>
        Order allow,deny
        Allow from all
     </IfVersion>
     <IfVersion >= 2.4>
       Require all granted
        </IfVersion>
     </Directory>
     ErrorLog ${APACHE_LOG_DIR}/error.log
     LogLevel info
     CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  
```

#### Next we want to install all requirements for the catalog app

1. `pip3 freeze` #where the project is to see all if the dependecies
2. save this to requirements.txt file to so you can read these files to install
3. `pip3 install -r requiremets.txt` uploads all the required dependencies


**Enable catalog.conf**
```
grader@ip-172-26-10-175:/etc/apache2/sites-available$ sudo a2ensite catalog.conf
Site catalog already enabled
```


### Third party resources

1. [Flask deployment domentation](http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/#creating-a-wsgi-file)
2. [Digital Ocean's deploy Flask app](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)


