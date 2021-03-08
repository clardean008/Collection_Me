## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](Diagrams/Azure_Virtual_Network_Week_13_P1.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _Yaml__ file may be used to install only certain pieces of it, such as Filebeat.

  - pentest.yml 
  - install-elk.yml
  - filebeat-playbook.yml
  - metricbeat-playbook.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
 Load balancing ensures that the application will be highly avalible, in addition to restricting _access_ to the network.
-What aspect of security do load balancers protect? load balancers protect organzations against distributed denial-of-service(DDoS) attacks. The load balencer does this by shifting attach traffic  from the companies server to a public cloud servier.

-What is the advantage of a jump box?_ a Jump box biggest advantages is that its a single point of access into the network and it is secure. All admin people use the jump box to login to the  network before performing any administrive tasks. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _log files_ and system _operating system_. 

- What does Filebeat watch for?_ Filebeats watches for log files of specified data, then ships them to the ELK stack for analysis. In an ELk stack filebeat plays the role of the logging agent, it is installed on the machines generating the log files. ref https://logz.io/blog/filebeat-tutorial/

- What does Metricbeat record?_ Metricbeat recordes metrics from the operating system and from servcie running on your server. It then sends the logs to the ELk stack for anysist.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator] to add/remove values from the table_.

| Name     | Function     | IP Address | Operating System   |
|----------|--------------|------------|--------------------|
| Jump Box | Gateway      | 10.0.0.4   | Linux Ubuntu 18.04 |
| Web-1    | Web -Server  | 10.0.0.5   | Linux Ubuntu 18.04 |
| Web-2    | Web - Server | 10.0.0.6   | Linux Ubuntu 18.04 |
| Web-3    | Web - Server | 10.0.0.7   | Linux Ubuntu 18.04 |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the _load balencer_ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Load Balencer 13.75.173.189 on Port 80 

Machines within the network can only be accessed by _SSH_.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_ I only allowed  the jump-box VM to ssh to the elk server VM from IP 10.0.0.4  

A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible | Allowed IP Addresses |
|-------------|---------------------|----------------------|
| Home to VN  | Yes                 | Home IP address     |
| Web to VN   | Yes                 | Home IP address     |
| ssh From JB | Yes                 | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?_ It saves the administrative team a lot of time, only having to write a few Playbooks to install everything on the all new VM or servers. If they didnt automate the configuration then the administrative team would have to install all software one after the other on all new servers. 

The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

- Get Docker.io
- Get Python3-pip
- install docker
- increase virtual memory
- Download and launch a docker elk container
- enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![docker ps](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring_
- Web-1 DVWA 10.0.0.5
- Web-2 DVWA 10.0.0.6
- Web-3 DVWA 10.0.0.7

We have installed the following Beats on these machines:
-  Specify which Beats you successfully installed_ Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.- Filebeat collects the following data syslogs/sudo commands/SSH logins/New users and groups and sends it to the  ELK stack to be analysed I would expect to see ssh logins and sudo commands. ref to 
![Filebeat running](Image/Filebeats.png)

- Filemetrics collects the following data CPU usage / Memory usage / Network IO and I would expect to see CPU, memory and network usage running but nothing pushing these system hardwares to hard. ref to 
![Metricneat running](Image/Metricbeats.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _filebeat-config.yml_ file to _/etc/filebeat/filebeat.yml
- Update the _hosts_ file to include...
[webservers]                                                                                          10.0.0.5 ansible_python_interpreter=/usr/bin/python3                            
10.0.0.6 ansible_python_interpreter=/usr/bin/python3 
10.0.0.7 ansible_python_interpreter=/usr/bin/python3 
                                                                                                                                                                                           [elk]                                                                                                 10.1.0.4 ansible_python_interpreter=/usr/bin/python3  

- Run the playbook, and navigate to _Web-1 DVWA_ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- Which file is the playbook? filebeat-config.yml
- Where do you copy it?_ /etc/filebeat/filebeat.yml
- Which file do you update to make Ansible run the playbook on a specific machine? The Hosts file
- How do I specify which machine to install the ELK server on versus which to install Filebeat on? in the .yml file on the hosts: line you specify the machine you want to install the software on ie hosts: weservers or hosts: elk 
- _Which URL do you navigate to in order to check that the ELK server is running? http://20.198.217.11:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

ssh to jump-box                          ( shh into jump box)
sudo docker container list -a            ( lists all container on jump box)
sudo docker start great_mahaviar         ( start docker container great_mahaviar)
sudo docker ps                           ( List docker container that are running)
sudo docker attach great_mahaviar        ( attch container great_mahaviar)
cd /etc/ansible                          ( Change director)
ls -la                                   ( List all file in dir)
nano hosts                               ( add weservers and elk machines)
nano ansible.cfg                         ( add user name)
nano pentest.yml                         ( write YAML file to install docker and DVWA on webservers)
ansible-playbook pentest.yml             ( run playbook to install docker and dvwa container)
ssh to Web-1                             ( ssh to Web-1)
sudo docker ps                           ( check to make sure the container has installed)
exit                                     ( Exit Web-1 return to jump-box)
nano install-elk.yml                     ( write yaml file to install docker and elk stack on elk VM)
ansible-playbook install-elk.yml         ( run playbook to install docker and the elk stack)
ssh to elk machine                       ( ssh to elk VM )
sudo docker ps                           ( check the the elk container is install and running)
exit                                     ( Exit elk vm and return  to jumpbox)
nano filebeat-config.yml                 ( create filebeat config file )
nano filebeat-playbook.yml               ( Write yaml file to install filebeat on web servers)
ansible-playbook filebeat-playbook.yml   ( run plaubook to install filebeat on webservers)
http://(ELK-Public IP:5601/app/kibana    ( Open a web broswer and Navigate to logs/ add log data / system logs click on check data click on system logs dashboard )
nano Metricbeat-config.yml               ( create metricbeat config file)
nano Metricbeats-playbook.yml            ( write yaml file to install metric beats on webservers)
ansible-playbook metricbeat-playbook.yml ( run playbook to install metricbeat on webservers) 
http://(ELK-Public IP:5601/app/kibana    ( Open a web broswer and navigate to metric data / docker metrics / click on check data, click on Docker Metric dashboard)


