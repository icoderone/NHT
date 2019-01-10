=====================
Foundation A Cluster
=====================

---------
Overview
---------
Note
Estimated time to complete: 120 MINUTES

In this exercise you will install a Foundation VM on a Single-Node Cluster, then foundation a 3-node cluster.

Table 1. IPs for Nodes:

+---------+---------------+----------------+---------------+
|Node	  |IPMI IP        |Hypervisor IP   |CVM IP         |
+=========+===============+================+===============+
| A       |10.21.xx.33	  |10.21.xx.25     |10.21.xx.29    |
+---------+---------------+----------------+---------------+
|B        |10.21.xx.34	  |10.21.xx.26     |10.21.xx.30    |
+---------+---------------+----------------+---------------+
|C        |10.21.xx.35	  |10.21.xx.27     |10.21.xx.31    |
+---------+---------------+----------------+---------------+
|D        |10.21.xx.36	  |10.21.xx.28     |10.21.xx.32    |
+---------+---------------+----------------+---------------+

* IPMI User:ADMIN, Password:ADMIN
* In following steps, you may replace ‘xx’ with your assigned cluster ID

---------
Staging your Environment
---------
A Hosted POC reservation provides a fully imaged cluster consisting of 4 nodes. To keep the lab self-contained within a single, physical block, you will:
•	Destroy the existing cluster
•	Create a single node "cluster" using Node D
•	Install the Foundation VM on Node D
•	Use Foundation to image Nodes A, B, and C and create a 3-node cluster
Using an SSH client, connect to the Node A CVM IP (e.g. 10.21.xx.29) in your assigned block using the following credentials:
•	Username - nutanix
•	Password - <HPOC Cluster Password>
Execute the following commands to power off any running VMs on the cluster, stop cluster services, and destroy the existing cluster:
acli -y vm.off \*
cluster stop        # Enter 'Y' when prompted to proceed
cluster destroy     # Enter 'Y' when prompted to proceed

---------
Create A Single-Node Cluster on Node-D 
---------
SSH to Node-D CVM, type ‘ssh nutanix@10.21.xx.32’ and enter
then execute following commands
RUN cluster -s 10.21.xx.32 create, then type ‘y’
RUN cluster status to check if Cluster is UP; RUN if not UP cluster start 
RUN ncli cluster edit-params new-name=POC0xx-D
RUN ncli cluster add-to-name-servers servers=10.21.253.10
RUN ncli cluster set-external-ip-address external-ip-address=10.21.xx.40
RUN ncli user reset-password user-name='admin' password=techX2019!
Install Foundation VM on Node-D Cluster
Create foundation image 
In Prism > Storage > Storage Pool, select default storage pool and click update, then rename it to ‘SP01’
Click + Storage Container to create a new container “Images”
 

Go to configuration page and navigate to Image Configuration, click Upload Image
Fill out the following fields and click Save:
•	Name – Foundation
•	Image Type – DISK
•	Storage Conainer – Images
•	URL – http://download.nutanix.com/foundation/foundation-4.3/Foundation_VM-4.3-disk-0.qcow2
 
---------
Create the Foundation VM on AHV
---------
In Prism > VM, click + Create VM
 
Click +Add New Disk
Fill out the following fields and click Add:
•	Type – Foundation
•	Operation – Clone from Image Service
•	Bus Type – SCSI
•	Image – Foundation

 
Click +Add New NIC, select primary network and click Add
 
Click Save at the bottom. After task complete, power on Foundation VM.
Foundation Nodes(A, B, C)
Launch Console of Foundation VM. Startup the Foundation VM and login with User: nutanix and Password: nutanix/4u
 
Click the time on top right, change the time zone of the Foundation VM to local time zone with User: root, Password: nutanix/4u
 

 
Access the Foundation VM IP if it has IP, if not, please set IP 10.21.xx.51 for Foundation VM
Double-click set_foundation_ip_address > Run in Terminal.
Select Device configuration and press Return.
 
Select eth0 and press Return.
 
