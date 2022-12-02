---
layout: post
title:  "Using AWS Organizations"
author: Gaurav
categories: [ Java, AWS ]
---
One company has many departments. Imagine if a company is using only one single AWS account for all the departments.
Following issues are likely to surface:
* No clear visibility
* No ownership
* Forgotten resources (EC2 running for years)
* No cost optimization
* No separation of environments (same dev/test/prod)
* No proper user management leading to accidental deletions of production workloads

AWS organizations allows the following:
- Link multiple AWS accounts to consolidate billing and cost reporting
- Share resources
- Common IAM policies and other access control strategies across accounts
- It's free

### Root Master Account
The account used to create the organization using AWS console is termed as root master account.
We need to protect the root master account for our organization by following general security practices like:
- Using secrets manager
- Generating strong random password
- Enabling Multi-factor authentication

The 'Create organization' pops up two options:
- Create a full organization with single payer and centralized cost tracking, lets you create and invite accoutns, allows policy based
 controls and helps simplify organization wide management of AWS services
- Create an organization with only consolidated billing features.

Root master account cannot join another organization until its current organization is deleted.

Once an organization is created, we can add member accounts (existing or new).

For an existing account the root user of the account must accept the invite.

Organization Units can be created and the accounts can be assigned to them.