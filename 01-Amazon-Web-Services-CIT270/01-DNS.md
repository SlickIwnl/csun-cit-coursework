# 01: DNS and EC2

**Course:** CIT 270, Amazon Web Services  
**My screenshots and results:** Coming soon

## What I was setting up

I wanted my class domain to point to an EC2 instance so I could use the domain to reach my web server and SSH into the machine.

## Steps I followed

1. I got a domain for the class project, then opened my AWS Academy Learner Lab and started the AWS lab.
2. In Route 53, I created a **public hosted zone** with my domain name. Route 53 gave me four name servers, so I went back to my domain registrar and changed the domain's name servers to those four values.
3. I launched an Amazon Linux EC2 instance with the key pair provided for the class. I allowed HTTP and HTTPS traffic for the website and set up SSH access from my IP.
4. I assigned an **Elastic IP** to my EC2 instance. Normally, the automatically assigned public IPv4 address can change when you stop and start an instance. That would mean checking the new address and fixing my DNS record before using the domain again. The Elastic IP gave me one stable address to use, which saved me that headache. Simply logging in over SSH does not change the IP.
5. In my Route 53 hosted zone, I created an **A record** for the main domain. I left the record name blank and pointed it to the Elastic IP, not the instance's old automatically assigned public IP.
6. I downloaded the Learner Lab's private key and kept it on my computer. Then I used it to SSH into the instance. Once DNS was working, the command looked like this:

```bash
ssh -i /path/to/labsuser.pem ec2-user@your-domain.example
```

7. I checked that I could reach the Amazon Linux terminal. If the domain didn't work right away, I checked the name servers and A record, then gave DNS some time to update.

## My documentation

- **Domain and registrar:** [Add if you want these public]
- **EC2 setup and Elastic IP:** [Add your settings, but leave out account details]
- **Screenshots:** [Add your own]
- **What I learned or had to troubleshoot:** [Add your notes]

## References

- [CIT 270 DNS workflow I used for the lab sequence](https://github.com/Csimmons117/csun_cit/blob/main/03-AmazonWebServices270/01-DNS.md)
- [AWS: Configure DNS for a domain](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring-new-domain.html)
- [AWS: Associate an Elastic IP with an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-eips.html)
