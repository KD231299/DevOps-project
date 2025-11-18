🟦 Kubernetes Deployment on AWS EKS — DevOps Project
Containerized App • EKS Cluster • HPA Autoscaling • Prometheus & Grafana Monitoring
This project demonstrates how to deploy a Dockerized web application to AWS EKS (Kubernetes) with load balancing and autoscaling.
It also includes CI/CD best practices, GitHub version control, and Infrastructure-as-Code principles.

🚀 Architecture Overview:
Developer → GitHub → Docker Build → DockerHub → EKS Cluster → LoadBalancer → User
                                                         │
                                                + HPA Autoscaling
📁 Project Structure:
app/
│── Dockerfile
│── index.html
│── style.css
│── script.js
│── deployment.yaml
└── service.yaml

📌 Features:
✔ Dockerized static web application
✔ Deployed to AWS EKS using kubectl & YAML
✔ Exposed using LoadBalancer Service
✔ Autoscaling enabled via Horizontal Pod Autoscaler (HPA)
✔ Monitoring using Prometheus + Grafana
✔ Hosted code on GitHub
✔ Demonstratable live URL via EKS LoadBalancer

🛠️ Technologies Used:
•	AWS EKS
•	Kubernetes (Deployments, Services, HPA)
•	Docker & DockerHub
•	Git & GitHub
•	Ubuntu EC2
•	YAML / kubectl

🧰 Commands Summary:
1️⃣ Install Required Tools:
Install Docker and unzip
sudo apt update
sudo apt install -y unzip curl git docker.io
Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws configure
Install kubectl
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.30.0/2024-05-01/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
Install eksctl
curl -s "https://api.github.com/repos/eksctl-io/eksctl/releases/latest" \
| grep "linux_amd64.tar.gz" | cut -d '"' -f 4 | wget -qi -

tar -xzf eksctl*.tar.gz
sudo mv eksctl /usr/local/bin/

2️⃣ Create EKS Cluster:
cluster.yaml:
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: project3-eks
  region: us-east-1

nodeGroups:
  - name: nodegroup-1
    instanceType: t3.small
    desiredCapacity: 2
    minSize: 2
    maxSize: 4
    volumeSize: 20

Create cluster:
	eksctl create cluster -f cluster.yaml

3️⃣ Build & Push Docker Image:
docker build -t kd231299/k8s-app:v1 .
docker push kd231299/k8s-app:v1

4️⃣ Deploy to Kubernetes:
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
Check:
kubectl get pods
kubectl get svc

5️⃣ Enable Autoscaling:
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
Create HPA:
	kubectl autoscale deployment k8s-app --cpu-percent=50 --min=2 --max=6

🌐 Live Application URL:
Get LoadBalancer:
kubectl get svc
