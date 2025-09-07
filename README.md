## 🏗️ Multi‑Tier WordPress on AWS – Highly Available, Auto‑Scaling Stack

### 📌 Overview  
Production‑grade WordPress deployment engineered on AWS with multi‑AZ resilience, auto scaling, and shared storage. Built using CloudFormation, EC2, Auto Scaling, ALB, Amazon Aurora (RDS), and EFS. Designed for repeatable, zero‑to‑prod spins with minimal manual ops.

---

### 🎯 Project Goals  
- Deploy WordPress across multiple Availability Zones for high availability  
- Automate network, compute, and data layers via Infrastructure as Code  
- Scale application servers automatically based on load  
- Use managed database (Aurora) and shared storage (EFS) for durability  
- Validate end‑to‑end access via ALB DNS with health checks

---

### 🛠️ Services Used  
- AWS CloudFormation  
- Amazon EC2 + Launch Template  
- Amazon EC2 Auto Scaling  
- Application Load Balancer (ALB) + Target Groups  
- Amazon Aurora (MySQL-compatible) on RDS  
- Amazon EFS  
- Amazon VPC (Subnets, IGW, NAT), Security Groups, IAM

---

### 🚀 Setup Steps  
1. Provisioned VPC, subnets (public/app/db), route tables, IGW, and NAT with CloudFormation  
2. Deployed Aurora (writer + reader) in Multi‑AZ and captured writer endpoint and DB name  
3. Created EFS with mount targets in both AZs for shared wp‑content  
4. Configured ALB and target group with health checks on `/wp-login.php`  
5. Built an EC2 launch template with user data to install WordPress, mount EFS, and wire `wp-config.php` to Aurora  
6. Created an Auto Scaling Group (desired=2, min=2, max=4) attached to ALB  
7. Validated ALB DNS → WordPress login → Admin dashboard

---

### 🧩 Deployment Summary  
- Network foundation deployed via CloudFormation (VPC, subnets, NAT, security groups)  
- Aurora Multi‑AZ DB cluster integrated with application tier  
- EFS provides regional, shared storage across app instances  
- ALB balances traffic and performs health checks against app instances  
- Auto Scaling Group ensures elasticity and resilience during spikes and failures

---

### 🧪 Test Scenario  
Accessed the ALB DNS and verified:  
✅ Target group health turns healthy after bootstrap  
✅ `/wp-login.php` loads successfully  
✅ WordPress admin dashboard accessible post‑login

---

### ✅ Real‑World Validation  
- Multi‑AZ design sustains availability during instance failures or AZ disruptions  
- Shared EFS eliminates content drift across scaling events  
- CloudFormation reduces manual provisioning effort by ~70% and standardizes environments

---

### 📸 Screenshot Checklist  
- CloudFormation: Stack created (network + launch template)  
- VPC/Subnets: Public, app, and DB layout across two AZs  
- RDS Aurora: Cluster (writer/reader) and connectivity details  
- EFS: File system and mount targets in both AZs  
- ALB: Created with target group and health checks  
- ASG: Instances InService and healthy behind ALB  
- Validation: WordPress login page and admin dashboard

---

### 📚 Learnings  
- Health check tuning (path, interval, grace period) speeds stabilization  
- NAT + private app subnets are essential for secure package installs during bootstrap  
- EFS enables stateless app instances and clean scale‑out behavior  
- CloudFormation parameters (DB name, writer endpoint, EFS ID, ALB DNS) drive reproducible builds

---

### 📈 Next Steps  
- Add SSM Parameter Store/Secrets Manager for DB credentials  
- Enable Aurora backups and performance insights  
- Add CloudWatch dashboards and ALB access logs  
- Introduce blue‑green or rolling deployments for safe updates  
- Integrate WAF for Layer‑7 protections

---

## 🧠 Why This Matters  
This architecture mirrors production patterns used by enterprises: decoupled tiers, managed data services, and automated scaling for predictable reliability and cost control. It’s a practical template for migrating CMS workloads with minimal ops overhead.

---

### 👨‍💻 Author Note  
Akshith — Cloud builder focused on production‑ready, multi‑AZ architectures using Infrastructure as Code, managed data services, and operational excellence.
