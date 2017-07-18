---------#DEMO_ANSIBLE_1---------------------------------

This is a very simple project.
In my lab I use two vMX and Ubuntu Server with ansible and jsnapy.

root@ubuntu#ansible --version

ansible 2.3.1.0

python version = 2.7.12 (default, Jul 18 2016, 15:00:48) [GCC 4.6.3]

root@ubuntu#jsnapy --version

JSNAPy version: 1.2.1.dev0

root@ubuntu#ansible-galaxy --version

ansible-galaxy 2.3.1.0

In my lab I had two vMX with the next versions:
14.1R1.10 and 16.2R2.8.
In both equipments I had the next configuration at the services hierarchy.

First I checked if there any issue in the netconf connetion between the server and routers.
From Serveer:
root@ubuntu$ ssh -p 830 -s 192.168.111.3 netconf

Password:

<-- No zombies were killed during the creation of this user interface --> 

<-- user root, class super-user --> 

<--hello xmlns="urn:ietf:params:xml:ns:netconf:base:1.0--> 

  <capabilities> 
  
....... omited output 

  </capabilities> 
  
  <session-id>4665</session-id> 
  
</hello> 



In the vMX:
@vMX1> show system connections | match 830    
tcp4       0      0  192.168.111.3.830                             192.168.111.130.34516                         ESTABLISHED
tcp4       0      0  *.830                                         *.*                                           LISTEN

Then, I created two files called hosts and ansible.cnf.
The file hosts contain the hosts information and ansible.cnf has the local configuration of Ansible.

After I created a folder called group_vars with file called all.yml.
The file called all.yml has the login information for all equipments.
In this file the attribute { junos_host} is taken from the file "hosts".

The next folder that I created was the folder called host_vars where I created two files with the specific information for each vMX.(vMX1.yml and vMX2.yml)

Then I created another folder called Template, and in this folder I created a file called lo0.j2 (jinaja2). This file contain the configuration that I need sent to the vMX in jinja2 format. For call the loopback IP, I must to said in the file the path from where it must take the information.
For example:
 {{host.loopback.ip}}/{{host.loopback.mask}} For this the path, it search in the folder /host_vars, and in each file it serch first the label host, then serach loopbacklabel  and the search IP.
 
 After, I created the playbook called playbook1.yml, this playbook has five task.
 The task called "Creacion directorio" create a folder called "DEMO_ANSIBLE" with files that have the name of each host.
 The task called "Archivos de Configuracion", take the file lo0.j2 and for each host create and file with .cnf extension in the path defined like DEMO_ANSIBLE/{{inventory_hostname}}/{{inventory_hostname}}.conf.
 The third task take each file with .conf extension and the apply in the each equipment (vMX).
 The task called "check" uses jsnapy to check the status of the interface loopback that it created before. It uses the file called test_loopback.yml for make the test.
 Finally the last task is called "Check" it test the results, it makes a search in the register called debug_jsnapy (It was define in the task "check") and search for key word total_failed.
 The file called test_loopback.yml have the command that it excute in the equipments, in this case is "show interface terse".
 
 To excute the playbook the command is:  ansible-playbook playbook1.yml 
 
 
 
 
 
 
