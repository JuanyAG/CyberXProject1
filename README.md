# CyberXProject1
Week13 HW_GitFundamentals
The files in this repository were used to configure the network depicted below.
Note: The following image link needs to be updated. Replace diagram_filename.png with the name of your diagram image file.
https://drive.google.com/file/d/1d0_3zMLBbC04mXMzWZbHFwhRhfu1Rbp0/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.
TODO: Enter the playbook file.
---
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    Copy: 
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

    # Use systemd module
  - name: Enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes
This document contains the following details:
Description of the Topology
Access Policies
ELK Configuration
Beats in Use
Machines Being Monitored
How to Use the Ansible Build
Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
 What aspect of security do load balancers protect? What is the advantage of a jump box?
Load balancers protect the network by adding an additional layer of security that redirects traffic to more than one web server and also prevents DDos attacks. Access Controls verifies that only authorized users such as administrations have access to connect; such as those that need to know have access. 
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _files within the network and system metrics_.
 What does Filebeat watch for?
Filebeat forwards and logs data changed; it monitors the elasticsearch log files system for sudden changes
 What does Metricbeat record?
Metricbeat records the metric and statistic changes, as well the amount of logins in the network
The configuration details of each machine may be found below. Note: Use the Markdown Table Generator to add/remove values from the table.
Name
Function
IP Address
Operating System
Jump Box
Gateway
10.0.0.1
Linux
DVWA-1
Web Server
10.0.0.5
Linux
DVWA-2
Web Server
10.0.0.6
Linux
ELK VM
Monitoring
10.1.0.4
Linux

Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the __jumpbox__ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 
TODO: 99.151.194.137
Machines within the network can only be accessed by _other VM within the network_.
TODO: Which machine did you allow to access your ELK VM? What was its IP address?
Web-1 and Web-2; IP’s are 10.0.0.5 and 10.0.0.6
A summary of the access policies in place can be found in the table below.
Name
Publicly Accessible
Allowed IP Addresses
Jump Box
yes
99.151.194.137
Elk
No
10.0.0.5 and 10.0.0.6
DVWA-1

DVWA-2
No

No
10.0.0.5 and 10.0.0.6

10.0.0.5 and 10.0.0.6

Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
?What is the main advantage of automating configuration with Ansible?
The main purpose is to help with the automation of infrastrucutre as code; basically it simplifies complicated tasks into playbooks and reduces time/human error in deploying applications and software 
The playbook implements the following tasks:
TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; 
Install docker.io
Install python3-pip
Install docker; this is the Docker python pip module
Download and run “sebp/elk:761
Run playbook with command “ansible-playbook elk.yml
SSH into Elk VM to confirm the connection was made
Run command in ELk VM with command: “sudo docker ps”
The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
Note: The following image link needs to be updated. Replace docker_ps_output.png with the name of your screenshot image file.
https://drive.google.com/file/d/1peFcekfD6b9X6cO0zU8SyjxXouc6THrs/view?usp=sharing
Target Machines & Beats
This ELK server is configured to monitor the following machines:
TODO: List the IP addresses of the machines you are monitoring
Elk is monitoring the following: 
DVWA-1
Web Server
10.0.0.5
DVWA-2
Web Server
10.0.0.6

We have installed the following Beats on these machines:
TODO: Specify which Beats you successfully installed
The following beats have been installed: Filabeat, Metricbeat, and Packetbeat
These Beats allow us to collect the following information from each machine:
TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., Winlogbeat collects Windows logs, which we use to track user logon events, etc.
Filabeat; is known as a “shipper” which is a tool that is installed within the server, it is able to monitor log files, along with collecting the amount of logins as well as failed logins, and then forwards this information to Elasticsearch to then be analyzed, stored, or search in the future. Filabeat can also be used to prevent attacks by collecting the syslogs.
Metricbeat is also a “shipper” ; it takes the collected metrics and statistics and then ships it to Elasticsearch to be stored and analyzed. Metricbeat collects CPU statistics, memory, and network usage.
Packetbeat captures live network packets just like Wireshark, along with being able to analyze the packets between the applications servers. It decodes the traffic based on the set protocols, it can be run on a dedicated server or on each application server.
Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:
Copy the _elk_install.yml file to /etc/ansible/elk_install.yml
Update the _hosts_ file to include… the IP specific virtual machines
Run the playbook, and navigate to _your browser and type “http://ElkIP:5601/app/kibana”  to check that the installation worked as expected.
TODO: Answer the following questions to fill in the blanks:
Which file is the playbook? A: Ansible Where do you copy it?
Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
_Which URL do you navigate to in order to check that the ELK server is running?
The answers are shown below:
Copy the _elk_install.yml file to /etc/ansible/elk_install.yml
Update the _hosts_ file to include… the IP specific virtual machines
Run the playbook, and navigate to _your browser and type “http://ElkIP:5601/app/kibana”  to check that the installation worked as expected.

Activity File: Exploring Kibana
Add the sample web log data to Kibana.


Answer the following questions:


In the last 7 days, how many unique visitors were located in India? 26 visitors


In the last 24 hours, of the visitors from China, how many were using Mac OSX? 0 visitors 


In the last 2 days, what percentage of visitors received 404 errors? How about 503 errors? 404 - 57.143% & 503 42.857%


In the last 7 days, what country produced the majority of the traffic on the website? China


Of the traffic that's coming from that country, what time of day had the highest amount of activity? Hour 13 


