# 01: DNS and EC2

**Course:** CIT 270, Amazon Web Services  
**My screenshots and results:** Coming soon

## What I was setting up

I wanted my class domain to point to an EC2 instance so I could use the domain to reach my web server and SSH into the machine.

## Steps I followed

1. I shopped around for a domain before buying one for the class project. I ended up using **Namecheap**, but any domain registrar would work for these steps. After that, I opened my AWS Academy Learner Lab and started the AWS lab.
2. In Route 53, I created a **public hosted zone** with my domain name. Route 53 gave me four name servers, so I went back to my domain registrar and changed the domain's name servers to those four values.
3. I launched an Amazon Linux EC2 instance with the key pair provided for the class. I allowed HTTP and HTTPS traffic for the website and set up SSH access from my IP.
4. I assigned an **Elastic IP** to my EC2 instance. Normally, the automatically assigned public IPv4 address can change when you stop and start an instance. Without an Elastic IP, I'd have to find the new public IP and update the A record in Route 53 every time the instance got a new address. Until I did that, my domain would still point to the old IP, including when I tried to SSH using the domain. The Elastic IP gave me one stable address to use, which saved me that headache. Simply logging in over SSH does not change the IP.
5. In my Route 53 hosted zone, I created an **A record** for the main domain. I left the record name blank and pointed it to the Elastic IP, not the instance's old automatically assigned public IP.
6. I downloaded the Learner Lab's private key (`labsuser.pem`) to my computer. Before running SSH, I changed my PowerShell directory to the folder where the PEM file was downloaded. In this example, it is **Downloads**. I used `ls` to make sure the key was there:

```powershell
PS C:\Users\<your-name>> cd .\Downloads\
PS C:\Users\<your-name>\Downloads> ls
# Check the listing for labsuser.pem before continuing.
PS C:\Users\<your-name>\Downloads> ssh -i .\labsuser.pem ec2-user@your-domain.example
```

> The `-i .\labsuser.pem` part looks for the key in the **current directory**. If I ran this command from another folder, SSH would not find that file and would give me an identity-file error. I could also use the PEM file's full path with `-i` instead of changing directories. I would replace the sample username, domain, and key name with my own values.

7. I checked that I could reach the Amazon Linux terminal. If the domain didn't work right away, I checked the name servers and A record, then gave DNS some time to update. I'll add my actual terminal output and screenshot when I document my session.

## My documentation

- **Domain and registrar:** [Add if you want these public]
- **EC2 setup and Elastic IP:** [Add your settings, but leave out account details]
- **Screenshots:** [Add your own]
- **What I learned or had to troubleshoot:** [Add your notes]

## References

- [CIT 270 DNS workflow I used for the lab sequence](https://github.com/Csimmons117/csun_cit/blob/main/03-AmazonWebServices270/01-DNS.md)
- [AWS: Configure DNS for a domain](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring-new-domain.html)
- [AWS: Associate an Elastic IP with an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-eips.html)
