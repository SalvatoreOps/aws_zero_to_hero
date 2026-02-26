# Day 2 - AWS IAM (Identity and Access Management) - Complete Guide

## What is IAM?

IAM is AWS's global security service that controls who can access your AWS resources and what they can do with those resources. It is the foundation of AWS security and is available at no additional charge.

Think of IAM as the security guard and key management system for your AWS account. Without IAM, anyone with your root password has unlimited access to everything. With IAM, you implement the principle of least privilege.

## Why IAM is Critical in AWS

**1. Security Boundary**
- Prevents unauthorized access to sensitive resources (S3 buckets, databases, production servers)
- Protects against accidental deletion or modification of critical infrastructure

**2. Granular Access Control**
- Grant specific permissions (e.g., "read-only access to S3 bucket X" vs "full admin access")
- Control access based on IP address, time of day, or whether using MFA

**3. Multi-User Management**
- Enables teams to share AWS accounts safely without sharing root credentials
- Supports hundreds of users with different job functions (developers, auditors, security teams)

**4. Compliance and Auditing**
- Tracks who did what and when via CloudTrail integration
- Required for SOC 2, PCI-DSS, HIPAA, and other compliance frameworks

**5. Temporary Credentials**
- Issue short-lived access keys instead of long-term passwords
- Essential for applications and automated scripts

## Core IAM Components

### 1. Users
- Represent individual people or applications needing AWS access
- Can have long-term credentials (passwords, access keys) or temporary credentials
- Best practice: Avoid using the root user for daily tasks

### 2. Groups
- Collections of users (e.g., "Developers", "Finance-Team", "ReadOnly-Admins")
- Attach policies to groups, and users inherit those permissions
- Simplifies permission management - add/remove users from groups rather than editing individual policies

### 3. Roles
- Similar to users but intended for temporary access
- Used by: AWS services (EC2, Lambda), external identities (SAML, OIDC), or cross-account access
- No long-term credentials attached - generates temporary security tokens automatically

### 4. Policies
- JSON documents defining permissions (allow/deny)
- Types:
  - **Managed Policies**: AWS-managed or customer-managed reusable policies
  - **Inline Policies**: Embedded directly in a user/group/role (not reusable)
  - **Permission Boundaries**: Set maximum permissions an entity can have

### 5. Identity Providers (IdP)
- Connect corporate directories (Active Directory) or social providers (Google, Facebook)
- Supports SAML 2.0 and OpenID Connect (OIDC)

## Key Concepts Explained

### Authentication vs Authorization
- **Authentication**: Proving who you are (username/password, access keys, MFA)
- **Authorization**: Determining what you're allowed to do (evaluating policies)

### The Principle of Least Privilege
Grant only the minimum permissions required to perform a task. If someone needs to restart EC2 instances, don't give them permission to delete databases.

### Policy Evaluation Logic
1. Explicit DENY overrides everything
2. Explicit ALLOW permits action (if no deny exists)
3. Implicit DENY (no matching allow) = access denied

## Essential IAM Features

**Multi-Factor Authentication (MFA)**
- Hardware or virtual MFA devices for root and IAM users
- Required for sensitive operations (deleting production resources)

**Access Keys vs Console Access**
- Console access: Username/password for web interface
- Programmatic access: Access Key ID + Secret Access Key for CLI/SDK/API

**IAM Access Analyzer**
- Identifies resources shared with external entities
- Detects unintended public access or cross-account permissions

**Service Control Policies (SCP)** - Organizations
- Guardrails that restrict maximum available permissions across AWS accounts
- Applied at the organization level (not individual IAM entities)

**Temporary Credentials**
- STS (Security Token Service) generates session tokens valid 15 minutes to 36 hours
- Used by IAM Roles, assumed by users or services

## Common IAM Scenarios

**Scenario 1: Web Application Hosting**
- EC2 instance assumes IAM Role to read/write S3 bucket
- No hardcoded credentials in application code
- Role automatically rotates temporary credentials

**Scenario 2: Developer Team Access**
- Group: "Developers" with policies allowing EC2/RDS management in dev environment
- Group: "Senior-Engineers" with additional production read access
- Individual users added to appropriate groups

**Scenario 3: Third-Party Contractor**
- Create IAM user with limited permissions
- Set password policy requiring MFA
- Set expiration date on access keys

**Scenario 4: Cross-Account Access**
- Company has multiple AWS accounts (Prod, Dev, Billing)
- Users in Dev account assume role in Prod account for specific tasks
- No need to create duplicate users across accounts

## Best Practices

**Account Security**
1. Lock away root user access keys (delete them if possible)
2. Enable MFA on root account immediately
3. Create individual IAM users - don't share credentials
4. Use groups to assign permissions to job functions

**Credential Management**
5. Rotate access keys regularly (every 90 days)
6. Never commit access keys to code repositories
7. Use IAM Roles for EC2/Lambda/ECS instead of embedded keys
8. Delete unused credentials (IAM Credential Report helps identify these)

**Policy Design**
9. Start with zero permissions, add only what's needed
10. Use managed policies for common job functions (ReadOnlyAccess, PowerUserAccess)
11. Regularly audit permissions with IAM Access Analyzer
12. Use condition keys to restrict access (IP ranges, VPC endpoints, encryption requirements)

**Monitoring**
13. Enable CloudTrail to log all IAM API calls
14. Set up alerts for root account usage
15. Review IAM Credential Report monthly

## Policy Structure Example

Basic JSON policy structure:

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::company-backups/*",
            "Condition": {
                "Bool": {
                    "aws:MultiFactorAuthPresent": "true"
                }
            }
        }
    ]
}

This policy allows reading/writing S3 objects only when MFA is enabled.

## Common Mistakes to Avoid

- Using root account for daily operations
- Giving "*" (all actions) on "*" (all resources) to regular users
- Hardcoding access keys in mobile apps or GitHub repos
- Not enabling MFA
- Keeping inactive users for months/years
- Using inline policies when managed policies would suffice
- Not using IAM Roles for service-to-service communication

## Integration with Other AWS Services

IAM works with virtually every AWS service:
- **EC2**: Instance profiles (IAM Roles for EC2)
- **S3**: Bucket policies work alongside IAM policies
- **Lambda**: Execution roles determine what AWS resources function can access
- **RDS**: IAM database authentication (MySQL, PostgreSQL)
- **Organizations**: SCPs + IAM for multi-account governance

## Getting Started Checklist

1. Create an Admin IAM user (not root) with MFA
2. Delete or lock root access keys
3. Enable CloudTrail
4. Create groups for your team structure
5. Apply AWS managed policies (ReadOnlyAccess, etc.) as starting points
6. Set password policy (minimum length, complexity, rotation)
7. Enable IAM Access Analyzer
8. Set up billing alerts (even though IAM is free, resources it protects cost money)

## Limitations to Know

- 5000 IAM users per account (soft limit)
- 10 groups per user
- 2 access keys per user maximum
- Policy size limits (managed: 6KB, inline: 2KB for users)
- IAM is eventually consistent (changes take time to propagate globally)

## Summary

IAM is not optional in AWS - it is the security backbone. Proper IAM configuration prevents data breaches, accidental infrastructure destruction, and compliance violations. Start restrictive, use roles over users when possible, enable MFA everywhere, and audit regularly.

For detailed policy syntax and advanced features like Permission Boundaries or IAM Conditions, refer to the official AWS IAM documentation.

---

*Last Updated: 2024*  
*Author: SalvatoreOps*  
*License: Public Domain / Free to share*

---