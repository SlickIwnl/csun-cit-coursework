# 03: SSL Certificate and HTTPS

**Course:** CIT 270, Amazon Web Services  
**My screenshots and results:** Coming soon

## What I was setting up

The website worked over HTTP, so the next step was to configure Apache for my domain and add a certificate for HTTPS.

## Steps I followed

1. I started the AWS lab, checked that my domain's A record still pointed to my Elastic IP, and made sure the site loaded over HTTP.
2. I checked that Apache was running and enabled it to start when the instance booted:

```bash
sudo systemctl status httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

3. I got the `webserver.conf` file provided for the class, opened it on my computer, and changed `ServerName` to my own domain.
4. I copied the edited file to the instance with `scp` and my SSH key. Here is the PowerShell command layout, assuming `webserver.conf` and `labsuser.pem` are in Downloads:

```powershell
PS C:\Users\<your-name>> cd .\Downloads\
PS C:\Users\<your-name>\Downloads> ls
# Confirm webserver.conf and labsuser.pem are both here.
PS C:\Users\<your-name>\Downloads> scp -i .\labsuser.pem .\webserver.conf ec2-user@your-domain.example:~
```

> Just like SSH, these relative file paths work only when PowerShell is in the folder containing the files. I would use full paths if they were saved elsewhere.

5. On the instance, I copied the file into Apache's `conf.d` directory. I checked the configuration before restarting the service:

```bash
sudo cp ~/webserver.conf /etc/httpd/conf.d/webserver.conf
sudo apachectl configtest
sudo systemctl restart httpd
```

6. I installed Certbot and its Apache plugin, then ran Certbot for the site:

```bash
sudo dnf install -y certbot python3-certbot-apache
sudo certbot --apache
```

7. I followed the prompts for my domain and checked the site at `https://your-domain.example` to see whether the certificate was working. If the Apache config test or Certbot returned an error, I would check that error before rerunning anything.

## My documentation

- **Config changes I made:** [Add yours]
- **Screenshot of the HTTPS result:** [Add your own]
- **Certificate details and renewal:** [Add what you observed]
- **Problems I ran into and what I learned:** [Add your notes]

## References

- [CIT 270 SSL workflow I used for the lab sequence](https://github.com/Csimmons117/csun_cit/blob/main/03-AmazonWebServices270/03-SSL-cert.md)
- [AWS: SSL/TLS on Amazon Linux 2023](https://docs.aws.amazon.com/linux/al2023/ug/SSL-on-amazon-linux-2023.html)
- [Certbot: Apache instructions](https://certbot.eff.org/instructions?os=pip&ws=apache)
