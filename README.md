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
  
7. ****

  
##Resources##
[How To Add, Delete, and Grant Sudo Privileges to Users on a Debian VPS](https://www.digitalocean.com/community/tutorials/how-to-add-delete-and-grant-sudo-privileges-to-users-on-a-debian-vps)

[How to change the ssh port number](http://www.2daygeek.com/how-to-change-the-ssh-port-number/)

[How do I change the timezone to UTC?](http://askubuntu.com/questions/117359/how-do-i-change-the-timezone-to-utc)

