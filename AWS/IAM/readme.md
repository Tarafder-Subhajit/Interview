# Top 25 AWS IAM Interview Questions and Answers

> **Purpose:** Beginner-to-intermediate interview preparation for Cloud, DevOps, and AWS roles. Answers are concise enough for revision, with practical examples and follow-up points.

## Quick Revision Map

- **Core identities:** Root user, IAM users, groups, roles
- **Authorization:** Identity-based policies, resource-based policies, permission boundaries, SCPs
- **Temporary access:** AWS STS, role assumption, federation
- **Security:** Least privilege, MFA, credential rotation, IAM Access Analyzer
- **Troubleshooting:** Explicit deny, policy evaluation, CloudTrail, Policy Simulator

---

## 1. What is AWS IAM?

**Answer:**

AWS Identity and Access Management (IAM) is a global AWS service used to control **authentication** and **authorization** for AWS resources.

- **Authentication:** Verifies who is making the request.
- **Authorization:** Determines what the authenticated identity is allowed to do.
- IAM supports users, groups, roles, policies, federation, MFA, and temporary credentials.

**Interview example:** An EC2 instance can use an IAM role to read objects from a specific S3 bucket without storing access keys on the server.

---

## 2. What is the difference between an IAM user, group, and role?

**Answer:**

| IAM identity | Purpose | Credentials |
|---|---|---|
| **User** | Represents a person or application requiring long-term identity | Password and/or access keys |
| **Group** | Collection of IAM users used to assign common permissions | No credentials |
| **Role** | Assumable identity for temporary access | Temporary credentials from AWS STS |

**Key point:** A role is assumed, while a user normally has credentials directly associated with it.

---

## 3. What is an IAM policy?

**Answer:**

An IAM policy is a JSON document that defines permissions. A policy normally includes:

