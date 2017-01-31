# Linux-Server-Configuration

##Server Information##
**IP Address:** 52.25.83.208

**SSH port:** 2200


##Web Application URL##
http://ec2-52-25-83-208.us-west-2.compute.amazonaws.com/


##Configuration Summary##
1. **Launch Virtual Machine**
  1. Downloaded private key from Udacity account
  2. In terminal, moved downloaded file using command `mv ~/Downloads/udacity_key.rsa ~/.ssh/`
  3. Changed permission of moved file so owner can read and write file `chmod 600 ~/.ssh/udacity_key.rsa`
  4. Securely logged in to server `ssh -i ~/.ssh/udacity_key.rsa root@52.25.83.208`


2. **Add User and Set Permissions**
  1. While still logged in securely to server, in terminal ran command `sudo adduser grader`
  2. Gave grader sudo permissions by using command `sudo visudo` and adding line `grader    ALL=(ALL:ALL) ALL` under "#User privilege specification"
  3. Gave grader ssh access
    1. Created .ssh folder `sudo mkdir /home/grader/.ssh`
    2. Copy authorized_keys file to new folder `sudo cp ~/.ssh/authorized_keys /home/grader/.ssh/authorized_keys`
    3. Changed ownership of new folder `chown grader:grader /home/grader/.ssh`
    4. Changed permissions of new folder so grader can only read, write, and execute `chmod 700 /home/grader/.ssh`
    5. Changed ownership of copied file `chown grader:grader /home/grader/.ssh/authorized_keys`
    6. Changed permissions of copied file so grader can only read and write `chmod 600 /home/grader/.ssh/authorized_keys`

3. **Update All Currently Installed Packages**
  1. `sudo apt-get update`
  2. `sudo apt-get upgrade`
  3. `sudo apt-get autoremove`
  
4. **Change the SSH Port**
  1. In terminal, ran command `sudo nano /etc/ssh/sshd_config`
  2. Changed line "Port 22" to "Port 2200"

5. **Configure the Uncomplicated Firewall (UFW)**
  1. Denied all incoming connections `sudo ufw default deny incoming`
  2. Allowed all outgoing connections`sudo ufw default allow outgoing`
  3. Allowed incoming connections for SSH `sudo ufw allow 2200/tcp`
  4. Allowed incoming connections for HTTP `sudo ufw allow 80/tcp`
  5. Allowed incoming connections for NTP`sudo ufw allow 123/tcp`
  6. Turned on firewall `sudo ufw enable`
  7. Double checked configuration `sudo ufw status`
  8. Restarted service: `sudo service ssh restart`

6. **Configure the local timezone**
  1. Ran command `sudo dpkg-reconfigure tzdata`
  2. In menu that popped up, selected "None of the above" then "UTC"
  
7. **Install and Configure Apache to Serve a Python mod_wsgi Application**
  1. Installed Apache by running command `sudo apt-get install apache2`
  2. Installed mod-wsgi `sudo apt-get install libapache2-mod-wsgi`

8. **Install and Configure PostgreSQL**
  1. Installed postgresql `sudo apt-get install postgresql`
  2. Checked for no remote connections allowed `sudo nano /etc/postgresql/9.3/main/pg_hba.conf`
  3. Added user catalog and add database catalog with limited permissions by running these commands:
    1. `sudo su - postgres`
    2. `psql`
    3. `CREATE DATABASE catalog;`
    4. `CREATE USER catalog WITH PASSWORD 'password';`
    5. `GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;`
    6. `\q`
    7. `exit`
    
  
  
  `/etc/apache2/sites-enabled/000-default.conf`
  `sudo apache2ctl restart`
  
##Resources##
[How To Add, Delete, and Grant Sudo Privileges to Users on a Debian VPS](https://www.digitalocean.com/community/tutorials/how-to-add-delete-and-grant-sudo-privileges-to-users-on-a-debian-vps)

[How to change the ssh port number](http://www.2daygeek.com/how-to-change-the-ssh-port-number/)

[How do I change the timezone to UTC?](http://askubuntu.com/questions/117359/how-do-i-change-the-timezone-to-utc)

[“Permission denied (publickey) after adding “grader” user and changing ssh port](https://discussions.udacity.com/t/permission-denied-publickey-after-adding-grader-user-and-changing-ssh-port/207087)

[PostgreSQL add or create a user account and grant permission for database](https://www.cyberciti.biz/faq/howto-add-postgresql-user-account/)

