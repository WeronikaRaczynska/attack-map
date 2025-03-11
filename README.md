# 🚀 Building a Honeypot + SOC in Azure

In this project, I set up a basic Security Operations Center (SOC) in the cloud using a free Azure subscription. 
The goal was to create a honeypot, expose it to the internet, and monitor real-world attacks while learning about log analysis and security monitoring.

# 🍯 Setting Up a Honeypot

The core idea was to set up a honeypot—a deliberately exposed virtual machine (VM) to attract attackers and log their activity. The process involved:

✔️ Creating a virtual machine in Azure

✔️ Opening it up to the public internet

✔️ Observing and analyzing incoming attacks

All failed attack attempts were logged and forwarded to a Security Information and Event Management (SIEM) system. This allowed for real-time monitoring, querying, and visualizing attacker locations on a map.

# ⚙️  Infrastructure Setup

To build the environment, I deployed:

✔️ Resource Group: A container to organize Azure resources

✔️ Virtual Network: A private network within Azure

✔️ Virtual Machine (VM): A cloud-based Windows instance

✔️ Network Security Group (NSG): A firewall managing inbound and outbound traffic

# 🔓 Configuring Network Security

To attract attackers, I configured the NSG to expose the VM:

1. Removed the default Remote Desktop Protocol (RDP) rule

2. Added a new inbound rule with the following settings:

    - Destination: Any
    
    - Service: Custom
    
    - Port: Any (*, meaning all ports open)
    
    - Protocol: Any
    
    - Priority: 100 (lower priority means the rule is evaluated sooner)
    
    - Name: danger (to indicate the rule is intentionally dangerous)

![image](https://github.com/user-attachments/assets/11e21cbe-638f-4926-afe6-ed50896828ef)

# 💻 Accessing the Virtual Machine and turning off Firewall

I connected to the VM using Remote Desktop Protocol (RDP) and verified its exposure by:

- Pinging the public IP from a local machine

![image](https://github.com/user-attachments/assets/5c17ae64-2242-43aa-a803-232e19c5a475)

- Disabling Windows Firewall to increase visibility

![image](https://github.com/user-attachments/assets/37695435-e6a3-4a11-8f21-a1789c3dfc78)

# 📝 Exploring Local Logs

To capture failed login attempts, I intentionally tried logging in with a fake account (e.g., employee). These failed attempts were recorded in Windows Event Viewer under:

📌 Event ID 4625 – Failed logon attempts

![image](https://github.com/user-attachments/assets/9f7b17c1-46f6-4c13-bd94-6c0d0c82c6d6)

# 🚨 Setting Up Microsoft Sentinel (SIEM)

1. Created a Log Analytics Workspace to store logs centrally

2. Installed Microsoft Sentinel for real-time security analysis

3. Connected the VM to Sentinel using the Azure Monitoring Agent

4. Configured a Data Collection Rule (DCR) to forward logs from the VM

![image](https://github.com/user-attachments/assets/6a97e106-80dd-41ef-b6eb-d6c35b0f016e)

# 📡 Monitoring Agent Installation and Log Forwarding

Once the Azure Monitoring Agent was installed, logs started flowing into Sentinel. To check log activity, I ran queries in the Log Analytics Workspace, filtering for Event ID 4625 (failed logins).

![image](https://github.com/user-attachments/assets/44a7e76e-2680-4601-a747-ec1619688278)

# 🗺️ Mapping IP Addresses to Physical Locations

To geolocate attackers, I uploaded an IP-to-location spreadsheet into Sentinel. This contained:

- IP ranges
  
- Latitude & longitude
  
- City & country information
  
Using this data, Sentinel automatically mapped attackers based on their IP addresses.

📌 Example: IP 185.56.73.169 mapped to Poland, identified as an ISP-owned block.

A watch list was created to store geographic information. 

![image](https://github.com/user-attachments/assets/6714c5ee-66d4-41db-87b2-20af0842d63d)

# 🔍 Using the Watch List to Look Up IP Addresses

After uploading the watch list, I ran a KQL query to join security logs with the location data, enriching logs with attacker city, country, and coordinates.

![image](https://github.com/user-attachments/assets/bec68670-b521-4b08-a6ee-a13588f08b9d)

# 🛡️ Creating an Attack Map in Sentinel 

To visualize attacks, I built a real-time attack map in Sentinel:

✔️ Created a new workbook

✔️ Used KQL queries to plot failed logins on a map

✔️ Applied a heatmap to highlight attack intensity

📌 The map updated dynamically, displaying real-time hacker activity worldwide.

![image](https://github.com/user-attachments/assets/5ece9dee-5856-4ded-8331-4d5ff124e2d4)

# 🗺️ Attack Map

The final attack map displayed:

✔️ Locations of attackers

✔️ Frequency of login attempts

✔️ Geographical distribution over time

![image](https://github.com/user-attachments/assets/4bd5c47d-6e8d-4bf2-8070-24d4c294e037)

As attack sources changed, the attack map evolved, displaying new data.

# ⏳ Map after 15h

![2map](https://github.com/user-attachments/assets/d1aa2d0e-7a02-43cf-bddb-56181350dedd)


# 📝 Final Recap

Key Takeaways from This Project

✅ Set up a honeypot in Azure

✅ Exposed the VM to attackers & logged failed login attempts

✅ Forwarded logs to Microsoft Sentinel (SIEM)

✅ Used KQL queries to analyze security events

✅ Built a real-time attack map to visualize hacker locations

💡 This project demonstrated how real-world cyber threats can be monitored using cloud-based SIEM solutions like Sentinel. 


