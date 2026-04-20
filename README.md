# AWS IAM Least Privilege for EC2

This project demonstrates how to enforce least privilege access control in AWS using IAM, restricting EC2 actions based on environment tags and validating permissions through simulation and real testing.

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-iam)

---

![Image](https://github.com/sigrid-fr/aws-iam-least-privilege-ec2/blob/main/top.png)

---

## Context

In cloud environments, multiple users need access to infrastructure resources. However, unrestricted access can lead to accidental or unauthorized actions, especially in production environments.

## Problem

Developers require access to EC2 instances for daily operations, but:

- Unrestricted permissions can allow actions on production resources
- Users may accidentally stop or modify critical instances
- Tag manipulation can lead to privilege escalation

## Solution

To address this, I implemented an IAM-based access control strategy:

- Created IAM users and groups for controlled access
- Applied a custom IAM policy based on resource tags (Env)
- Allowed EC2 actions only for development resources
- Denied tag modification actions to prevent privilege escalation
- Enabled read-only visibility across all environments

## Validation

The solution was validated using:

- IAM Policy Simulator
- Manual testing with IAM user

### Expected behavior

- Development instance → Allowed
- Production instance → Denied
- Tag modification → Denied

## Key Concepts Learned

- AWS IAM (users, groups, policies)
- Amazon EC2
- IAM Policy Simulator
- Least Privilege Principle
- Role-Based Access Control (RBAC)
- Resource tagging for environment isolation

## Result

The implementation successfully:

- Restricted access to development resources only
- Prevented unauthorized actions in production
- Ensured secure and controlled access to cloud infrastructure

---

## Tags

### My tag configuration

I used the Env tag on my EC2 instances, assigning values such as "development" and "production" to distinguish environments.

![Image](https://github.com/sigrid-fr/aws-iam-least-privilege-ec2/blob/main/tag.png)

---

## IAM Policies

### Policy Effect

I've created a policy that allows the user to perform all EC2 actions only on resources tagged as Env=development, while still allowing read-only access (Describe) to all EC2 resources. It also explicitly denies creating or deleting tags, preventing the user from changing tags to gain unauthorized access.

### Json Policy

```json
{    
  "Version": "2012-10-17",    
  "Statement": [        
    {            
      "Effect": "Allow",            
      "Action": "ec2:*",            
      "Resource": "*",            
      "Condition": {                
        "StringEquals": {                    
          "ec2:ResourceTag/Env": "development"                
        }            
      }        
    },        
    {            
      "Effect": "Allow",            
      "Action": "ec2:Describe*",            
      "Resource": "*"        
    },        
    {            
      "Effect": "Deny",            
      "Action": [                
        "ec2:DeleteTags",                
        "ec2:CreateTags"            
      ],            
      "Resource": "*"        
    }    
  ] 
}
```

---

## Account Alias

An account alias is a friendly name for your AWS account that you can use instead of your account ID to sign in to the AWS Management Console.

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_0eb4439b)

---

## IAM Users and User Groups

## Logging in as an IAM User

I created an IAM user with access limited to the development environment. After logging in as this user, several dashboard panels displayed "Access Denied", confirming that the policy correctly restricts access to certain features and prevents interaction with production resources.

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_6f2ab446)

---

## Testing IAM Policies

### Stopping the production instance

When attempting to stop the production instance manually, the action was correctly denied, confirming that the user does not have permission to perform this operation.

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_0e7a9d6a)

### Stopping the development instance

When simulating the "StopInstances" action on the development instance, the result was successful, confirming that the policy correctly granted the required permission.

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_1811801c)

---

## IAM Policy Simulator

I used the IAM Policy Simulator to validate permissions for deleting tags and terminating EC2 instances. Both actions were initially denied.
After refining the simulation to target development resources with the correct permissions, instance termination was successfully allowed.
The delete tags action remained denied, as it requires a separate policy, reinforcing proper permission boundaries and least privilege principles.

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_069d8a621)

---
