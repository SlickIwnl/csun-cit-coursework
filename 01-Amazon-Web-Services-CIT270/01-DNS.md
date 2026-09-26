# 01 — DNS and EC2

**Course:** CIT 270 — Amazon Web Services  
**My documentation:** In progress

## Goal

Connect a domain to an Amazon Linux EC2 instance using Route 53, then log in to the instance over SSH.

## Steps

1. Register a domain for the class project. The reference lab used GoDaddy; record the registrar and domain you used without publishing account details.
2. Open the AWS Academy Learner Lab through the course page, start the lab, and enter the AWS console.
3. In **Route 53 → Hosted zones**, create a **public hosted zone** for the domain.
4. Copy the hosted zone's four **NS (name server)** values. At the domain registrar, replace its name servers with those values. DNS changes may take time to appear.
5. In **EC2**, launch an **Amazon Linux 2023** instance. The reference lab selected the class key pair named `vockey`. Use the key pair and instance settings assigned in your class.
6. In the instance's security group, allow HTTP (80) and HTTPS (443) for the website. Allow SSH (22) from your own IP or the range your class requires.
7. Find the instance's public IPv4 address. In the Route 53 hosted zone, create an **A record** for the root domain (leave the record name empty) pointing to that address. If the instance receives a different public IP later, update the record.
8. Download the Learner Lab's private key, if your lab provides one (the reference calls it `labsuser.pem`). Keep the key private. From your computer, connect using the instance's public DNS name or your domain:

```bash
ssh -i /path/to/labsuser.pem ec2-user@your-domain.example
```

9. Confirm that you reach the Amazon Linux shell. If the domain does not resolve yet, check the registrar's name servers, the A record, and DNS propagation.

## My work

- **Domain/registrar:** [Add yours if you want it public]
- **Instance configuration:** [Add AMI, instance type, and lab choices]
- **What I observed:** [Add DNS and SSH results]
- **Screenshots or diagram:** [Add your own files and links]
- **What I learned:** [Explain the roles of the registrar, Route 53, an A record, and EC2]

## References

- [Classmate's CIT 270 DNS workflow](https://github.com/Csimmons117/csun_cit/blob/main/03-AmazonWebServices270/01-DNS.md)
- [AWS: Configure DNS routing for a new domain](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring-new-domain.html)
- [AWS: Connect to a Linux instance with SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-linux-inst-ssh.html)
