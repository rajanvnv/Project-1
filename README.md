## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.
Note: https://github.com/rajanvnv/Project-1/blob/main/Diagrams/Project%201%20Diagram.pdf


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml and config file may be used to install only certain pieces of it, such as Filebeat.

## Playbook file : 
https://github.com/rajanvnv/Project-1/blob/main/Ansible/install-elk.yml.txt


## Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.

What aspect of security do load balancers protect? 
Availability, Web Traffic, Web Security

What is the advantage of a jump box?
Automation, Security, Network Segmentation, Access Control

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

What does Filebeat watch for?
Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing. When you start Filebeat, it starts one or more inputs that look in the locations you’ve specified for log data. For each log that Filebeat locates, Filebeat starts a harvester. Each harvester reads a single log for new content and sends the new log data to libbeat, which aggregates the events and sends the aggregated data to the output that you’ve configured for Filebeat.

What does Metricbeat record?
Metricbeat helps you monitor your servers and the services they host by collecting metrics from the operating system and services.

The configuration details of each machine may be found below.
Note: Use the Markdown Table Generator to add/remove values from the table.


|Name                   |Function       |IP Address             |Operating system  |
|-----------------------|---------------|-----------------------|------------------|
|Jump box provisioner   |Gateway        |10.0.0.4/52.188.212.208|Linux             |   
|Web1                   |Webserver      |10.0.0.5               |Linux             |
|Web2                   |Webserver      |10.0.0.6               |Linux             |
|ELK                    |ELK server     |10.1.0.4               |Linux             |
|Load Balancer          |Load Balancer  |Static External IP     |Linux             |
|Workstation            |Access Control |Public IP              |Linux             |


## Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the ELK server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

 - Workstation Public IP through TCP 5601.

Machines within the network can only be accessed by Workstation and Jump-Box-Provisioner. Which machine did you allow to access your ELK VM? What was its IP address?

 - Jump-Box-Provisioner IP : 10.0.0.4 via SSH port 22
 - Workstation Public IP via port TCP 5601

A summary of the access policies in place can be found in the table below.

|Name                   |Publicly Accessible|Allowed IP Address                  |
|-----------------------|-------------------|------------------------------------|
|Jump box provisioner   |No                 |Workstation Public IP on SSH 22     |
|Web1                   |No                 |10.0.0.5 on SSH 22                  |
|Web2                   |No                 |10.0.0.6 on SSH 22                  |
|ELK                    |No                 |Workstation Public IP using TCP 5601|
|Load Balancer          |No                 |Workstation Public IP on HTTP 80    |


## Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible lets you quickly and easily deploy multitier apps. You won't need to write custom code to automate your systems; you list the tasks required to be done by writing a playbook, and Ansible will figure out how to get your systems to the state you want them to be in.

What is the main advantage of automating configuration with Ansible?
The following are the advantages of using ansible:
- Simple: Ansible uses a simple syntax written in YAML called playbooks. YAML is a human-readable data serialization language. It is extraordinarily simple. 

- Agentless: Finally, Ansible is completely agentless. There are no agents/software or additional firewall ports that you need to install on the  client systems or hosts which   you want to automate. 

- Powerful & Flexible: Ansible has powerful features that can enable you to model even the most complex IT workflows. 

- Efficient: No extra software on your servers means more resources for your applications. Also, since Ansible modules work via JSON, Ansible is extensible with modules written in a programming language you already know. 

The playbook implements the following tasks:

Specify a different group of machines as well as a different remote user
  - name: Config elk VM with Docker
    hosts: elk
    remote_user: sysadmin
    become: true
    tasks:

Increase System Memory :
 - name: Use more memory
  sysctl:
    name: vm.max_map_count
    value: '262144'
    state: present
    reload: yes

Install the following services:
   `docker.io`
   `python3-pip`
   `docker`

Launching and Exposing the container with these published ports:
 `5601:5601` 
 `9200:9200`
 `5044:5044`

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
Note: The following image link needs to be updated. Replace docker_ps_output.png with the name of your screenshot image file.


## Target Machines & Beats
This ELK server is configured to monitor the following machines:

Web1 : 10.0.0.5
Web2 : 10.0.0.6

We have installed the following Beats on these machines:

ELK Server, Web1 and Web2

The ELK Stack Installed are: FileBeat and MetricBeat
These Beats allow us to collect the following information from each machine:

Filebeat: log events
Metricbeat: metrics and system statistics

## Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

For ELK VM Configuration:

Copy the Ansible ELK Installation and VM Configuration
Run the playbook using this command : ansible-playbook install-elk.yml
For FILEBEAT:

Download Filebeat playbook usng this command:
curl -L -O https://github.com/rajanvnv/Project-1/blob/main/Ansible/filebeat-config.yml.txt > /etc/ansible/files/filebeat-config.yml
Copy the '/etc/ansible/files/filebeat-config.yml' file to '/etc/filebeat/filebeat-playbook.yml'
Update the filebeat-playbook.yml file to include installer
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
Update the filebeat-config.yml file root@c1e0a059c0b0:/etc/ansible/files# nano filebeat-config.yml
output.elasticsearch:
  #Array of hosts to connect to.
 hosts: ["10.1.0.4:9200"]
  username: "elastic"
  password: "changeme” 

 setup.kibana:
  host: "10.1.0.4:5601"
Run the playbook using this command ansible-playbook filebeat-playbook.yml and navigate to Kibana > Logs : Add log data > System logs > 5:Module Status > Check data to check that the installation worked as expected.
For METRICBEAT:

Download Metricbeat playbook using this command:
curl -L -O https://github.com/rajanvnv/Project-1/blob/main/Ansible/metricbeat-config.yml.txt > /etc/ansible/files/metricbeat-config.yml
Copy the /etc/ansible/files/metricbeat file to /etc/metricbeat/metricbeat-playbook.yml
Update the filebeat-playbook.yml file to include installer
curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb
Update the metricbeat file rename to metricbeat-config.yml
root@c1e0a059c0b0:/etc/ansible/files# nano metricbeat-config.yml

output.elasticsearch:
#Array of hosts to connect to.
hosts: ["10.1.0.4:9200"]
  username: "elastic"
  password: "changeme"

setup.kibana:
  host: "10.1.0.4:5601"
Run the playbook, (ansible-playbook metricbeat-playbook.yml) and navigate to Kibana > Add Metric Data > Docker Metrics > Module Status to check that the installation worked as expected.

## ADDITONAL NOTES:

How to get Filebeat installer :
Login to Kibana > Logs : Add log data > System logs > DEB > Getting started
Copy: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

How to get the Metricbeat installer:
Login to Kibana > Add Metric Data > Docker Metrics > DEB > Getting Started
Copy: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb

Which file is the playbook? 

Answer : For the ANSIBLE : https://github.com/rajanvnv/Project-1/blob/main/Ansible/install-elk.yml.txt

Answer : For FILEBEAT: https://github.com/rajanvnv/Project-1/blob/main/Ansible/filebeat.yml.txt

Answer: For METRICBEAT: https://github.com/rajanvnv/Project-1/blob/main/Ansible/metricbeat.yml.txt
