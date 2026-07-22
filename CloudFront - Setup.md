# Amazon CloudFront - AWS Console Setup (Step-by-Step)

## Step 1: Open Amazon CloudFront

1. Sign in to the **AWS Management Console**.
2. In the search bar, type:

```text
CloudFront
```

3. Select **Amazon CloudFront** from the search results.

**Purpose:**
Amazon CloudFront is AWS's global Content Delivery Network (CDN) service. It helps deliver websites, APIs, images, videos, and static content with low latency by serving content from the nearest AWS Edge Location.
<img width="940" height="613" alt="image" src="https://github.com/user-attachments/assets/fc6575b7-fe84-4c7e-97c7-7cc6a95b5b3e" />

---

## Step 2: Create a CloudFront Distribution

Click:

```text
Create Distribution
```
<img width="940" height="422" alt="image" src="https://github.com/user-attachments/assets/1076b887-2814-4357-bc0c-1b131bb95774" />

A **Distribution** is the primary CloudFront resource that defines:

- Which origin CloudFront should connect to
- How content should be cached
- Security settings
- Custom domain configuration
- HTTPS configuration
- Logging
- AWS WAF integration

<img width="940" height="392" alt="image" src="https://github.com/user-attachments/assets/27292db3-b499-4eaa-876f-01541a45a847" />

<img width="940" height="455" alt="image" src="https://github.com/user-attachments/assets/2b787249-33b7-4e03-97d4-64936bc74562" />

<img width="940" height="479" alt="image" src="https://github.com/user-attachments/assets/7ac9d53b-ce99-4dd9-875d-7ee19f7d3441" />


---

# Step 3: Specify Origin

The **Origin** is where your application or content is hosted.
<img width="940" height="380" alt="image" src="https://github.com/user-attachments/assets/9fc855b7-78f7-48ad-ad09-87ec3903b11b" />

CloudFront retrieves content from the origin whenever it is not available in the cache.

### Supported Origin Types

### 1. Amazon S3

Use when hosting:

- Static websites
- Images
- CSS
- JavaScript
- Single Page Applications (SPA)
- Documents

**Example**

```text
my-static-website.s3.amazonaws.com
```

---

### 2. Elastic Load Balancer (Recommended)

Use when your application runs behind an Application Load Balancer.
<img width="940" height="467" alt="image" src="https://github.com/user-attachments/assets/9bd7564c-a410-49de-ba54-bbc24c1c35c2" />

<img width="940" height="246" alt="image" src="https://github.com/user-attachments/assets/bdeec852-8826-4e54-90e8-619a8aa3266a" />


Typical architecture:

```text
CloudFront
      │
      ▼
Application Load Balancer
      │
      ▼
EC2 / EKS
```

This is the most common production setup.

---

### 3. API Gateway

Use for REST APIs or HTTP APIs.

Architecture

```text
CloudFront
      │
      ▼
API Gateway
      │
      ▼
Lambda / Backend
```

---

### 4. AWS Elemental MediaPackage

Used for:

- Live Streaming
- Video on Demand (VOD)

---

### 5. VPC Origin

Use when your application is hosted inside a private VPC.

Examples:

- Private Application Load Balancer
- Private EC2
- Internal applications

---

### 6. Other

Use for any publicly accessible HTTP/HTTPS server.

Examples:

- Nginx
- Apache
- IIS
- Applications hosted on Azure
- Applications hosted on GCP

---

# Step 4: Select Origin

Click

```text
Browse Load Balancers
```

Choose your Application Load Balancer.

Example

```text
prod-alb
```

CloudFront automatically retrieves the DNS name.

Example

```text
prod-alb-123456.ap-south-1.elb.amazonaws.com
```

---

# Step 5: Origin Path (Optional)

Origin Path specifies a folder inside the origin.

Example

```text
/app
```

CloudFront requests

```text
https://origin/app/index.html
```

**Production Recommendation**

Leave this field blank unless your application is hosted in a subdirectory.

---

# Step 6: Origin Settings

CloudFront asks how it should communicate with the origin.

### Option 1 (Recommended)

```text
Use Recommended Origin Settings
```

AWS automatically configures:

- HTTPS communication
- Connection timeout
- Keep-alive timeout
- Security settings

Recommended for most production workloads.

---

### Option 2

```text
Customize Origin Settings
```

Choose this only if you need to configure:

