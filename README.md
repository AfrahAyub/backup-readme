# aws-panw-vmseries-cft-deployment
This repository contains cloudformation templates to deploy the vm-series with gatewayloadbalancer lab on AWS

## Outline

![diagram](https://github.com/AfrahAyub/aws-panw-vmseries-cft-deployment/assets/93593501/abc4170d-dc38-4519-927b-a56bc5224527)

- aws-panw-gwlb-cfn-root.yaml. This is the root stack that will be deployed. The other template files are nested from this one.


You can set up this environment in the following way:

### Rapid S3 Setup

**Note:** You will need access to AWS CloudShell for this mode of setup.

1. Login to the AWS Console and change to the region of your choosing. Supported regions are:
    - eu-north-1
    - eu-west-1
    - us-east-1
    - us-east-2
    - us-west-1
    - us-west-2
2. Open AWS CloudShell, wait for the CLI prompt to show up.
3. Clone the github repository.
```
git clone https://github.com/AfrahAyub/panw-aws-jam-challenge-resources.git && cd panw-aws-jam-challenge-resources
```
4. Run the setup command.
```
./setup-cft.sh
```

Once the script completes execution, you should be able to see the output as shown below.
```
Setup completed successfully. Please proceed to CFT deployment.
Please use the below Template URL for CFT deployment.
TEMPLATE_URL = https://panw-aws-resources-506b9ea8-ce65-4416-8f5d-288991b33a30.s3.us-east-1.amazonaws.com/panw-vmseries-gwlb/aws-panw-gwlb-cfn-root.yaml
```
5. You can now deploy a CloudFormation Template stack using the S3 Template URL seen from the setup script output.
6. please create a new EC2 key pair in the region where you are going to deploy the setup script and once you have uploaded the setup script please rename the ec2 key pair and provide the name of the key-pair that you have generated


## please go through the following cases in order to run the use cases


## Case 1
## Task

**Redirect all outbound traffic from the Beer Store Data Database Server to the Palo Alto Networks Firewall**

1. First, login to the Firewall. (**Helpful Info Section**)

2. Check the Firewall Monitor traffic logs to verify if you can see any traffic from the Beer Store Data Database Server. ((**Helpful Info Section**)

3. Update AWS routing to redirect the Beer Store Data Database Server outbound traffic for inspection by VM-Series through the Transit Gateway. <br />
<br />

## Task Validation

- Once you made the appropriate changes in the AWS routing you can log into the **Beer Store Data Database Server** via the SSM service and test with the **curl** command if the EC2 instance has internet access.
  - example curl command **sudo curl www.google.de** 


- If the curl command isn't working in the **Beer Store Data Database Server**, check the Palo Alto Networks Firewall Monitor Logs to see which Application is now blocked from the Firewall. 

- You should see the following example log in the firewall monitoring. By adding the following filter **( zone.src eq internal ) and ( zone.dst eq external )** into the Monitor Logs filter bar.<br />
   Some fields in the example log were removed.
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task1-deny2.png" alt="VPC Logs" width="1500" /></p>
<br />

- Input the Name of the blocked Application in the answer field to complete the task. <br />
<br />

## Helpful Info
**To Login into the VM Series Firewall Web UI**
- Identify the Elastic IP (Security VM-Series Management) of the EC2 Instance named "Security VM-Series"
- Open a browser window and navigate to https://("Security VM-Series-EIP")
- Login with the following credentials:
  - Username: admin
  - Password: Pal0Alt0@123

**How to see the Traffic Logs inside the Firewall**
- Login into the firewall
- Inside the firewall navigate to Monitor -> Traffic
- See the following picture as an example <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/example.png" alt="Monitor Logs" width="1500" /></p>
- In the Monitor Traffic window change the refresh timer from **Manual** to **10 seconds** by clicking on the dropdown field on the top right as the picture below shows
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task1-10.png" alt="Monitor Logs" width="1500" /></p>


**Login into the Beer Store Data Database Server**
- Use the Session Manager to log into the Server
- The name of the VM is "Beer Store Data Database"

**How to find the server's private IP?**
- On the AWS Console go to EC2
- On the EC2 Dashboard click on Instances
- The following EC2 instances are used by the lab:
  - Beer Store Data Database
  - Beer Store Frontend Webserver
  - Security VM-Series (Palo Alto Networks Firewall)<br />
<br />


## Please go through this section in case you are not able to view the traffic
1:Did you check the VPC Route Table?
You should check if the Route Tables of the Beer Store Data VPC is pointing to the correct Ressource

2:Traffic still not showing in the Firewall?
If you still can't see the traffic in the Firewall monitoring. Please do the following:

1. Login into the AWS console
2. Go to VPC
3. Select in Filter by VPC field the "Beer Store Data VPC"
4. As next go to Route Tables and select the Beer Store Data Private route table
5. In the route table click on Routes (see below)
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task1-routes.png" /></p>
<br />
6. Click Edit routes and do the following changes:

  1. Remove the route 10.0.0.0/8 -> Target TGW
  2.  Change the route 0.0.0.0/0 -> TGW
  3. click Save

7. Once you made the changes your routle should looks like the example below
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task1-clue2-new.png" /></p>
<br />

## case 2
## Task
1. First check on the Firewall if you can see any logs between the Beer Store Data Database Server and the Beer Store Frontend Webserver. You can add the following filter into the Firewall Monitor **( zone.src eq internal ) and ( zone.dst eq internal )**
2. After seeing no Logs in the Firewall, you recongize that you have to solve some AWS routing issues. For any help, see Clue 1
3. After solving the AWS routing, you should be able to see the following Monitor Logs inside the Firewall
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task2-ssh-new.png" alt="SSH Logs" width="1500" /></p>
<br />

## Task Validation
- Once you made the appropriate changes in AWS check if can see now traffic between the **Beer Store Data Database Server** and the **Beer Store Frontend Webserver** by typing the following filter in the Firewall Monitor **( zone.src eq internal ) and ( zone.dst eq internal )**
- If you can see now the traffic between both EC2 Instances, type the Port number of the ssh application which is used for communication between both EC2 Instances.
<br />
<br />

## Helpful info
**How to see the Traffic Logs inside the Firewall**
- Login into the firewall
- Inside the Firewall, and Navigate to Monitor -> Traffic
- See the following picture as an example <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/example.png" alt="Monitor Logs" width="1500" /></p>
<br />

## Inventory
- Palo Alto Networks NGFW VM-Series
- Amazon EC2
- Amazon VPC
- AWS Systems Manager (SSM)
- AWS Lambda
- AWS Tranist Gateway
- AWS Gateway Load Balancer <br />
<br />

## please go through this section in case you are not able to view the logs
step 1:Did you looked into the TGW routing?
1. Login into the AWS console
2. Go to VPC Services and select under Transit Gateways the Transit gateway route tables
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task2-tgw-rt.png" /></p>
<br />

3. Select the Spoke TGW Route Table
4. In the Route table click on Propagations
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task2-tgw2.png" /></p>
<br />

5. Select each propagations one by one and click delete propagations. Repeat it until both are deleted.
6. Your TGW Route table should looks like the following after the deletion
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task2-clue1.png" /></p>
<br />

step 2:Can't find the logs inside the Firewall Monitor?
1. Log into the Palo Alto Networks VM-Series Firewall
2. Go to Monitor -> Traffic
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/example.png" /></p>
<br />
3. In the Filter field paste the the following filter ( zone.src eq internal ) and ( zone.dst eq internal ) and ( app eq ssh )
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task2-filter.png" /></p>
<br />

step 3:
1. In the Monitor logs use the same filter as in Clue 2 and have a look at the column TO PORT
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task2-clue3.png" /></p>
<br />


## case 3
## Background


## Task
1. You realized that you have no Inbound inspection on the Beer Store by looking into the Firewall monitor logs and adding the following filter  **(( zone.src eq frontend ) and ( zone.dst eq frontend )) or (( zone.src eq external ) and ( zone.dst eq internal ))**. 
2. You should now redirect the traffic from the Beer Frontend VPC to the Firewall. For help see Clue 1
3. Now after you redirect inbound traffic through the Firewall, you should connect to the **Beer Store Frontend Webserver** (HTTP) over the Public IP. You should be able to see the following Webpage. This is the entrypoint to the Log4j Attack. The attack is being automatically generated, you do not need to authenticate to the beer store frontend before proceeding to the next step
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/beerstore.png" alt="Beer Store" width="500" /></p>
   In the Firewall Monitor Logs, you should see the following log by entering the Filter ( addr.src in YOUR-PIP ). Replace "YOUR-PIP" with your local Public IP (Logs can take up to 1 min to be shown in the Monitor) <br />
   In case you still don't see any traffic logs, check the Internet Edge route table or check Clue 2
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task3-logs.png" alt="Logs" width="1000" /></p>
4. Next we will check the Firewall Monitor Threat logs, to see if any unexpected behaviour is happening. In the Threat Logs, you can see some **Log4j** Attacks. But for some reason, the Firewall isn't blocking them, just alerting. See the picture below as an example.
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/alert-log4j-new.png" alt="alert" width="1000" /></p>
5. Now you have to change/update the Threat Profile on the Security Policy to block/reset the vulnerable traffic. To perform the change on the Security Policy follow the instructions below:
   1. On the Firewall tab on the browser, navigate to the Policies tab, and select Secuirty on the left pane
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/pol1.png" width="1000" /></p>
   2. Now you can see all the Security rules. Click on the Rule **Allow inbound** frontend rule, and a new window will open
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/pol2.png" width="1000" /></p>
   3. On the new window click on **Actions** tab
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/pol3.png" width="500" /></p>
   4. On the **Profile Setting** section you can see that under **Vulnerability Protection**,the **alert** profile is selected. That profile will only alert and not block or reset any communication.
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/pol4.png" width="500" /></p>
   5. Change the **Vulnerability Protection** from **alert** to **strict**.
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/pol5.png" width="500" /></p>
   6. Click **OK**, and the window will close automatically.
   7. Next, we have to commit the changes you made to the firewall. Click on the **Commit** button in the top right corner.
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/pol6.png" width="500" /></p>
   8. A new window will open. Here you will have to click on **Commit** button
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/pol7.png" width="500" /></p>
   9. Wait for Status Complete and Result Successful and close the Window
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/pol8.png" width="500" /></p>
6. After changing the **Vulnerability Protection** Profile from **alert** to **strict** you should see the following logs under the **Threat section** of the logs
   <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/blocked-log4j-new.png" width="1000" /></p>
<br />
<br /> 

## Task Validation
- After you make the appropriate changes in AWS routing and on the Palo Alto Networks Firewall  it should have successfully blocked the attack to the **Beer Store Frontend Webserver** and you should be able to see a **reset-both** log entry in the Palo Alto Networks Monitor Logs -> Threat.
<br />
<br />

### Helpful info
How to see the Threat Logs inside the Firewall
- Log into the firewall
- Inside the Firewall Navigate to Monitor -> Threat
- See the following picture as an example <p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/threat.png" alt="Monitor Threat Logs" width="1000" /></p>
<br />

## please refer this section in case you are not able to view the threat
Clue 1:Did you checked the AWS route table?
1. Login into the AWS console
2. Go to VPC
3. Select in Filter by VPC field the "Beer Store Frontend VPC"
4. As next go to Route Tables and select the Beer Store Frontend Public route table
5. In the route table click on Routes (see below)
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task3-clue1-pup-rt.png" /></p>
<br />

6. Click Edit routes and do the following change:
 1. Change the route 0.0.0.0/0 -> Gateway Load Balancer Endpoint
 2. click Save

7. Once you made the changes your routle should looks like the example below
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task3-clue1.png" /></p>
<br />

Clue 2:Did you checked the Internet Route Table?
1. Login into the AWS console
2. Go to VPC
3. Select in Filter by VPC field the "Beer Store Frontend VPC"
4. As next go to Route Tables and select the Beer Store Frontend IGW Edge route table
5. In the route table click on Routes (see below)
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task3-clue2-igw-rt.png" /></p>
<br />

6. Click Edit routes and do the following change:
  - Add the route 10.1.2.0/24 -> Gateway Load Balancer Endpoint
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task3-clue2-igw-rt2.png" /></p>
<br />

  - click Save

7. Once you made the changes your routle should looks like the example below
<br />
<p><img src="https://aws-jam-challenge-resources.s3.amazonaws.com/panw-vmseries-gwlb/task3-clue2.png" /></p>
<br />
