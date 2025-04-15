Whenever you create k8s pods you can integrate the iam role with the kubernets service account so that you can talk to any other aws service.

When you run **Kubernetes Pods** on AWS (like in EKS), sometimes those pods need to access **other AWS services** like S3, DynamoDB, etc.  
But AWS needs to know:  
**“Who is this pod, and is it allowed to access this service?**


### 🔐 Problem:

By default, a Kubernetes pod doesn’t have **IAM permissions** — the AWS Identity and Access Management system doesn't know about it.

---


You can **link an IAM Role to a Kubernetes Service Account**. This allows pods that use that service account to get temporary AWS credentials and access other AWS services securely.

Here’s how it works:

- You **create an IAM Role** with the necessary AWS permissions (e.g., access to S3, DynamoDB, etc.).
- You **map that IAM Role to a Kubernetes Service Account** using IAM Roles for Service Accounts (IRSA).
- Then, you **run your Pod using that Service Account**.

As a result, your pod automatically gets **temporary credentials** through the mapped IAM Role, without storing any AWS keys.

This setup allows your pod to **talk to other AWS services securely** and in an AWS-native way.


---

### 🛠️ Why do we use it with IAM?

When you integrate a **Kubernetes Service Account with an IAM Role** (using IRSA in EKS), you're saying:

> "Any pod that uses this service account should be allowed to assume this IAM Role and access AWS services."