# Assignment 01 — Drupal on AWS

**Course:** CIT 270 — Amazon Web Services  
**Project website:** [drupal.ricky270.website](https://drupal.ricky270.website)  
**Status:** Completed and successfully verified during the lab (the AWS Academy lab may need to be running for the site to be available).

## Overview

For this assignment, I deployed a Drupal content management system (CMS) on AWS. I used an Amazon EC2 instance to run the web server and Drupal application, Amazon RDS for the database, and Amazon Route 53 to connect my domain to the server. I also configured HTTPS and worked through a few connection and installation issues.

This project helped me practice connecting different AWS services to host a working website.

## AWS services and software

| Component | What I used it for |
| --- | --- |
| Amazon EC2 | Hosted the Drupal application and Apache web server |
| Amazon Linux 2023 | EC2 operating system |
| Apache HTTP Server | Served the website and Drupal application |
| PHP 8.3 and Composer | Ran Drupal and installed PHP dependencies |
| Amazon RDS (MySQL-compatible database) | Stored Drupal's database |
| Amazon Route 53 | Managed the domain's DNS records |
| Namecheap | Registered `ricky270.website` and delegated DNS to Route 53 |
| HTTPS/TLS | Encrypted connections to the website |
| Drupal 11 | Content management system |
| Drush | Managed and verified Drupal from the command line |
| Amazon S3 | Used in separate CIT 270 coursework; not required for Drupal's runtime architecture |

## Architecture

```text
Browser
  |
  | HTTPS: drupal.ricky270.website
  v
Domain registrar: Namecheap
  | (nameservers delegated to Route 53)
  v
Amazon Route 53 (DNS)
  |
  v
Amazon EC2 (Amazon Linux 2023)
  |-- Apache HTTP Server
  |-- PHP 8.3 / Composer
  |-- Drupal 11 + Drush
  |
  | MySQL connection using SSL/TLS
  v
Amazon RDS (Drupal database)
```

## What I configured

### 1. Domain name and DNS

I registered the domain `ricky270.website` through Namecheap and configured its nameservers to use my Route 53 hosted zone. I used DNS records to point my Drupal subdomain at the EC2-hosted web server.

**Evidence to capture:** Route 53 hosted-zone record for `drupal.ricky270.website`, with sensitive account details hidden.

### 2. EC2 and the Apache web server

I launched an Amazon Linux 2023 EC2 instance, connected to it using SSH and a private `.pem` key, installed Apache, and configured the web server to serve my site. I assigned an Elastic IP to keep the server's public IP address consistent.

Useful verification commands:

```bash
sudo systemctl status httpd
sudo systemctl enable httpd
```

**Evidence to capture:** EC2 instance details, Apache service status, and web-server configuration (without secrets).

### 3. PHP and Drupal installation

I configured PHP 8.3 and Composer to support Drupal, created the necessary Drupal configuration and files directory, and completed the application setup.

Drupal's site URL is:

**https://drupal.ricky270.website**

I later used Drush to check that Drupal was installed and able to bootstrap successfully.

**Evidence to capture:** Public Drupal home page and successful Drush status output.

### 4. Database on Amazon RDS

I used an RDS-hosted MySQL-compatible database rather than storing the application database on the web server. I configured Drupal's `settings.php` database connection and resolved an SSL/TLS requirement from the database server.

During troubleshooting, the database enforced `require_secure_transport=ON`, which meant the application needed an encrypted connection and the appropriate CA certificate configuration.

The working configuration was verified with:

- Database driver: `mysql`
- Database name: `drupal_clean`
- Database connection: successful
- Drupal bootstrap: successful

I also tightened the RDS security-group access during grading feedback. **Security reminder:** the database port (3306) should normally permit traffic only from the appropriate EC2 security group or other explicitly authorized sources, not the public internet. The final security-group rules should be checked directly before calling them verified.

**Evidence to capture:** RDS connectivity details and security-group inbound rules, without displaying credentials.

### 5. HTTPS

I configured the website to use HTTPS so browsers could access it securely at `https://drupal.ricky270.website`.

**Evidence to capture:** Site loaded over HTTPS and certificate details (hide personal or account-sensitive fields as appropriate).

### 6. Drupal administrative access

After resolving installation and connection issues, I regained access to the Drupal admin account. The administrator username was `ricky`.

Do not put the password or the contents of `settings.php` in a public repository.

## Final verification

The following status was observed during the successful Drupal check:

```text
Drupal version   : 11.4.8
Site URI         : https://drupal.ricky270.website
DB driver        : mysql
DB name          : drupal_clean
Database         : Connected
Drupal bootstrap : Successful
Default theme    : olivero
Admin theme      : claro
PHP version      : 8.3.33
```

The configuration was working when tested. Availability after the AWS Academy lab is stopped or resumed needs to be checked again.

## Troubleshooting and what I learned

**Drupal could not connect to the database.** I checked the database host, port, settings, and security group. The RDS instance required encrypted transport, so I worked on the SSL configuration and CA bundle.

**Drupal installation/configuration needed adjustments.** I reviewed the web-server paths, Drupal files directory, `settings.php`, and database settings before the site successfully bootstrapped.

**Security-group permissions were too broad.** Feedback on my assignment noted overly open database access. The important lesson was to limit port 3306 to trusted application sources.

**AWS lab availability and cost.** Stopped lab resources can interrupt the website or database. I need to check the lab status before testing the site and avoid leaving chargeable resources running unnecessarily.

## Screenshots still needed

These are **not yet captured or uploaded**. I plan to use OpenCode with Playwright browser automation to collect real evidence from my own AWS environment.

- [ ] EC2 instance and associated networking
- [ ] Route 53 DNS record for the Drupal subdomain
- [ ] Apache verification
- [ ] RDS database details and authorized inbound access
- [ ] Drupal website loaded over HTTPS
- [ ] Drupal administrative dashboard (redacted where needed)
- [ ] Successful Drush status output

> The AWS console and resources may change between lab sessions. Do not invent screenshots, recreate resources, or start chargeable services just to fill this checklist.

## Next steps

Add verified screenshots under `screenshots/` and insert them beside the matching sections above. Review final RDS security-group rules and HTTPS status before treating them as current evidence.
