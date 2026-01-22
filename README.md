# Create AWS EKS Cluster and Node Groups using Terraform

This project demonstrates how to build a **basic yet production-aligned Amazon EKS cluster** using **Terraform**, including **public and private managed node groups**, IAM roles, networking integration, and operational verification.

The repository is designed to help understand **how EKS works under the hood**, not just how to click through the console.

---

## What This Project Covers

- Create an Amazon EKS control plane using Terraform
- Configure IAM roles for EKS cluster and node groups
- Deploy **public and private managed node groups**
- Integrate EKS with an existing VPC (public & private subnets)
- Configure EKS API endpoint access (public/private)
- Verify cluster, networking, and system components
- Deploy and validate Kubernetes resources
- Perform cost-aware cleanup and optimization

---

## Architecture Overview

**High-level design:**

- Amazon EKS Control Plane (managed by AWS)
- Public Node Group (for demos and access)
- Private Node Group (for real-world workloads)
- Bastion Host for secure access
- VPC with public and private subnets
- NAT Gateway for private node outbound access

---

## Repository Structure

```bash
01-ekscluster-terraform-manifests/
├── c1-versions.tf
├── c2-01-generic-variables.tf
├── c2-02-local-values.tf
├── c3-01-vpc-variables.tf
├── c3-02-vpc-module.tf
├── c3-03-vpc-outputs.tf
├── c4-ec2-bastion/
├── c5-eks/
│   ├── c5-01-eks-variables.tf
│   ├── c5-03-iamrole-for-eks-cluster.tf
│   ├── c5-04-iamrole-for-eks-nodegroup.tf
│   ├── c5-05-securitygroups-eks.tf
│   ├── c5-06-eks-cluster.tf
│   ├── c5-07-eks-node-group-public.tf
│   ├── c5-08-eks-node-group-private.tf
│   ├── c5-02-eks-outputs.tf
├── eks.auto.tfvars
```

---

## Prerequisites

- AWS account
- AWS CLI configured (`aws configure`)
- Terraform installed (v1.x recommended)
- kubectl CLI installed
- EC2 key pair for node group SSH access

---

## Step-by-Step Flow

### Step 1: Create EKS Cluster
- Define cluster name, version, and networking
- Configure API server endpoint access
- Enable control plane logging

### Step 2: IAM Roles
- IAM role for EKS control plane
- IAM role for EKS managed node groups
- Attach required AWS-managed policies

### Step 3: Node Groups
- **Public node group** for demos and learning
- **Private node group** for real-world workload placement
- Configure scaling, instance types, and AMI

### Step 4: Deploy Infrastructure

```bash
terraform init
terraform validate
terraform plan
terraform apply -auto-approve
```

---

## Verify EKS Cluster

### AWS Console
- EKS → Clusters → Overview
- Compute → Node groups
- Networking → VPC & subnets
- Logging → Control plane logs

### CLI Verification

```bash
aws eks --region us-east-1 update-kubeconfig --name <cluster-name>
kubectl get nodes
kubectl get nodes -o wide
kubectl get pods -n kube-system
```

---

## Kubernetes System Components

Verified components:
- CoreDNS (Deployment)
- aws-node (CNI DaemonSet)
- kube-proxy (DaemonSet)
- kube-system namespace resources

All system images are pulled from **Amazon ECR**, not Docker Hub.

---

## Cost Optimization Demonstration

To reduce costs for demos:

- Private node group can be commented out
- Bastion host can be stopped when not in use
- Single-node public node group used for remaining demos

This shows **real-world cost-conscious infrastructure management**.

---

## Cleanup

```bash
terraform destroy -auto-approve
rm -rf .terraform* terraform.tfstate*
```

---

## Key Learning Outcomes

- Understand EKS architecture beyond the console
- Learn how EKS networking and IAM integration works
- Practice Terraform-driven Kubernetes provisioning
- Gain confidence troubleshooting EKS internals
- Apply cost optimization strategies in cloud environments

---


