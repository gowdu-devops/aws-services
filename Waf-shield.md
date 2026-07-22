# AWS WAF Complete Setup Notes

## 1. Open AWS WAF
- AWS Console → **WAF & Shield**
- Click **Create protection pack (Web ACL)**.

![Step1](image24.png)

## 2. Tell us about your application
- **App Category:** Enterprise & business applications
- **App Focus:** Web

## 3. Select resource type
For CloudFront:
- Choose **CloudFront distribution**
- Region scope: **Global**

## 4. Initial protections
Choose **Essential rules** or **Build your own**.

Recommended managed rules:
- AWS Core Rule Set
- Known Bad Inputs
- SQLi Rule Set
- Linux Rule Set
- Anonymous IP List
- Amazon IP Reputation
- Bot Control (optional, paid)

## 5. Name the Protection Pack
Example:
- Name: `prod-cloudfront-waf`
- Description: AWS WAF Web ACL for CloudFront Production

## 6. Create the Protection Pack
Click **Create protection pack (web ACL)**.

## 7. Manage Rules
Recommended order:
1. BlockCountries
2. RateLimit
3. Anonymous IP List
4. Known Bad Inputs
5. SQLi Rule Set
6. Linux Rule Set
7. Core Rule Set
8. Bot Control

## 8. Architecture

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
EC2 / EKS
   │
RDS
```

## AWS Shield Standard
- Automatically enabled.
- No manual creation required.
- Protects CloudFront, Route53, ALB, NLB.

## ACM
- Create certificate.
- CloudFront certificate must be in us-east-1.
- ALB certificate must be in the ALB region.

## Interview Answer
"In production I configure Route53, CloudFront, Shield Standard, WAF, ACM and ALB. WAF is attached to CloudFront to stop Layer 7 attacks at the edge before traffic reaches the origin."
