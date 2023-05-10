# Active Directory With Bulk User Creation For A Business 

## Description

The walkthrough details the creation of an Active Directory Lab environment using VMWare. Initially, a Microsoft Server was set up to run Active Directory. Next, a Domain Controller was configured to run a domain. Then, a Powershell script was utilized to create over 1000 users in Active Directory. Afterwards, new accounts were logged into on another client that uses the domain to connect to the internet. The lab simulates a business environment and requires a Microsoft Server 2019 ISO, a Windows 10 Enterprise ISO, VMWare, and a Powershell script.

## Tools and Technologies: 

Active Directory
PowerShell
CMD

## Environments Used
VMWare
Microsoft Server 2019
Windows 10

## Links
VMWare: https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html
Microsoft Server 2019: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019
Windows 10 ISO: https://www.microsoft.com/en-us/software-download/windows10

## Project Network Diagram

<img src="https://i.imgur.com/IfxvoYS.png" height="80%" width="80%" alt="Network Diagram"/>
<br />
<br />

## Project Walk-through

To start, I will set up the Domain Controller virtual machine. I require two network adapters. The first adapter is NAT, which uses the IP address of my home router's host. The second adapter is an Internal Network Adapter that enables communication between the Domain Controller and other virtual machines. I'll be using VMnet0 for the Internal Network, and you can refer to the diagram at the beginning for more details.

<img src="https://i.imgur.com/UHBjxOd.jpg" height="80%" width="80%" alt="Configuring the Network Adapter for the Domain Controller Virtual Machine"/>
<br />
<br />
<br/>
<img src="https://i.imgur.com/7CLcFGU.jpg" height="80%" width="80%" alt="Configuring the Network Adapter for the Domain Controller Virtual Machine"/>
<br />
<br />
<b>Once the Windows Server 2019 is downloaded on the Virtual Machine, the initial step involves configuring the two network adapters. One adapter is external while the other is internal.</b> <br/>
<img src="https://i.imgur.com/SwAH8sj.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Now I need to determine which NIC is set up for NAT. Based on the fact that its DNS is set to localdomain, I can confirm that Ethernet0 is the NIC configured for NAT.</b> <br/>
<img src="https://i.imgur.com/JE7zCDh.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<img src="https://i.imgur.com/mjczWGf.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>I rename the adapters to make it easier to distinguish between them. This will be useful later on when configuring the Domain Controller and DHCP.</b> <br/>
<img src="https://i.imgur.com/iiYpjCy.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Next, I proceed to configure the internal network adapter by assigning it an IP address according to the provided diagram (172.16.0.1). Since the domain controller acts as the gateway, there's no need to assign a default gateway to this adapter. As for the DNS server, I assign it an IP address based on the diagram as well because when we install Active Directory, it automatically installs DNS. I set the DNS server address as a loopback address so it pings itself. This makes it easier for me to manage the DNS server in the future.</b> <br/>
<img src="https://i.imgur.com/JIPk50Q.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Having determined the external and internal network adapters, I proceed to simplify the PC name to "DC" (short for Domain Controller). Renaming the PC requires a restart, which poses no problem.</b> <br/>
<img src="https://i.imgur.com/TklrxzC.jpg" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />
<b>After the restart, I proceed to download Active Directory on the Domain Controller. Although the video was cut short, I was able to complete the download successfully.</b> <br/>

https://user-images.githubusercontent.com/108043108/176958623-4276b4d1-3c6e-469c-8875-49007d003aa2.mp4

<br />
<br />
<p align="center">
<b>I have installed Active Directory Domain Services on the server, but I still need to configure it as a domain.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176959446-c4992d67-9bdc-4a5b-8229-e6d18850e29d.mp4

<br />
<br />
<p align="center">
<b>After promoting the server to a domain, a restart is required. Once I log back in, it is evident that the domain has been successfully created as my admin account now displays MYDOMAIN in front of it.</b> <br/>
</p>
<img src="https://i.imgur.com/BmLk2Gc.jpg" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />
<p align="center">
<b>I will now create a dedicated domain Admin account instead of using the built-in Admin account. </b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176960581-a0934287-267c-464e-8eee-8b85ae523910.mp4