- Origin Protocol (HTTP or HTTPS)
- Custom Headers
- Connection Timeout
- Keep-Alive Timeout
- Origin Shield
- TLS Settings

---

# Step 7: Cache Settings

CloudFront determines when cached content should be used.

### Option 1 (Recommended)

```text
Use Recommended Cache Settings
```

AWS automatically selects an appropriate cache policy.

Recommended for production.

---

### Option 2

```text
Customize Cache Settings
```

Use when you need to configure:

- Minimum TTL
- Default TTL
- Maximum TTL
- Query Strings
- Cookies
- HTTP Headers

---

# Step 8: Security

CloudFront provides security integration.
<img width="940" height="246" alt="image" src="https://github.com/user-attachments/assets/352b1805-6140-4a58-b213-5e42581f5d9d" />

### AWS Shield Standard

Automatically enabled.

Provides protection against:

- Layer 3 DDoS attacks
- Layer 4 DDoS attacks

No additional configuration is required.

---

### AWS WAF

Attach an existing Web ACL.
<img width="940" height="439" alt="image" src="https://github.com/user-attachments/assets/d59f3371-9606-45a4-9fa4-3ff5f256eda7" />

Example

```text
prod-cloudfront-waf
```

AWS WAF protects against:

- SQL Injection
- Cross-Site Scripting (XSS)
- Bots
- Rate-based attacks
- Geographic restrictions

---

# Step 9: Alternate Domain Name (CNAME)

Default CloudFront URL

```text
d123abcd.cloudfront.net
```

To use your own domain:

```text
www.example.com
```

or

```text
app.example.com
```

Add the custom domain as an Alternate Domain Name (CNAME).

---

# Step 10: SSL Certificate

Attach an AWS Certificate Manager (ACM) certificate.
<img width="940" height="439" alt="image" src="https://github.com/user-attachments/assets/86f4cf5e-ffe5-4973-80ee-4f7f329ad1c1" />

**Important**

CloudFront accepts ACM certificates **only** from:

```text
us-east-1 (N. Virginia)
```

Example

```text
example.com
```

---

# Step 11: Viewer Protocol Policy

Choose

```text
Redirect HTTP to HTTPS
```

This automatically redirects users from HTTP to HTTPS.

Production recommendation:

Always use HTTPS.

---

# Step 12: Cache Policy

For static websites:

```text
CachingOptimized
```

For REST APIs:

```text
CachingDisabled
```

---

# Step 13: Compression

Enable

```text
Yes
```

CloudFront compresses supported content using:

- Gzip
- Brotli

Benefits:

- Faster downloads
- Lower bandwidth usage

---

# Step 14: Price Class

Options:

### Price Class All

Uses all AWS Edge Locations.

Best performance.

---

### Price Class 200

Uses:

- North America
- Europe
- Asia
- Middle East

Lower cost.

---

### Price Class 100

Uses:

- North America
- Europe

Lowest cost.

---

# Step 15: Logging

Enable

```text
Standard Logging
```

Store logs in an Amazon S3 bucket.

Benefits:

- Auditing
- Monitoring
- Troubleshooting
- Security analysis

---

# Step 16: Create Distribution

Click

```text
Create Distribution
```

Deployment usually takes **5–15 minutes**.

The status changes from:

```text
In Progress
```

to

```text
Deployed
```

---

# Step 17: Test the Distribution

CloudFront generates a domain similar to:

```text
d123abc456.cloudfront.net
```

Open:

```text
https://d123abc456.cloudfront.net
```

If using a custom domain:

1. Create an Alias record in Route 53.
2. Point the domain to the CloudFront distribution.
3. Test using:

```text
https://www.example.com
```

---

# Production Architecture

```text
                 Internet
                     │
                     ▼
                Amazon Route 53
                     │
                     ▼
             Amazon CloudFront
                     │
      AWS Shield Standard (Automatic)
                     │
                     ▼
                 AWS WAF
                     │
                     ▼
      Application Load Balancer (ALB)
                     │
                     ▼
              EC2 / EKS Application
                     │
                     ▼
                 Amazon RDS
```

---

# Best Practices

- Use HTTPS only.
- Create ACM certificates in **us-east-1**.
- Attach AWS WAF to CloudFront.
- Leave Origin Path blank unless required.
- Use recommended Origin and Cache settings unless you have specific requirements.
- Enable compression.
- Enable access logging.
- Use Route 53 Alias records for custom domains.
- Monitor CloudFront using CloudWatch.
