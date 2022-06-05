# Elk-Stack-Project
Elk Stack Project for Cyber Security bootcamp 
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![image](https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Diagrams/elk_stack_network_map.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Elk Deployment file may be used to install only certain pieces of it, such as Filebeat.

[install-elk.yml](https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Ansible/install-elk.yml)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic/access to the network.
- What aspect of security do load balancers protect? Load balances help protect a network from a Denial-of-Service attack by allowing the extra traffic to move to a different server.
- What is the advantage of a jump box? A jump box forces all traffic to a single node; it allows SSH for added security and reduces traffic to a network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data logs and system traffic/access.
- What does Filebeat watch for? Filebeat watches for the specified log files and locations, forwarding them to Elasticsearch for indexing. 
- What does Metricbeat record? Metricbeat records metrics from Operating Systems and services running on a server. The statistics and metrics gathered by Metricbeat are forwarded to Elasticsearch.

The configuration details of each machine may be found below.

| Name     | Function  | IP Address | Operating System |
|----------|-----------|------------|------------------|
| JumpBox  | Gateway   | 10.0.0.8   | Ubuntu 18.04     |
| Web-1    | DVWA      | 10.0.0.6   | Ubuntu 18.04     |
| Web-2    | DVWA      | 10.0.0.7   | Ubuntu 18.04     |
| ElkStack | Elk Stack | 10.1.0.4   | Ubuntu 18.04     |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP Address

Machines within the network can only be accessed by the JumpBox Provisioner, via port 22.
- Which machine did you allow to access your ELK VM? The JumpBox Provisioner from IP 10.0.0.8.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses   |
|----------|---------------------|------------------------|
| JumpBox  | Yes                 | Personal IP            |
| Web-1    | Yes                 | 10.0.0.8               |
| Web-2    | Yes                 | 10.0.0.8               |
| ElkStack | Yes                 | 10.1.0.8 & Personal IP |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows continuity amongst all of the scripts. Ansible also allows for load balancing on multiple machines automatically using a Playbook, reducing the risk of human error.

The Playbook implements the following tasks:
- Specifies which group of machines in the header
- Installs services
  - docker.io
  - python3-pip
  - docker
- Increases memory size
- Downloads containers and customizes what ports to use
  - Note: Containers must be started with published ports 5601:5601; 9200:9200; 5044:5044
- Automatically starts services

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image](https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Diagrams/sudo-docker-ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 (10.0.0.6)
- Web 2 (10.0.0.7)

We have installed the following Beats on these machines:
- Metricbeat
- Filebeat

These Beats allow us to collect the following information from each machine:
- Metricbeat collects statistics and metrics from the network, then sends them to Elasticsearch. Statistics and metrics can include data on performance, availability, and CPU memory.
- Filebeat collects system logs and files before sending them to Elasticsearch. This Beat allows analysts to monitor for suspicous activity. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the [metricbeat-config.yml](https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Ansible/metricbeat-config.yml) file to /etc/ansible/roles.
- Update the configuration file to include the private IP address of the Elk Stack server.
- Run the playbook, and navigate to the ElkStack Virtual Machine at http://{ELK machine internal IP}:5601/app/kibana to check that the installation worked as expected.

- Which file is the playbook? [filebeat-playbook.yml](https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Ansible/filebeat-playbook.yml) and [metricbeat-playbook.yml](https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Ansible/metricbeat-playbook.yml)
- Where do you copy it?
  - Filebeat: curl https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Ansible/filebeat-playbook.yml >'/etc/ansible/filebeat-playbook.yml'
  - Metricbeat: curl https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Ansible/metricbeat-playbook.yml > '/etc/ansible/metric-playbook.yml'
- Which file do you update to make Ansible run the playbook on a specific machine? Update the Ansible [Hosts](https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Ansible/hosts.yml) file in /etc/ansible/hosts within the container.
- How do I specify which machine to install the ELK server on versus which to install Filebeat on? This can be controlled in the [Playbook](https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Ansible/install-elk.yml).
- Which URL do you navigate to in order to check that the ELK server is running? http://{ELK machine internal IP}:5601/app/kibana

### Kibana Captures

Metricbeat Kibana Capture <br>
![image](https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Diagrams/metricbeatcapture.png)

Filebeat Kibana Capture <br>
![image](https://github.com/tnovakovich1/Elk-Stack-Project/blob/68a5c89b0c5d90e89d13fc4b7ddd6c676bc9a37c/Diagrams/filebeatcapture.png)