Note
Use the arrow keys to navigate between menu items.
Replacing the octet(s) that correspond to your HPOC network, fill out the following fields, select OK and press Return:
•	Use DHCP - Press Space to de-select
•	Static IP - 10.21.xx.xx (Foundation VM IP)
•	Netmask - 255.255.255.128
•	Gateway - 10.21.xx.1
 

Note
The Foundation VM IP address should be in the same subnet as the target IP range for the CVM/hypervisor of the nodes being imaged. As Foundation is typically performed on a flat switch and not on a production network, the Foundation IP can generally be any IP in the target subnet that doesn't conflict with the CVM/hypervisor/IPMI IP of a targeted node.
Select Save and press Return.
 
Select Save & Quit and press Return.

---------
Running Foundation
---------

From within the Foundation VM console, launch Nutanix Foundation from the desktop.
Note
Foundation can be accessed via any browser at http://<Foundation VM IP>:8000/gui/index.html
On the Start page, fill out the following fields and click Next:
•	network – eth0
•	Select your hardware platform: Autodetect
•	Netmask of Every Hypervisor and CVM - 255.255.255.128
•	Gateway of Every IPMI - 10.21.xx.1
•	Netmask of Every IPMI - 255.255.255.128
•	Gateway of Every Hypervisor and CVM - 10.21.xx.1
 
Foundation will automatically discover any hosts in the same IPv6 Link Local broadcast domain that is not already part of a cluster.
Note
When transferring POC assets in the field, it's not uncommon to receive a cluster that wasn't properly destroyed at the conclusion of the previous POC. In that case, the nodes are already part of existing clusters and will not be discovered.
If nodes could not be discovered automatically, you can manually specify the MAC address of your assigned node. There are at least 2 methods to know MAC address remotely
Method.1 Identify MAC Address (BMC MAC address) of Nodes (A, B, C) by accessing IPMI IP for each node
Method.2 Identify MAC Address of Nodes (A, B, C) by login AHV host with User: root, Password: nutanix/4u for each node
Selecting NODE, click Range Autofill in drop-down list of Tools, replacing the octet(s) that correspond to your HPOC network, fill out the following fields and select Next:
•	IPMI IP - 10.21.xx.33
•	Hypervisor IP - 10.21.xx.25
•	CVM IP - 10.21.xx.29
•	Node A Hypervisor Hostname – POCxx-1
 
Fill out the following fields and click Next:
•	Cluster Name – POC0xx-ABC
•	Timezone of Every Hypervisor and CVM – your local timezone
•	Cluster Redundancy Factor - 2
•	Cluster Virtual IP - 10.21.xx.37
Cluster Virtual IP needs to be within the same subnet as the CVM/hypervisor.
•	NTP Servers of Every Hypervisor and CVM - 10.21.253.10
•	DNS Servers of Every Hypervisor and CVM - 10.21.253.10
DNS and NTP servers should be captured as part of install planning with the customer.
•	vRAM Allocation for Every CVM, in Gigabytes - 32
Refer to AOS Release Notes > Controller VM Memory Configurations for guidance on CVM Memory Allocation.
  

By default, Foundation does not have any AOS or hypervisor images. To upload AOS or hypervisor files, click Manage AOS Files.
 
Download your desired AOS package (5.8.2 in this lab) from the Nutanix Portal .
Note
If downloading the AOS package within the Foundation VM, the .tar.gz package can also be moved to ~/foundation/nos rather than uploaded to Foundation through the web UI. After moving the package into the proper directory, click Manage AOS Files > Refresh.

 


Fill out the following fields and click Next:
•	Select a hypervisor installer - AHV, AHV installer bundled inside the AOS installer
 
Select Fill with Nutanix defaults from the Tools dropdown menu to populate the credentials used to access IPMI on each node.

 
Click Start > Proceed and continue to monitor Foundation progress through the Foundation web console. Click the Log link to view the realtime log output from your node.
 
Note
Every AOS release contains a version of AHV bundled with that release.

When all CVMs are ready, Foundation initiates the cluster creation process.
 
Open https://< 10.21.xx.37>:9440 in your browser and log in with the following credentials:
•	Username - admin
•	Password – Nutanix/4u 
•	Change Password – techX2019!
 

Takeaways
•	Nutanix Foundation provides one click cluster installation.
•	Easy to setup and easy to use.


    
