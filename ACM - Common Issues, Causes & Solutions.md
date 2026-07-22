# AWS ACM - Common Issues, Causes & Solutions

This document covers the most common AWS Certificate Manager (ACM) issues encountered in real-time production environments, along with their causes and solutions.

---

# 1. Certificate Status Remains "Pending Validation"

## Issue

After requesting a certificate, the status remains:

```text
Pending Validation
```

## Possible Causes

- DNS CNAME validation record was not created.
- Incorrect CNAME record.
- DNS propagation has not completed.
- Wrong hosted zone selected.

## Solution

- Verify the CNAME record in Route 53 or your DNS provider.
- Ensure the **Name** and **Value** exactly match the ACM-provided values.
- Wait for DNS propagation (typically a few minutes, but can take several hours).
- Confirm you are validating the correct domain.

---

# 2. Certificate Not Visible in CloudFront

## Issue

The ACM certificate does not appear in the CloudFront certificate list.

## Cause

CloudFront only supports ACM certificates created in:

```text
us-east-1 (N. Virginia)
```

## Solution

- Request the ACM certificate in **us-east-1**.
- Complete DNS validation.
- Attach the certificate to the CloudFront distribution.

---

# 3. Certificate Not Visible in Application Load Balancer (ALB)

## Issue

The certificate cannot be selected while configuring an HTTPS listener.

## Cause

The certificate and the ALB are in different AWS Regions.

Example:

```text
Certificate Region : us-east-1

ALB Region         : ap-south-1
```

## Solution

- Request or import the ACM certificate in the **same region** as the ALB.
- Refresh the listener page and select the certificate.

---

# 4. Browser Shows "Not Secure"

## Issue

The website displays **"Not Secure"** even after attaching an ACM certificate.

## Possible Causes

- HTTP is still being used.
- HTTPS listener (443) is missing.
- Certificate is not attached.
- Domain name does not match the certificate.
- Browser cache.

## Solution

- Configure an HTTPS (443) listener.
- Attach the ACM certificate.
- Redirect HTTP (80) to HTTPS (443).
- Verify that the certificate includes the requested domain.
- Clear browser cache and test again.

---

# 5. SSL Certificate Name Mismatch

## Issue

Browser displays:

```text
NET::ERR_CERT_COMMON_NAME_INVALID
```

## Cause

Certificate was issued for:

```text
example.com
```

But users access:

```text
api.example.com
```

## Solution

Use one of the following:

- Wildcard certificate

```text
*.example.com
```

- Multi-domain certificate (SAN)
- Separate certificate for the required subdomain

---

# 6. HTTPS Listener Returns 502 or 503

## Issue

HTTPS is enabled, but users receive:

```text
502 Bad Gateway
```

or

```text
503 Service Unavailable
```

## Possible Causes

- Target Group unhealthy.
- Backend application is stopped.
- Security Group blocks traffic.
- Health check configuration is incorrect.
- Application is listening on the wrong port.

## Solution

- Verify Target Group health.
- Ensure the application is running.
- Check Security Groups.
- Review Health Check path and port.
- Verify backend application logs.

---

# 7. Certificate Expired

## Issue

Browser displays:

```text
Certificate Expired
```

## Cause

Usually occurs with imported certificates because ACM does **not** automatically renew them.

## Solution

- Renew the certificate with the Certificate Authority.
- Import the renewed certificate into ACM.
- Replace the old certificate on CloudFront or ALB.

> AWS-issued public certificates renew automatically if DNS validation remains valid.

---

# 8. HTTPS Works but HTTP Doesn't Redirect

## Issue

Users can access:

```text
http://example.com
```

instead of automatically redirecting to HTTPS.

## Cause

No HTTP to HTTPS redirect rule exists.

## Solution

Configure the ALB listener:

```text
HTTP (80)

↓

Redirect

↓

HTTPS (443)
```

---

# 9. Domain Validation Fails

## Issue

The certificate is never issued.

## Possible Causes

- Incorrect CNAME record.
- Typographical error.
- Missing DNS record.
- Deleted validation record.
- Wrong hosted zone.

## Solution

- Compare ACM CNAME values with DNS.
- Recreate the DNS record if required.
- Wait for DNS propagation.
- Verify the hosted zone.

---

# 10. Imported Certificate Doesn't Renew

## Cause

ACM only renews **AWS-issued public certificates** automatically.

Imported certificates require manual renewal.

## Solution

- Renew the certificate externally.
- Import the updated certificate into ACM.
- Replace the old certificate on all associated resources.

---

# 11. Certificate Deleted Accidentally

## Issue

HTTPS stops working because the ACM certificate has been deleted.

## Solution

- Request or import a new certificate.
- Complete validation.
- Attach it again to CloudFront or ALB.
- Verify HTTPS functionality.

---

# 12. CloudFront Still Shows the Old Certificate

## Issue

CloudFront continues serving the previous certificate after updating ACM.

## Cause

CloudFront distribution deployment is still in progress.

## Solution

- Wait until the distribution status becomes:

```text
Deployed
```

- Clear browser cache if necessary.
- Test again after propagation.

---

# Quick Troubleshooting Checklist

| Issue | Where to Check |
|--------|----------------|
| Pending Validation | ACM → Certificate Status, Route 53 CNAME |
| Certificate Missing in CloudFront | ACM Region (must be us-east-1) |
| Certificate Missing in ALB | ACM Region must match ALB Region |
| HTTPS Not Working | ALB HTTPS Listener (443) |
| Not Secure | Certificate Attachment, HTTPS Listener |
| 502/503 Error | Target Group Health |
| Certificate Expired | ACM Expiration Date |
| Validation Failed | DNS CNAME Record |
| HTTP Not Redirecting | ALB HTTP Listener Rule |
| Old Certificate | CloudFront Deployment Status |

---

# Interview Questions

### Q1. Why is my ACM certificate still in "Pending Validation"?

**Answer:**  
The DNS validation CNAME record is missing, incorrect, or DNS propagation has not completed.

---

### Q2. Why can't I see my certificate in CloudFront?

**Answer:**  
CloudFront only supports ACM certificates created in the **us-east-1 (N. Virginia)** region.

---

### Q3. Why can't I attach my ACM certificate to an ALB?

**Answer:**  
The certificate must be in the **same AWS region** as the Application Load Balancer.

---

### Q4. Why do I get `NET::ERR_CERT_COMMON_NAME_INVALID`?

**Answer:**  
The certificate does not include the domain or subdomain that users are accessing.

---

### Q5. How do you troubleshoot HTTPS issues in production?

**Answer:**

1. Check ACM certificate status.
2. Verify DNS validation.
3. Ensure the certificate is attached to CloudFront or ALB.
4. Verify HTTPS (443) listener.
5. Check Target Group health.
6. Verify Security Groups.
7. Test with `curl` or `openssl`.
8. Review CloudFront or ALB logs.

---

# Summary

- Always use **DNS Validation** for production.
- CloudFront certificates must be created in **us-east-1**.
- ALB certificates must be in the **same AWS region** as the ALB.
- AWS-issued certificates renew automatically.
- Imported certificates require manual renewal.
- Verify DNS, listeners, and target groups before troubleshooting ACM.
