# Active Directory Project

## Objective

To build and secure an Active Directory lab environment while simulating real-world cyberattacks and monitoring them using a SIEM solution. The project focuses on hands-on experience with domain administration, attack detection, log analysis, and security monitoring in a controlled virtual lab.

 
### Skills Learned

- Installation and configuration of Active Directory Domain Services (AD DS)
- Creating and managing users, groups, and organizational units (OUs)
- Domain controller and DNS configuration
- Implementing Group Policy Objects (GPOs) for security enforcement
- Joining Windows client machines to the domain
- Monitoring Windows security logs and domain activity
- Simulating attacks using Kali Linux tools
- Detecting and analyzing malicious activity using Splunk SIEM
- Log forwarding and event correlation
- Understanding Active Directory attack techniques and defense strategies
- Basic incident detection and response workflow
- Troubleshooting AD, DNS, and logging issues


### Tools Used

- Windows Server 2022 – Domain Controller
- Active Directory Domain Services (AD DS)
- Group Policy Management Console (GPMC)
- DNS Manager
- PowerShell (Active Directory administration)
- Kali Linux – Attacker virtual machine
- Ubuntu Server – Splunk Enterprise SIEM
- Splunk Universal Forwarder
- Windows 11 – Domain-joined client machines
- VirtualBox
- Atomic RedTeam

## Steps


<img width="710" height="810" alt="AD drawio" src="https://github.com/user-attachments/assets/1448af4b-f25b-4cda-9aa5-2a9813012936" />

### Set up and configuration on home lab environments