- `Version`
- `Statement`
- `Effect`: `Allow` or `Deny`
- `Action`: API operations such as `s3:GetObject`
- `Resource`: Resources to which the actions apply
- `Condition`: Optional restrictions

Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-bucket/*"
    }
  ]
}
```

---

## 4. What is the difference between AWS managed and customer managed policies?

**Answer:**

- **AWS managed policies:** Created and maintained by AWS. They are easy to use but may provide broader access than required.
- **Customer managed policies:** Created and maintained within your AWS account. They offer better control, versioning, and reuse.
- **Inline policies:** Embedded directly into one user, group, or role and have a one-to-one relationship with that identity.

**Best practice:** Use customer managed policies tailored to least privilege for production workloads.

---

## 5. What is the principle of least privilege?

**Answer:**

Least privilege means granting only the permissions required to complete a specific task, for only the required resources and time.

**Practical approach:**

1. Start with limited permissions.
2. Specify exact actions and resource ARNs.
3. Add conditions such as region, IP address, tags, or MFA.
4. Review CloudTrail activity.
5. Use IAM Access Analyzer to refine the policy.

Avoid broad permissions such as `Action: "*"` and `Resource: "*"` unless there is a justified administrative use case.

---

## 6. What is the AWS account root user, and how should it be secured?

**Answer:**

The root user is created with the AWS account and has complete access, including certain tasks that cannot be delegated.

**Security controls:**

- Do not use root for daily administration.
- Enable MFA, preferably a hardware security key where appropriate.
- Do not create root access keys.
- Use a strong, unique password.
- Store recovery information securely.
- Monitor root-user activity with CloudTrail and alerts.

---

## 7. What is an IAM role, and when would you use one?

**Answer:**

An IAM role is an AWS identity with permissions that can be assumed by a trusted principal. It provides temporary credentials through AWS STS.

**Common use cases:**

- EC2 accessing S3 or DynamoDB
- Lambda calling another AWS service
- Cross-account access
- CI/CD systems deploying AWS resources
- Federated users accessing AWS accounts
- EKS workloads using IAM roles for service accounts or EKS Pod Identity

Roles are preferred over long-term access keys for workloads.

---

## 8. What is a trust policy?

**Answer:**

A trust policy is a resource-based policy attached to an IAM role. It defines **who or what can assume the role**.

For example, a role may trust:

- An AWS service such as EC2
- Another AWS account
- A SAML or OIDC identity provider
- A specific IAM role or principal

**Difference:** The trust policy controls who can assume the role; the role's permissions policies control what the role can do after assumption.

---

## 9. What is AWS STS?

**Answer:**

AWS Security Token Service (STS) provides temporary security credentials consisting of:

- Access key ID
- Secret access key
- Session token
- Expiration time

Common API operations include `AssumeRole`, `AssumeRoleWithSAML`, and `AssumeRoleWithWebIdentity`.

Temporary credentials reduce the risk associated with long-lived access keys.

---

## 10. How does cross-account access work in AWS IAM?

**Answer:**

A typical cross-account setup uses an IAM role in the target account.

1. The target account creates a role.
2. Its trust policy trusts a principal from the source account.
3. The role receives permissions for target resources.
4. The source principal receives permission to call `sts:AssumeRole`.
5. The source principal assumes the role and receives temporary credentials.

For third-party access, use an `ExternalId` condition to reduce the confused-deputy risk.

---

## 11. How does AWS evaluate multiple IAM policies?

**Answer:**

The simplified evaluation logic is:

1. Every request starts with an **implicit deny**.
2. An applicable **explicit allow** can grant access.
3. Any applicable **explicit deny** overrides an allow.
4. Permissions may also be limited by a permissions boundary, SCP, session policy, or resource policy.

**Interview phrase:** Explicit deny wins.

---

## 12. What is the difference between identity-based and resource-based policies?

**Answer:**

- **Identity-based policy:** Attached to a user, group, or role and defines what that identity can do.
- **Resource-based policy:** Attached to a resource and defines which principals can access it.

Examples of resource-based policies include S3 bucket policies, KMS key policies, SQS queue policies, SNS topic policies, and role trust policies.

Resource-based policies are commonly used for cross-account access.

---

## 13. What is a permissions boundary?

**Answer:**

A permissions boundary sets the maximum permissions that an IAM user or role can receive from identity-based policies.

If an identity policy allows an action but the boundary does not allow it, the action is not permitted.

**Use case:** Allow a development team to create roles while preventing those roles from exceeding an approved permission ceiling.

**Important:** A permissions boundary does not grant permissions by itself.

---

## 14. What is an AWS Organizations Service Control Policy (SCP)?

**Answer:**

An SCP sets the maximum available permissions for member accounts or organizational units in AWS Organizations.

- SCPs do not grant permissions.
- They restrict what IAM identities can be allowed to do.
- They can establish organization-wide guardrails.
- An explicit deny in an SCP cannot be overridden by an account administrator.

**Example:** Deny disabling CloudTrail or restrict resource creation to approved AWS Regions.

---

## 15. What is the difference between a permissions boundary and an SCP?

**Answer:**

| Feature | Permissions boundary | SCP |
|---|---|---|
| Applied to | IAM user or role | AWS account or organizational unit |
| Scope | One identity | Organization/account-level guardrail |
| Grants access | No | No |
| Main use | Delegated IAM administration | Central governance across accounts |

The effective permission must be allowed by all applicable control layers.

---

## 16. What is IAM federation?

**Answer:**

Federation allows users authenticated by an external identity provider to access AWS without separate long-term IAM user credentials.

Common approaches include:

- AWS IAM Identity Center
- SAML 2.0 federation with enterprise identity providers
- OIDC federation for web or CI/CD identities
- Active Directory integration

Federated users normally assume roles and receive temporary credentials.

---

## 17. What is AWS IAM Identity Center?

**Answer:**

AWS IAM Identity Center provides centralized workforce access to multiple AWS accounts and supported applications.

It supports:

- Integration with external identity providers
- Users and groups
- Permission sets
- Multi-account access through AWS Organizations
- Temporary role-based access

It is generally preferred over creating separate IAM users for workforce access in each AWS account.

---

## 18. What is MFA, and why is it important in IAM?

**Answer:**

Multi-factor authentication requires an additional authentication factor besides a password.

MFA is particularly important for:

- Root users
- Privileged administrators
- Sensitive API operations
- Role assumption in high-risk environments

Policies can use the `aws:MultiFactorAuthPresent` condition key to require MFA for selected actions.

---

## 19. What is the difference between access keys and temporary credentials?

**Answer:**

- **Access keys for IAM users:** Long-term credentials that remain valid until deactivated or deleted.
- **Temporary credentials:** Time-limited credentials issued by AWS STS and include a session token.

Temporary credentials are safer for roles, federation, EC2, Lambda, EKS, and CI/CD because they expire automatically and reduce secret-management overhead.

---

## 20. How should applications running on EC2 access AWS services securely?

**Answer:**

Attach an IAM role to the EC2 instance through an instance profile.

The application or AWS SDK retrieves temporary credentials automatically from the EC2 Instance Metadata Service. Use IMDSv2 where possible.

Do not hard-code access keys in:

- Source code
- AMIs
- User data
- Configuration files
- Environment variables committed to a repository

---

## 21. What are IAM policy conditions?

**Answer:**

Conditions make permissions more specific by evaluating request context.

Common condition keys include:

- `aws:SourceIp`
- `aws:RequestedRegion`
- `aws:PrincipalTag`
- `aws:ResourceTag`
- `aws:MultiFactorAuthPresent`
- `aws:SourceArn`
- `aws:SourceAccount`
- `sts:ExternalId`

**Example:** Allow starting EC2 instances only when the resource has `Environment=Development`.

---

## 22. What is attribute-based access control (ABAC) in AWS?

**Answer:**

ABAC grants permissions based on attributes represented by tags, rather than listing every resource individually.

Example design:

- Tag a role with `Project=Payments`.
- Tag resources with `Project=Payments`.
- Allow access when the principal tag matches the resource tag.

ABAC can simplify permission management in environments where projects and resources change frequently. It requires strong tag governance.

---

## 23. What is IAM Access Analyzer?

**Answer:**

IAM Access Analyzer helps identify and improve access configurations.

It can:

- Detect external or public access to supported resources
- Validate IAM policies
- Identify policy errors and security warnings
- Generate policies based on observed CloudTrail access activity
- Help review unused access in applicable analyzer configurations

It supports least-privilege refinement and cross-account exposure reviews.

---

## 24. How do you troubleshoot an `AccessDenied` error?

**Answer:**

Use this sequence:

1. Identify the principal making the request.
2. Confirm the exact API action and resource ARN.
3. Review identity-based policies.
4. Review resource-based policies.
5. Check for explicit denies.
6. Check SCPs, permissions boundaries, session policies, and VPC endpoint policies.
7. Check KMS key policy if encryption is involved.
8. Review CloudTrail event details.
9. Use the IAM Policy Simulator when appropriate.

**Common failure:** Allowing `s3:GetObject` on the bucket ARN instead of the object ARN ending in `/*`.

---

## 25. What IAM security best practices would you implement in a production AWS environment?

**Answer:**

1. Secure the root user and enable MFA.
2. Use federation and IAM Identity Center for workforce access.
3. Use IAM roles and temporary credentials for workloads.
4. Apply least privilege and avoid wildcard permissions.
5. Use customer managed policies for controlled production access.
6. Require MFA for privileged operations.
7. Remove unused users, roles, policies, and credentials.
8. Use IAM Access Analyzer and policy validation.
9. Use SCPs and permissions boundaries as guardrails.
10. Log activity with CloudTrail and create security alerts.
11. Use conditions and tags for contextual access.
12. Rotate or eliminate long-term access keys.
13. Review cross-account trusts and use `ExternalId` where applicable.
14. Protect sensitive actions with separation of duties and approval controls.

---

# Scenario-Based Follow-Up Questions

These are useful follow-ups after the main 25 questions:

1. An EC2 application needs read-only access to one S3 prefix. How would you design the role and policy?
2. A vendor needs temporary access to your AWS account. How would you prevent the confused-deputy problem?
3. A developer has an `Allow` policy but still receives `AccessDenied`. Which policy layers would you check?
4. How would you prevent administrators in member accounts from disabling CloudTrail?
5. How would you provide GitHub Actions access to AWS without storing long-term access keys?
6. How would you restrict deployments to a specific region and tagged resources?
7. How would you allow a team to create IAM roles without allowing privilege escalation?

---

# Interview Tips

- Explain the difference between **authentication** and **authorization** clearly.
- Remember: **explicit deny overrides allow**.
- Prefer **roles and temporary credentials** over access keys.
- Differentiate **trust policy** from **permissions policy**.
- Mention all major guardrails when troubleshooting: **SCP, boundary, session policy, resource policy, and endpoint policy**.
- Give one real DevOps example, such as EC2-to-S3, Lambda-to-DynamoDB, EKS workload access, or CI/CD role assumption.

---
