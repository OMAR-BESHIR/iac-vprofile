#  AWS EKS Infrastructure with Terraform & GitHub Actions for vprofile project

This repository contains the **Infrastructure as Code (IaC)** for deploying a production-ready **Amazon EKS Cluster** using Terraform. The entire infrastructure lifecycle is automated via **GitHub Actions CI/CD**.

## 🏗️ Architecture Overview

The following diagram illustrates the workflow and the cloud infrastructure components involved in this project:

![Architecture Diagram](Screenshot%202026-03-15%20163518.png)

### Key Components:
* **Terraform:** Managed infrastructure provisioning (VPC, EKS, Node Groups).
* **GitHub Actions:** Automates Terraform Plan and Apply on `main` branch.
* **Amazon EKS:** Managed Kubernetes service.
* **Ingress Nginx:** Deployed automatically after cluster setup for traffic routing.

---

## ☁️ Provisioned Resources

Once the pipeline completes, the following resources are live on AWS:

### 1. Amazon EKS Cluster
The cluster is configured with the latest stable Kubernetes version and high availability settings.

![EKS Cluster Dashboard](Screenshot%202026-03-11%20153022.png)

### 2. Managed Node Groups
Scalable worker nodes (e.g., `t3.small`) are managed within a VPC subnet to handle application workloads.

![EKS Node Groups](Screenshot%202026-03-11%20153048.png)

---

## 🛠️ CI/CD Workflow Breakdown

The GitHub Actions workflow (`Vprofile IAC`) performs the following steps:

1.  **Checkout Code:** Pulls the Terraform scripts.
2.  **AWS Authentication:** Uses GitHub Secrets for secure access.
3.  **Terraform Init & Validate:** Initializes the backend (S3) and checks syntax.
4.  **Terraform Plan:** Generates an execution plan to preview changes.
5.  **Terraform Apply:** Triggered only on `main` branch via `workflow_dispatch`.
6.  **Post-Deployment:** * Updates `kubeconfig`.
    * Verifies node status (`kubectl get nodes`).
    * Installs **NGINX Ingress Controller** automatically.

## 🚀 How to Use

1.  **Prerequisites:** * AWS Account & IAM User with admin permissions.
    * S3 Bucket for Terraform State.
2.  **GitHub Secrets:** Add `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `BUCKET_TF_STATE` to your repo secrets.
3.  **Trigger:** Push to `main` or manually trigger the workflow from the **Actions** tab.