<br />
<br />
<p align="center">
<b>I created a domain-specific admin account, but it does not have administrative privileges. I need to go to Active Directory and elevate this new account to an Administrator. Once that is done, I log out of the built-in Admin account and log in to my newly created Domain Admin account with administrative privileges.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176961105-3f8df2e0-f1e2-490b-a457-e1dd3fc2470a.mp4

<br />
<br />
<p align="center">
<b>Next, I proceed to install and set up the RAS/NAT on the Domain Controller, which will enable my Windows 10 client computer to connect to the internet via the internal network using the Domain Controller as a gateway.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176961534-0afd1ae0-c421-4af7-ae76-7b2cefe43bdd.mp4

<br />
<br />
<p align="center">
<b>After installing the RAS/NAT role, the next step is to configure the Routing and Remote Access.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176961801-c6ed71d1-4f12-4775-82d5-853cac5260da.mp4

<br />
<br />
<p align="center">
<b>Excellent! Now that the Remote Access role has been installed and configured, we can proceed with setting up a DHCP server. This will enable our Windows 10 clients to receive IP addresses and access the internet.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176962485-229ae237-b9e9-4178-b857-4741d318d33e.mp4

<br />
<br />
<p align="center">
<b>Great! Now that the DHCP role is installed, it's time to configure it and set up a scope. DHCP allows computers on the network to automatically be assigned an IP address. In this case, I will be creating a scope that assigns IP addresses in the range of 172.16.0.100-200, effectively allowing the DHCP to give out 100 IP addresses. I will also set the lease duration to 20 days. The reason for the lease duration is that when an IP address is assigned, it can't be used by other devices. If all 100 IP addresses are used up and new devices try to connect, they won't be able to get an IP address and connect to the internet. The lease duration sets the amount of time that an IP address can be leased to a device before being recycled. For example, if this were a café that offered Wi-Fi and the average time a person spent inside the café was 2 hours, it would make no sense to lease an IP address to them for 20 days because that would effectively lock up that IP address for that amount of time, and no one else could use it. In that case, I would recommend setting the lease duration to under 4 hours and giving a larger range. Since this is just a VM, the lease duration doesn't matter..</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176962663-866da4fd-de06-4549-9b86-3925a674bb3d.mp4

<br />
<br />
<p align="center">
<b>To enable web browsing on the Domain Controller, I have to disable the security features temporarily. However, in a production environment, this would be a security risk and should never be done. Since this is only a lab environment, it is not a problem. Although it is still possible to browse the internet without disabling the security features, it can be annoying as it will trigger warning messages for every webpage visited.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176964588-4b7ab303-c338-4037-8142-996bde30cac3.mp4

<br />
<br />
<p align="center">
<b>After successfully configuring Active Directory and the Domain Controller, I proceed to use a Powershell script to create more than 1000 user accounts.</b> <br/>
</p>
<img src="https://i.imgur.com/ISI6fPb.jpg" height="80%" width="80%" alt="Using Powershell to create 1000 user accounts in bulk"/>
<br />
<br />
<p align="center">
<b>Here is a video of the script running!</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176953922-60b62f24-fd3f-41b4-ae0c-8780afc7b708.mp4

