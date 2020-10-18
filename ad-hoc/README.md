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
```
$ Ansible abc -m yum -a "name = demo-tomcat-1 state = absent" 
```
The following command checks the latest version of package is installed.
```
$ Ansible abc -m yum -a "name = demo-tomcat-1 state = latest" 
```
Gathering Facts
Facts can be used for implementing conditional statements in playbook. You can find adhoc information of all your facts through the following Ad-hoc command −
```
$ Ansible all -m setup 
```

Tasks:
# Check free memory  of Hosts group with ansible ad-hoc command
```
ansible -i testhosts hosts   -m  shell -a "free -m"
```

# Check disk available of all-servers group with ansible ad-hoc command
```
ansible -i testhosts  localhost all-servers   -m  shell -a "df -h"
```

 3. Install top package only  on the localhost , using all-servers  group with ansible ad-hoc command

#Install top package only  on the localhost , using all-servers  group with ansible ad-hoc comm
```
ansible -i testhosts  all-servers   -m  yum -a "name=htop state=latest" --limit localhost
```

#move testfiles from /tmp folder to /tmp/destination folder  , using ansible ad-hoc command
```
ansible -i testhosts localhost -m command -a “mv /tmp/testfile1 /tmp/destination”
```
#  change the perm  of /tmp/destination
```
ansible -i testhosts all-servers  -m file -a "path=/tmp/destination/testfile1 mode='777'"
```
# setup module 
```
ansible -i testhosts all -m setup -a "gather_subset=all"
```
# filter setup module
```
ansible -i testhosts all -m setup -a "filter=ansible_distribution"
```
filter distribution major version of os distribution 
```
ansible -i testhosts all -m setup -a "filter=ansible_distribution_major_version"

ansible -i testhosts all -m setup -a "filter=ansible_distribution_*"
```


Run ansible ad-hoc to check the hostnames of the development group servers
```
$ ansible -i inventory/devinventory development -m setup -a "filter=ansible_host*"
$ ansible -i inventory/devinventory development -m setup -a "filter=ansible_hostname”
```

Check which servers are running apache. Ad-hoc command. 
```
$ ansible -i inventory/dev.yaml  devservers -m shell -a "systemctl status httpd.servic "
$ ansible -i inventory/dev.yaml all -m command -a "httpd --version"
```

