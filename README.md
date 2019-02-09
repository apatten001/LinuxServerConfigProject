# LinuxServerConfigProject

### IP address and SSH port
1. **Public IP address:** 34.233.120.191 **SSH port:** 2200
2. **URL of the hosted web app:** 


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
2. `sudo apt-get dist-upgrade`


### Update timezone to UTC

Commands used:
1. `sudo timedateactl set timezone UTC`

### ADD grader as a user

Commands used:
1. `sudo adduser grader`

### Add an SSH key for the grader

There are two way to do this. 
1. You can use the ssh-keygen command on your local computer. 
After you will create an .ssh directory and within the directory a authorized_keys file
where all of the grader's authorized keys will be stored.

2. Add the private key to the authorized_keys file stored at `/home/grader/.ssh/authorized_keys` 

### Next configure and enable the uncomplicated firewall(ufw)

1. `sudo apt-get ufw` # install ufw 
2. `sudo ufw allow 2200`
3. `sudo allow www`
4. `sudo allow ufw 123`
5. `sudo allow ufw http`
6. `sudo ufw enable`
7. `sudo ufw status` # to check the status of the firewall


### Modify the sshd_config file to only allow ssh into the 2200 port

* `cd /etc/ssh`
* ` sudo nano sshd_config` # edit line 5 so that it reads Port 2200 instead of 22
* Also change PermitRootLogin to 'no' instead of 'prohibit-password' 
