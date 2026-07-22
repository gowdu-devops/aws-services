# AWS WAF Complete Setup Notes

## 1. Open AWS WAF
- AWS Console → **WAF & Shield**
- Click **Create protection pack (Web ACL)**.

<img width="940" height="403" alt="image" src="https://github.com/user-attachments/assets/5a2fe253-4349-4cbd-9bcf-b4d283acdb2b" />

<img width="940" height="421" alt="image" src="https://github.com/user-attachments/assets/8c1ffb35-a3ed-42f6-9c6e-f2579f3e8085" />

## 2. Tell us about your application
- **App Category:** Enterprise & business applications
- **App Focus:** Web
<img width="940" height="446" alt="image" src="https://github.com/user-attachments/assets/ffcea180-1c88-4c06-b5fa-34ae2c45b07d" />
<img width="940" height="397" alt="image" src="https://github.com/user-attachments/assets/f61415fb-089f-47bc-a1d8-bd383491b97e" />
<img width="940" height="418" alt="image" src="https://github.com/user-attachments/assets/3d828796-dac8-4136-a703-810d3de0c791" />


## 3. Select resource type
For CloudFront:
- Choose **CloudFront distribution**
- Region scope: **Global**
<img width="940" height="117" alt="image" src="https://github.com/user-attachments/assets/32490e1b-6f4a-4ef6-8577-0fd8c08466f7" />


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
<img width="940" height="435" alt="image" src="https://github.com/user-attachments/assets/e5f80a9e-4da4-4ff5-969b-7164844fc21f" />


## 5. Name the Protection Pack
Example:
- Name: `prod-cloudfront-waf`
- Description: AWS WAF Web ACL for CloudFront Production
<img width="940" height="340" alt="image" src="https://github.com/user-attachments/assets/dd491f2b-2ad0-4e92-bba7-cb60c17f9cf4" />


## 6. Create the Protection Pack
Click **Create protection pack (web ACL)**.
<img width="940" height="398" alt="image" src="https://github.com/user-attachments/assets/9e3a142d-f4c5-4e42-86c4-ee8cd3ebeb1a" />


## 7. Manage Rules
<img width="940" height="404" alt="image" src="https://github.com/user-attachments/assets/943b7279-7dfe-41e6-9691-2088c5dee3db" />
<img width="940" height="388" alt="image" src="https://github.com/user-attachments/assets/b856ef4e-994f-4e7c-88ec-bacf1b8d491b" />

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