<br />
<br />
<p align="center">
<b> The PowerShell script has successfully created over 1000 user accounts, and the output confirmation shows that the accounts have been created correctly. However, there were some duplicate account names that were not created. To handle this issue, I can modify the script to include code that specifies what to do in case of duplicates, such as adding a number to the end of the account name. If you want to see the complete code, you can refer to the CREATE_USERS.ps1 file at the top of this repository.</b> <br/>
</p>
<img src="https://i.imgur.com/MhlDg1o.jpg" height="80%" width="80%" alt="Using Powershell to create 1000 user accounts in bulk"/>
<br />
<br />
<p align="center">
<b>Next, I proceed to create a new virtual machine named CLIENT1, which will serve as a user in the domain.</b> <br/>
</p>
<img src="https://i.imgur.com/wvBRBWf.jpg" height="80%" width="80%" alt="Setting up new Virtual Machine"/>
<br />
<br />
<p align="center">
<b>I reconfigured the network adapter of the CLIENT1 virtual machine to prevent it from being NAT and accessing the internet on the local network. The only way for this virtual machine to connect to the internet is by receiving an IP address from the Domain Controller running on the Server VM. As shown in the diagram, I changed the network adapter settings to make sure it is on the same internal network as the Domain Controller, which in this case is VMnet0.</b>  <br/>
</p>
<img src="https://i.imgur.com/6IjDUEj.jpg" height="80%" width="80%" alt="Configuring the VM Network Adapter"/>
<br />
<br />
<p align="center">
<b>After configuring a separate virtual machine to simulate an employee logging into the domain, I can rename the computer to CLIENT1 and join it to the mydomain.com domain by selecting the appropriate box. During this process, I am prompted to enter login credentials, and I choose to use the Administrator account that I set up earlier.</b>  <br/>
</p>
<img src="https://i.imgur.com/ceB3tDJ.jpg" height="80%" width="80%" alt="Configuring the Client VM"/>
<br />
<br />
<p align="center">
<b>I Successfully join the domain as a member!</b>  <br/>
</p>
<img src="https://i.imgur.com/euBgIXf.jpg" height="80%" width="80%" alt="Configuring the Client VM"/>
<br />
<br />
<p align="center">
<b>I logged into a user account that was created using the Powershell script to test if everything is configured correctly. Instead of logging into the user account that was created when the virtual machine was made, I tried to log into a user account that was created in the MYDOMAIN domain.</b>  <br/>
</p>
<img src="https://i.imgur.com/dPeaySX.gif" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<p align="center">
<b>I opened the command prompt to check if the client VM was assigned the IP address properly by the DC. The output showed that the client was successfully leased an IP address by the domain controller, as indicated by the red circle. When I tested the connection by pinging the domain, it worked, as indicated by the yellow circle.</b>  <br/>
</p>
<img src="https://i.imgur.com/QBWuCS9.jpg" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<p align="center">
<b>I perform a final test to ensure that the work environment and bulk user creation script is working. I log into the CLIENT1 virtual machine with a user account created by the Powershell script and confirm that I am able to access the internet through the network connection provided by the Domain Controller. I also verify that I am able to access shared folders on the Server VM and that the user account has the appropriate permissions to access them. With everything working as expected, I consider the lab environment set up successfully.</b>  <br/>
</p>
<img src="https://i.imgur.com/j6ZRHPz.jpg" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<p align="center">
<b>I return to the server VM and check the DHCP console to see how many IP addresses have been leased out. As seen in the red circle, the CLIENT1 virtual machine has been successfully leased an IP address. In a real company environment, there would be potentially hundreds or even thousands of leased addresses in this console, depending on the lease duration set. In this lab environment, I set the lease duration to 20 days.</b>  <br/>
</p>
<img src="https://i.imgur.com/qvNKa7v.jpg" height="80%" width="80%" alt="Checking leased addresses"/>
<br />
<br />
<p align="center">
<b>Here is an alternative method to check the number of devices connected to the domain. We can observe that the CLIENT1 computer is being accurately recognized in Active Directory. However, in a real-world scenario, this folder would likely contain thousands of devices.</b>  <br/>
</p>
<img src="https://i.imgur.com/A2dMovv.jpg" height="80%" width="80%" alt="Checking the computers in Active Dirctory"/>
<br />
<br />
<p align="center">
<b>Here, I am scrolling through all the user accounts I created with Powershell, and I can see that over 1000 have been successfully created. It is important to note that if this were a real production environment, the number of user accounts would vary depending on the size of the company and its workforce.</b>  <br/>
</p>
<img src="https://i.imgur.com/POpjnf9.gif" height="80%" width="80%" alt="Checking the Users created by Powershell"/>
<br />
<br />
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
