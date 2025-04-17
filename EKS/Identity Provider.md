
### **In the EKS Context:**
When you associate an **OIDC identity provider** with your EKS cluster:

1. **Identity Provider (IdP)**: Authenticates a **Kubernetes service account** (acting as an identity).

2. **IAM**: Once the identity is authenticated by the IdP, IAM roles allow that service account to access AWS resources.


- **IdP** ensures that only **authorized identities** can request access.
- **IAM** ensures that **authenticated identities** can only do the actions they're **permitted** to do.


---

This layered approach increases security by ensuring that:

1. Only **trusted** entities (verified by the IdP) can access the system.
2. Once verified, the system ensures that each entity only has access to what they need (via IAM policies).