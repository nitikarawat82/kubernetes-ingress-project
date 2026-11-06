# 🚀 Kubernetes Ingress Practical

A simple Kubernetes project demonstrating **path-based routing using NGINX Ingress Controller**.

## 📌 About the Project

In this project, we deploy two different applications on a Kubernetes cluster and access them using a single entry point.

- 🌐 `/nginx` – Displays the default NGINX welcome page.
- 💻 `/myapp` – Displays my personal website built using Python Flask.

The main goal is to understand Kubernetes Deployments, Services, Ingress, and how traffic is routed to different applications.

## 🛠️ Technologies Used

- AWS EC2
- Docker
- Docker Hub
- Kubernetes (Kind)
- NGINX Ingress Controller
- Python Flask
- Git & GitHub
- Shell Scripting

## 📂 Project Structure

```text
kubernetes-ingress-project/
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   ├── Dockerfile
│   └── templates/
│       └── index.html
│
├── kubernetes/
│   ├── kind-config.yaml
│   ├── nginx-deployment.yaml
│   ├── nginx-service.yaml
│   ├── myapp-deployment.yaml
│   ├── myapp-service.yaml
│   └── ingress.yaml
│
├── install.sh
└── README.md
```

## ⚙️ How to Run

### Step 1: Create an AWS EC2 Instance

- Launch an Ubuntu EC2 instance.
- Configure the Security Group to allow:
  - SSH (22) – My IP
  - HTTP (80) – 0.0.0.0/0
- Connect to the EC2 instance using SSH.

### Step 2: Clone the GitHub Repository

```bash
git clone https://github.com/nitikarawat82/kubernetes-ingress-project.git

cd kubernetes-ingress-project
```

### Step 3: Install Required Tools

Run the installation script to install Docker, kubectl, and Kind.

```bash
chmod +x install.sh

./install.sh
```

### Step 4: Create the Kubernetes Cluster

```bash
kind create cluster --name mycluster --config kubernetes/kind-config.yaml
```

The cluster contains:

- 1 Control Plane Node
- 2 Worker Nodes

The Kind configuration also maps host port 80 to port 80 on the control-plane node.

### Step 5: Build the Flask Docker Image

Build the Docker image from the application directory.

```bash
docker build -t ingress-practical .
```

### Step 6: Push the Docker Image to Docker Hub

Log in to Docker Hub:

```bash
docker login
```

Tag the image with your Docker Hub username:

```bash
docker tag ingress-practical YOUR-DOCKERHUB-USERNAME/ingress-practical:v1
```

Push the image:

```bash
docker push YOUR-DOCKERHUB-USERNAME/my-personal-website:v1
```

**Important:** Update the image field in `kubernetes/myapp-deployment.yaml` with your Docker Hub image:

```yaml
image: YOUR-DOCKERHUB-USERNAME/my-personal-website:v1
imagePullPolicy: Always
```

Kind will pull the image from Docker Hub when the Deployment creates its Pods. 

### Step 7: Install NGINX Ingress Controller

Clone the official F5 NGINX Ingress Controller repository:

```bash
git clone https://github.com/nginx/kubernetes-ingress.git --branch v5.7.0

cd kubernetes-ingress
```

Create the namespace and service account:

```bash
kubectl apply -f deployments/common/ns-and-sa.yaml
```

Apply RBAC permissions:

```bash
kubectl apply -f deployments/rbac/rbac.yaml
```

Create the NGINX configuration:

```bash
kubectl apply -f deployments/common/nginx-config.yaml
```

Create the IngressClass:

```bash
kubectl apply -f deployments/common/ingress-class.yaml
```

Install the required Custom Resource Definitions:

```bash
kubectl apply -f https://raw.githubusercontent.com/nginx/kubernetes-ingress/v5.7.0/deploy/crds.yaml
```

Install the NGINX Ingress Controller:

```bash
kubectl apply -f deployments/daemon-set/nginx-ingress.yaml
```

Ensure the control-plane node has the ingress-ready label:

```bash
kubectl label node mycluster-control-plane ingress-ready=true
```

Configure the controller to run on the control-plane node, which has host port 80 mapped:

```bash
kubectl patch daemonset nginx-ingress -n nginx-ingress --type=merge -p '{"spec":{"template":{"spec":{"nodeSelector":{"ingress-ready":"true"},"tolerations":[{"key":"node-role.kubernetes.io/control-plane","operator":"Exists","effect":"NoSchedule"}]}}}}'
```

Verify the controller:

```bash
kubectl get pods -n nginx-ingress -o wide
```

### Step 8: Deploy Applications and Services

Go back to your project directory:

```bash
cd ~/kubernetes-ingress-project
```

Apply the Kubernetes manifests:

```bash
kubectl apply -f kubernetes/
```

This creates the NGINX and Flask Deployments, Services, and Ingress resource.

### Step 9: Verify Deployments and Pods

```bash
kubectl get nodes

kubectl get deployments

kubectl get pods

kubectl get services

kubectl get ingress

kubectl get pods -n nginx-ingress
```

## 🌍 Accessing the Applications

Once the Kubernetes Deployments and Ingress configuration are successfully applied, access the applications using your EC2 Public IP with the nip.io hostname.

**1. NGINX Application**

```text
http://EC2-PUBLIC-IP.nip.io/nginx
```

Displays the default NGINX welcome page.

**2. My Personal Website**

```text
http://EC2-PUBLIC-IP.nip.io/myapp
```

Displays my personal website built using Python Flask.

**Example:**

```text
http://54.242.200.247.nip.io/nginx
http://54.242.200.247.nip.io/myapp
```

Make sure port 80 is allowed in the EC2 Security Group and the NGINX Ingress Controller is running.

## 🌍 Expected Output

| URL | Application |
|---|---|
| `/nginx` | NGINX Welcome Page |
| `/myapp` | Personal Website |

Both applications are accessed through a single EC2 public IP using path-based routing.

## 📚 What I Learned

- Creating and managing Kubernetes clusters using Kind.
- Deploying applications using Kubernetes Deployments.
- Exposing applications using ClusterIP Services.
- Building Docker images and pushing them to Docker Hub.
- Installing and configuring NGINX Ingress Controller.
- Implementing path-based routing.
- Managing Kubernetes resources using kubectl.
- Automating tool installation using Shell Scripting.
- Deploying applications on AWS EC2.

## 📝 Detailed Documentation

Want to understand the complete implementation?

I've shared the detailed explanation, configurations, and step-by-step practical guide on my Hashnode blog.

👉 **[Read the Complete Project on Hashnode](YOUR_HASHNODE_BLOG_LINK)**

---

⭐ If you find this project useful, feel free to explore and share your feedback!
