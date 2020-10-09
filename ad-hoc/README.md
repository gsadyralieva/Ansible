Ad hoc commands are commands which can be run individually to perform quick functions. These commands need not be performed later.

For example, you have to reboot all your company servers. For this, you will run the Adhoc commands from ‘/usr/bin/ansible’.

These ad-hoc commands are not used for configuration management and deployment, because these commands are of one time usage.

ansible-playbook is used for configuration management and deployment.

Parallelism and Shell Commands
Reboot your company server in 12 parallel forks at time. For this, we need to set up SSHagent for connection.
```
$ ssh-agent bash 
$ ssh-add ~/.ssh/id_rsa 
```
To run reboot for all your company servers in a group, 'abc', in 12 parallel forks −
```
$ Ansible abc -a "/sbin/reboot" -f 12
```
By default, Ansible will run the above Ad-hoc commands form current user account. If you want to change this behavior, you will have to pass the username in Ad-hoc commands as follows −
```
$ Ansible abc -a "/sbin/reboot" -f 12 -u username
```
File Transfer
You can use the Ad-hoc commands for doing SCP (Secure Copy Protocol) lots of files in parallel on multiple machines.

Transferring file to many servers/machines
```
$ Ansible abc -m copy -a "src = /etc/yum.conf dest = /tmp/yum.conf"
```
Creating new directory
```
$ Ansible abc -m file -a "dest = /path/user1/new mode = 777 owner = user1 group = user1 state = directory" 
```
Deleting whole directory and files
```
$ Ansible abc -m file -a "dest = /path/user1/new state = absent"
```
Managing Packages
The Ad-hoc commands are available for yum and apt. Following are some Ad-hoc commands using yum.

The following command checks if yum package is installed or not, but does not update it.
```
$ Ansible abc -m yum -a "name = demo-tomcat-1 state = present"
```
The following command check the package is not installed.

$ Ansible abc -m yum -a "name = demo-tomcat-1 state = absent" 
The following command checks the latest version of package is installed.

$ Ansible abc -m yum -a "name = demo-tomcat-1 state = latest" 
Gathering Facts
Facts can be used for implementing conditional statements in playbook. You can find adhoc information of all your facts through the following Ad-hoc command −

$ Ansible all -m setup 
