# N-11-Block-google.com-Access-in-VPC-using-DNS-Firewall
Objective  Create a VPC with internet access, verify connectivity to google.com, then block the domain using DNS Firewall and confirm access is denied

Part 1 – Create Network with Internet Access
1️⃣ Create VPC

Go to VPC → Create VPC

Name: DNS-Firewall-VPC
IPv4 CIDR: 10.0.0.0/16

2️⃣ Create Public Subnet

Name: Public-Subnet
CIDR: 10.0.1.0/24
Enable Auto-assign Public IP


3️⃣ Create Internet Gateway (IGW)
Create IGW → Name: DNS-IGW
Attach to VPC


4️⃣ Create Route Table
Name: Public-RT
Add Route:
Destination: 0.0.0.0/0
Target: Internet Gateway
Associate with Public Subnet


💻 Part 2 – Launch EC2 and Test Internet

5️⃣ Launch EC2 Instance
Amazon Linux
Choose created VPC and Public Subnet
Enable Auto-assign Public IP
Create key pair
Security Group:
Allow SSH (22) from your IP


6️⃣ Connect to EC2
ssh -i key.pem ec2-user@<Public-IP>

7️⃣ Test Internet Access
ping google.com

You should get replies.
This confirms internet is working.


🔥 Part 3 – Configure DNS Firewall to Block google.com
Now we block domain-level access.

8️⃣ Create Domain List
Go to:
Route 53 → DNS Firewall → Domain lists
Create domain list
Name: Block-Google
Enter one domain per line:
google.com
Create


9️⃣ Create Rule Group
Go to:
DNS Firewall → Rule groups
Create rule group
Name: Block-Google-RuleGroup
Add rule:
Choose Custom Domain List
Select: Block-Google
Action: Block
Add rule


🔟 Associate Rule Group with VPC
Select Rule Group
Associate VPC
Choose: DNS-Firewall-VPC


🧪 Part 4 – Test Blocking
Go back to EC2:
ping google.com

Now it should fail with:
Temporary failure in name resolution

That means DNS query is blocked successfully.

📌 Important Concept
Here’s what this really means:


Internet Gateway allows internet traffic.
But DNS Firewall blocks domain name resolution.
So even though internet exists, domain cannot be resolved
IP access may still work unless separately restricted.
