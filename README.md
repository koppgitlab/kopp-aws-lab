# 🛠️ Kopp AWS Lab  

**Epic 2 – AWS Cloud Engineering Lab & Personal Website (MVP by Sept 26, 2025)**  

This repository contains Infrastructure-as-Code (IaC), CI/CD pipeline definitions, and application code that support the design and deployment of a **personal AWS hosting environment**. The lab demonstrates **modern cloud delivery practices** including:  

- Infrastructure as Code (Terraform)  
- CI/CD DevSecOps pipelines (GitHub Actions → AWS via OIDC)  
- AWS-native services (App Runner, ECR, Route 53, ACM, CloudWatch, DynamoDB, SES)  
- Secure IAM design (least privilege, role boundaries, visitor read-only access)  
- Cost visibility (AWS Budgets + Anomaly Detection)  
- Portfolio website deployed as a **containerized app**  

The environment supports a **live portfolio site** that also allows **visitors (e.g., recruiters/interviewers)** to request **temporary read-only access** to inspect the cloud environment.  

---

## 🎯 Purpose  

This lab serves two goals:  

1. **Skill development & demonstration**  
   - Apply cloud engineering, DevSecOps, and FinOps practices in a hands-on project.  
   - Showcase ability to design, deploy, and manage a secure, cost-conscious AWS environment.  

2. **Portfolio site**  
   - Provide a live website that highlights my professional skills.  
   - Demonstrate modern delivery practices, not just “click-to-deploy” shortcuts.  

> For full context, see the Confluence page: *Epic 2 – AWS Cloud Engineering Lab & Personal Website (Architecture Vision)*  

---

## 🔭 MVP (v1.0) Scope – Target by Sept 26, 2025  

- ⬜ Containerized portfolio website app built with Docker, stored in **Amazon ECR**  
- ⬜ Deployed using **AWS App Runner** (or ECS Fargate as alternative)  
- ⬜ Public URL live with **ACM TLS cert**; optional domain via Route 53  
- ⬜ **CI/CD pipeline** (GitHub Actions → AWS OIDC) with stages for build, test, SAST, SCA, secrets scan, container scan, IaC scan, deploy  
- ⬜ Terraform modules for all resources (ECR, App Runner, Route 53, IAM, Budgets, DynamoDB, SES)  
- ⬜ **Visitor access workflow v1.0**:  
  - Website form → API Gateway + Lambda + DynamoDB  
  - Manual approval by owner via SES email  
  - Short-lived federated console URL (~60 minutes) with **read-only IAM role** (explicit denies on sensitive services)  
- ⬜ Tagging standards and cost visibility enabled  

---

## 🔐 Post-MVP (v1.1) Enhancements  

Planned for Sprint 3+ after MVP release:  

- Identity Center integration for temporary visitor accounts (manual approval remains)  
- Demo AWS account separation for safer visitor access  
- Automated cleanup via EventBridge + Lambda (expire/revoke sessions)  
- Stronger abuse protection (WAF, rate limiting, email verification, request quotas)  
- Scoped read-only roles limited to specific ARNs/resources (App Runner, ECS services, ECR repos, CloudWatch groups)  
- Visitor UX: request duration options (30/60/90 min), “What you can see” explainer, friendlier error pages  

---

## 🛡️ Architecture (MVP v1.0)  

- **Compute:** AWS App Runner (1 vCPU, 2 GB RAM)  
- **Container Registry:** Amazon ECR  
- **CI/CD:** GitHub Actions with OIDC → AWS IAM Role  
- **IaC:** Terraform OSS (S3 backend + DynamoDB lock table)  
- **Networking:** Public App Runner endpoint (TLS by ACM)  
- **DNS:** Route 53 (optional custom domain)  
- **Observability:** CloudWatch Logs + simple alarms  
- **Visitor Access:** API Gateway + Lambda + DynamoDB + SES + temporary read-only IAM role  

---

## 🧩 Repo Structure (proposed)  

kopp-aws-lab/  
├── app/ # Portfolio website containerized app  
├── terraform/ # IaC modules  
│ ├── apprunner/  
│ ├── ecr/  
│ ├── iam/  
│ ├── dns/  
│ ├── budgets/  
│ └── visitor-access/  
├── .github/workflows/ # GitHub Actions CI/CD pipeline definitions  
├── README.md  
└── LICENSE

---

## 🚦 CI/CD Pipeline (first iteration)  

**Triggering:**  
- PR → deploy to **dev** environment (ephemeral option later)  
- Tag (v1.0.0) → deploy to **prod**  

**Stages:**  
1. Build & unit test  
2. Static analysis (CodeQL / Sonar)  
3. Dependency audit (`npm audit`, `pip-audit`)  
4. Secrets scan (Gitleaks)  
5. Container scan (Trivy)  
6. IaC scan (tflint, tfsec)  
7. Push image → ECR  
8. Deploy → App Runner  
9. Post-deploy smoke test  

---

## 💰 Cost Awareness  

- Free Tier covers most of year one.  
- Post-Free Tier: ~**$45–50/month** baseline for:  
  - App Runner compute (~$40)  
  - Route 53 + domain (~$1.50)  
  - Logs/alarms (~$2)  
  - DynamoDB/SES/API GW/Lambda (~$1)  
- Scaling: each new App Runner service adds ~$30–40/month.  

> FinOps principle: right-size, tag, monitor, and use budgets/anomaly detection.  

---

## 📅 Roadmap  

- **Sprint 1 (Sept 1–12, 2025):**  
  - AWS Free Tier account created  
  - Terraform backend (S3+DynamoDB) set up  
  - IAM roles defined (TerraformDeployer, GitHubActionsDeploy, AppRunnerServiceRole, ReadOnlyAudit)  
  - GitHub repo + CI skeleton created  

- **Sprint 2 (Sept 13–26, 2025):**  
  - MVP portfolio website deployed live (App Runner)  
  - CI/CD pipeline delivering to dev/prod  
  - Visitor access v1.0 implemented (manual approval → console URL)  
  - Confluence stakeholder dashboard updated  

- **Sprint 3+:**  
  - Visitor access v1.1 (Identity Center, Demo account separation, auto-cleanup)  
  - Additional app services/microservices  
  - CloudFront CDN, improved observability, FinOps dashboards  

---

## 📖 Related Links  

- 🔗 [Epic 2 – AWS Cloud Engineering Lab & Personal Website (Architecture Vision)](add Confluence link here)  
- 🔗 [Stakeholder Dashboard](add Confluence link here)  
- 🔗 [Epic 1 – Jira & Confluence Learning Roadmap](add Confluence link here)  

---

## 📜 License  

MIT License — see [LICENSE](./LICENSE) for details.  

---