- Set up VirtualBox and Install Kali Linux (Attacker machine), Windows11(Target machine), Windows Server 2022(Active Directory), Ubuntu Server(Splunk server)
> Learn how to install all of them from [here](https://www.youtube.com/watch?v=2cEj3bS5C0Q)

![2](https://github.com/user-attachments/assets/13bc7151-2104-47c7-9225-777b93ebf80c)

- Set up NAT Network for our VMs to have the internet access and put them in the same network
- Go to VirtualBox > Tools(bullet point) > Network > NAT Networks(Tab) > click Create > At the bottom put the name of the network you want, in my case is "AD-Project" > IPv4 Prefix set according to the diagram we created (in my case is `192.168.100.0/24`) > Click Apply
- Go to each machine and change Network to "NAT Network" also "AD-Project" we have created

### Installing Splunk SIEM on Ubuntu
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

### Splunk Universal Forwarder on a Windows machine & Sysmon

- Now we go to Windows 11, rename to "Target-Win11"
- Set static IP address according to our diagram
- IP address: `192.168.100.50`
- Subnet mask: `255.255.255.0`
- Default gateway: `192.168.100.1`

- Go to Splunk website and download "Splunk Universal Forwarder" for windows 11 
> [!NOTE]
> The details on install Splunk and Sysmon, follow along <a href="https://www.youtube.com/watch?v=uXRxoPKX65Q">HERE</a> from 12.50 minutes

- We have to install and configure Windows Server with both Splunk&Sysmon
- Computer name: `ADDC-01`
- IP address: `192.168.100.40`
- Subnet mask: `255.255.255.0`
- Default gateway: `192.168.100.1`

- If we have everything set correctly, we should see 2 hosts when we search on Splunk 192.168.100.22:8000 > Search & report > index="endpoint"

 ### Installation and configuration of Active Directory Domain Services

- Next, we are going to install and configure Active Directory on Windows Server 2022 then promote it to a domain controller
- Follow along <a href="https://www.youtube.com/watch?v=1XeDht_B-bA">Here</a>
- In my case I called Root domain name "projectad.local"
> [!NOTE]
> The domain name must have a top level domain. Example:<name.somgthing>

- After this, we will create user <a href="https://www.youtube.com/watch?v=1XeDht_B-bA">Here</a> from 5.10 minutes

![5](https://github.com/user-attachments/assets/2aa8ff29-f66e-4c96-b166-0ca9214c17e9)
> [!NOTE]
> In Active Directory Users and Computers (ADUC), where we can  create, manage, > and delete users, computers, and groups, organize them into Organizational
> Units (OUs), set permissions and security policies, reset passwords, manage
> group memberships, and delegate control, essentially handling most day-to-day > administrative tasks for network identity and access. It's a graphical tool
> (MMC snap-in) for centralized management of domain resources

### Creating and managing users, groups, and organizational units (OUs)

- Right click on projectad.local > New > Organizational Unit > name it "IT"
- Inside this unit, right click > New > User
- First name: `Anna`
- Last name: `Brown`
- User logon name: `abrown`

- Create another unit as "HR" and a user in this unit
- First name: `John`
- Last name: `Green`
- User logon name: `jgreen`
> [!NOTE]
> Unchecked "User must change password at next logon" since we are in lab environment

- Now we have our Active Directory set up and our server is now a Domain Controller, we will go to Windows 11 and join to the `projectad.local` domain by using `Anna Brown` account

### Joining Windows client machines to the domain

- Head to Windows 11 (Target machine) by keep the AD machine running and change "Preferred DNS server" to domain controller `192.168.100.40` > OK
- Search for `Advance System Settings` > choose Computer Name tab > Change > selected Domain > type in `PROJECTAD.LOCAL` > Reboot
- Login as `abrown` by selected "Other user"

- Next, we will use Kali Linux to perform a Brute Force attack onto users
- Then we will use Splunk to view telemetry for this activity
- Set up and install Atomic Redteam to run a test

### Simulating attacks using Kali Linux tools 

- First, set Kali to
- IP: `192.168.100.85`
- Network: `24`
- Gateway: `192.168.100.1`
- DNS servers: `8.8.8.8`

- Update and upgrade our repositories by type in
```
sudo apt-get update && sudo apt-get upgrade -y
```
- Setting up our attack by first create a new directory called `AD-Project`
```
mkdir AD-Project
```
- We will use "Crowbar" tool for attacking
```
sudo apt-get install -y crowbar
```
> [!NOTE]
> Crowbar is a Brute forcing tool, Brute force the RDP service on a single host with a specified username and wordlist,
> using 1 thread.

- We will use a popular word list called "Rockyou" that come with Kali Linux
- Go to `cd /usr/share/wordlists/` then when you type `ls` you should see a file called `rockyou.txt.gz`
- Unzip it `sudo gunzip rockyou.txt.gz` now we have `rockyou.txt` file
- We now copy this to our directory
```
cp rockyou.txt ~/Desktop/AD-Project
```
```
cd ~/Desktop/AD-Project
````
- In this project we will use only first 20 lines of "rockyou.txt"
```
head -n 20 rockyou.txt > passwords.txt
````
- Let's pretend we know a certain password of our target
 ````
nano passwords.txt
````
- Add a certain password at the bottom > save it

![6](https://github.com/user-attachments/assets/5156d12a-7fb3-47f0-a76e-b349369c3482)

- Before we launch our attack, go to Target machine to enable "Remote Desktop"
- Go to "Advance system settings" > login with admintrator and password > Remote tab > Selected "Allow remote connections to this computer" > Select Users... > Add... > in my case I made 2 users `Anna Brown` `John Green`

- Back to Kali, we will use Crowbar as a tool to perform brute force attack

```
crowbar -h
```
- To see what options available for us, in our case we will use..
```
crowbar -b rdp -u abrown -C passwords.txt -s 192.168.100.50/32
```
> [!NOTE]
> CIDR notation /32 specifies a single, unique IPv4 address, meaning all 32 bits of the address are fixed for the network portion, leaving zero bits for hosts, effectively creating a
> "host route" for one specific device, preventing communication with other devices on the same subnet, often used for loopbacks or security isolation

<!--( SHOW SCREEN SHOT OF RESULT OF SUCCESSFUL BRUTE FORCE)-->


###  Detecting and analyzing malicious activity using Splunk SIEM

- We now go to Splunk and see what telemetry we have generated
- Select "Search & Reporting"
- We know that our attack is occurred within the past 15 minutes and knew the target, let's narrow down our search to find the events related to "abrown"

```
index="endpoint" abrown
````
- Look at the `EventCode` we will see `4625` event count as `20`
> [!NOTE]
> 4625(F): An account failed to log on.

- And `4624` event count as `1`
> [!NOTE]
> 4624(S): An account was successfully logged on.

- As we expected, because we made a list of `passwords.txt` that contain 21 passwords that the last one is the correct one
- If we look closely at the event, each of them happened at the same time which can be clear indication of Brute Force activity

- Next we will install Atomic Red Team on our target machine then we can run some test on it

 ### Atomic RedTeam to generate the attack
 
- Open up PowerShell with administrator privileges

```
Set-ExecutionPolicy Bypass CurrentUser
```
- Before install, let's set an exclusion fir the entire C: as Microsoft Defender will detect and remove some of the files from Atomic red team
- Go to "Virus & threat protection" > manage settings > Click "Add or remove exclusions" > Select "Folder" > Select C drive

- Atomic Red Team Powershell Command:
```
IEX (IWR  -UseBasicParsing);
Install-AtomicRedTeam -getAtomics
```
- Then press Y and hit enter to install the dependencies
- After that go to C: > Atomic RedTeam > atomics
- Inside we will see a bunch of technique IDs and these map back to MITRE ATT&CK framework
- Try with `T1136.001` " Create Account: Local Account"
- To use the command we type in..
```
Invoke-AtomicTest T1136.001
```
- This will automatically generate telemetry based on "Create local account"
<!--Add pic-->

###  Detecting and analyzing malicious activity using Splunk SIEM

- Go to Splunk and try to check the events
> [!NOTE]
> Meaning we can build alerts to detect on this activity in the future

Credit : Thanks <a href="https://www.youtube.com/@MyDFIR">MyDFIR</a> 
