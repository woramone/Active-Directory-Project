# Active-Directory-Project

## Objective

<!-- The Basic Home Labs Project is an initiative focused on hands-on experimentation using simple, cost-effective home laboratory environments. The project aims to design and deploy controlled test systems to generate and collect system, network, and security telemetry, enabling the analysis and detection of malware behavior on a target machine. -->
 
### Skills Learned

<!-- - Learning how to set up and configuration on home lab environments
- Detecting malicious activity using telemetry and security indicators
- Using SIEM and log analysis tools to correlate events across data sources -->


### Tools Used

<!-- - Nmap (Network Mapper), find open ports/services, identify operating systems, and detect vulnerabilities by sending specially crafted packets to targets and analyzing responses.
- Create Payload Using Msfvenom
- Security Information and Event Management (SIEM) system for log ingestion and analysis. -->

## Steps

<img width="710" height="810" alt="Active Directory Project drawio" src="https://github.com/user-attachments/assets/4e11b776-e3f5-427d-8db9-4d9bba9725c7" />

- Use draw.io to draw lab diagram for this Project
- Set up VirtualBox and Install Kali Linux (Attacker machine), Windows11(Target machine), Windows Server 2022(Active Directory), Ubuntu Server(Splunk server)
> Learn how to install all of them from [here](https://www.youtube.com/watch?v=2cEj3bS5C0Q)

![2](https://github.com/user-attachments/assets/13bc7151-2104-47c7-9225-777b93ebf80c)

- Set up NAT Network for our VMs to have the internet access and put them in the same network
- Go to VirtualBox > Tools(bullet point) > Network > NAT Networks(Tab) > click Create > At the bottom put the name of the network you want, in my case is "AD-Project" > IPv4 Prefix set according to the diagram we created (in my case is 192.168.100.0/24) > Click Apply
- Go to each machine and change Network to "NAT Network" also "AD-Project" we have created

- Open our Splunk server(Ubuntu server)
- Type in 
```
sudo nano /etc/netplan/50-cloud-init.yaml
```










Credit : Thanks <a href="https://www.youtube.com/@MyDFIR">MyDFIR</a> <a href="https://www.youtube.com/watch?v=-8X7Ay4YCoA&list=PLG6KGSNK4PuBWmX9NykU0wnWamjxdKhDJ&index=4">Learning video</a>