List all the types of downloaded files that have been identified for the last 7 days, along with a short description of each file type (use Google if you aren't sure about a particular file type).
Types of Files dowloaded:
Gz: compressed archive file
Css: cascade style sheet file, file that contains contents HTML (webpage) details 
Zip: compressed file, it can contain any sort of data/information/text
Deb: debian software package file, mainly used in linux systems
Rpm: Red hat package manager; mainly used with linux to install software packages
Now that you have a feel for the data, Let's dive a bit deeper. Look at the chart that shows Unique Visitors Vs. Average Bytes.


Locate the time frame in the last 7 days with the most amount of bytes (activity).
In your own words, is there anything that seems potentially strange about this activity?
On 12/16 at 1500 & 1800, 1218 at 1800, 12/19 at 1500, 12/20 at 1500, and 12/21 at 1500 there was only one visitor. The highest average bytes was on 12/20
Filter the data by this event.


What is the timestamp for this event?  Between hour 15:45
What kind of file was downloaded? Type if file does not display
From what country did this activity originate? China
What HTTP response codes were encountered by this visitor? 200 - 100%
Switch to the Kibana Discover page to see more details about this activity.


What is the source IP address of this activity? 158.113.198.78
What are the geo coordinates of this activity? Lat: 39.97338139 Lon -90.40373556
What OS was the source machine running? Windows XP
What is the full URL that was accessed? http://www.elastic.co/downloads
From what website did the visitor's traffic originate? Facebook
Finish your investigation with a short overview of your insights.


What do you think the user was doing?  Trying to login to the users facebook account 
Was the file they downloaded malicious? If not, what is the file used for? No, to try and see if the HTTP port is open and gain loggin creds
Is there anything that seems suspicious about this activity? Yes, no malicious file was downloaded
Is any of the traffic you inspected potentially outside of compliance guidelines? The possible attacker had no issues logging into the other users facebook. 








Domain: Network Security
Question 1: Faulty Firewall
Suppose you have a firewall that's supposed to block SSH connections, but instead lets them through. How would you debug it?
Make sure each section of your response answers the questions laid out below. ​
Restate the Problem: The firewall port rules are set to block all SSH connections but the rule is not being applied and allowing all SSH connections.


Provide a Concrete Example Scenario


In Project 1, did you allow SSH traffic to all of the VMs on your network? Yes
Which VMs did accept SSH connections? All VM’s
What happens if you try to connect to a VM that does not accept SSH connections? It will show as “connection refused”. Why? It could be for a number of reasons; such as the protocol is denying the request, there is an error with the IP/using the wrong IP, the port is not open, the SSH server is not installed, the public key is not accurate. or the server is not running at all. 
Explain the Solution Requirements


If one of your Project 1 VMs accepted SSH connections, what would you assume the source of the error is? My assumptions would be: did I insert/copy the correct public key, is the port open and if the server is running and if the port rules are accurate.
Which general configurations would you double-check? Public Key, port rules, IP’s
What actions would you take to test that your new configurations are effective? Attempt to SSH into the VM’s
Explain the Solution Details


Which specific panes in the Azure UI would you look at to investigate the problem? Check the network setting under the specific VM > check the inbound rules > check port 80/22 > check the public key > verify it is the correct IP address 
Which specific configurations and controls would you check? Check the Network Security rules, protocol type, action that it is set to, the source, and destination. And check the Ansible hosts files to check the IP’s.
What would you look for, specifically? IP errors, make sure the SSH service is up and running/installed, check which port is being used for SSH, and/or use the command line to check the SSH status, or reset the pub key.
How would you attempt to connect to your VMs to test that your fix is effective? Restart the VM and SSH into one of the VM’s
Identify Advantages/Disadvantages of the Solution


Does your solution guarantee that the Project 1 network is now "immune" to all unauthorized access? No


What monitoring controls might you add to ensure that you identify any suspicious authentication attempts?​ Network Security Groups/Resource Groups: Assign rules for specific ranges, disable ports right after use, apply the zero trust approach - assume all the systems within the network can be trusted. 
Question 2: Unsecured Web Server
Suppose you find a server running HTTP on port 80, despite compliance guidelines requiring encryption in motion. What do you do? ​​
Restate the Problem: Compliance guidelines requires traffic to be encrypted but, HTTP is used for unencrypted communication, it should be set as “HTTPS” which is used for encrypted traffic and typically runs on port 443


Provide a Concrete Example Scenario


In Project 1, did you have servers running HTTP on port 80? Yes If so, why was it permissible to do so? Because all the machines were being accessed within one network. 


In a real deployment, which specific machine would you configure differently? The Jump-Box VM How, and why? Change the security group rule service to HTTPS and change the “source port ranges” to one specific port and/or port ranges - this will verify which ports traffic will be allowed or denied access. 


Explain the Solution Requirements


Why is running HTTP on port 80 a potential problem? HTTP supports web traffic from web browsing (which is human readable) and is unencrypted communication/packets, if open this leaves the user vulnerable to an attacker gaining administrative access to the website or the web server it self
How would you reconfigure a server to serve HTTP traffic safely?  By reconfiguring the firewall rules to limit traffic to a specific port only when the server is running.
How does this solution fix the problem? The rule will only allow traffic from the specific port assigned. 
Explain the Solution Details


Which tools and technologies would you use to implement this solution in Project 1? Network Security - Inbound Port Rules
How, specifically, would you use these tools to harden your deployment? Change the security group rule service to HTTPS and change the “source port ranges” to one specific port “80”
Identify Advantages and Disadvantages of the Solution


Will your solution break clients that used to communicate with the server over port 80? No
Do you have to do any work to keep this solution running long term? A: No. Or can you simply "set it and forget it?”





Activity File: Kibana Continued





wget-DoS:
Run the wget command in a loop to generate a ton of web requests.
Command: for i in {1..1000}; do wget 10.0.0.5; done
Open the Metrics page for the web machine you attacked and answer the following questions:
Which of the VM metrics were affected the most from this traffic? A: Load and Metrics
