## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](/Images/elknet.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the webserver_playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

  - Ansible/elkserver_playbook.yml
  - Ansible/webserver_playbook.yml

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
-Load balancers facilitate a redundant system architeture allowing services to remain available as long as at least one VM in the backend pool is operational.
-The benefit of using a jumpbox is that it reduces the attack surface for a threat actor.  Reducing the number of systems exposed to the internet simplifies
 security controls needed to ensure all systems are safe.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the metrics and system files.
- Filebeat collects data about the file system.
- Metricbeat collects machine metrics such as uptime.

The configuration details of each machine may be found below.

| Hostname | Function                | IP Address | Operating System        |
|----------|-------------------------|------------|-------------------------|
| jumpbox  | gateway and provisioner | 10.0.0.4   | Ubuntu Server 18.04 LTS |
| web1     | web server              | 10.0.0.5   | Ubuntu Server 18.04 LTS |
| web2     | web server              | 10.0.0.6   | Ubuntu Server 18.04 LTS |
| web3     | web server              | 10.0.0.7   | Ubuntu Server 18.04 LTS |
| elk      | ELK server              | 10.1.0.4   | Ubuntu Server 18.04 LTS |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine and elk server can accept connections from the Internet. Access to these machines are only allowed from the following IP addresses:
- MY PRIVATE IP ADDRESS (no posting this on github)

Machines within the network can only be accessed by SSH.
- the jumpbox system is the only VM permitted to access the ELK server over SSH from its internal IP: 10.0.0.4.

A summary of the access policies in place can be found in the table below.

| Hostname | Publicly Accessible | Public IP Address | Allowed IP Address           |
|----------|---------------------|-------------------|------------------------------|
| jumpbox  | yes                 | 40.78.100.186     | MY PRIVATE IP ADDRESS        |
| web1     | no                  | none              | 10.0.0.4, 10.0.0.6, 10.0.0.7 |
| web2     | no                  | none              | 10.0.0.4, 10.0.0.5, 10.0.0.7 |
| web3     | no                  | none              | 10.0.0.4, 10.0.0.5, 10.0.0.6 |
| elk      | yes                 | 52.183.19.118     | 10.0.0.4                     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
...provisioning is efficient, automatic, and identical for subsequent rebuilds.  Should the ELK server ever need to be rebuilt anyone 
   can restore it to the proper configuration

The playbook implements the following tasks:
  1. reconfigures memory usage on the elk server to function properly while running Elasticsearch, Logstash, and Kibana
  2. installs docker.io and python3-pip with the apt module
  3. enables the docker service with the service module
  4. installs docker for python with the pip module
  5. downloads, configures, and starts the elk docker container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![docker ps output](Images/docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

I have installed the following Beats on these machines:
- Filebeats
- Metricbeats

These Beats allow us to collect the following information from each machine:
- Using filebeats we collect log data and events.  For example, filebeats sends system.syslog whcih we can use to detect configuration changes.
- Using metricbeats we collect system metrics.  For example, metricbeats sends CPU idle time and memory usage which we can use to detect abnormal resource usage.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the elkserver_playbook.yml and webserver_playbook.yml files to /etc/ansible/files/.
- Update /etc/ansible/hosts file to include your VMs.  Add you webserver VMs to the [webserver] group and ELK server to the [elk] group.
- Update /etc/ansible/ansible.cfg with the correct remote_user that can access your VMs.
- Then run elkserver_playbook.yml then webserver_playbook.yml
- Navigate to http://<your_elk_server_public_IP>:5601 to check that the installation worked as expected.
