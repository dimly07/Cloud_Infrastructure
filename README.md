## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Cloud_Infrastructure/Diagrams/Red_Team_Diagram.PNG

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  Filebeat-playbook.yml

---
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-$

    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

    # Use command module
  - name: Enable and Configure System Module
    command: filebeat modules enable system

    # Use command module
  - name: Setup filebeat
    command: filebeat setup

    # Use command module
  - name: Start filebeat service
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

Load balancing ensures that the application will be highly available, in addition to restricting external access to the network.
What aspect of security do load balancers protect? What is the advantage of a jump box?

Load balancers add an additional layer of security in front of the web servers without needing to make administrative changes. 
An advantage of having a jump box also provides an additional layer of security to proxy external connections into the internal network without logging directly into the machine.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file system and system health.

What does Filebeat watch for?_
Filebeat is a lightweight shipper for forwarding and centralizing log data.
Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

What does Metricbeat record?_
Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server.
Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

##REFERENCE##
https://www.google.com/search?rlz=1C1CHBD_enUS900US900&sxsrf=ALeKk02PABt_jkvdVYtCDOMBpQ6g6gclAw%3A1600995095499&ei=Fz9tX9rxHcX0tAanoZnABw&q=what+is+filebeat+and+metricbeat&oq=what+is+filebeat%3F&gs_lcp=CgZwc3ktYWIQAxgAMgIIADICCAAyAggAMgIIADIGCAAQFhAeMgYIABAWEB4yBggAEBYQHjIGCAAQFhAeMgYIABAWEB4yBggAEBYQHjoHCAAQRxCwAzoECCMQJzoFCAAQkQI6CAgAELEDEIMBOgsILhCxAxDHARCjAjoOCC4QsQMQgwEQxwEQowI6BQgAELEDOgQIABBDOgcIABAUEIcCOgQIABAKUMLiF1ig9Bdggf8XaABwAHgBgAF_iAGQDJIBBDE0LjSYAQCgAQGqAQdnd3Mtd2l6yAEIwAEB&sclient=psy-ab

The configuration details of each machine may be found below.

_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| WEb-1    | Container| 10.0.0.5   | Linux            |
| Web-2    | Container| 10.0.0.6   | Linux            |
| Web-3    | Container| 10.0.0.7   | Linux            |
| Elk-Box  | log file/monitoring |	10.1.0.4| Linux   |	

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
73.5.185.30

Machines within the network can only be accessed by SSH.
Which machine did you allow to access your ELK VM?
Jump Box
 
What was its IP address?_10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 73.5.185.30          |
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             |
| Web-3	   | No					 | 10.0.0.4				|
| Elk-Box  | Yes				 | 75.5.185.30	

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
What is the main advantage of automating configuration with Ansible?_

Automating via Ansible allows for faster deployment of end devices, applications, services and minimizes configuration errors and increases consistency.

The playbook implements the following tasks:

- Installs docker.io
- Installs pip3
- Installs Docker Python Modules
- Increases Memory
- Downloads and Launches Docker Container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Images/sudo_docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
List the IP addresses of the machines you are monitoring

- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

We have installed the following Beats on these machines:
Specify which Beats you successfully installed

- Filebeat
- Microbeat

These Beats allow us to collect the following information from each machine:
In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible.
- Update the etc/ansible/host/ file to include the Elk Box server IP address
- Run the ansible-playbook /etc/ansible/install-elk.yml from the jump-box ansible container, and navigate to ELK server by SSH and run 'sudo docker ps' to verify that ELK server is running. You should then beable to access the ELK server by URL https://http://20.185.234.196:5601/

Answer the following questions to fill in the blanks:
- Which file is the playbook? 
install-elk.yml

Where do you copy it?
You first SSH to the jumpbox, then start the ansible container, then copy the install-elk.yml file into the /etc/ansible directory.

- Which file do you update to make Ansible run the playbook on a specific machine?
The file is located in the ansible container at /etc/ansible/hosts

- How do I specify which machine to install the ELK server on versus which to install Filebeat on?
The Elk server is specified in the /etc/ansible/hosts file specified with the 10.1.0.4 ansible_python_interpreter=usr/bin/python3 under the Elk category.
File beat installation is specified in the filebeat.playbook.yml to install on all hosts

- Which URL do you navigate to in order to check that the ELK server is running?
navigate to the public IP address and port 5601 https://http://20.185.234.196:5601/
