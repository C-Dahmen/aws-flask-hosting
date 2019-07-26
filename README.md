# Udacity Project 3 Full Stack  Nanodegree

This project takes a baseline installation of a Linux distribution hosted on an AWS lightsail server and installs updates, secures the server from a number of attack vectors, and installs/configures web and database servers, preparing it to host a Flask web application.

##  Usage

i. Public IP address: 100.27.46.102.  SSH port: 2200

ii. Complete URL to the hosted web application: http://100.27.46.102

iii. The following configuration changes were made:

  Updated all available packages with:
  ``` bash
  sudo apt-get packages
  ```
  Upgraded all available packages with:
  ```bash
  sudo apt-get upgrade
  ```
  Changed SSH port from default port 22 to 2200 using:
  ```bash
  sudo nano /etc/ssh/sshd_config
  ```
  and changing the port from:
  ```bash
  Port 22
  ```  
  to
  ```bash
  Port 2200
  ```
  Added port 2200 to the list of allowed ports in lightsail.aws.amazon > networking > firewall.
  Created user called grader using
   ```bash
   sudo adduser grader
   ```
  Granted the grader user sudo access by cd into /etc/sudoers.d:
  ```bash
    sudo su -  grader
    sudo mv 90-cloud-init-user grader
  ```
  And then updating the grader user changing the line ubuntu ALL=(ALL) NOPASSWD:ALL to grader ALL=(ALL) NOPASSWD:ALL. Creating a new keypair for that user by creating a new .ssh folder and creating a new authorized_keys file in the .ssh directory
   ``` bash
    touch .ssh/authorized_keys
  ```
  setting the permissions for each one using
  ``` bash
  chmod 700 .ssh
  ```
  and
  ``` bash
   chmod 600 .ssh/authorized_keys
   ```
  Creating the keypair in my local machine using ssh-keygen, installing the .pub public key generated by ssh-keygen into the previously created .ssh/authorized_keys
  Forced key based authentication and disabled root login by modifying the following lines in the /etc/ssh/sshd_config file:
  ``` bash
    Password Authentication no
    Permit Root Login no
  ```
  Set up the firewall by:
  ``` bash
    sudo ufw default deny incoming
    sudo ufw default allow outgoing
    sudo ufw allow 2200/tcp
    sudo ufw allow www
    sudo ufw allow ntp
    sudo ufw enable
  ```
  Installed  apache:
  ``` bash
  sudo apt-get install apache2   
  ```
  Installed mod with:
  ``` bash
  sudo apt-get install libapache2-mod-wsgi
  ```
  Installed git with
  ``` bash
  sudo apt-get install git
  ```
  Configured apache to serve the mod-wsgi app by modifying the 000-default.conf file, adding the DocumentRoot /var/www/html/myapp.
  Added a myapp.wsgi file in the myapp folder that called the views.py application.
  Created a virtual env within the myapp file using
  ``` bash
  sudo pip install virtualenv
  ```
  Activated the virtualenv using
  ``` bash
   source venv/bin/activate
  ```
  Installed necessary packages for app to run using pip.
  Added a database folder and changed permissions for both the database and the database folder for the default user www-data to be able to access and write to the database.

iv.Third party resources:

 * https://aws.amazon.com/premiumsupport/knowledge-center/new-user-accounts-linux-instance/
 * https://flask.palletsprojects.com/en/1.1.x/patterns/sqlite3/
 * https://modwsgi.readthedocs.io/en/develop/
 * https://flask.palletsprojects.com/en/1.1.x/deploying/mod_wsgi/#creating-a-wsgi-file
 * https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
