# Kubernetes Security  

## Module 1. Kubernetes and Cloud Security Basics
### 1. Kubernetes Security
- Why do we need security?
- Kubernetes Architecture Diagram
- Kubernetes Security Best Practices  

### 2. The 4 C's of Kubernetes Security  
- Cloud, Cluster, Container, Code  


----------------------------------------------------------------------------------------------------------


## Module 2. Cluster Security  
### 1. Kubernetes Cluster Security Fundamentals  

As we have seen already, the kube-apiserver is at the center of all operations within Kubernetes. We interact with it through the kube control utility or by accessing the API directly, and through that you can perform almost any operation on the cluster. Controlling access to the API server itself. We need to make two types of decisions. Who can access the cluster and what can they do?

#### Who can access?
By the authentication mechanisms, there are many ways to authenticate to the API server:
- Files - Username and Passwords
- Files - Username and Tokens
- Certificates
- External Authentication providers - LDAP
- Service Accounts
#### What can they do?
Authorization is implemented using role-based access controls where users are associated to groups with specific permissions. In addition, there are other authorization modules like the attribute-based access control
- RBAC Authorization
- ABAC Authorization
- Node Authorization
- Webhook Mod Communication among components is secured using TLS encryption

As we know, there are many nodess, virtual,components... work with together. Beside, we have:
* users like administrators that access the cluster  to perform administrative tasks
* the developers that access the cluster to test or deploy applications
* end users who access the applications deployed on the cluster
* Third party applications (Bots) access the cluster for integration purpose
   
====> To secure our cluster, by securing the communication between internal components and securing management access to the cluster through authentication and authorization mechanisms

