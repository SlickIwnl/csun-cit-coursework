# 02: Apache Web Server

**Course:** CIT 270, Amazon Web Services  
**My screenshots and results:** Coming soon

## What I was setting up

After getting my domain and EC2 instance working, I set up Apache and PHP so the instance could serve a website.

## Steps I followed

1. I SSH'd into the Amazon Linux instance from the [DNS setup](01-DNS.md).
2. I updated the instance and installed Apache with the PHP packages for the class lab:

```bash
sudo dnf upgrade -y
sudo dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
```

3. I used `systemctl` to check Apache. It was installed, but I needed to start it. I also enabled it so it could start when the instance booted:

```bash
sudo systemctl status httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```

4. I opened my domain over HTTP to see whether the server responded. If it hadn't loaded, I would have checked that Apache was running and that the security group allowed port 80.
5. I looked at `/var/www/html`, the folder Apache serves files from. For this lab, I changed ownership of the `html` directory so `ec2-user` could add a page:

```bash
ls -ld /var/www/html
sudo chown ec2-user /var/www/html
```

6. I created an `index.php` file in that folder to test PHP:

```php
<?php
phpinfo();
?>
```

7. I reloaded the site and checked for the PHP information page. After documenting the result, I would remove or replace that test page because it shows details about the server.

## My documentation

- **Commands or settings I changed:** [Add yours]
- **Screenshot of the site:** [Add your own]
- **What worked or gave me trouble:** [Add your notes]
- **What I learned about Apache and PHP:** [Add your explanation]

## References

- [CIT 270 Apache workflow I used for the lab sequence](https://github.com/Csimmons117/csun_cit/blob/main/03-AmazonWebServices270/02-Apache.md)
- [AWS: Install a LAMP server on Amazon Linux 2023](https://docs.aws.amazon.com/linux/al2023/ug/ec2-lamp-amazon-linux-2023.html)
