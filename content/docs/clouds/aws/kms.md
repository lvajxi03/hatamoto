+++
date = '2025-12-14T14:14:23+02:00'
title = 'KMS'
menus = 'main'
type = 'page'
+++

AWS KMS (Key Management Service) is a managed service for creating, storing, and controlling cryptographic keys used to encrypt your data across AWS.

Think of it as centralized, auditable key control.

## What KMS actually does

KMS lets you:

* Create and manage encryption keys (KMS keys)
* Control who can use keys (IAM + key policies)
* Encrypt and decrypt data without handling raw keys
* Rotate keys automatically
* Audit all key usage via CloudTrail

You never get direct access to the key material (unless you import your own).

## What KMS is used for (everywhere)

Most AWS services integrate with KMS automatically:

| Service                 | What KMS encrypts          |
| ----------------------- | -------------------------- |
| **EBS**                 | Disk volumes & snapshots   |
| **S3**                  | Objects (SSE-KMS)          |
| **RDS / Aurora**        | Database storage & backups |
| **EFS**                 | File systems               |
| **Secrets Manager**     | Secrets                    |
| **SSM Parameter Store** | Secure strings             |
| **CloudWatch Logs**     | Log groups                 |
| **Lambda**              | Environment variables      |
| **SQS / SNS**           | Messages                   |
| **EKS**                 | Kubernetes secrets         |

If you’ve used encryption in AWS, you’ve probably used KMS — even unknowingly.

## Types of KMS keys
1️⃣ AWS-managed keys

* Created and managed by AWS
* One per service (e.g. aws/s3)
* Free
* Limited control

2️⃣ Customer-managed keys (CMK)

* You create and manage them
* Custom permissions
* Key rotation
* Costs money

3️⃣ Imported key material

* You bring your own key
* You control expiration
* Advanced use cases

## How encryption works with KMS (simple)

KMS uses envelope encryption:
```
Data → encrypted with Data Key
Data Key → encrypted with KMS Key
```

* KMS never sees your plaintext data
* Only encrypts/decrypts small data keys
* Very fast and scalable

## Access control (very important)

KMS access is controlled by two layers:

1. IAM policies
2. Key policy (mandatory)

Both must allow access.

Example (concept):
```
{
  "Effect": "Allow",
  "Action": "kms:Decrypt",
  "Resource": "*"
}
```

If either denies → access denied.

## Auditing & compliance

Every KMS operation is logged to CloudTrail:

* Who used the key
* When
* From where
* For which service

This is huge for compliance (HIPAA, PCI, SOC2).

## Pricing (high level)

* AWS-managed keys: free
* Customer-managed keys: monthly fee per key
* API calls: small per-request cost

### What KMS is NOT

❌ Not a password manager \
❌ Not a general encryption library \
❌ Not HSM access (unless using CloudHSM) \
❌ Not for large data encryption directly
