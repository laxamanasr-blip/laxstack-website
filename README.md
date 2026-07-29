# 🌐 LaxStack Portfolio Website

A cloud-native static portfolio website built on AWS to showcase my cloud infrastructure, networking, and automation projects.

The website is designed using a serverless architecture, leveraging Amazon S3 for hosting, Amazon CloudFront for global content delivery, Amazon Route 53 for DNS management, AWS Certificate Manager (ACM) for HTTPS, and AWS WAF for application-layer protection.

To streamline deployments, the entire release process is automated using GitHub Actions, GitHub OpenID Connect (OIDC), AWS IAM, AWS STS, and cross-account role assumption, allowing secure deployments without storing long-lived AWS credentials.

---

# Live Website

https://laxstack.com

---

# Project Objectives

This project had two primary goals:

1. Build a highly available and secure static portfolio website using AWS managed services.
2. Implement a production-style CI/CD pipeline that automatically deploys website updates with modern security practices.

---

# Website Infrastructure

The website itself follows a fully managed, serverless architecture.

```text
Internet
     │
     ▼
Route 53
     │
     ▼
CloudFront CDN
     │
     ▼
AWS WAF
     │
     ▼
Amazon S3
```

The website consists of static HTML, CSS, JavaScript, images, and project documentation stored inside an Amazon S3 bucket.

Rather than exposing the S3 bucket directly to the Internet, requests first pass through Amazon CloudFront, providing global edge caching, HTTPS termination, and improved performance.

DNS is managed through Amazon Route 53, which routes requests for **laxstack.com** to the CloudFront distribution.

AWS Certificate Manager (ACM) provides the SSL/TLS certificate used by CloudFront to securely serve the website over HTTPS.

AWS WAF protects the website from common web attacks and unsolicited traffic by filtering requests before they reach CloudFront.

---

# AWS Services Used

## Amazon S3

Amazon S3 serves as the origin for the website.

Responsibilities:

- Stores all static website files
- Highly durable object storage
- Serverless hosting
- Origin for CloudFront

---

## Amazon CloudFront

CloudFront acts as the Content Delivery Network (CDN).

Responsibilities:

- Global edge caching
- Lower latency
- HTTPS termination
- Origin shielding
- Reduced load on S3

---

## Amazon Route 53

Route 53 provides authoritative DNS for the domain.

Responsibilities:

- Domain resolution
- Alias record to CloudFront
- Highly available DNS service

---

## AWS Certificate Manager (ACM)

ACM automatically manages the SSL certificate used by CloudFront.

Responsibilities:

- HTTPS encryption
- Automatic certificate renewal
- Secure communication

---

## AWS WAF

AWS WAF protects the application before requests reach CloudFront.

Responsibilities:

- Block malicious requests
- Protect against common exploits
- Filter unwanted traffic
- Improve security posture

---

# Deployment Automation

Once the website infrastructure was complete, the next objective was eliminating manual deployments.

Initially, every website update required:

- Uploading files manually to Amazon S3
- Creating a CloudFront invalidation
- Waiting for the CDN cache to refresh

This process was repetitive, error-prone, and did not scale.

To solve this, I implemented a fully automated CI/CD pipeline using GitHub Actions and AWS Identity Federation.

```
git push
     │
     ▼
GitHub Actions
     │
     ▼
GitHub OIDC
     │
     ▼
AWS STS
     │
     ▼
Deploy Website to S3
     │
     ▼
Assume Cross-Account IAM Role
     │
     ▼
CloudFront Invalidation
```

From this point onward, every push to the `main` branch automatically publishes the latest version of the website.