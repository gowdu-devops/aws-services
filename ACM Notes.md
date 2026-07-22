# AWS Certificate Manager (ACM) - Theory & Creation

# What is AWS ACM?

AWS Certificate Manager (ACM) is a service used to create, manage, and automatically renew SSL/TLS certificates for AWS services.

It helps secure websites and applications by enabling HTTPS.

---

# Why Do We Need ACM?

Without ACM

```text
User
   │
HTTP (Port 80)
   │
Website
```

Problems

- Data is not encrypted.
- Passwords and sensitive information can be intercepted.
- Browser shows "Not Secure".

---

With ACM

```text
User
   │
HTTPS (Port 443)
   │
AWS ACM Certificate
   │
Website
```

Benefits

- Encrypts data
- Secure communication
- Trusted by browsers
- Automatic renewal (AWS-issued public certificates)

---

# Features

- Free Public SSL Certificate
- Automatic Renewal
- DNS Validation
- Email Validation
- Wildcard Certificate Support
- Integration with AWS Services

---

# AWS Services Supported

- CloudFront
- Application Load Balancer (ALB)
- API Gateway
- App Runner
- Network Load Balancer (TLS)

> ACM certificates cannot be attached directly to EC2 instances.

---

# Types of Certificates

## Public Certificate

Used for

```
example.com
```

```
www.example.com
```

---

## Wildcard Certificate

Used for

```
*.example.com
```

Covers

- app.example.com
- api.example.com
- dev.example.com

---

# Validation Methods

## DNS Validation (Recommended)

- Easy
- Automatic Renewal
- Best for Production

---

## Email Validation

AWS sends an email to the domain owner for approval.

---

# ACM Creation (AWS Console)

## Step 1

Login to AWS Console

Search

```
Certificate Manager
```

Open

```
AWS Certificate Manager
```

---

## Step 2

Click

```
Request Certificate
```

---

## Step 3

Select

```
Request a public certificate
```

Click

```
Next
```

---

## Step 4

Enter Domain Name

Example

```
example.com
```

or

```
*.example.com
```

Click

```
Next
```

---

## Step 5

Choose Validation Method

Select

```
DNS Validation
```

Click

```
Next
```

---

## Step 6

Review Details

Click

```
Request
```

---

## Step 7

Validate Domain

If using Route53

Click

```
Create records in Route53
```

AWS automatically creates the DNS validation record.

Wait until Status becomes

```
Issued
```

---

# Attach ACM to CloudFront

Open

```
CloudFront
```

↓

Select Distribution

↓

Edit

↓

Alternate Domain Name

↓

Select ACM Certificate

↓

Save

> CloudFront requires the ACM certificate to be created in **us-east-1 (N. Virginia)**.

---

# Attach ACM to ALB

Open

```
EC2
```

↓

Load Balancers

↓

Select ALB

↓

Listeners

↓

HTTPS : 443

↓

Choose ACM Certificate

↓

Save

> For ALB, the ACM certificate must be in the **same AWS region** as the ALB.

---

# Architecture

```text
Internet
     │
     ▼
Route53
     │
     ▼
CloudFront
     │
 ACM Certificate
     │
     ▼
AWS WAF
     │
     ▼
Application Load Balancer
     │
     ▼
EC2
```

---

# Best Practices

- Use DNS Validation.
- Use HTTPS only.
- Use Wildcard Certificates if multiple subdomains exist.
- Use CloudFront with ACM.
- Use HTTPS Listener on ALB.
- Monitor certificate expiry for imported certificates.

---

# Interview Questions

### What is AWS ACM?

AWS Certificate Manager is a service used to provision, manage, and automatically renew SSL/TLS certificates for supported AWS services.

---

### Why do we use ACM?

To enable HTTPS and encrypt communication between users and applications.

---

### Can ACM be attached to EC2?

No.

It can only be attached to supported AWS services such as CloudFront, ALB, API Gateway, and App Runner.

---

### Which validation method is recommended?

DNS Validation because it supports automatic renewal.

---

### Which region should be used for a CloudFront certificate?

```
us-east-1 (N. Virginia)
```

---

### Which region should be used for an ALB certificate?

The same region where the Application Load Balancer is deployed.

---

# Summary

- ACM provides SSL/TLS certificates.
- Enables HTTPS.
- Supports automatic renewal.
- Use DNS validation for production.
- Attach certificates to CloudFront or ALB, not directly to EC2.
