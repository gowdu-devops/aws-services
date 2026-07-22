# Amazon CloudFront - Common Issues and Solutions

---

# 1. Distribution Stuck in "Deploying"

## Cause

- New distribution is being propagated across AWS Edge Locations.
- Configuration changes are still being deployed.

## Solution

- Wait 5–15 minutes (sometimes longer for major updates).
- Refresh the CloudFront console.
- Check the distribution status until it changes to **Deployed**.

---

# 2. 403 Access Denied

## Causes

- Incorrect S3 Bucket Policy
- Missing Origin Access Control (OAC)
- AWS WAF blocking requests
- Wrong Alternate Domain Name (CNAME)
- File does not exist
- Incorrect permissions

## Solution

- Verify the S3 bucket policy.
- Configure OAC for S3 origins.
- Check AWS WAF logs.
- Verify the requested object exists.
- Confirm Alternate Domain Name settings.

---

# 3. 404 Not Found

## Causes

- Object doesn't exist.
- Wrong Origin Path.
- Incorrect URL.

## Solution

- Verify the file exists.
- Check the Origin Path.
- Upload the missing file.
- Test directly against the origin.

---

# 4. 502 Bad Gateway

## Causes

- Application Load Balancer is unhealthy.
- EC2 application is down.
- SSL/TLS mismatch.
- Wrong origin configuration.

## Solution

- Check Target Group health.
- Verify ALB Listener.
- Restart application.
- Verify HTTPS configuration.

Commands

```bash
curl -I https://example.com
```

```bash
aws elbv2 describe-target-health --target-group-arn <arn>
```

---

# 5. 503 Service Unavailable

## Causes

- Origin server unavailable.
- Application crashed.
- Auto Scaling has no healthy instances.

## Solution

- Verify EC2 status.
- Check Target Group health.
- Restart application.
- Review CloudWatch metrics.

---

# 6. 504 Gateway Timeout

## Causes

- Backend response is too slow.
- Origin timeout.
- Network issues.

## Solution

- Optimize backend.
- Increase origin timeout if appropriate.
- Check ALB and application logs.

---

# 7. Cache Not Updating

## Causes

- Old content still cached.
- TTL has not expired.

## Solution

Create Cache Invalidation.

Console

```
CloudFront
↓

Invalidations

↓

Create Invalidation

↓

Path

/*
```

CLI

```bash
aws cloudfront create-invalidation \
--distribution-id E123456789 \
--paths "/*"
```

---

# 8. Website Shows Old Content

## Causes

- Browser cache.
- CloudFront cache.
- Long TTL.

## Solution

- Perform cache invalidation.
- Clear browser cache.
- Reduce TTL if frequent updates are expected.

---

# 9. HTTPS Not Working

## Causes

- ACM certificate missing.
- Wrong certificate.
- Certificate expired.
- HTTP only configuration.

## Solution

- Attach ACM certificate.
- Verify certificate status is **Issued**.
- Configure HTTPS.
- Redirect HTTP to HTTPS.

---

# 10. ACM Certificate Not Visible

## Cause

CloudFront only accepts ACM certificates created in:

```
us-east-1 (N. Virginia)
```

## Solution

Create a new ACM certificate in **us-east-1** and attach it to the distribution.

---

# 11. Custom Domain Not Working

## Causes

- Route53 not configured.
- DNS propagation.
- Missing Alternate Domain Name.
- Certificate mismatch.

## Solution

- Add the Alternate Domain Name to CloudFront.
- Update Route53 Alias record.
- Verify ACM certificate includes the domain.

---

# 12. High Latency

## Causes

- Cache Miss.
- Origin located far away.
- Small Price Class.
- Slow backend.

## Solution

- Increase cache hit ratio.
- Optimize origin performance.
- Use **Price Class All**.
- Monitor CloudWatch metrics.

---

# 13. Origin Unreachable

## Causes

- ALB deleted.
- Wrong Origin DNS.
- Security Group restrictions.
- Backend unavailable.

## Solution

- Verify Origin Domain.
- Test ALB directly.
- Allow CloudFront traffic.
- Check Security Groups.

---

# 14. AWS WAF Blocking Requests

## Causes

- SQL Injection rule.
- XSS rule.
- Geo restriction.
- Rate limit exceeded.

## Solution

- Review WAF logs.
- Test using allowed IPs.
- Adjust WAF rules if necessary.

---

# 15. Cache Hit Ratio Low

## Causes

- Very low TTL.
- Frequent content changes.
- Too many query strings or headers included in the cache key.

## Solution

- Increase TTL where appropriate.
- Optimize Cache Policy.
- Reduce unnecessary forwarded headers and query strings.

---

# 16. Distribution Creation Fails

## Causes

- Account verification required.
- Invalid origin.
- Missing permissions.

## Solution

- Verify your AWS account.
- Check IAM permissions.
- Verify the origin exists.
- Contact AWS Support if account verification is required.

---

# 17. Origin SSL Certificate Error

## Causes

- Self-signed certificate.
- Expired certificate.
- Domain mismatch.

## Solution

- Install a valid SSL certificate.
- Renew expired certificates.
- Verify certificate hostname.

---

# 18. Access Logs Not Generated

## Causes

- Logging disabled.
- Incorrect S3 bucket permissions.

## Solution

- Enable Standard Logging.
- Verify S3 bucket permissions.

---

# 19. Cache Invalidation Takes Time

## Cause

CloudFront must propagate the invalidation to all Edge Locations.

## Solution

- Wait a few minutes.
- Monitor the invalidation status in the console.

---

# 20. Route53 Alias Not Working

## Causes

- Wrong Alias target.
- DNS propagation.
- Hosted Zone mismatch.

## Solution

- Verify the Alias record points to the correct CloudFront distribution.
- Wait for DNS propagation.
- Confirm you're editing the correct Hosted Zone.

---

# Troubleshooting Commands

## Test Website

```bash
curl -I https://example.com
```

---

## Check SSL

```bash
openssl s_client -connect example.com:443
```

---

## DNS Lookup

```bash
nslookup example.com
```

---

## DIG

```bash
dig example.com
```

---

## List CloudFront Distributions

```bash
aws cloudfront list-distributions
```

---

## Distribution Details

```bash
aws cloudfront get-distribution --id <distribution-id>
```

---

## Cache Invalidation

```bash
aws cloudfront create-invalidation \
--distribution-id <distribution-id> \
--paths "/*"
```

---

# Interview Scenario

**Question:** Users report that your website is still showing old images after deployment. What would you do?

**Answer:**

1. Check whether CloudFront is serving cached content.
2. Review the TTL configured in the cache policy.
3. Create a cache invalidation (`/*` or the specific file path).
4. Verify that the origin contains the updated files.
5. Clear the browser cache and test again.
6. Confirm the updated content is now being served.

---

# Quick Troubleshooting Flow

```text
Website Issue
      │
      ▼
CloudFront Status (Deployed?)
      │
      ▼
DNS (Route53)
      │
      ▼
HTTPS (ACM)
      │
      ▼
WAF Rules
      │
      ▼
Origin (ALB/S3)
      │
      ▼
Target Group Health
      │
      ▼
Application Logs
      │
      ▼
CloudWatch Metrics
      │
      ▼
Cache Invalidation (if needed)
```
