## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[https://github.com/GregVelarde/GitHub-Project/blob/main/Diagram/Project1_Diagram.PNG]



These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-filebeat.yml file may be used to install only certain pieces of it, such as Filebeat.

  ---
  - name: installing and launching filebeat
    hosts: webserver
    become: yes
    tasks:

  - name: download filebeat deb
    command: curl -L -O
    https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: service filebeat start

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

- Load balancer protect application from malicious attacker, against unauthorized access, and protects against denial-of-service (DDoS)

- Jump box is exposed to the public internet and sets in front of other machines that are not exposed to the public internet which controls access to other machines by allowing connection from specific IP addreses and forwarding to those machines.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.

- Filebeat monitors the log files or the location that are specified, collects log events, and forwards it to either Elasticsearch or Logstash for indexing

- Metric collects metrics from the operating system and from services running on the server.

The configuration details of each machine may be found below.

| Name                 | Function   | IP Address | Operating Sytem |
|----------------------|------------|------------|-----------------|
| Jump-Box-Provisioner | Gateway    | 10.0.0.4   | Linux           |
| ELKServer            | Analytics  | 10.0.1.4   | Linux           |
| Web-1                | Web Server | 10.0.0.8   | Linux           |
| Web-2                | Web Server | 10.0.0.9   | Linux           |
| Web-2                | Web Server | 10.0.0.11  | Linux           |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- 172.124.234.97


Machines within the network can only be accessed by SSH.

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Address |
|----------------------|---------------------|--------------------|
| Jump-Box-Provisioner |         Yes         | 172.124.234.97     |
|                      |                     |                    |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible is agentless and easy to use configuration deployment tool.

The playbook implements the following tasks:

- Increase Virtual Memory
- Install docker.io
- Install Python3-pip
- Install Python Docker
- Download and launch a docker web container


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.




### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- 10.0.0.8
- 10.0.0.9
- 10.0.0.11

We have installed the following Beats on these machines:

- filebeat-7.4.0-amd64.deb
- metricbeat-7.4.0-amd64.deb

These Beats allow us to collect the following information from each machine:

- Filebeat Collect log files, and events; whereas, metricbeat collects metrics from the operating system and from services running on the server.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the beat configuration file to web servers.
- Update the hosts file to include the node IP Address
  [webservers]
  10.0.0.8 ansible_python_interpreter=/usr/bin/python3
  10.0.0.9 ansible_python_interpreter=/usr/bin/python3
  10.0.0.11 ansible_python_interpreter=/usr/bin/python3

  [elk]
  10.1.0.4 ansible_python_interpreter=/usr/bin/python3 

- Run the playbook, and navigate to ELK Module Status and click Check Data and then click Verify Incoming Data to check that the installation worked as expected.

- Filebeat-config.yml and metricbeat-config.yml playbook is copied to web server /etc/filebeat for filebeat and /etc/metribeat/ for metribeat.
- Update host file to include specific IP address of the webserver to make Ansible run of those specific machines. Playbook hosts must be specified which machine to install on.
- In order to check that the ELK is running navigate to URL http://20.75.50.49:5601/app/kibana.

Command to download playbook is to use curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat
