# AWS WAF & AWS Shield - Complete Theory and Creation Guide

# What is AWS WAF?

AWS WAF (Web Application Firewall) is a Layer 7 security service that protects web applications from common web attacks by inspecting HTTP and HTTPS requests before they reach your application.

AWS WAF filters incoming traffic based on rules that you configure.

---

# Why do we need AWS WAF?

Without AWS WAF

```text
Internet
     │
     ▼
Application
```

Any attacker can directly send malicious requests to the application.

Examples

- SQL Injection
- Cross Site Scripting (XSS)
- Bot attacks
- HTTP Flood
- Bad IP Requests

---

With AWS WAF

```text
Internet
     │
     ▼
AWS WAF
     │
     ▼
Application
```

AWS WAF analyzes every request.

If the request is malicious

❌ Block

If the request is legitimate

✅ Allow

---

# Features of AWS WAF

- Layer 7 Firewall
- SQL Injection Protection
- Cross Site Scripting (XSS)
- Rate Limiting
- Geo Blocking
- IP Allow List
- IP Block List
- Bot Protection
- Managed Rules
- Custom Rules
- Logging
- CloudWatch Integration

---

# Supported AWS Services

AWS WAF can be attached to

- CloudFront
- Application Load Balancer
- API Gateway
- AppSync
- Cognito
- App Runner
- Verified Access

---

# Components of AWS WAF

## Protection Pack (Web ACL)

Protection Pack is the new name for Web ACL.

It is a container that contains rules.

Example

```text
Protection Pack

│

├── Rule 1

├── Rule 2

├── Rule 3

└── Rule 4
```

---

## Rules

Rules inspect requests.

Example

```
Block SQL Injection
```

```
Block XSS
```

```
Rate Limit
```

```
Geo Block
```

---

## Managed Rules

AWS creates and maintains these rules.

Examples

- AWS Core Rule Set
- Known Bad Inputs
- SQLi Rule Set
- Linux Rule Set
- Amazon IP Reputation
- Anonymous IP List
- Bot Control

---

## Custom Rules

Created by the customer.

Examples

- Block Country
- Allow Office IP
- Block Admin URL
- Rate Limit
- CAPTCHA

---

# Types of Attacks

## SQL Injection

Example

```sql
' OR 1=1 --
```

Blocked by

AWS Managed SQLi Rule Set

---

## Cross Site Scripting (XSS)

Example

```html
<script>alert("Hacked")</script>
```

Blocked by

AWS Core Rule Set

---

## HTTP Flood

Thousands of requests

Blocked by

Rate Limit Rule

---

## Bot Attack

Automated requests

Blocked by

Bot Control

---

## Anonymous Users

Traffic from VPN

Proxy

TOR

Blocked by

Anonymous IP List

---

# AWS WAF Creation

## Step 1

Open AWS Console

Search

```
WAF & Shield
```

Open

```
AWS WAF
```

---

## Step 2

Click

```
Create Protection Pack (Web ACL)
```

---

## Step 3

Tell AWS about your application

App Category

```
Enterprise & Business Applications
```

App Focus

```
Web
```

---

## Step 4

Select Resource Type

If using CloudFront

```
CloudFront Distribution
```

If using Load Balancer

```
Application Load Balancer
```

Production Recommendation

```
CloudFront
```

because traffic is blocked at AWS Edge Locations.

---

## Step 5

Choose Initial Protections

Recommended

```
Essential Rules
```

or

```
Build Your Own
```

---

## Step 6

Enter Name

Example

```
prod-cloudfront-waf
```

Description

```
AWS WAF for Production CloudFront
```

---

## Step 7

Click

```
Create Protection Pack
```

---

# Managed Rules

Recommended

```
AWS Core Rule Set
```

```
Known Bad Inputs
```

```
SQLi Rule Set
```

```
Linux Rule Set
```

```
Amazon IP Reputation
```

```
Anonymous IP List
```

```
Bot Control
```

---

# Custom Rules

## Rate Limit

```
1000 Requests

5 Minutes

Action

Block
```

---

## Block Countries

Example

```
China

Russia

North Korea
```

Action

```
Block
```

---

## Allow Office IP

Create

IP Set

```
203.0.113.10/32
```

Action

```
Allow
```

---

## Block Admin URL

```
/admin
```

Action

```
Block
```

---

## CAPTCHA

```
/login
```

Action

```
CAPTCHA
```

---

# Recommended Rule Order

```
1. Block Countries

2. Rate Limit

3. Anonymous IP List

4. Known Bad Inputs

5. SQLi Rule Set

6. Linux Rule Set

7. AWS Core Rule Set

8. Bot Control
```

---

# Logging

AWS WAF supports

- CloudWatch Logs
- Amazon S3
- Kinesis Data Firehose

Logs include

- Source IP
- URI
- Country
- Headers
- Matched Rule
- Action

---

# AWS Shield

AWS Shield protects AWS resources from DDoS attacks.

---

# Types of AWS Shield

## AWS Shield Standard

Free

Automatically Enabled

Protects

- CloudFront
- Route53
- ALB
- NLB
- Global Accelerator

No manual setup required.

---

## AWS Shield Advanced

Paid

Additional Features

- Advanced DDoS Protection
- DDoS Response Team
- Cost Protection
- Attack Visibility
- Advanced Monitoring

---

# AWS Shield Standard Architecture

```text
Internet
      │
      ▼
AWS Shield Standard
      │
      ▼
CloudFront
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

# WAF vs Shield

| AWS WAF | AWS Shield |
|----------|------------|
| Layer 7 | Layer 3 & Layer 4 |
| SQL Injection | DDoS |
| XSS | SYN Flood |
| HTTP Flood | UDP Flood |
| Bots | Network Attacks |
| Rate Limit | Automatic Protection |

---

# Production Architecture

```text
                 Internet
                     │
                     ▼
                 Route53
                     │
                     ▼
                CloudFront
                     │
        AWS Shield Standard
                     │
                     ▼
                  AWS WAF
                     │
                     ▼
       Application Load Balancer
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

- Attach WAF to CloudFront.
- Use AWS Shield Standard for automatic DDoS protection.
- Enable AWS Managed Rule Groups.
- Add custom rules for Rate Limiting and Geo Blocking.
- Enable WAF logging.
- Use HTTPS with ACM.
- Monitor blocked requests using CloudWatch.

---

# Interview Questions

### What is AWS WAF?

AWS WAF is a Layer 7 Web Application Firewall that protects web applications from attacks such as SQL Injection, XSS, bots, and HTTP floods.

---

### Why attach WAF to CloudFront?

Because CloudFront blocks malicious traffic at AWS Edge Locations before requests reach the Application Load Balancer.

---

### What is AWS Shield Standard?

AWS Shield Standard is a free AWS service that automatically protects CloudFront, Route53, ALB, NLB, and Global Accelerator from Layer 3 and Layer 4 DDoS attacks.

---

### Do we need to configure AWS Shield Standard?

No.

AWS automatically enables Shield Standard for supported AWS services.

---

### Which is the best production architecture?

```text
Internet
      │
Route53
      │
CloudFront
      │
AWS Shield Standard
      │
AWS WAF
      │
Application Load Balancer
      │
EC2
      │
RDS
```
