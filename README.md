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


2. **Add "grader" and Set Permissions**
  1. While still logged in securely to server, in terminal ran command `sudo adduser grader`
  2. Gave grader sudo permissions by using command `sudo visudo` and adding line `grader    ALL=(ALL:ALL) ALL` under "#User privilege specification"

3. **Update All Currently Installed Packages**

In terminal, used these commands:
  1. `sudo apt-get update`
  2. `sudo apt-get upgrade`
  3. `sudo apt-get autoremove`
  
4. **Change the SSH port from 22 to 2200**
  1. In terminal, ran command `sudo nano /etc/ssh/sshd_config`
  2. Changed line "Port 22" to "Port 2200"

5. **Configure the Uncomplicated Firewall (UFW)**

Allowed incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123) by running commands:
  1. `sudo ufw default deny incoming`
  2. `sudo ufw default allow outgoing`
  3. `sudo ufw allow 2200/tcp`
  4. `sudo ufw allow 80/tcp`
  5. `sudo ufw allow 123/tcp`
  6. `sudo ufw enable`
  7. double checked configuration `sudo ufw status`
  8. restarted service: `sudo service ssh restart`

  
##Resources##
[How To Add, Delete, and Grant Sudo Privileges to Users on a Debian VPS](https://www.digitalocean.com/community/tutorials/how-to-add-delete-and-grant-sudo-privileges-to-users-on-a-debian-vps)

[How to change the ssh port number](http://www.2daygeek.com/how-to-change-the-ssh-port-number/)

