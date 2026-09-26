# 03 — SSL Certificate and HTTPS

**Course:** CIT 270 — Amazon Web Services  
**My documentation:** In progress

## Goal

Configure the Apache site for the class domain and obtain a certificate so the site loads over HTTPS.

## Steps

1. Start the AWS lab and check that the domain's **A record** points to the instance's current public IPv4 address. Load the page over HTTP first.
2. Check Apache. Start it if needed, and enable it for future boots:

```bash
sudo systemctl status httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

3. Download the course-provided `webserver.conf` from your regular class Canvas page, if your instructor supplied it. Edit its `ServerName` to use your own domain. Do not reuse another student's domain or config file.
4. From your computer, copy the edited file to your instance with the private key you used for SSH:

```bash
scp -i /path/to/labsuser.pem webserver.conf ec2-user@your-domain.example:~
```

5. On the instance, put the file in Apache's configuration directory. Check the configuration before restarting Apache:

```bash
sudo cp ~/webserver.conf /etc/httpd/conf.d/webserver.conf
sudo apachectl configtest
sudo systemctl restart httpd
```

6. Install Certbot and its Apache plugin. Package names can depend on the Amazon Linux image; the reference lab installed `certbot` and the Apache plugin through `dnf`:

```bash
sudo dnf install -y certbot python3-certbot-apache
sudo certbot --apache
```

7. Follow Certbot's prompts for **your domain**. If Apache's configuration test fails, read and fix the reported error before trying again.
8. Open `https://your-domain.example`. Confirm that the browser reports a valid certificate for your domain. Record how certificate renewal is configured and test it if your lab permits.

## My work

- **Domain and configuration choices:** [Add yours]
- **Certificate result:** [Add the issuer, expiration date, and your own screenshot]
- **Issues and fixes:** [Add any configuration or validation errors you encountered]
- **What I learned:** [Explain how DNS, Apache, and the certificate work together]

## References

- [Classmate's CIT 270 SSL certificate workflow](https://github.com/Csimmons117/csun_cit/blob/main/03-AmazonWebServices270/03-SSL-cert.md)
- [AWS: Configure SSL/TLS on Amazon Linux 2023](https://docs.aws.amazon.com/linux/al2023/ug/SSL-on-amazon-linux-2023.html)
- [Certbot: Apache instructions](https://certbot.eff.org/instructions?os=pip&ws=apache)
