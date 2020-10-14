## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/CoreyJay44/Cyber-Security-Project1/blob/main/Images/RedTeam%20Virtual%20Network%20Topology.PNG "RedTeam Net")

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml files may be used to install only certain pieces of it, such as Filebeat.

  - [yml Playbooks](https://github.com/CoreyJay44/Cyber-Security-Project1/tree/main/YML%20Playbooks)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
- Load Balancers protect the virtual network against DDoS attacks by shifting attack traffic between the virtual servers. 
- The advantage of the Jump-Box is that it is "hidden" from the public. It can only be accessed by the administrator to perform tasks on the rest of the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VM Network and system metrics.
- Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- Metricbeat collects and logs the system metrics of your network. e.g. CPU and Memory usage

The configuration details of each machine may be found below.

| Name     | Function  | IP Address | Operating System |
|----------|-----------|------------|------------------|
| Jump Box | Gateway   | 10.0.0.4   | Linux            |
| Web-1    | Web Server| 10.0.0.5   | Linux            |
| Web-2    | Web Server| 10.0.0.6   | Linux            |
| Web-3    | Web Server| 10.0.0.7   | Linux            |
| Elk      | Monitoring| 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the designated Admin's public IP address

Machines within the network can only be accessed by each other. Jump Box can SSH into the Web Servers and Elk Servers. Web Servers sent log information to the Elk Server


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | Admin Public IP      |
| Web-1    | No                  | 10.0.0.1-254         |
| Web-2    | No                  | 10.0.0.1-254         |
| Web-3    | No                  | 10.0.0.1-254         |
| Elk      | No                  | 10.0.0.1-254         |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Using Ansible in this way allows us to quickly install, update, and add web servers to our network using the same playbooks.

The playbook implements the following tasks:
- First we install docker on all network machines so they will be able to recieve and install containers. 
- Ansible is installed on the Jump Box VM to distribute containers to other VMs on the network. 
- Ansible playbooks are used to install the ELK stack container on the ELK server and a 'Beats' containers on the Web servers

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/CoreyJay44/Cyber-Security-Project1/blob/main/Images/Docker%20ps.PNG "Docker ps")

Elk Playbook used can be found [Here](https://github.com/CoreyJay44/Cyber-Security-Project1/blob/main/YML%20Playbooks/elk-playbook.yml)



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 - 10.0.0.5
- Web-2 - 10.0.0.6
- Web-3 - 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- <b>Filebeat</b> - Filebeat detects changes to the filesystem. We are using this to monitor our Web Log data. 
- <b>Metricbeat</b> - Metricbeat detects changes in system metrics, such as CPU usage. We use it to detect SSH login attempts, failed sudo escalations, and CPU/RAM statistics.

The Ansible Playbooks used can be found [here](https://github.com/CoreyJay44/Cyber-Security-Project1/tree/main/YML%20Playbooks)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the .yml files to `/etc/ansible` directory.
- Update the hosts file to be as follows. This will assign the VM servers to their server groups for the Ansible Playbooks. 
``` 
   [webservers]
    10.0.0.5
    10.0.0.6
    10.0.0.7
   [elkservers]
    10.1.0.4
```
 - To run the playbooks and install Elk, Filebeat, and Metric beat run the following commands:
```
cd /etc/ansible
ansible-playbook elk-playbook.yml
ansible-playbook filebeat-playbook.yml
ansible-playbook metricbeat-playbook.yml 
```

- Then, run: `curl http://10.0.0.8:5601`. (You can also use the loadbalancer's public IP address) This is the address of Kibana. If the installation succeeded, this command should print HTML to the console. 
