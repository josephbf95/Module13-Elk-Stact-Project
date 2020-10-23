## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

- Images/Elk_Stack_Project.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - elk.yml.
  - filebeat-playbook.yml
  - filebeat-config.yml
  - metricbeat-setup.yml
  - metricbeat-config.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting connections to the network.
- Load Balancers protect availability of the service in the CIA Triad What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics.
- Filebeat watches for log files and creates a logstash
- Metricbeat records server and operating system metrics. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function  | IP Address | Operating System |
|----------|-----------|------------|------------------|
| Jump Box | Gateway   | 10.0.0.4   | Linux            |
| Web-1    | Web Server| 10.0.0.5   | Linux            |
| Web-2    | Web Server| 10.0.0.6   | Linux            |
| Web-3    | Web Server| 10.0.0.7   | Linux            |
| ELK-1    | Elk Stack | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Load Balancer machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- My Public IP

Machines within the network can only be accessed by the Jump Box Ansible Container.
- Jump Box Provisioner 10.0.0.4 Ansible Container

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses     |
|----------|---------------------|----------------------    |
| Jump Box | No                  | My public ip             |
| Web-1    | No                  | 10.0.0.4                 |
| Web-2    | No                  | 10.0.0.4                 |
| Web-3    | No                  | 10.0.0.4                 |
| ELK-1    | No                  | 10.0.0.4 My public ip    |
| Web-LB   | Yes                 | My public IP             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible is lightweight and simplifies the creation of an elk stack configuration using ansible playbooks.

The playbook implements the following tasks:
- Install docker.io, python3-pip, and docker python
- Increase virtual memory to 262144 for ELK-1 Machine
- Launch a Docker Web Container named elk using image sebp/elk: 761
- Include port mappings for ports 5601, 9200, and 5044

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

-Images/docker_ps_output.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.0.0.5
- Web-2: 10.0.0.6
- Web-3: 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat aggregate system logs such as syslog and auditd
- Metricbeat monitors system metric such as server uptime and cpu speed

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the elk.yml file to /etc/ansible.
- Update the /etc/ansible/host file to include elk host and specify ansible_python_interpreter=/usr/bin/python3 for elk machine's internal ip. 
- Run the playbook, and navigate to http://[your.ELK-VM.External.IP]:5601/app/kibana to check that the installation worked as expected.



_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

Setup Elk Stack:
- Update Ansbile Hosts: sudo nano /etc/ansible/hosts/
- Add elk server group and elk server internal ip
- Run elk.yml
- Start elk container to launch server's kibana page

Install filebeat and metricbeat:
- run curl to download https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat to Ansible directory as filebeat-config.yml
- Edit config on line 1106 and 1806 to include internal ip for elk server
- Save filebeat.yml to /etc/ansible/roles
- Run filebeat.yml
- Copy and paste from https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat to metricbeat-config yml file to /etc/Ansible/ directory
- Add internal ip to lines below output.elasticsearch and setup.kibana, same as with filebeat-config.yml
- Run metricbeat.yml

