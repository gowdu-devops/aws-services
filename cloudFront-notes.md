# Amazon CloudFront - Theory

# What is Amazon CloudFront?

Amazon CloudFront is a **Content Delivery Network (CDN)** service provided by AWS.

It securely delivers websites, APIs, videos, images, CSS, JavaScript, and other web content to users through a global network of **Edge Locations**.

Instead of sending every request to your origin server, CloudFront caches frequently accessed content at locations closer to users, resulting in faster performance and reduced latency.

CloudFront integrates with:

- AWS WAF
- AWS Shield Standard
- AWS Certificate Manager (ACM)
- Amazon S3
- Application Load Balancer (ALB)
- Amazon EC2
- API Gateway
- Route 53
- Lambda@Edge

---

# Why Do We Need CloudFront?

Without CloudFront, every user request goes directly to the origin server.

Example:

User (India)

↓

Origin Server (USA)

Problems:

- High latency
- Slow page loading
- Increased load on the origin server
- Higher bandwidth usage
- Poor user experience

With CloudFront:

User (India)

↓

Nearest AWS Edge Location (Mumbai)

↓

Origin Server (Only if needed)

Benefits:

- Faster content delivery
- Reduced latency
- Reduced load on origin servers
- Better scalability
- Global availability
- Built-in DDoS protection (AWS Shield Standard)
- Integration with AWS WAF for web security
- Lower bandwidth consumption through caching

---

# What is a CDN?

**CDN (Content Delivery Network)** is a globally distributed network of servers that caches content closer to users.

Instead of fetching content from a single origin every time, the CDN serves cached content from the nearest server.

Example:

Origin Server (Mumbai)

│

├── Edge Location (Hyderabad)

├── Edge Location (Singapore)

├── Edge Location (London)

└── Edge Location (New York)

When a user in Singapore requests a file, CloudFront serves it from the Singapore Edge Location instead of Mumbai.

---

# How CloudFront Works

1. A user sends a request for a website or file.
2. DNS (Route 53) directs the request to the nearest CloudFront Edge Location.
3. CloudFront checks whether the requested object is already in its cache.
4. If the object exists (**Cache Hit**), CloudFront returns it immediately.
5. If the object does not exist (**Cache Miss**), CloudFront fetches it from the origin (ALB, S3, EC2, API Gateway, etc.).
6. CloudFront stores the object in the cache based on its TTL.
7. Future requests are served directly from the cache until the object expires or is invalidated.

Flow:

User

↓

Route 53

↓

CloudFront Edge Location

↓

Cache Hit?

├── Yes → Return Cached Content

└── No

↓

Origin Server

↓

Store in Cache

↓

Return Response

---

# CloudFront Components

## 1. Distribution

A Distribution is the main CloudFront configuration.

It defines:

- Origin
- Cache behavior
- Security settings
- Domain names
- SSL certificates
- Logging
- WAF integration

There are two common use cases:

- Web Distribution (used for websites, APIs, applications)
- Media streaming (through supported media origins)

---

## 2. Origin

The **Origin** is the backend source from which CloudFront retrieves content.

Supported origins:

- Amazon S3
- Application Load Balancer (ALB)
- Amazon EC2
- API Gateway
- AWS Elemental MediaPackage
- Custom HTTP/HTTPS servers
- VPC Origins (private applications)

Example:

CloudFront

↓

Application Load Balancer

↓

EC2 Instances

---

## 3. Edge Location

An **Edge Location** is an AWS Point of Presence (PoP) located around the world.

Responsibilities:

- Cache content
- Deliver content to nearby users
- Reduce latency
- Improve performance

Users are automatically routed to the closest Edge Location.

---

## 4. Regional Edge Cache

Regional Edge Cache sits between Edge Locations and the Origin.

Purpose:

- Stores larger or less frequently requested objects
- Reduces requests to the origin
- Improves cache efficiency
- Lowers origin bandwidth usage

Flow:

User

↓

Edge Location

↓

Regional Edge Cache

↓

Origin

---

## 5. Cache

Cache is temporary storage used by CloudFront to store frequently accessed content.

Examples:

- HTML
- CSS
- JavaScript
- Images
- Videos
- PDFs
- Fonts

Benefits:

- Faster responses
- Reduced latency
- Lower origin load

---

## 6. Cache Hit

A **Cache Hit** occurs when the requested object is already available in the Edge Location.

Flow:

User

↓

CloudFront

↓

Cached Object Found

↓

Response

Advantages:

- Very fast response
- No request to origin
- Lower bandwidth usage
- Reduced server load

---

## 7. Cache Miss

A **Cache Miss** occurs when the requested object is not available in the cache.

Flow:

User

↓

CloudFront

↓

Origin

↓

Retrieve Content

↓

Store in Cache

↓

Return Response

After the first request, subsequent requests usually become Cache Hits.

---

## 8. TTL (Time To Live)

TTL defines how long CloudFront stores an object in cache before requesting a fresh copy from the origin.

Types:

- Minimum TTL
- Default TTL
- Maximum TTL

Example:

Minimum TTL : 0 seconds

Default TTL : 86400 seconds (1 day)

Maximum TTL : 31536000 seconds (1 year)

Choosing an appropriate TTL helps balance content freshness and performance.

---

# Summary

CloudFront is AWS's global CDN service that improves website and application performance by caching content at Edge Locations around the world. It reduces latency, decreases load on origin servers, enhances scalability, and integrates with AWS WAF, AWS Shield Standard, and ACM to provide secure, high-performance content delivery.
