<h1>Best Practices and Commands for Setting Up Your Server</h1>

<h2>IAM / Accounts</h2>

Root Account  - disabled. We do not want to use a root account, rather, we'll use an admin account for elevated tasks.
Admin Account - used for updating/installing applications, as well as configuring firewall rules.
Docker User Account - used for setting up docker resources.
Local User Account - used for creating other resources (eg. scripting, programming, etc)


For the above, we will create groups. In the event we need to create a new account with similar permissions, we can simply add create the user and add them to the necessary group.


<h3><ins>Disable Root Account</ins></h3>
You'll want to disable the root account as well as disable root SSH login (shown later).

<ins>Locking the account:</ins>
sudo passwd -l root

<ins>Edit Root Shell to non-login</ins>
sudo usermod -s /sbin/nologin root

<h3><ins>Admin/sudo Account</ins></h3>
usermod -aG sudo [username]
newgrp sudo
<h3><ins>Docker User Account</ins></h3>
(The "docker" group is created when docker engine is installed.)

sudo usermod -aG docker [user]
newgrp docker

<h3><ins>Local User Account</ins></h3>
sudo groupadd [groupname]
sudo chgrp [groupname] /path/to/target

(This is the path to the file/directory .... adding -R will allow the changes to be made recursively)

sudo chmod 770 /path/to/target
This will give Read, Write, & Execute permissions to the given file/directory to the (1) Owner (2) Group
<h2>SSH Securely</h2>
<h3><ins>Generate & Add Key Pair for SSH Login</ins></h3>
Genereate Key Piar:
ssh-keygen -t ed25519 -C "your_email@example.com"
This will create two keys:
id_ed25519 & id_ed25519.pub

id_ed25519.pub needs to be copied to the Server
ssh-copy-id username@server_ip_address

If using windows you may have to do this manually.
Copy the public key to ~/.ssh/authorized_keys

Ensure that the folders have the security folders have the proper permissions:
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

<h3><ins>Disable root SSH and Password Authentication</ins></h3>

Inside the file: /etc/ssh/sshd_config
add the following lines:

PermitRootLogin no

PasswordAuthentication no


<h2>Firewall & Network</h2>

<h3><ins>Install Fail2Ban</ins></h3>
Fail2Ban is an intrusion prevention software that bans the IP of machines attempting brute-force login requests.
Install Fail2Ban by performing the following commands:

  sudo apt update
  
  sudo apt install fail2ban

Enable on boot by performing this command:

  sudo systemctl enable fail2ban


<h3><ins>Automatic System Upgrades</ins></h3>
unattended-upgrades works well, however I would like to run a cron job that checks for upgrade and sends a Webhook message via an app like discord.

<h3><ins>Trim Unused Packages</ins></h3>


