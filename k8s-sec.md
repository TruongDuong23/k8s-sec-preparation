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
Subjects are the entities (users, groups, or service accounts) that are granted permissions through RoleBindings or ClusterRoleBindings. They can be specified by their names or by their Kubernetes identities. There are two types of accounts in Kubernetes, a user account and a service account.
- The user account is used by humans. A user account could be for an administrator accessing the cluster to perform administrative tasks, or a developer accessing the cluster to deploy applications, et cetera.
- The service accounts are used by machines. A service account could be an account used by an application to interact with the Kubernetes cluster.

![image](https://github.com/user-attachments/assets/2f8bd9eb-19da-4b94-97b0-e5025cf73cdf)

In this example, you would create a Role with the necessary permissions (view and create Pods) and a RoleBinding that associates the Role with the user "alice" in the "app-namespace" namespace.

![image](https://github.com/user-attachments/assets/3421ad58-5eff-4ac0-a31e-4b3471c0fc1c)

This configuration grants the "alice" user the ability to view and create Pods within the "app-namespace" namespace.

##### 4. Resources and Verbs:
- Resources are the objects in Kubernetes (like pods, services, deployments, etc.) that you want to control access to.
- Verbs are the actions that can be performed on those resources (like get, list, create, update, delete).

![image](https://github.com/user-attachments/assets/17e3194b-dd28-482a-add3-8da96eecf942)

#### How RBAC Works:
1. **Authentication**: When a user or application attempts to interact with the Kubernetes API, they must first authenticate themselves (e.g., using certificates, tokens, etc.).
2. **Authorization**: After authentication, Kubernetes checks the user's permissions against the defined Roles and RoleBindings (or ClusterRoles and ClusterRoleBindings) to determine if the action is allowed.
3. **Decision**: If the user has the necessary permissions, the action is allowed; otherwise, it is denied.

Example: Jane wants to grant access to the cluster. Jane should have permission to create, list, get, update and delete pods in the development namespace. (Using certificates)

![image](https://github.com/user-attachments/assets/a6e65216-dec1-4114-9e4c-777242e513d3)

A user first create a key, then generate a certificate signing request using the key with her name on it, then sends the request to the administrator.

![image](https://github.com/user-attachments/assets/788d07c6-4fe0-4f00-a756-b8f36f22962b)

The request field is where you specify the certificate signing request sent by the user, but you don't specify it as plaintext. Instead, it must be encoded using the Base64 command. Then move the encoded text into the request field and then submit the request.

![image](https://github.com/user-attachments/assets/a8314400-8b35-4404-a59f-5325cedeb2f4)

![image](https://github.com/user-attachments/assets/76292fb3-eed3-4c37-badd-2989e6b22c9e)

Identify the new request and approve the request by running the **kubectl certificate approve** command.

![image](https://github.com/user-attachments/assets/edfc5420-b48f-422d-b803-e8e3890a2d99)

Next, create a role developer and rolebinding developer-role-binding, run the command:

![image](https://github.com/user-attachments/assets/ba6fb788-1136-4f53-a7e7-0a10f65a5a37)

To verify the permission from kubectl utility tool:

![image](https://github.com/user-attachments/assets/9b93a92c-4405-4e5c-928f-49c4b149b3f5)


#### Benefits of RBAC:
- **Granular Control**: RBAC allows for fine-grained access control, enabling administrators to specify exactly what actions users can perform on specific resources.
- **Least Privilege Principle**: By assigning only the necessary permissions to users based on their roles, RBAC helps enforce the principle of least privilege, reducing the risk of unauthorized access.
- **Ease of Management**: Roles and bindings can be easily managed and modified, making it simpler to adapt to changing organizational needs.


### 3. Security Auditing Tools  

![image](https://github.com/user-attachments/assets/643383ef-b474-4366-8e05-dee85d332980)

- What is CIS Benchmark?
  
**CIS Benchmarks** from the **Center for Internet Security (CIS)** are a set of best practices and guidelines developed by the Center for Internet Security (CIS) to help organizations secure their systems and data. These benchmarks provide detailed configuration recommendations for various operating systems, applications, cloud providers, and network devices, aimed at reducing vulnerabilities and enhancing overall security posture.

**Key features:**
- **Comprehensive Guidelines**: Each benchmark includes a list of security controls and configuration settings.
- **Consensus-Based**: Developed through collaboration among cybersecurity experts from various sectors.
- **Regular Updates**: Benchmarks are updated to reflect the latest security threats and best practices.
- **Free and Open**: Available for organizations of all sizes to access and implement.
  
- What is Penetration Testing?

Penetration Testing (or pen testing) is a simulated cyber attack against a computer system, network, or web application to identify vulnerabilities that could be exploited by malicious actors. The goal is to evaluate the security of the system and assess how well it can withstand an attack.

**Key Aspects:**
- **Types of Testing**: Black box, white box, and gray box testing.
- **Phases**: Planning, reconnaissance, scanning, exploitation, post-exploitation, and reporting.
- **Benefits**: Identifies vulnerabilities, improves security posture, and helps meet compliance requirements.

- Why need tools?
1. **Efficiency**: Automate the process of identifying vulnerabilities, saving time and resources.
2. **Accuracy**: Reduce human error in vulnerability assessments and configuration checks.
3. **Comprehensive Coverage**: Tools can scan large environments and provide a thorough assessment of security posture.
4. **Continuous Monitoring**: Many tools offer ongoing assessments, helping organizations stay ahead of emerging threats.
5. **Reporting**: Tools often provide detailed reports that help organizations understand their security status and prioritize remediation efforts.

 
- **Demo:**  
  - kube-bench (CIS Benchmark Testing)
    
   **kube-bench** is a tool that checks whether Kubernetes is deployed securely by running the CIS Kubernetes Benchmark tests. It automates the process of evaluating the security configuration of a Kubernetes cluster against the CIS Benchmark recommendations.

   **Key Features**:
     - **Automated Checks**: Runs a series of tests to verify compliance with the CIS Kubernetes Benchmark.
     - **Detailed Reporting**: Provides a report of the findings, including passed and failed checks.
     - **Remediation Guidance**: Offers recommendations for addressing any identified issues.

  - kube-hunter (Kubernetes Penetration Testing) is a penetration testing tool designed specifically for Kubernetes clusters. It helps identify security vulnerabilities by simulating attacks against the cluster.

    **Key Features**:
       - **Active Scanning**: Conducts active reconnaissance to find vulnerabilities in the Kubernetes environment.
       - **Attack Simulation**: Simulates various attack vectors to assess the security of the cluster.
       - **Detailed Reports**: Provides a report of vulnerabilities found, along with their severity and potential impact.


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

----------------------------------------------------------------------------------------------------------------------

## Module 4. Code Security  
### 1. Vulnerability Scanning & Intrusion Detection  
- What it does?  (Definitions, Threats, Benefits)

### 2. Scanning Container Images for CVEs  
- What is CVE?
- **CVE Scanning with Trivy** (Scans container images for known CVEs)  

### 3. Runtime Intrusion Detection  
- **Project Falco** and Intrusion Detection (Detecting abnormal behavior in clusters)  

---------------------------------------------------------------------------------------------------------------------------------

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
