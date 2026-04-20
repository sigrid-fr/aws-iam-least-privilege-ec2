# Cloud Security with AWS IAM

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-iam)

---

![Image](https://github.com/sigrid-fr/aws-iam-least-privilege-ec2/blob/main/cover.png)

---

## Introducing Today's Project!

### Project overview

This project demonstrates the implementation of least privilege access control in AWS using IAM, with a focus on policy validation and secure, controlled access to EC2 resources.

### Tools and concepts

In this project, I worked with AWS IAM for identity and access management, Amazon EC2 for compute provisioning, and the IAM Policy Simulator to validate permissions. I also applied key security concepts such as least privilege, role-based access control (RBAC), and environment-based access control using resource tags.

### Project reflection

This project took a few hours to complete, including configuring IAM policies, setting up EC2 instances, and validating permissions using the IAM Policy Simulator.

---

## Tags

### What I did in this step

Launch two Amazon EC2 instances to provision compute capacity for the application environment.

### Understanding tags

Tags are key-value pairs assigned to AWS resources that enable efficient organization, filtering, and management across environments.

### My tag configuration

I used the Env tag on my EC2 instances, assigning values such as "development" and "production" to distinguish environments.

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_2e0e5a5d)

---

## IAM Policies

### What I did in this step

In this step, I create an IAM policy to define controlled access to the development environment, following the principle of least privilege.

### Understanding IAM policies

IAM Policies are rules for who can do what with AWS resources. It's all about giving permissions to IAM users, groups, or roles, saying what they can or can't do on certain resources, and when those rules kick in.

### The policy I set up

For this project, I’ve set up a policy using JSON editor.

### Policy effect

I’ve created a policy that allows the user to perform all EC2 actions only on resources tagged as Env=development, while still allowing read-only access (Describe) to all EC2 resources. It also explicitly denies creating or deleting tags, preventing the user from changing tags to gain unauthorized access.

### Understanding Effect, Action, and Resource

The Effect, Action, and Resource attributes of a JSON policy means allow/deny, what can be done, and where it applies, respectively.

---

## My JSON Policy

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_1c864649)

---

## Account Alias

### What I did in this step

In this step, I configure an AWS account alias to provide a more user-friendly and simplified sign-in experience.

### Understanding account aliases

An account alias is a friendly name for your AWS account that you can use instead of your account ID to sign in to the AWS Management Console.

### Setting up my account alias

Now, my new AWS console sign-in URL is https://nextwork-alias-sigrid.signin.aws.amazon.com/console

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_0eb4439b)

---

## IAM Users and User Groups

### What I did in this step

In this step, I create a dedicated IAM group for all NextWork interns to enable centralized and consistent permission management.

### Understanding user groups

IAM user groups are collections of IAM users that simplify permission management.
By attaching policies to a group instead of individual users, permissions can be managed centrally and consistently across all members.

### Attaching policies to user groups

In this case, I attached the custom policy to the user group, ensuring that all users inherit the same access permissions.

### Understanding IAM users

IAM users are individual identities with defined permissions to access AWS resources, whereas user groups are collections of users that allow centralized and consistent permission management.

---

## Logging in as an IAM User

### Sharing sign-in details

There are two ways to share a new user's login details: by providing the console sign-in URL and credentials, or by downloading and securely sharing the credentials file (.csv).

### Observations from the IAM user dashboard

I created an IAM user with access limited to the development environment. After logging in as this user, several dashboard panels displayed "Access Denied", confirming that the policy correctly restricts access to certain features and prevents interaction with production resources.

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_6f2ab446)

---

## Testing IAM Policies

### What I did in this step

In this step, I verify that the user has the correct access to the development instance.

### Testing policy actions

I validated the IAM JSON policy by simulating the "StopInstances" action on EC2 instances in different environments. Access was correctly denied for the production instance and allowed for the development instance, demonstrating proper enforcement of least privilege.

### Stopping the production instance

When attempting to stop the production instance manually, the action was correctly denied, confirming that the user does not have permission to perform this operation.

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_0e7a9d6a)

### Stopping the development instance

When simulating the "StopInstances" action on the development instance, the result was successful, confirming that the policy correctly granted the required permission.

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_1811801c)

---

## IAM Policy Simulator

To extend this project, I leverage the IAM Policy Simulator to efficiently validate user permissions and ensure correct access control.

### Understanding the IAM Policy Simulator

The IAM Policy Simulator is a feature of AWS Identity and Access Management (IAM) that enables safe validation of permissions by simulating actions without affecting real resources. It is essential for verifying policy behavior, enforcing least privilege, and preventing misconfigurations in cloud environments.

### How I used the simulator

I used the IAM Policy Simulator to validate permissions for deleting tags and terminating EC2 instances. Both actions were initially denied.
After refining the simulation to target development resources with the correct permissions, instance termination was successfully allowed.
The delete tags action remained denied, as it requires a separate policy, reinforcing proper permission boundaries and least privilege principles.

![Image](http://learn.nextwork.org/triumphant_magenta_zany_bilberry/uploads/aws-security-iam_069d8a621)

---
