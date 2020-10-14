
# Project-1
Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Images/ELK.png]

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - Playbook File
   ---
  - name: Configure ELK1 with Docker
    hosts: elk
    remote_user: David
    become: true
    tasks:
 
    - name: Install Docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

    - name: Install docker python module
      pip:
        name: docker
        state: present

    - name: Increase virtual memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

    - name: download and launch the docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044


This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


 Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

What aspect of security do load balancers protect? the availibility of a server by distributing network traffic across multiple servers. What is the advantage of a jump box? It's easier to secure because all traffic is forced through a single node.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system traffic.
What does Filebeat watch for? Collects data and watches for changes.
What does Metricbeat record? Metrics such as CPU Usage and Uptime.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Ubuntu 18.04     |
| Web-1    |  DVWA    | 10.0.0.5   | Ubuntu 18.04     |
| Web-2    |  DVWA    | 10.0.0.6   | Ubuntu 18.04     |
| Web-3    |  DVWA    | 10.0.0.7   | Ubuntu 18.04     |
| ELK1     | ELKstack | 10.1.0.4   | Ubuntu 18.04     |

 Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
Add whitelisted IP addresses_ 73.37.4.236

Machines within the network can only be accessed by SSH.
Which machine did you allow to access your ELK VM?JumpBox. What was its IP address? 104.45.151.2

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| JumpBox  | No                  | Personal IP Addresss |
| Web-1    |Yes via Load Balancer| LB Public IP Address |
| Web-2    |Yes via Load Balancer| LB Public IP Address |
| ELK1     | No                  | Personal IP Addresss |

 Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
What is the main advantage of automating configuration with Ansible? Easily deploys multiple servers and Eliminates the possibility of human error.

The playbook implements the following tasks:
- Install Docker.io and pip3
- Install docker Python Module
- Increase Virtual Memory
- Download and configure elk container
- Establish Ports

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Images/docker.png]

 Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 10.0.0.5
- Web-2 10.0.0.6
- Web-3 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors and collects log events
- Metricbeat records metrics and statistical data from the Server

 Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml file to /etc/ansible/playbooks.
- Update the playbook file to include IP of the ELK Server
- Run the playbook, and navigate to kibanato check that the installation worked as expected.

- _Which file is the playbook? elk-install.yml Where do you copy it? /etc/ansible/playbook
- _Which file do you update to make Ansible run the playbook on a specific machine? /etc/ansible/hosts.cfg. How do I specify which machine to install the ELK server on versus which to install Filebeat on? On the Hosts file.
- _Which URL do you navigate to in order to check that the ELK server is running? http://your-IP:5601/app/kibana#/home?_g=()
