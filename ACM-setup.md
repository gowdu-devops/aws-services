# How to Create an AWS ACM Certificate (AWS Console)

This guide explains how to create an AWS Certificate Manager (ACM) certificate using the AWS Management Console.

---

## Step 1: Open AWS Certificate Manager

1. Log in to the **AWS Management Console**.
2. In the search bar, search for:

```text
Certificate Manager
```

3. Open **AWS Certificate Manager (ACM)**.

> **Screenshot:** <img width="940" height="600" alt="image" src="https://github.com/user-attachments/assets/10866350-3ee2-4476-a820-0c9d3fca7aa9" />
`

---

## Step 2: Request a Certificate

Click the following button:

```text
Request a certificate
```

> **Screenshot:** `images/acm-step2.png`

---

## Step 3: Choose Certificate Type

Select:

```text
Request a public certificate
```

Click:

```text
Next
```

> **Screenshot:** `images/acm-step3.png`

---

## Step 4: Enter the Domain Name

Enter the domain name you want to secure.

Example:

```text
example.com
```

or

```text
*.example.com
```

> **Note:** A wildcard certificate secures multiple subdomains such as:

```text
app.example.com
api.example.com
dev.example.com
```

Click:

```text
Next
```

> **Screenshot:** `images/acm-step4.png`

---

## Step 5: Choose the Validation Method

Select:

```text
DNS Validation
```

> **Recommended:** DNS Validation is the preferred method because ACM can automatically renew the certificate.

Click:

```text
Next
```

> **Screenshot:** `images/acm-step5.png`

---

## Step 6: Review and Request

Review all the certificate details.

Click:

```text
Request
```

> **Screenshot:** `images/acm-step6.png`

---

## Step 7: Validate the Certificate

If your domain is managed in **Amazon Route 53**:

1. Click:

```text
Create records in Route 53
```

2. AWS automatically creates the required **CNAME** validation record.

3. Wait until the certificate status changes to:

```text
Issued
```

> **Screenshot:** `images/acm-step7.png`

---

## If You Are Using Another DNS Provider

If your DNS is hosted outside AWS (for example, **GoDaddy**, **Cloudflare**, or **Namecheap**):

1. Copy the **CNAME Name** and **CNAME Value** displayed in ACM.
2. Log in to your DNS provider.
3. Create a new **CNAME** record using the values provided by ACM.
4. Save the DNS record.
5. Wait for DNS propagation.
6. Once validation is complete, ACM automatically changes the certificate status to:

```text
Issued
```

---

## Certificate Status

During the validation process, you may see:

```text
Pending Validation
```

After successful validation, the status changes to:

```text
Issued
```

---

## Next Step

After the certificate is issued, you can attach it to:

- CloudFront Distribution
- Application Load Balancer (ALB)
- API Gateway
- Network Load Balancer (TLS Listener)
- AWS App Runner

> **Important Notes:**
>
> - For **CloudFront**, create the ACM certificate in the **us-east-1 (N. Virginia)** region.
> - For an **Application Load Balancer**, create the ACM certificate in the **same AWS region** as the ALB.
