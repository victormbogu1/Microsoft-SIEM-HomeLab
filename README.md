# Microsoft Sentinel Live Attack Demonstration Home Lab
# Description
This project demonstrated the use of various Azure tools and resources to enhance security monitoring for an exposed VM. By leveraging Azure Log Analytics Workspace, Microsoft Defender for Cloud, and Azure Sentinel, I was able to collect, aggregate, and visualize attack data effectively. The use of PowerShell scripts to scan Event Viewer logs and send data to an external API for geolocation added an extra layer of analysis. This project provided valuable experience with SIEMs, cloud concepts, APIs, and Microsoft Azure. It was a fun and educational project, and I hope anyone reading this appreciates the work that went into it
# Utilities Used
 + Microsoft Sentinel (SIEM)
 + Log Analytic Workbooks
 + Microsoft Defender for Cloud
 + Virtual Machines
 + Remote Desktop
 + PowerShell
 + Event Viewer
 + API's
 + Firewalls
# Environments Used
 + Microsoft Azure
 + Windows 10 (21H2)
# Links
 + IPGeolocation: https://ipgeolocation.io/
# Program walk-through

[![Untitled-Diagram-drawio-2.png](https://i.postimg.cc/ryKYMHV7/Untitled-Diagram-drawio-2.png)](https://postimg.cc/Z0zLLVyL)

To kick off the project, I utilized my Microsoft Azure account to set up the required resources. I then registered on the IP Geolocation website to obtain an API key, enabling me to visualize the global locations of the attackers. With the API in hand, I began creating my virtual machines. Given that provisioning VMs can be a time-consuming process, I initiated it early, allowing it to run in the background while I moved on to the subsequent steps

[![Screenshot-2024-07-22-163111.png](https://i.postimg.cc/qBxzZLYY/Screenshot-2024-07-22-163111.png)](https://postimg.cc/QVV8Fpjk)

At this stage of the VM setup, it’s crucial to create a new Resource Group to organize and manage all the upcoming resources. In Azure, a Resource Group serves as a logical container, grouping related tools, services, and configurations together. This arrangement simplifies management, allowing for simultaneous creation or deletion of resources within the group, all sharing the same lifecycle. Resources outside this group will remain unaffected if the group is deleted. I named the Resource Group "Vic_Honeypot_Lab" and the virtual machine "honeypot-VM" for clarity.

I also selected a unique 40-character password with a mix of special characters, numbers, and both uppercase and lowercase letters to enhance security. Given that the exposed VM could attract brute force and dictionary attacks, a strong password was critical. For public inbound port rules, which dictate the open ports for VM connectivity, I configured access to allow only RDP (Remote Desktop Protocol) on port 3389. This ensures secure and controlled access to the VM.

[![Screenshot-2024-07-21-132429.png](https://i.postimg.cc/hts9Vn8C/Screenshot-2024-07-21-132429.png)](https://postimg.cc/hXJzqH5x)


The next action is to establish a new Network Security Group (NSG). An NSG functions like a Firewall, enabling the creation and enforcement of policies for managing both inbound and outbound traffic to Azure resources. For this task, I am opting not to impose any traffic restrictions. The goal is to permit unrestricted communication with the honeypot VM. There is a default inbound rule displayed in the images below; I will remove this rule and set up a new inbound rule to permit ALL traffic to the VM. In the Destination Port Ranges field, I specified an asterisk (*) to indicate any port range. I will also allow all protocols. I assigned a Priority of 100, as lower numbers indicate higher priority, ensuring this rule takes precedence over any conflicting rules. With these settings, all traffic will be permitted into our VM. It is important to note that such an approach would be highly inadvisable in a real production environment

[![Screenshot-2024-07-21-132724.png](https://i.postimg.cc/rwVVR2wF/Screenshot-2024-07-21-132724.png)](https://postimg.cc/crzyPj3V)

[![Screenshot-2024-07-21-132900.png](https://i.postimg.cc/y8LZkPBt/Screenshot-2024-07-21-132900.png)](https://postimg.cc/Hr7xNbr9)

[![Screenshot-2024-07-21-133.png](https://i.postimg.cc/G2mr9FL7/Screenshot-2024-07-21-133.png)](https://postimg.cc/2qMgKBBh)

[![Screenshot-2024-07-21-133430.png](https://i.postimg.cc/HkV0gR1j/Screenshot-2024-07-21-133430.png)](https://postimg.cc/87gfdy5V)

[![Screenshot-2024-07-21-133538.png](https://i.postimg.cc/HsQDtZhc/Screenshot-2024-07-21-133538.png)](https://postimg.cc/ykYb1PBs)

[![Screenshot-2024-07-21-133717.png](https://i.postimg.cc/FRRNWyLt/Screenshot-2024-07-21-133717.png)](https://postimg.cc/B8drt1pN)

As my VM is being provisioned, I can begin configuring the Log Analytics Workspace. These can be accessed through the Azure home dashboard or located via the search bar. I will create a Log Analytics Workspace with the name `loganalytic-honeyport` to store and analyze log data

[![Screenshot-2024-07-21-13413.png](https://i.postimg.cc/0Ncjt2r4/Screenshot-2024-07-21-13413.png)](https://postimg.cc/674BqKrV)

[![hh.png](https://i.postimg.cc/Kj4M73NQ/hh.png)](https://postimg.cc/zHZv8B4R)

I now proceed to Microsoft Defender for Cloud. This step is necessary to enroll in specific plans, which will enable data collection and aggregation for Microsoft Sentinel, to be utilized later. Additionally, I need to link my honeypot-VM to Microsoft Defender for data collection. Since my VM has been set up, it can now be connected to these services, known as Data Connectors. In the image below, I activate only the plans for Foundational CSPM and Servers. I am not using SQL servers, so that option remains disabled

[![Screenshot-2024-07-21-134735.png](https://i.postimg.cc/fygg5Xhb/Screenshot-2024-07-21-134735.png)](https://postimg.cc/xNMR1X8r)

[![Screenshot-2024-07-21-134840.png](https://i.postimg.cc/tJ27cqrJ/Screenshot-2024-07-21-134840.png)](https://postimg.cc/V00Yr8PP)


[![cool.png](https://i.postimg.cc/TPPv6BVy/cool.png)](https://postimg.cc/BjRV5Yws)


Now that my VM has been created, I can return to Log Analytics Workspaces and link my VM to send logs and security data to the Log Analytics workspace.


[![Screenshot-2024-07-21-1.png](https://i.postimg.cc/Z5rPf385/Screenshot-2024-07-21-1.png)](https://postimg.cc/21SBy19s)

[![Screenshot-2024-07-21-135959.png](https://i.postimg.cc/L5cfD6hP/Screenshot-2024-07-21-135959.png)](https://postimg.cc/QKqHxDQN)

I can now set up a Microsoft Sentinel resource and link it to my VM to activate SIEM capabilities

[![Screenshot-2024-07-21-140248.png](https://i.postimg.cc/P5zVRNYT/Screenshot-2024-07-21-140248.png)](https://postimg.cc/mPkScLm5)

With all configurations complete in the Azure dashboard, I can now access my VM to finalize its setup. The initial task is to obtain the VM’s public IP address to facilitate Remote Desktop (RDP) access. I navigate to the Virtual Machines section in Azure and locate the honeypot-VM, where I can find the highlighted public IP address

[![Screenshot-2024-07-21-140540.png](https://i.postimg.cc/FKHpCphV/Screenshot-2024-07-21-140540.png)](https://postimg.cc/9DsyrGRD)

At this point, I can access my VM using Remote Desktop. I launch the Remote Desktop application on my local PC, input the public IP address and necessary credentials, and establish the connection. Once connected, I proceed to configure the honeypot-VM

[![Screenshot-2024-07-21-140633.png](https://i.postimg.cc/Prn4nTtx/Screenshot-2024-07-21-140633.png)](https://postimg.cc/pyk8DN5b)

[![Screenshot-2024-07-21-141257.png](https://i.postimg.cc/XJ0J7HXw/Screenshot-2024-07-21-141257.png)](https://postimg.cc/WDXj57G3)

[![Screenshot-2024-07-21-141405.png](https://i.postimg.cc/3NwVmGwt/Screenshot-2024-07-21-141405.png)](https://postimg.cc/47rWTmVc)

[![Screenshot-2024-07-21-141530.png](https://i.postimg.cc/jjT0DGQd/Screenshot-2024-07-21-141530.png)](https://postimg.cc/mPm6qp50)

Once I have successfully logged into the VM via RDP, I proceed to open Event Viewer. This tool records all activities within a Windows system, assigning each action an EventID for easier navigation and browsing. For this project, we are specifically interested in EventID 4625, which corresponds to Failed Logon Attempts. These logs can be accessed under the Security tab. In the images below, I initiate another Remote Desktop session and attempt to log in with an incorrect password, resulting in a failed logon attempt that is subsequently recorded in Event Viewer

Next, I attempt to ping the VM from my local computer to verify direct connectivity and check if the VM is currently discoverable. The ping test fails, indicating that the VM is not discoverable, likely due to Windows Defender Firewall being enabled. The firewall is blocking Internet Contol Message Protocol (ICMP) Echo Requests, which makes the VM undiscoverable. This conclusion is drawn from the fact that pinging the VM's public IP address, 138.91.106.83, results in multiple "Request timed out" messages, indicating that the firewall is dropping the ICMP request packets

[![Screenshot-2024.png](https://i.postimg.cc/BvhjNcvD/Screenshot-2024.png)](https://postimg.cc/SXMQx9CN)

To ensure that my honeypot-VM is discoverable by anyone on the internet, I need to turn off the Windows Firewall. I accomplish this by searching for `wf.msc` in the Windows search bar. Once in the Windows Firewall settings, I proceed to disable all its features. Next, I open Command Prompt (CMD) on my local computer and ping the VM again. This time, I receive responses from the VM because the firewall is no longer blocking ICMP requests

[![Screenshot-2024-07-21-143800.png](https://i.postimg.cc/FHLkrJDX/Screenshot-2024-07-21-143800.png)](https://postimg.cc/yD78nxML)

[![Screenshot-2024-07-21-144004.png](https://i.postimg.cc/G2c4xgrb/Screenshot-2024-07-21-144004.png)](https://postimg.cc/PLRft4P7)

With my VM now accessible from the internet, I can proceed to set up my PowerShell script, which is central to this project. This script is designed to monitor Event Viewer specifically for EventID 4625, which corresponds to failed logon attempts. It will extract the IP addresses from these failed logons and send them to the IPgeolocation.io website to determine the geographic origin of the attempts.. This approach is used because the IP addresses recorded in Event Viewer lack geographical information. It is more efficient to leverage an external service to retrieve this data rather than building a geolocation feature from scratch. The PowerShell script will then receive the geographical data and save it as a string in a log file named `failed_rdp.log`. This log file will be used later in the project to visualize attacks in real-time using Microsoft Sentinel.

I start by opening PowerShell and pasting the script that I prepared before beginning this project. I save the script to the desktop with the name `Log_Exporter`. Next, I create an account on IPgeolocation.io to obtain my API key, which I then insert into the script. Note that the free API allows for 1,000 calls, which can be consumed quickly, potentially limiting the number of brute force attacks I can track. This setup is for implementation purposes; in a real company environment, all resources would be fully funded and provided

[![Screenshot-2024-07-21-15256y.png](https://i.postimg.cc/K8x42skB/Screenshot-2024-07-21-15256y.png)](https://postimg.cc/8jXTd4bz)

[![Screenshot-2024-07-21-152648.png](https://i.postimg.cc/ncth4w1g/Screenshot-2024-07-21-152648.png)](https://postimg.cc/nM0xZ2gG)

I execute my `Log_Exporter` script. The first image demonstrates that the script is operating correctly. The second image shows how the data is recorded in the `failed_rdp` log file in string format. This log file will be used later to train the AI in Log Analytics Workbooks and Microsoft Sentinel

[![Screenshot-2024-07-21-154050.png](https://i.postimg.cc/s2trVv6q/Screenshot-2024-07-21-154050.png)](https://postimg.cc/sB98mDR4)

[![Screenshot-2024-07-21-164247.png](https://i.postimg.cc/R08MNPFP/Screenshot-2024-07-21-164247.png)](https://postimg.cc/PLWG7Q1Z)

At this stage, the PowerShell script is functioning as intended, and I have intentionally triggered another failed logon attempt. It's evident that numerous individuals have already discovered my VM and are attempting to brute force it. Most of the attacks are originating from Taiwan. The rapid discovery of my VM was somewhat frustrating. I could have blocked the offending IP addresses or reactivated the firewall until my setup was complete, but the data gathered was valuable for training the AI in Azure, so I chose to let it continue.

The free API from IPgeolocation.io allows for only 1,000 calls, and this limit was reached quickly, impacting the project's progress. To continue receiving more attack data, I would need to pay $15 for an additional 150,000 API calls. However, since this is just a demonstration project to illustrate the implementation process, I will stick with the free API for now..

[![Screenshot-2024-07-22-190356.png](https://i.postimg.cc/zXZtjT6V/Screenshot-2024-07-22-190356.png)](https://postimg.cc/8s4mkf1S)

Now that my PowerShell script is functioning correctly, I proceed to Log Analytics Workbooks to create a custom log for importing my `failed_rdp` log into Log Analytics. In Log Analytics, I locate my VM and set up a Legacy Custom Log. The setup process requires a sample log file, which is stored within the VM.

Since I cannot directly download the log file from the VM to my local computer, I need to manually copy the log's contents. I access the logfile within the VM, copy the data, then return to my local computer. I open Notepad, paste the copied content, and save the file to my desktop. This file can then be imported into Log Analytics Workbooks. The sample data will be utilized to train Log Analytics

[![Screenshot-2024-07-21-165707.png](https://i.postimg.cc/wBZcBpd7/Screenshot-2024-07-21-165707.png)](https://postimg.cc/xkGbsB82)

[![Screenshot-2024-07-21-170021.png](https://i.postimg.cc/WbTmz8x6/Screenshot-2024-07-21-170021.png)](https://postimg.cc/yWp3rm1k)

Next, I need to provide the collection path. This is the location of the log file within the VM that Log Analytics will use to access the data. The correct path to the file is `C:\ProgramData\failed_rdp.log`. If this path is incorrect, Log Analytics will be unable to collect the log information.

Following this, I must assign a name to the custom log. I choose to name it `FAILED_RDP_WITH_GEO`. The `.CL` (Custom Log) extension will be automatically added to the name. This will essentially become the name of the table when querying the database later

[![Screenshot-2024-07-21-170711.png](https://i.postimg.cc/Jzhbcnrs/Screenshot-2024-07-21-170711.png)](https://postimg.cc/56ZYffFJ)

While the provisioning of the custom log is immediate, it takes some time for the data to sync from the VM to Log Analytics. In the meantime, I decided to query the Event Viewer, which should have already been synced. The picture confirms that it displays all the logs correctly.

After a while, I queried the newly created `FAILED_RDP_WITH_GEO` custom log, and it showed the expected information, indicating that the VM and Log Analytics are properly synced and exchanging data.

Instead of manually extracting the fields from the log for this project, I opted to run a script that directly transfers the data to Sentinel. This approach ensures that the data is correctly sent to Sentinel without needing to perform the field extraction manually

[![Screenshot-2024-07-21-171447.png](https://i.postimg.cc/nrZmsVSc/Screenshot-2024-07-21-171447.png)](https://postimg.cc/YGn0Z7wJ)

[![Screenshot-2024-07-uj.png](https://i.postimg.cc/5NPMk7Vs/Screenshot-2024-07-uj.png)](https://postimg.cc/SJz5nGj9)

[![Screenshot-2024-07-21-174720.png](https://i.postimg.cc/xCjJYpXc/Screenshot-2024-07-21-174720.png)](https://postimg.cc/2qJ87wFD)

The next step involves setting up a geomap to visualize and map the origins of the attacks or login attempts. I start by navigating to Microsoft Sentinel. In the initial image, I can see that the SIEM is correctly collecting and categorizing data. Although I did not configure any alerts for this project, it's certainly feasible and could be considered for future projects.

The picture reveals nearly 10,000 events and 7,700 security events sourced from Event Viewer on the VM, including 2,300 failed RDP attempts. Even though I haven’t completed the setup, the attackers from Taiwan were already busy trying to brute force into my VM. Best of luck to them!

[![Screenshotgu.png](https://i.postimg.cc/g0W4ypTQ/Screenshotgu.png)](https://postimg.cc/K10nF6FD)

Next, to create the desired map, I need to set up a new workbook in Sentinel. I start by accessing the Workbooks section. Since there are default graphs and widgets present, I first remove these, as they are not needed for the map I want to create. Once these elements are deleted, I can proceed to create a new workbook


[![Screenshot-2024-07-21-175217.png](https://i.postimg.cc/mgxSRRBk/Screenshot-2024-07-21-175217.png)](https://postimg.cc/ykPRXMJC)

[![Screenshot-2024-07-21-175321.png](https://i.postimg.cc/j5jHvSQg/Screenshot-2024-07-21-175321.png)](https://postimg.cc/06LKyx4D)

To create the map, I need to add a query to the workbook. This involves specifying the data and fields from Log Analytics that I want Microsoft Sentinel to use. Essentially, I'm defining the dataset that Sentinel will reference for generating the map

[![Screenshot-20.png](https://i.postimg.cc/Wz12ft5y/Screenshot-20.png)](https://postimg.cc/WFxLdpLm)

After querying the desired data points, the next step is to decide how to visualize them. In this case, I choose to display the data as a map. Utilized a visual map within Sentinel to monitor failed Remote Desktop Protocol (RDP) login attempts globally, identifying the geographic origins of potential threats. I select the map visualization option and configure it to plot attacks based on latitude and longitude. Although I could have used country-level data, some attacks did not include country information, so plotting by latitude and longitude is more accurate

I adjust the metric settings to size the bubbles according to event counts, meaning that a higher number of events results in a larger bubble. After applying these settings, the map is ready and displays the visual representation of the attacks.
[![Screenshot-2024-07-21-17582hgj.png](https://i.postimg.cc/9MJ2TCcY/Screenshot-2024-07-21-17582hgj.png)](https://postimg.cc/R662xr6W)


[![Screenshot-2024-07-21-184601.png](https://i.postimg.cc/sX8nbNmR/Screenshot-2024-07-21-184601.png)](https://postimg.cc/qhsX3QWm)

After a few hours, just before concluding the project, I observed a total of 3,600 attacks or failed login attempts, an increase from the previous 2,600. Notably, 94% of these attempts originated from Taiwan, indicating that automated brute-forcing software was being used to try numerous password and username combinations. Although there were additional attempts, my PowerShell script had to be restarted multiple times, which limited the total number of recorded attacks. Additionally, the free API has been exhausted, limiting further data collection.

This underscores the importance of using strong passwords and unique usernames to enhance security. As previously mentioned, I won't be able to track additional attacks due to the need for a paid API to continuously monitor attack origins. For this project, I utilized the free API calls, as it was primarily intended to demonstrate the utility and importance of Sentinel in organizational security.

[![Screenshot-2024-07-21-185530.png](https://i.postimg.cc/g0t46Tf6/Screenshot-2024-07-21-185530.png)](https://postimg.cc/wtJDC0x6)


[![Screenshot-2024-07-21-190248.png](https://i.postimg.cc/9M830bBx/Screenshot-2024-07-21-190248.png)](https://postimg.cc/S2M1tW38)


[![Screenshot-2024-07-21-194918.png](https://i.postimg.cc/Prb4MxgH/Screenshot-2024-07-21-194918.png)](https://postimg.cc/tYTWqXRv)


[![Screenshot-2024-07-21-195504.png](https://i.postimg.cc/Z5TLsmmd/Screenshot-2024-07-21-195504.png)](https://postimg.cc/QHy5VRH8)

# Solutions for Brute Force Attacks

 + Implementing Incident Response with Automated Playbooks: Configured automated playbooks in Sentinel to handle detected threats, facilitating prompt and efficient incident management.

 + Log Review: Conducted regular reviews of logs and alerts in the Log Analytics workspace to identify and address potential security incidents.

 + Strong Password Policies: Enforcing a rigorous password policy mandates the use of complex passwords that incorporate uppercase and lowercase letters, numbers, and special characters. This approach significantly enhances security and diminishes susceptibility to brute force attacks. Geographical Restrictions: Block or restrict access based on geographical locations that do not correspond to your organization's operational areas.

 + Account Lockout Mechanisms: Implement account lockout policies that temporarily disable accounts after a set number of failed login attempts, deterring repeated brute force attempts.

 + Regular Password Changes: Require periodic password changes to limit the lifespan of compromised credentials.

 + Monitoring and Logging: Continuously monitor login attempts and maintain detailed logs to identify and respond to potential brute force attacks promptly.

 + Security Awareness Training: Educate users about the importance of strong passwords and safe practices to prevent inadvertent security breaches.
