# 04: RDS and phpMyAdmin

**Course:** CIT 270, Amazon Web Services  
**My screenshots and results:** Coming soon

## What I was setting up

This lab connected a MariaDB database in Amazon RDS to my Linux EC2 instance. I also put phpMyAdmin on the web server so I could manage the database through a browser. The web server was already set up with HTTPS in the [SSL certificate lab](03-SSL-cert.md).

## Create the database in RDS

1. I started the AWS Learner Lab, searched for **RDS**, and chose **Create database**.
2. I selected **MariaDB** as the engine and the class's database template (Free tier if that option was available). I gave the DB instance a name, kept or set the master username, and created a password. I won't put that password in this repo.
3. I created the DB instance in the same VPC as the EC2 web server and noted the RDS endpoint once it became available.
4. In the RDS security group, I allowed MariaDB/MySQL traffic on port **3306** with my EC2 instance's **security group as the source**. That let the web server connect without opening the database to everyone on the internet.

## Test the connection from EC2

1. I SSH'd into my Linux instance and installed the MariaDB client:

```bash
sudo dnf install -y mariadb105
```

2. I used the endpoint from the RDS page to connect. The `-p` option asks for the password without putting it in the command. This example shows where the prompt should appear, without including my password or claiming it is my saved output:

```console
[ec2-user@your-instance ~]$ mysql -h <your-rds-endpoint> -u <your-db-username> -p
Enter password:
MariaDB [(none)]> exit
```

3. I checked for the MariaDB prompt, then used `exit` when I was done. If it didn't connect, the first things to check were the endpoint, the RDS security group, and whether the database was running.

## Put phpMyAdmin on the web server

1. I checked the [official phpMyAdmin downloads page](https://www.phpmyadmin.net/downloads/) for the package used in class. The older class example used version 5.2.1, so I would choose the version actually assigned or currently available rather than hard-code that old download.
2. On the EC2 instance, I installed the PHP extensions needed by the AWS LAMP guide and restarted the services:

```bash
sudo dnf install -y php-mbstring php-xml
sudo systemctl restart httpd
sudo systemctl restart php-fpm
```

3. I downloaded and extracted phpMyAdmin into its **own folder** under `/var/www/html`. Keeping it in one folder made it easier to find and avoided moving unrelated files from my home directory into the website.
4. I opened the phpMyAdmin page over HTTPS and checked that it loaded. The next check was signing in with the RDS database details. I'll document the exact configuration and any login issue I hit here once I add my own notes.

## My documentation

- **RDS choices and EC2 connection test:** [Add your own details, without passwords or account IDs]
- **phpMyAdmin URL and screenshot:** [Add your own screenshot]
- **Login or connection troubleshooting:** [Add what happened and how you fixed it]
- **What I learned:** [Explain RDS, security groups, and why phpMyAdmin needs HTTPS]

When the lab is done, I stop the RDS instance if I don't need it running. A stopped RDS instance can restart automatically after seven days, and storage can still have charges, so I check its status again later.

## References

- [CIT 270 phpMyAdmin lab sequence](https://github.com/Csimmons117/csun_cit/blob/main/03-AmazonWebServices270/04-PHPMyAdmin.md)
- [AWS: Connect an EC2 instance to an RDS database](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/ec2-rds-connect.html)
- [AWS: Install a LAMP server on Amazon Linux 2023](https://docs.aws.amazon.com/linux/al2023/ug/ec2-lamp-amazon-linux-2023.html)
