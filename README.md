<h1>MalikCyber - Cloud Cybersecurity Homelab Red/Blue Team</h1>

<h2>Description</h2>
Leveraging AWS, we will be creating three different virtual machines; a Security Tools Box (Ubuntu), A Kali Linux Attacker Machine, and a Windows 10 Workstation. All three of these machines will be connected togeher in its own Isolated network via a VPC(Virtual Private Cloud). Once completed, we'll be able to successfully run Red/Blue team simulations. 
<br />


<h2>Languages and Utilities Used</h2>

- <b>Bash</b>
- <b>AWS EC2</b> 
- <b>Splunk</b>

<h2>Environments Used </h2>

- <b>Windows 10 </b> 
-  <b>Kali Linux </b>
-  <b>Ubuntu (Linux) </b> 


<h2>Program walk-through:</h2>

<p align="center">
Network Topology: <br/>
<img src="https://imgur.com/1fW7L0Z.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
<br />
<br />
AWS:  <br/>
- Use us-east-2 region.:  <br/> 
<br /> 
Create VPC:  <br/> 
Create VPC, with one public subnet, route table, and IGW:  <br/> 
- VPC IP Address Block:10.0.0.0/16 (default):  <br/> 
- Public Subnet - Cybersecurity Homelab: 10.0.0.0/24:  <br/> 
- Route Table - Cybersecurity Homelab: Default:  <br/> 
- IGW: Default:  <br/> 
<img src="https://imgur.com/YrmdxUg.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
<br />
<br />
Key Pair: <br/>
 CP Pem in .ssh Directory and CHMOD <br/>
 - We are going to move our pem file from downloads to desktop (Physically) <br/> 
 - Then we are going to use *cd Desktop <br/> 
 ---> -> cp Cybersecurity-Homelab.pem /Users/maliktambwe/.ssh <br/> 
 <br />
 Then we are going to close terminal and open it again and use <br/> 
 - Cd ~/.ssh<br/> 
 - And then ls to list directory<br/> 
<img src="https://imgur.com/u5OHfDL.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
<br />
<br />
Change permissions for Homelab file to only have Read permissions [sudo chmod 400 Cybersecurity-Homelab.pem]:  <br/>
<img src="https://imgur.com/NhulCxn.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
<br />
<br />
 <br/>
Security  Groups <br/>
Cybersecurity Homelab SG: <br/>
- All ICMP - IPv4 - 0.0.0.0/0  <br/>
- RDP - Your IP Address  <br/>
- SSH - Your IP Address  <br/>
Security Tools - Netspectrum SG:  <br/>
- Use default provided by the AMI.  <br/>
- Custom TCP - 9997 - 0.0.0.0/0  <br/>
  <br/>
 <br/>
 <br/>
Kali Linux  <br/>
-&BuvN4fApX(8%lsMr0LQRUK=GTXdEPj <br/>

<br />
<br />

AWS Configuration  <br/>
- AMI: Kali Linux AMI by Offensive Security. <br/>
- Instance Type: t2.micro . <br/>
- Volume Size: EBS with 12 GIB. <br/>

- Deploy in Public Subnet Cybersecurity Homelab. Use the Cybersecurity Homelab SG<br/>
<img src="https://imgur.com/vfd7ilV.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
<br />
<br />
  Connect into Kali Attacker Instance  <br/>
<img src="https://imgur.com/vGeC02R.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
<img src="https://imgur.com/NVAqtt0.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>

<br />
<br />
<br />
XRDP   <br />
Resource: https://www.kali.org/docs/general-use/xfce-with-rdp/   <br />
The configuration file (for changing the default port) is in /etc/xrdp/xrdp.ini .   <br />
<br />
<br />
Create Script
<br />
#!/bin/sh <br />  <br />
echo "[i] Updating and upgrading Kali (this will take a while)"  <br />
apt-get update  <br />
apt-get full-upgrade -y  <br />
echo "[i] Installing Xfce4 & xrdp (this will take a while as well)"  <br />
apt-get install -y kali-desktop-xfce xorg xrdp  <br />
echo "[i] Configuring xrdp to listen to port 3389 (but not starting the service)"  <br />
sed -i 's/port=3389/port=3389/g' /etc/xrdp/xrdp.ini  <br />
<br />
Install <br />
- Run ./remote.sh  <br />
<br />
Change Password <br />
- echo kali:kali | sudo chpasswd <br />
<br />

<br />
Start XRDP <br />
- sudo systemctl enable xrdp --now <br />
- systemctl status xrdp <br />
<br />


<br />
Windows (Vulnerable Client) <br/>
AWS Configuration: <br/>
- AMI: Windows Server 2022 Base. <br/>
- Instance Type: t2.micro . <br/>
- Volume Size: EBS with 30 GIB <br/>
<img src="https://imgur.com/wC2YbRy.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
Login <br />
- Go to the Connect option in AWS, choose RDP client, go to "Get Password", upload the private key associated with the EC2 instance, the private key should be in .pem format. Take note of the password, copy to Notepad for notes. <br />
💡 Turn off Windows Defender Firewall to have Kali access Windows.<br />
<br />

<br />
Security Tools (Ubuntu Desktop)  <br/>
AWS Configuration  <br/>
- AMI: Use the AMI provided by Netspectrum.  <br/>
- Instance Type: t3.large  <br/>
- Volume Size: EBS with 30 GIB.  <br/>

<img src="https://imgur.com/6QbJmRC.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
Default Username/Password on Netspectrum <br/>
- Username: ubuntu <br/>
- Password: EC2 Instance ID <br/>
- Go to public IP address of the instance in browser. <br/>
- Change VNC password using the command line shortcut on Desktop. <br/>
  <br/>
  <br/>
  
<br />
Change SSH Password <br />
- sudo nano /etc/ssh/sshd_config .  <br />
- Change PasswordAuthentication to yes .  <br />
- Change the ubuntu user's password with passwd , the default password is the EC2 instance ID.  <br />
 <br />
 <br/>
<br/>

Netspectrum Security Group Inbound Rules: Edit the default Inbound Rules   <br />
In order a few more custom rules  <br />
- Go to Security Groups  <br />
- Choose Netspectrum default ubuntu security group and “Edit Inbound Rules”<br />
- Add Rule
----- 1) Custom TCP, Port 9997, 0.0.0.0/0  <br />
----- 2) All ICMP - IPv4 0.0.0.0/0  <br />
-------- We will need these rules when we configure Splunk and test the Connection  <br />
  <img src="https://imgur.com/P2Im9hj.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
