# 02 — Apache Web Server

**Course:** CIT 270 — Amazon Web Services  
**My documentation:** In progress

## Goal

Install Apache and PHP on the EC2 instance from [01 — DNS and EC2](01-DNS.md), then confirm that a page loads through the domain.

## Steps

1. Connect to your Amazon Linux 2023 instance with the lab's SSH key.
2. Update packages and install Apache plus the PHP packages used in the class LAMP exercise:

```bash
sudo dnf upgrade -y
sudo dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
```

3. Check whether Apache is running. If it is inactive, start it. Enable it to start when the instance boots:

```bash
sudo systemctl status httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```

4. Visit `http://your-domain.example` and confirm that the server responds. Check the A record and HTTP security group rule if it does not.
5. Inspect the Apache document root, `/var/www/html`, and its permissions. The reference lab changed the owner of the `html` directory to `ec2-user` so the student could create files there:

```bash
ls -ld /var/www/html
sudo chown ec2-user /var/www/html
```

6. Create a temporary `index.php` inside `/var/www/html` containing the PHP test used in class:

```php
<?php
phpinfo();
?>
```

7. Reload your domain and confirm the PHP information page appears. Once you have recorded the result, remove or replace this test page because it exposes server configuration details.

## My work

- **Commands I ran:** [Add any changes from the example]
- **What the page showed:** [Add your own screenshot or description]
- **Problems and fixes:** [Add troubleshooting notes]
- **What I learned:** [Explain Apache, the document root, and PHP]

## References

- [Classmate's CIT 270 Apache workflow](https://github.com/Csimmons117/csun_cit/blob/main/03-AmazonWebServices270/02-Apache.md)
- [AWS: Install a LAMP server on Amazon Linux 2023](https://docs.aws.amazon.com/linux/al2023/ug/ec2-lamp-amazon-linux-2023.html)
