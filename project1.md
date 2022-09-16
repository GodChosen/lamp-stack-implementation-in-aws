## LAMP STACK IMPLEMENTATION
To ensure a successful completion of this project, here are some prerequisites:
- Have VS Code and some of it's extensions installed.
- Create AWS account and launch an EC2 Instance
- Create a GitHub account and create a personal access token (PAT)
- Create OpenSSH key pair in your local system and use the public key to create SSH key in GitHub

Below are the steps I followed to implement a LAMP STACK in AWS
### 1.  Install Apache Server and Update the Firewall Rule
- Connect to the EC2 instance
On my local PC, I opened the folder created above and saved the downloaded key from AWS. Then I launched my VS Code, clicked on Terminal and ran the following commands.
```
# Grant permission to access and use the key
sudo chmod 400 devops-practice-key1.pem 

# ssh into the EC2 instance
ssh -i devops-practice-key1.pem ubuntu@ec2-3-89-222-110.compute-1.amazonaws.com
```

- Install Apache server
To install Apache server, I had to update the apt packages first before using it to install Apache. Below are the commands used: 
```
#update a list of packages in package manager
sudo apt update -y

#run apache2 package installation
sudo apt install apache2 -y

#confirm that apache is running
sudo systemctl status apache2
```
**Result screenshot:**
![](%https://github.com/GodChosen/lamp-stack-implementation-in-aws/blob/master/apache-running.PNG%)

- Confirm that apache server is rendering from within the EC2 instance
`curl http://localhost:80`
**Result Screenshot:**
![](%https://github.com/GodChosen/lamp-stack-implementation-in-aws/blob/master/working-apache.PNG%)

- Update the firewall rules
To enable users to access the web server over the internet, I had to open port 80 (for HTTP traffic) using the AWS console. I created a new inbound rule in the security group attached to the EC2 instance with port 80 open to traffic from anywhere.
**Result Screenshot:**
![](%https://github.com/GodChosen/lamp-stack-implementation-in-aws/blob/master/firewall-updated.PNG%)

- Test if you can access Apache server from the internet
To test, I used the command `curl -s http://169.254.169.254/latest/meta-data/public-ipv4` to obtain the public IP address of my EC2 instance and then navigated to that IP address using the web browser of my local system.
**Result Screenshot:**
![](%https://github.com/GodChosen/lamp-stack-implementation-in-aws/blob/master/apache-in-webbrowser.PNG%)




