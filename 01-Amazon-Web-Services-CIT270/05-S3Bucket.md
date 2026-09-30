# 05: S3 Buckets

**Course:** CIT 270, Amazon Web Services  
**My screenshots and results:** Coming soon

## What I was setting up

This part of the class used S3 in two ways: a private bucket for files and a separate bucket for a static website. I kept them separate because the website files need public read access, while the backup files should stay private.

## Create an encrypted bucket and upload files

1. In **S3**, I chose **Create bucket** and picked a unique name for the private bucket.
2. Under **Default encryption**, I selected **server-side encryption with AWS KMS keys (SSE-KMS)** and chose the KMS key available for the lab. Then I created the bucket.
3. I opened the bucket and uploaded a small test file. I left its storage class as **S3 Standard** and checked the encryption settings on the object.
4. I uploaded a larger test file as well. The class example used an Alpine Linux download, but I'll list the file I actually used when I add my screenshots.
5. I checked the bucket's **Objects** and **Properties** pages to confirm both uploads and their settings.

## Add a lifecycle rule

1. In the private bucket, I opened **Management** and chose **Create lifecycle rule**.
2. I named the rule something like **Move to Glacier** and chose the scope assigned in class. The example applies it to all objects in the bucket.
3. Under lifecycle actions, I chose to transition **current versions** of objects to a Glacier storage class after **1 day**, then saved the rule.
4. I planned to check the objects again after the rule became eligible. The exact transition depends on the storage class and object size, so I'll add what actually happened instead of assuming every test file moved.

## Create a separate static website bucket

1. I chose a subdomain for the site, such as `s3-test.your-domain.example`. The **bucket name has to match the full domain or subdomain** I want to use for the website, including the dots.
2. I created a second bucket with that name. I downloaded the class's `S3Website.zip`, extracted it, and uploaded the website files themselves, including `index.html`.
3. In the website bucket's **Properties**, I enabled **Static website hosting** and set `index.html` as the index document.
4. In **Permissions**, I changed the bucket's Block Public Access settings for this website bucket and added a policy that grants **read access only** to its objects. I didn't put private backup files in this bucket. The policy pattern is:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::s3-test.your-domain.example/*"
    }
  ]
}
```

5. I tested the S3 **website endpoint** first. In Route 53, I created an **A alias record** for the subdomain and pointed it to the matching S3 website endpoint. I did **not** use the `1.1.1.1` value shown in the older example because that isn't the S3 website's address.
6. I opened the subdomain to confirm the page loaded. S3 website endpoints serve HTTP; HTTPS for a public site would require a service such as CloudFront in front of the bucket.

## My documentation

- **Private bucket and encryption:** [Add your settings and screenshots]
- **Files uploaded and lifecycle result:** [Add what you observed]
- **Website bucket and Route 53 record:** [Add your own bucket/subdomain and screenshots]
- **Problems and fixes:** [Add your notes]
- **What I learned:** [Explain S3 objects, encryption, lifecycle rules, and static hosting]

## References

- [CIT 270 S3 lab sequence](https://github.com/Csimmons117/csun_cit/blob/main/03-AmazonWebServices270/05-S3Bucket.md)
- [AWS: Default bucket encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/default-bucket-encryption.html)
- [AWS: S3 Lifecycle transitions](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-transition-general-considerations.html)
- [AWS: Route traffic to an S3 website bucket](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/RoutingToS3Bucket.html)
- [AWS: Website access permissions](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteAccessPermissionsReqd.html)
