# 🚀 Kubernetes Ingress Practical

A simple Kubernetes project demonstrating **path-based routing using Nginx Ingress Controller**.

## 📌 About the Project

In this project, we deploy two different applications on a Kubernetes cluster and access them using a single entry point.

- 🌐 `/nginx` – Displays the default Nginx welcome page.
- 💻 `/myapp` – Displays my personal website built using Python Flask.

The main goal is to understand Kubernetes Deployments, Services, Ingress, and how traffic is routed to different applications.

## 🛠️ Technologies Used

- AWS EC2
- Docker
- Kubernetes (Kind)
- Nginx Ingress Controller
- Python Flask
- Git & GitHub
- Shell Scripting

## 📂 Project Structure

```text
kubernetes-ingress-practical/
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
- Configure the Security Group to allow SSH (22) and HTTP (80).
- Connect to the EC2 instance using SSH.

### Step 2: Clone the GitHub Repository

```text
git clone YOUR_GITHUB_REPOSITORY_URL

cd kubernetes-ingress-practical
```

### Step 3: Install Required Tools

Run the installation script to install Docker, kubectl, and Kind.

```text
chmod +x install.sh

./install.sh
```

### Step 4: Create the Kubernetes Cluster

```text
kind create cluster --config kubernetes/kind-config.yaml
```

The cluster contains:
- 1 Control Plane Node
- 2 Worker Nodes

### Step 5: Build the Flask Docker Image

```text
docker build -t my-personal-website:v1 ./app
```

### Step 6: Load the Image into Kind

```text
kind load docker-image my-personal-website:v1
```

### Step 7: Install Nginx Ingress Controller

Install the Nginx Ingress Controller using the official Kind installation instructions.

### Step 8: Deploy Applications and Services

```text
kubectl apply -f kubernetes/
```

### Step 9: Verify Deployments and Pods

```text
kubectl get nodes

kubectl get deployments

kubectl get pods

kubectl get services

kubectl get ingress
```

## 🌍 Accessing the Applications

Once the Kubernetes deployments and Ingress configuration are successfully applied, access the applications using your EC2 Public IP.

**1. Nginx Application**

```text
http://<EC2-PUBLIC-IP>/nginx
```

Displays the default Nginx welcome page.

**2. My Personal Website**

```text
http://<EC2-PUBLIC-IP>/myapp
```

Displays my personal website built using Python Flask.

**Example:**

```text
http://13.200.xx.xx/nginx
http://13.200.xx.xx/myapp
```

Make sure port 80 is allowed in the EC2 Security Group and the Nginx Ingress Controller is running.

## 🌍 Expected Output

| URL | Application |
|---|---|
| `/nginx` | Nginx Welcome Page |
| `/myapp` | Personal Website |

Access both applications using your EC2 Public IP.

## 📚 What I Learned

- Creating and managing Kubernetes clusters using Kind.
- Deploying applications using Kubernetes Deployments.
- Exposing applications using ClusterIP Services.
- Configuring Nginx Ingress Controller.
- Implementing path-based routing.
- Building and deploying Docker images.
- Automating tool installation using Shell Scripting.
- Deploying applications on AWS EC2.

## 📝 Detailed Documentation

Want to understand the complete implementation?

I've shared the detailed explanation, configurations, and step-by-step practical guide on my Hashnode blog.

👉 **[Read the Complete Project on Hashnode](YOUR_HASHNODE_BLOG_LINK)**

---

⭐ If you find this project useful, feel free to explore and share your feedback!
