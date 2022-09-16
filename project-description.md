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
<br />
![Apache Status](Screenshots/apache-running.PNG)

- Confirm that apache server is rendering from within the EC2 instance
`curl http://localhost:80`


**Result Screenshot:**
<br />
![Working Apache](Screenshots/working-apache.PNG)

- Update the firewall rules
To enable users to access the web server over the internet, I had to open port 80 (for HTTP traffic) using the AWS console. I created a new inbound rule in the security group attached to the EC2 instance with port 80 open to traffic from anywhere.

**Result Screenshot:**
<br />
![Security Group Firewall](Screenshots/firewall-updated.PNG)

- Test if you can access Apache server from the internet
To test, I used the command `curl -s http://169.254.169.254/latest/meta-data/public-ipv4` to obtain the public IP address of my EC2 instance and then navigated to that IP address using the web browser of my local system.

**Result Screenshot:**
<br />
![Apache Accessible from Internet](Screenshots/apache-in-webbrowser.PNG)

### 2.  Install MySQL Database
I followed the steps below to install and configure MySQL:

- use apt package installer to install MySQL
```
sudo apt install mysql-server -y
```

- Connect to the MySQL server as the administrative database user root
```
sudo mysql
```

- Set password for the system root user
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```

- Run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system.
```
sudo mysql_secure_installation
```

- To configure the VALIDATE PASSWORD PLUGIN, select Yes when prompted. If the feature is enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error.

- Set the MySQL root password.

- Press Y and hit the ENTER key when prompted to change the root password, remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.

- Test logging into the MySQL console
```
sudo mysql -p
```

**Result Screenshot:**
<br />
![MySQL Login Success](Screenshots/mysql-login.PNG)

**Note:** At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. For that reason, when creating database users for PHP applications on MySQL 8, you’ll need to make sure they’re configured to use mysql_native_password instead.