![image](https://github.com/user-attachments/assets/c679fab7-03e0-486b-afdf-191253346ef9)

In addition, Kubernetes admins need to know which tools Kubernetes offers natively to address security concerns, and which types of third-party security tools they’ll need to integrate with their clusters to fill in the gaps. This is also a complex topic because, although Kubernetes isn’t a security platform, it provides certain types of native security tooling, such as Role-Based Access Control (RBAC).


### 2. Securing Clusters with RBAC  
- What is RBAC (Cluster Roles/Roles, Cluster Role Bindings/Role Bindings, Users, Service Accounts)
  
RBAC, or Role-Based Access Control, is a method used to restrict system access to authorized users based on their roles within an organization. In the context of Kubernetes, RBAC is a powerful mechanism that allows administrators to define what actions users and applications can perform on Kubernetes resources. 

![image](https://github.com/user-attachments/assets/2a4b85ec-1f56-468a-9b42-5aeaa3284e74)
  
#### Key Concepts of RBAC in Kubernetes:
##### 1. Roles and ClusterRoles:
- **Role**: A set of permissions defined within a specific namespace. It specifies what actions (like get, list, create, update, delete) can be performed on which resources (like pods, services, deployments) within that namespace.

![image](https://github.com/user-attachments/assets/03eb8c78-2de0-4574-b234-307224ac9b01)

- **ClusterRoles**: Similar to a Role, but it applies across the entire cluster, not limited to a single namespace. ClusterRoles can be used to grant permissions to cluster-scoped resources (like nodes) or to be reused across multiple namespaces.

![image](https://github.com/user-attachments/assets/c8191c13-bab0-4413-9215-7fc04a9fd50e)

##### 2. RoleBindings and ClusterRoleBindings:
- **RoleBindings**: Associates a Role with a user or a set of users (or service accounts) within a specific namespace. It grants the permissions defined in the Role to the specified subjects.

![image](https://github.com/user-attachments/assets/97efa8f7-0f70-4a40-bfba-3bf25810453f)

- **ClusterRoleBindings**: Similar to RoleBinding, but it associates a ClusterRole with users or service accounts at the cluster level, granting permissions across all namespaces.

![image](https://github.com/user-attachments/assets/fa5e40bb-069d-4afd-a10b-e8c667ef6f81)

##### 3. Subjects:
Subjects are the entities (users, groups, or service accounts) that are granted permissions through RoleBindings or ClusterRoleBindings.

To create a service account, run command

![image](https://github.com/user-attachments/assets/5d3c4d1f-3b88-4214-b8f6-712b9ff47632)




##### 4. Resources and Verbs:
- Resources are the objects in Kubernetes (like pods, services, deployments, etc.) that you want to control access to.
- Verbs are the actions that can be performed on those resources (like get, list, create, update, delete).

![image](https://github.com/user-attachments/assets/17e3194b-dd28-482a-add3-8da96eecf942)

#### How RBAC Works:
1. **Authentication**: When a user or application attempts to interact with the Kubernetes API, they must first authenticate themselves (e.g., using certificates, tokens, etc.).
2. **Authorization**: After authentication, Kubernetes checks the user's permissions against the defined Roles and RoleBindings (or ClusterRoles and ClusterRoleBindings) to determine if the action is allowed.
3. **Decision**: If the user has the necessary permissions, the action is allowed; otherwise, it is denied.

#### Benefits of RBAC:
- **Granular Control**: RBAC allows for fine-grained access control, enabling administrators to specify exactly what actions users can perform on specific resources.
- **Least Privilege Principle**: By assigning only the necessary permissions to users based on their roles, RBAC helps enforce the principle of least privilege, reducing the risk of unauthorized access.
- **Ease of Management**: Roles and bindings can be easily managed and modified, making it simpler to adapt to changing organizational needs.



### 3. Security Auditing Tools  

![image](https://github.com/user-attachments/assets/643383ef-b474-4366-8e05-dee85d332980)

- What is CIS Benchmark?
**CIS Benchmarks** from the **Center for Internet Security (CIS)** are a set of globally recognized and consensus-driven best practices to help security practitioners implement and manage their cybersecurity defenses

- What is Penetration Testing?
- Why need tools?  
- **Demo:**  
  - kube-bench (CIS Benchmark Testing)
  - kube-hunter (Kubernetes Penetration Testing)  


-------------------------------------------------------------------------------------------------------------------------------------------------------

## Module 3. Container Security  
### 1. Understanding Containers & Isolation  
- Virtualization vs. Containers  
- Container Isolation (cgroups, chroot, namespaces)  

### 2. Container Breakout  
- What is container breakout?  
- Example: [Pentest Monkey - Chroot Breakout](https://pentestmonkey.net/blog/chroot-breakout-perl)  
- Real-world example: **"Dirty Cow" exploit** *(If needed)*  

### 3. Preventing Container Breakout  
- Using Kubernetes Security Context  
- Avoid Mounting Host’s Root Directory  
- Limit Service Account’s Privileges  
- Limiting Linux Kernel Calls  

### 4. Using Kubernetes' Built-in Security Features  
- Pod Security Admissions  
- Network Policies  

### 5. Extending Security with External Tools  
- **Why?** → Shortcomings of Built-in Features  
- **Policy Enforcement with:**  
  - OPA Gatekeeper (M9sweeper)
  - kubesec (Evaluates YAML manifests) 

## Module 4. Code Security  
### 1. Vulnerability Scanning & Intrusion Detection  
- What it does?  (Definitions, Threats, Benefits)

### 2. Scanning Container Images for CVEs  
- What is CVE?
- **CVE Scanning with Trivy** (Scans container images for known CVEs)  

### 3. Runtime Intrusion Detection  
- **Project Falco** and Intrusion Detection (Detecting abnormal behavior in clusters)  

---

## Why is Falco put in Code Security Scope?  
- While Falco operates at the **cluster level**, it detects **container breakouts, privilege escalation, and runtime anomalies**.  
- Falco is an **active runtime security tool**, detecting **real-time threats** after containers are running.  

---

## Reference  
- **DevOpsCon - Kubernetes Security Workshop:** [Workshop Link](https://devopscon.io/kubernetes-ecosystem/kubernetes-security-workshop/)  
  - **Slide:** [Google Slides](https://docs.google.com/presentation/d/1b8P19XOtsLzpS5vJtlUhzTVPEByhEQB6wBGotc7c_PE/edit#slide=id.g2010fd6445c_0_667)  
- **Jake - Short Kubernetes Security Workshop:** [Workshop PDF](https://conf42.github.io/static/slides/Conf42%20Kube%20Native%202023%20-%20Jacob%20Beasley.pdf)  
  - **Lab Guide:** [Google Doc](https://docs.google.com/document/d/18wwz2vxDK1kdvCyUMXdr7XcKF0bQ8yrfuRi067RwP8A/edit?tab=t.0#heading=h.3qrtvg9967oy)  
- **Scotty - Kubernetes Security Workshop:** [GitHub Repository](https://github.com/scotty-c/kubernetes-security-workshop/tree/master)
- **Cybereason - Container Escape:** [Referral Link](https://www.cybereason.com/blog/container-escape-all-you-need-is-cap-capabilities)
- **Pentestmonkey - Breaking Out of a Chroot Jail Using PERL:** [Referral Link](https://pentestmonkey.net/blog/chroot-breakout-perl)
- **OWASP - Kubernetes Security Cheat Sheet:** [Referral Link](https://cheatsheetseries.owasp.org/cheatsheets/Kubernetes_Security_Cheat_Sheet.html)
- **Gatekeeper Introduction:** [Referral Link](https://open-policy-agent.github.io/gatekeeper/website/docs/)
- **Open Policy Agent Introduction:** [Referral Link](https://www.openpolicyagent.org/)
- **OPA Gatekeeper: Policy and Governance for Kubernetes:** [Referral Link](https://kubernetes.io/blog/2019/08/06/opa-gatekeeper-policy-and-governance-for-kubernetes/)