<br/>

Download Splunk onto Security Tools (Ubuntu Desktop)  <br/>
   - Resource: https://www.inmotionhosting.com/support/security/install-splunk/#setup  <br/>
   - Sign-up / Login into Splunk. Navigate to URL: https://www.splunk.com/en_us/download/splunkenterprise  <br/>
   - Download the deb version of Splunk URL for the Ubuntu instance.  <br/>
   - Download: sudo dpkg -i splunk-deb  <br/>
   - Navigate to splunk directory: /opt/splunk/bin  <br/>
   - Start splunk and create a user. sudo ./splunk start .  <br/>
   - Create an Splunk admin and username.  <br/>
   - Log into dashboard.  <br/>
<br/>

Download Splunk onto Security Tools (Ubuntu Desktop)  <br/>
   - Resource: https://geek-university.com/install-a-splunk-forwarder-on-windows/  <br/>
   - Go to Splunk Enterprise -> Settings -> Forwarding and receiving -> Configure receiving -> Ensure "Listen on this port" has 9997. If not, add 9997 as a listener.  <br/>
   - Go to Microsoft Edge, type in the following URL: https://www.splunk.com/en_us/download/universalforwarder/  <br/>
   - Log into Splunk. Navigate to the Windows Server download.  <br/>
<img src="https://imgur.com/lhy2LRU.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
<br/>
Navigate to the Downloads Folder. Click on the .msi file and follow Wizard options.  <br/>
<img src="https://imgur.com/baaGTyU.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
- Run through default options until getting to the "Windows Events Logs" view. Select "Application", "Security", and "System" log types. <br/>

- Create an administrator account. You can use the same credentials for the Splunk Enterprise Dashboard. <br/>

- Skip "Deployment Server" view. <br/>
<img src="https://imgur.com/BbXFHeM.png" height="80%" width="80%" alt="Red Team - Blue Team Cyber Home Lab Cloud Environment"/>
- Add the private IP address of Ubuntu Security Tools and use the default port of 9997. <br/>

- Select "Install". <br/>
<br/>
<br/>



Add Security Event Logs to win-security Index <br/>
  > Go to Splunk Enterprise in the Security Tools box. Navigate to Settings -> Indexes -> New Index -> Name Index "win-security" -> Use default options. <br/>
  <br/>
  > Go to Windows VM. Navigate to C:\Program Files\SplunkUniversalForwarder\etc\system\local . <br/>
  <br/>
  > Create a new file (you can copy the outputs.conf file to keep the same format type). Rename to inputs.conf . <br/>
  <br/>
  > Add the following to the inputs.conf . <br/>
  <br/>
           > [WinEventLog://Security] <br/>
             index = win-security <br/>
             disabled = 0 <br/>
             <br/>
             <br/>
  > Open CMD. Navigate to cd C:\Program Files\SplunkUniversalForwarder\bin . Type splunk.exe restart to restart Splunk service with new settings. Wait until the CMD output state "Done!". <br/>
  <br/>
  > Go to Splunk Enterprise -> Search & Reporting -> Add index="win-security"  <br/>         
 <br/>
 <br/>



 Teneable Nessus  <br/>
  > Download Tenable Nessus version Ubuntu - amd64: https://www.tenable.com/downloads/nessus?loginAttempted=true  <br/>
  <br/>

  > Run: dpkg -i "Nessus-<version number>-debian6_amd64.deb" .   <br/>
  <br/>

  > Start Nessus: sudo systemctl start nessusd.service   <br/>
  <br/>

  > Navigate to the URL: http://localhost:8834 . Choose Nessus Essentials -> Register with a new account -> Set a username/password for the console -> Wait for plugins to setup.  <br/>

<br/>

</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
