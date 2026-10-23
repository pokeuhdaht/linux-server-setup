<h1>Best Practices and Commands for Setting Up Your Server</h1>

<h2><u>IAM / Accounts</u></h2>
Root Account  - disabled. We do not want to use a root account, rather, we'll use an admin account for elevated tasks.
Admin Account - used for updating/installing applications, as well as configuring firewall rules.
Docker User Account - used for setting up docker resources.
Local User Account - used for creating other resources (eg. scripting, programming, etc)


For the above, we will create groups. In the event we need to create a new account with similar permissions, we can simply add create the user and add them to the necessary group.


Disable Root Account
===================
PermitRootLogin no

Admin/sudo Account
===================
usermod -aG sudo [username]
newgrp sudo

Docker User Account (The "docker" group is created when docker engine is installed.)
===================
sudo usermod -aG docker [user]
newgrp docker

Local User Account
===================
sudo groupadd [groupname]
sudo chgrp [groupname] /path/to/target       # This is the path to the file/directory   .... adding -R will allow the changes to be made recursively

sudo chmod 770 /path/to/target               # This will give Read, Write, & Execute permissions to the given file/directory to the (1) Owner (2) Group 



