# VPC-Peering

VPC Peering
VIRTUAL PRIVATE CLOUD


Created this on Frankfurt region N.Virginia REGION

VPC Peering
CASE Scenario 

WEB-SERVER		DB-SERVER			APP-SERVER
172.16.0.0/16		192.168.0.0/16		10.0.0.0/16
SUBNET 1			SUBNET 2			SUBNET 3 				
USA-EAST-1A		USA-EAST-1B		USA-EAST-1C

STEP 1
FOR App_Server
CHANGE THE REGION OF THE VPC TO ANOTHER VPC REGION. USING N.Virginia REGION
Serach for VPC
Create VPC  and More
Name-tag “App_Server”
IPv4    = 10.0.0.0/16
Number of Availability zone select 1a
Number of public subnets 1
Number of private subnets 0
Create VPC


STEP
REPEAT AGAIN FOR Web_Server
CHANGE THE REGION OF THE VPC TO ANOTHER VPC REGION. N.Virginia REGION
Serach for VPC
Create VPC  and More
Name-tag “Web_Server”
IPv4    = 172.16.0.0/16
Number of Availability zone select 1b
Number of public subnets 1
Number of private subnets 0
Create VPC

GO TO YOU VPC
You see App_Server and Web_server
Copy the App_Server vpc  ID “vpc-090d1946339bf9681”
Go To Security Group
Paste on search vpc-090d1946339bf9681
Click on security group  id
Click on edit inbound rule
Delete any rule found then 
Click on Add
Select from drop-down All Traffic , source select Anywhere IPv4
Save rules


GO BACK TO YOUR VPC 
You see App_Server and Web_server
Copy the Web_Server vpc  ID “vpc-0115a57c07aa59136”
Go To Security Group
Paste on search vpc-0115a57c07aa59136
Click on security group  id
Click on edit inbound rule
Delete any rule found then 
Click on Add
Select from drop-down All Traffic , source select Anywhere IPv4
Save rules

NOTE ONLY ONE WAS USE FOR THE SECURITY GROUP CONFIGURATION WHICH IS Web_Server



STEP 3
LAUCH INSTANCE
Create EC2 Instance
Make sure the vm is created in N.Virginia REGION
Name “App_Server
OS - Ubuntu
Create Key-pair
Network setting click on Edith
Select from drop-down for VPC and select App-server
Select from drop-down for Subnetnand select App-server
Auto-assign public IP SELECT Enable
Firewall(secuity group) SELECT Select exiting security  group
Select from Drop-Down Common Security groups
Click on Lauch Instance


LAUCH INSTANCE
Create EC2 Instance
Make sure the vm is created in N.Virginia REGION
Name “Web_Server
OS - Ubuntu
Create Key-pair
Network setting click on Edith
Select from drop-down for VPC and select Web-server
Select from drop-down for Subnet nand select Web-server
Auto-assign public IP SELECT Enable
Firewall(secuity group) SELECT Select exiting security  group
Select from Drop-Down Common Security groups
Click on Lauch Instance


VPC FOR DB_Server
CHANGE THE REGION OF THE VPC TO ANOTHER VPC REGION. USING Ohio USA
Serach for VPC
Create VPC  and More
Name-tag “DB_Server”
IPv4    = 192.168.0.0/16
Number of Availability zone select 1b
Number of public subnets 1
Number of private subnets 0
Create VPC

Step

GO BACK TO YOUR VPC 
Ohio USA REGION
Copy the DB_Server vpc  ID “vpc-0115a57c07aa59136”
Go To Security Group
Paste on search vpc-0115a57c07aa59136
Click on security group  id
Click on edit inbound rule
Delete any rule found then 
Click on Add
Select from drop-down All Traffic , source select Anywhere IPv4
Save rules


STEP

Create EC2 Instance
Make sure the vm is created Ohio REGION
Name “DB_Server
OS - Ubuntu
Create Key-pair
Network setting click on Edith
Select from drop-down for VPC and select DB-server
Select from drop-down for Subne tCnand select DB-server
Auto-assign public IP SELECT Enable
Firewall(secuity group) SELECT Select exiting security  group
Select from Drop-Down Common Security groups
Click on Lauch Instance



STEP 
Connect both Servers on N.Virginia
web_server and App_Server on the Terminal 

Try and ping both together
Run code for web_server 
Sudo su -
Ping 10-0-4-192

