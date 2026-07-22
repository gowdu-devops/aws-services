# AWS Certificate Manager (ACM) - Complete Notes

# What is AWS ACM?

AWS Certificate Manager (ACM) is a service that allows you to provision, manage, and automatically renew SSL/TLS certificates for supported AWS services.

It enables secure HTTPS communication between clients and your applications.

---

# Why Do We Need ACM?

Without ACM

```text
User
 │
 ▼
HTTP
 │
 ▼
Application
```

Problems

- Data travels unencrypted.
- Vulnerable to man-in-the-middle attacks.
- Browsers may mark the site as "Not Secure".

---

With ACM

```text
User
 │
 ▼
HTTPS
 │
 ▼
Application
```

Benefits

- Encrypts data in transit.
- Secures sensitive information.
- Builds user trust.
- Automatic renewal for AWS-issued public certificates.

---

# Features

- Free AWS-issued public SSL/TLS certificates
- Automatic renewal
- DNS and Email validation
- Easy integration with AWS services
- Supports imported certificates

---

# Types of Certificates

## Public Certificate

Used for public websites.

Example

```
https://www.example.com
```

---

## Private Certificate

Issued through AWS Private CA for internal applications.

Example

```
https://internal.example.local
```

---

# Services That Support ACM

- CloudFront
- Application Load Balancer (ALB)
- Network Load Balancer (TLS listeners)
- API Gateway
- App Runner
- Elastic Beanstalk (through supported integrations)

> ACM certificates cannot be attached directly to EC2 instances.

---

# ACM Creation

## Step 1

Open AWS Console

Search

```
Certificate Manager
```
<img width="940" height="505" alt="image" src="https://github.com/user-attachments/assets/a5734cfa-d929-43fd-8c2f-955969104031" />

---

## Step 2

Click

```
Request Certificate
```
<img width="940" height="284" alt="image" src="https://github.com/user-attachments/assets/516746f1-365f-4748-b26a-4e745d3bd519" />
<img width="940" height="260" alt="image" src="https://github.com/user-attachments/assets/ff6e0fb0-7b97-4a91-9c39-0666f592a43c" />
<img width="940" height="402" alt="image" src="https://github.com/user-attachments/assets/2081a97e-88b2-427e-bc5b-7efdb9a8b132" />

---

## Step 3

Choose

```
Public Certificate
```
<img width="940" height="228" alt="image" src="https://github.com/user-attachments/assets/91130812-0c17-4a9b-989c-603adbc0701b" />

Click

```
Next
```
<img width="940" height="151" alt="image" src="https://github.com/user-attachments/assets/4f6e20cb-730c-4a88-95b7-708af6b1025b" />

---

## Step 4

Enter Domain Name
<img width="940" height="228" alt="image" src="https://github.com/user-attachments/assets/d0a48c98-8550-4055-a9db-dc4160f89140" />

Example

```
example.com
```

or

```
*.example.com
```

Wildcard certificates secure multiple subdomains such as:

- app.example.com
- api.example.com
- dev.example.com

---

## Step 5

Validation Method

Choose

```
DNS Validation
```
<img width="940" height="319" alt="image" src="https://github.com/user-attachments/assets/f425b378-083c-4b38-924d-6381d75c34e3" />
<img width="940" height="525" alt="image" src="https://github.com/user-attachments/assets/7be608d5-ce33-4890-9b40-95fedef2b7a1" />
Recommended because it supports automatic renewal.

(Email validation is also available but is less commonly used.)

---
<img width="940" height="475" alt="image" src="https://github.com/user-attachments/assets/17e09df2-3a48-41f8-b95d-380b8890c314" />
<img width="940" height="505" alt="image" src="https://github.com/user-attachments/assets/e1d43230-9db0-422e-ac11-1b96a305410e" />


## Step 6

Create DNS Record

If your DNS is hosted in Route 53:

Click

```
Create records in Route 53
```
![Uploading image.png…]()

ACM creates the validation record automatically.

After DNS validation completes:

```
Status

Issued
```

---

# Attach ACM to CloudFront

CloudFront

↓

Distribution

↓

Settings

↓

Custom SSL Certificate

↓

Select ACM Certificate

> **Important:** The ACM certificate used with CloudFront **must be created in the `us-east-1` (N. Virginia) region**, regardless of where your application runs.

---

# Attach ACM to ALB

EC2 Console

↓

Load Balancers

↓

Select ALB

↓

Listeners

↓

HTTPS (443)

↓

Choose ACM Certificate

↓

Save

The ACM certificate for an ALB must be in the **same AWS region** as the ALB.

---

# HTTPS Flow

```text
User
 │
 ▼
HTTPS
 │
 ▼
CloudFront
 │
 ▼
ACM Certificate
 │
 ▼
AWS WAF
 │
 ▼
ALB
 │
 ▼
EC2
```

---

# Production Architecture

```text
                 Internet
                     │
                     ▼
                 Route53
                     │
                     ▼
          CloudFront + ACM
                     │
        AWS Shield Standard
                     │
                     ▼
                  AWS WAF
                     │
                     ▼
       Application Load Balancer
             (HTTPS + ACM)
                     │
                     ▼
               Target Group
                     │
                     ▼
                  EC2 / EKS
                     │
                     ▼
                     RDS
```

---

# Best Practices

- Use DNS validation.
- Use wildcard certificates when appropriate.
- Use HTTPS everywhere.
- Use CloudFront with ACM.
- Use an HTTPS listener on the ALB.
- Renew imported certificates before they expire.
- For CloudFront, always request the certificate in `us-east-1`.

---

# Interview Questions

## What is ACM?

AWS Certificate Manager is a managed service that provisions, manages, and automatically renews SSL/TLS certificates for supported AWS services.

---

## Can ACM be attached directly to an EC2 instance?

No.

ACM certificates cannot be attached directly to EC2 instances. They are attached to supported AWS services such as CloudFront and Application Load Balancers.

---

## Which validation method do you prefer?

DNS Validation.

It supports automatic renewal and is the recommended option for production.

---

## Which region should be used for a CloudFront ACM certificate?

```
us-east-1 (N. Virginia)
```

---

## Which region should be used for an ALB ACM certificate?

The same AWS region where the ALB is deployed.

---

## Production Interview Answer

"In production, I request a public ACM certificate, validate the domain using DNS validation, attach the certificate to the CloudFront distribution (certificate in us-east-1) and to the HTTPS listener of the Application Load Balancer (certificate in the ALB's region). This enables secure HTTPS communication while ACM automatically renews AWS-issued public certificates."
