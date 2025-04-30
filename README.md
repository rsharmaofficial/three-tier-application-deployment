
# 🚀 TWS Three-Tier App Challenge

Welcome to the **#TWSThreeTierAppChallenge**! This repository is dedicated to deploying a **Three-Tier Web Application** using **ReactJS**, **NodeJS**, and **MongoDB**, orchestrated on **AWS EKS**. Participants are encouraged to enhance the application creatively and submit Pull Requests (PRs). Merged PRs will be rewarded with exciting prizes! 🏆

---

## 📁 Repository Structure

```
three-tier-application-deployment/
├── Application-Code/
│   ├── frontend/        # ReactJS Frontend
│   └── backend/         # NodeJS Backend
├── Jenkins-Pipeline-Code/     # Jenkins CI/CD Pipelines
├── Jenkins-Server-TF/         # Terraform scripts for Jenkins setup
├── Kubernetes-Manifests-file/ # Kubernetes YAMLs for EKS deployment
├── assets/                    # Screenshots and diagrams
├── LICENSE
└── README.md
```

---

## 🛠️ Tech Stack & Tools

- **Frontend**: ReactJS
- **Backend**: NodeJS
- **Database**: MongoDB
- **Containerization**: Docker
- **CI/CD**: Jenkins, SonarQube
- **Infrastructure as Code**: Terraform
- **Orchestration**: Kubernetes on AWS EKS
- **Monitoring**: Prometheus, Grafana
- **GitOps**: ArgoCD

---

## 🧩 Project Components

### 1. Application Code

Located in the `Application-Code/` directory:
- `frontend/`: Contains the ReactJS frontend application.
- `backend/`: Contains the NodeJS backend application.

### 2. Jenkins Pipeline Code

Found in the `Jenkins-Pipeline-Code/` directory:
- Jenkinsfiles and scripts to automate the CI/CD pipeline.

### 3. Jenkins Server Terraform

Located in the `Jenkins-Server-TF/` directory:
- Terraform scripts to provision Jenkins server infrastructure on AWS.

### 4. Kubernetes Manifests

Found in the `Kubernetes-Manifests-file/` directory:
- YAML files to deploy the application components on AWS EKS.

---

## 🚀 Deployment Guide

### Prerequisites

- AWS account with necessary permissions.
- Basic knowledge of Docker, Kubernetes, and AWS services.

### Steps

1. **IAM Configuration**
   - Create an IAM user `eks-admin` with `AdministratorAccess`.
   - Generate Access Key and Secret Access Key.

2. **EC2 Setup**
   - Launch an Ubuntu EC2 instance in your preferred region (e.g., `us-west-2`).
   - SSH into the instance.

3. **Install AWS CLI v2**
   ```bash
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   sudo apt install unzip
   unzip awscliv2.zip
   sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin --update
   aws configure
   ```

4. **Install Docker**
   ```bash
   sudo apt-get update
   sudo apt install docker.io
   sudo chown $USER /var/run/docker.sock
   ```

5. **Install kubectl**
   ```bash
   curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
   chmod +x ./kubectl
   sudo mv ./kubectl /usr/local/bin
   kubectl version --short --client
   ```

6. **Install eksctl**
   ```bash
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   eksctl version
   ```

7. **Setup EKS Cluster**
   ```bash
   eksctl create cluster --name three-tier-cluster --region us-west-2 --node-type t2.medium --nodes-min 2 --nodes-max 2
   aws eks update-kubeconfig --region us-west-2 --name three-tier-cluster
   kubectl get nodes
   ```

8. **Deploy Kubernetes Manifests**
   ```bash
   kubectl create namespace workshop
   kubectl apply -f Kubernetes-Manifests-file/
   ```

9. **Install AWS Load Balancer Controller**
   ```bash
   curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
   aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json
   eksctl utils associate-iam-oidc-provider --region=us-west-2 --cluster=three-tier-cluster --approve
   eksctl create iamserviceaccount      --cluster=three-tier-cluster      --namespace=kube-system      --name=aws-load-balancer-controller      --role-name AmazonEKSLoadBalancerControllerRole      --attach-policy-arn=arn:aws:iam::<your-account-id>:policy/AWSLoadBalancerControllerIAMPolicy      --approve      --region=us-west-2
   ```

10. **Deploy AWS Load Balancer Controller**
    ```bash
    sudo snap install helm --classic
    helm repo add eks https://aws.github.io/eks-charts
    helm repo update eks
    helm install aws-load-balancer-controller eks/aws-load-balancer-controller       -n kube-system       --set clusterName=three-tier-cluster       --set serviceAccount.create=false       --set serviceAccount.name=aws-load-balancer-controller
    kubectl get deployment -n kube-system aws-load-balancer-controller
    ```

---

## 🧹 Cleanup

To delete the EKS cluster and associated resources:

```bash
eksctl delete cluster --name three-tier-cluster --region us-west-2
```

Additionally, ensure to:
- Terminate the EC2 instance created earlier.
- Delete the Load Balancer and associated security groups via the AWS Console.

---

## 🤝 Contribution Guidelines

1. Fork the repository.
2. Create a new branch for your feature or enhancement.
3. Make your changes and ensure they adhere to the project's coding standards.
4. Submit a Pull Request with a detailed description of your changes.

---

## 🎁 Rewards

Successful PR merges will be eligible for exciting prizes! Show off your DevOps skills and contribute to the community.

---

## 📞 Support

For any queries or issues, please open an [issue](https://github.com/rsharmaofficial/three-tier-application-deployment/issues) in the repository.

---

Happy Learning! 🚀👨‍💻👩‍💻