Run code App_Server
Sudo su -
Ping 172-16-4-93

YOU NOTICE IS NOT PEERING

GO BACK TO AWS CONSOLE
NEED TO CREATE PEERING CONNECTION BETWEEN THEM
Search for Peering on N.Virginia
Click on create peering connection
Enter “NAME “    App_Web
VPC ID (Requester) SELECT (App_Sever_Vpc)
Select another VPC to Peer with
Account > My account 
Region > this Region (us-east1)
SELECT from DropDown > Vpc (web-server-vpc)
Click Create Peering Connection

NOTE THIS, THE  IMMEDIATELY YOU FINISHED PEERING CONNECTION 
Click on Actions
Click on Accept Request


NEXT
Search for Route Tables       Region N.Virginia 
Click on route tables
Click on App_Server-rbt-public
Click on Routes
Click on Edith routes
Click on Add 1Web_Server  172.16.0.0/16   target = Peering connection  > select from drop-down > per
Click on Save changes


NEXT 
Search for Route Tables       Region N.Virginia 
Click on route tables
Click on Web_Server-rbt-public
Click on Routes
Click on Edith routes
Click on Add App_Server  10.0.0.0/16   target = Peering connection  > select from drop-down > per
Click on Save changes



GO TO THE TERMAINAL 
Ping enter the 172.16.0.0/16
Ping enter 10.0.4.192


DB_VPC PEERING
GO TO Ohio Region for the Peering Connection
Search for Peering connection
Click on create Peering connection
VPC ID (Requester) SELECT (DB_Sever_Vpc)
Select another VPC to Peer with
Account > My account 
Region > this Region (usEast(N.Virginia)(us-east-1)

GO TO YOUR VPCs to COPY VPC ID FOR App-Server-vpc and Paste
SELECT from DropDown > VPC ID  FOR App_Serve-Vpc (vpc-0adfb90843563f13b)
Click Create Peering Connection

NEXT
NOTE THIS, THE  IMMEDIATELY YOU FINISHED PEERING CONNECTION FOR DIFFERENT REGIONS
YOU NEED TO GO THE REGION YOU ADDED WHICH IS N.Virginia
Click on Peering connection. ( status is “Pending acceptance”
Click on Actions
Click on Accept Request. ( status is “Active”)


NEXT
Go to Route Table
Click on DB_Server-rtb-public
Click on Routes
Click on Edith Route
Click on Add Route 10.0.0.0/16    target peering connection  (app_server)
Save changes


NEXT
GO TO ROUTE TABLE FOR N.VIRGINIA REGION
Select App_Server-rtb-public
Click on Routes
Click on Edith Routes
Click on Add route ENTER THE DB_SERVER 192.168.0.0/16 Target is Peering Connection select the first pair not Web_App
Save changes
 
TRY PING DB_Server to App_server

NEXT 
Create a Peering Connection between  Web-DB servers
Click on  create peering connection
VPC ID (Requester) SELECT (Web_Sever_Vpc)
Select another VPC to Peer with
Account > My account 
Region > this Region (usEast(Ohio)(us-east-1)

GO TO YOUR VPCs to COPY VPC ID FOR DB_Server-vpc and Paste
SELECT from DropDown > VPC ID  FOR DB_Serve-Vpc (vpc-0adfb90843563f13b)
Click Create Peering Connection

NEXT
NOTE THIS, THE  IMMEDIATELY YOU FINISHED PEERING CONNECTION FOR DIFFERENT REGIONS
YOU NEED TO GO THE REGION YOU ADDED WHICH IS Ohio USA
Click on Peering connection. ( status is “Pending acceptance”
Click on Actions
Click on Accept Request. ( status is “Active”)
EDIT THE NAME OF THE PEERING CONNECTIONS
 

NEXT
Go to Route Table
Click on Web_Server-rtb-public
Click on Routes
Click on Edith Route
Click on Add Route 192.168.0.0/16    target peering connection  (DB_server)
Save changes


NEXT
Go to Route Table
Click on DB_Server-rtb-public
Click on Routes
Click on Edith Route
Click on Add Route 172.16.0.0/16   target peering connection  (Web_server)
Save changes



CONNECT THE THREE SERVER 
App_Server with Web_Server ping  10.0.4.192
Web_Server with DB_Server ping  172.16.4.93
DB_Server with App_Server ping  192.168.3.203 
