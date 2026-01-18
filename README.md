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
- Add input according to our diagram
![3](https://github.com/user-attachments/assets/8f0ea9d0-b4d0-4aa8-8a5f-a9f872b9ccf3)

- Save and exit then type in
```
sudo netplan apply
```
```
ip a
```
- We should see the IP address 192.168.100.22/24
- Check the internet activity by ping to google.com

- We now will download Splunk for Linux (.deb) on Splunk website
- Install add-ons for Ubuntu Linux
```
sudo apt-get install virtualbox-guest-additions-iso
```
>[!NOTE]
>I have encountered some issues during this step, so I have to tpye in below commands before
> ````
> sudo apt update
> sudo apt install build-essential dkms linux-headers-$(uname -r)
> sudo reboot
> ````

- Click on "Devices" at the top > Shared Folders Settings > Click add icon on the right > Add folder path where we downloaded Splunk for Linux (.deb) > Selected read-only, auto mount, make permanent > Ok

- On Splunk server virtual machine
- Type in
```
sudo apt-get install virtualbox-guest-utils
```
```
sudo reboot
```
```
sudo adduser <user_name> vboxsf
```
- Next, we will create a new directory called "share"
```
mkdir share
```
- Mount our shared folder onto our "share" directory
```
sudo mount -t vboxsf -o uid=1000,gid=1000 <Your_share_folder_path> share/
```
```
ls -la
```
- We should see our share directory highlighed
![4](https://github.com/user-attachments/assets/153997e2-c2d1-4643-b452-7a81ff97570f)

- Go to share folder and type in
```
ls -la
```
```
sudo dpkg -i <splunk_file>
```
- After completed install Splunk now we go to Splunk directory onto our server
```
cd /opt/splunk
```
- Chage into user splunk by typing in
```
sudo -u splunk bash
```
```
cd bin
````
- Change directory to /bin then start to run the installer
```
./splunk start
```
- We'll get a license and terms agreements, we can go to last page and type in y to accept
- Login with username and password
- To make sure every time Splunk starts up everytime our virtual machine reboot
````
exit
````
```
cd bin
```
```
sudo ./splunk enable boot-start -user splunk
```
- Now we go to Windows 11, rename to "Target-Win11"
- Set static IP address according to our diagram







Credit : Thanks <a href="https://www.youtube.com/@MyDFIR">MyDFIR</a> 
