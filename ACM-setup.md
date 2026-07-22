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


---

## Step 2: Request a Certificate

Click the following button:

```text
Request a certificate
```

> **Screenshot:** <img width="940" height="284" alt="image" src="https://github.com/user-attachments/assets/05ed12a0-0553-4200-885e-bddbc768522a" />


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

> **Screenshot:** <img width="940" height="260" alt="image" src="https://github.com/user-attachments/assets/24161562-6fbd-4429-b7ef-f1360f614979" />
<img width="940" height="402" alt="image" src="https://github.com/user-attachments/assets/0a746015-d2cc-4c30-84ad-2e855f276c98" />


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

> **Screenshot:** `<img width="940" height="228" alt="image" src="https://github.com/user-attachments/assets/6440f44e-f2c2-4be8-bc20-38327c84f681" />


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

> **Screenshot:** `<img width="940" height="151" alt="image" src="https://github.com/user-attachments/assets/168407d0-fcd4-43ac-9bd5-213ce8355fb4" />
<img width="940" height="319" alt="image" src="https://github.com/user-attachments/assets/2736ea82-abbe-4589-877c-c7c61b0d6917" />

`

---

## Step 6: Review and Request

Review all the certificate details.

Click:

```text
Request
```

> **Screenshot:** `<img width="940" height="525" alt="image" src="https://github.com/user-attachments/assets/2ac88243-56b6-4e31-bac9-ff93075a0520" />
<img width="940" height="475" alt="image" src="https://github.com/user-attachments/assets/939c146a-151b-44fd-80fe-8fafefba690e" />

`

---

## Step 7: Validate the Certificate

If your domain is managed in **Amazon Route 53**:

1. Click:

```text
Create records in Route 53
```

2. AWS automatically creates the required **CNAME** validation record.
<img width="940" height="505" alt="image" src="https://github.com/user-attachments/assets/847d3e67-db50-4aca-9e31-b24938ffbcf1" />

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
