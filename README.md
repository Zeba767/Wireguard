Setting up an EC2 instance with a WireGuard VPN and configuring a client to connect to the server
Part 1: Launch and Configure the EC2 Instance
Step 1: Launch an EC2 Instance
•	Log in to AWS Management Console:
•	Navigate to the AWS Management Console and log in.
 ![image](https://github.com/user-attachments/assets/0e4d4933-544b-46a5-bbf6-d1ea48bba167)

•	Launch a New EC2 Instance:
•	Go to the EC2 dashboard
•	Click on “Launch Instance.”
•	Choose an AMI: Select Ubuntu Server 24.04 LTS.
•	Instance Type: Choose t2.micro (free tier eligible).
•	Generate a new key pair.
•	Configure Security Group: (Add a rule to allow SSH (port 22) for your IP).
•	Launch a EC2 instance.

![image](https://github.com/user-attachments/assets/d99bf01c-6e10-4993-af58-1b858940ba78)


•	Connect to the instance, copy the ssh key, and open the terminal to connect to the EC2 instance.
![image](https://github.com/user-attachments/assets/70f652dd-293a-4b98-9169-7e2ea165571b)

•	EC2 instance is connected.
![image](https://github.com/user-attachments/assets/0ac114b2-9fa1-40ba-ac85-cce0307c622d)
 
Step 2: Install WireGuard on the EC2 Instance
•	Update the System:-Commands to update the package lists and upgrade installed packages
o	sudo apt-get update- (Updates the list of available packages and their versions.)
o	sudo apt-get upgrade –y (Upgrades all installed packages to their latest versions.)
![image](https://github.com/user-attachments/assets/5ed8c8f0-02e9-454d-8e69-048a95347232)
 
•	Install WireGuard:
o	sudo apt-get install -y wireguard-( Installs the specified package)
 ![image](https://github.com/user-attachments/assets/7709468d-1af0-47e3-a651-e714c97c6bc0)

Step 3: Configure WireGuard on the EC2 Instance
•	Generate WireGuard Keys:
o	sudo mkdir -p /etc/wireguard (Creates the directory)
o	cd /etc/wireguard (change the directory)
o	umask 077- (Sets the file creation mode to ensure that the keys are only readable by the file owner.)
o	sudo wg genkey | sudo tee server_private.key | sudo wg pubkey | sudo tee server_public.key (Generate Wireguard key)
•	Verify File permission
o	sudo ls -l /etc/wireguard/
![image](https://github.com/user-attachments/assets/beaee6e3-cdd5-4e20-8732-b923b0cd287c)
 	
•	Create the WireGuard Configuration File:
o	sudo nano /etc/wireguard/wg0.conf –(command open the file)
 ![image](https://github.com/user-attachments/assets/3c515993-0899-454d-8a37-a0281757fb0e)

•	Enable IP Forwarding:- to allow traffic to pass through server
•	Enable the IP forwarding and loads the system configuration so that changes take effect immediately
 ![image](https://github.com/user-attachments/assets/7b0315a3-6e45-473a-a111-adddf50e71dc)
•	Set Up Firewall Rules:- Add firewall rules to allow traffic through the VPN
 ![image](https://github.com/user-attachments/assets/fcc87a36-7ce7-4f95-95bf-6aa6d58a1c4f)

•	Start and Enable WireGuard:
o	sudo systemctl start wg-quick@wg0 –(Start wireguard interface)
o	sudo systemctl enable wg-quick@wg0rrrr
Part 2: Configure the WireGuard Client
Step 4: Generate Client Keys
•	Generate Keys on the Client Machine:
 ![image](https://github.com/user-attachments/assets/39d5aaae-1f2f-4841-9d4b-b044efef22ce)

Step 5: Configure the WireGuard Client
•	Create Client Configuration File:
o	nano client-wg0.conf 
 ![image](https://github.com/user-attachments/assets/913af608-0bb3-4cbb-a99e-92401396cdb5)

•	Set Up the Client:
o	sudo cp client-wg0.conf /etc/wireguard/wg0.conf
o	sudo wg-quick up wg0
Part 3: Testing and Verification
Step 6: Test the Connection
•	Ping the WireGuard Server:
o	ping 10.0.0.1
 ![image](https://github.com/user-attachments/assets/cd38f1e8-285e-4314-b446-37fa44d5d3fa)
![image](https://github.com/user-attachments/assets/fc5e9fc8-e65d-4798-8fca-e7fc31e1d43f)

•	Verify Internet Access Through VPN:
o	curl http://ifconfig.me

![image](https://github.com/user-attachments/assets/68ba86c8-d86f-4384-83a8-4b2640cf1d00)
![image](https://github.com/user-attachments/assets/1326ed9e-b263-47cf-a7ec-3d06449b3b8b)
 
 
