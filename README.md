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
└── README.md
```

## ⚙️ How to Run

**1. Clone the repository**

```bash
git clone YOUR_GITHUB_REPOSITORY_URL
cd kubernetes-ingress-practical
```

**2. Create the Kind cluster**

```bash
kind create cluster --config kubernetes/kind-config.yaml
```

**3. Build the Flask Docker image**

```bash
docker build -t my-personal-website:v1 ./app
```

**4. Load the image into Kind**

```bash
kind load docker-image my-personal-website:v1
```

**5. Install Nginx Ingress Controller**

Install the Nginx Ingress Controller using the official Kind installation instructions.

**6. Deploy applications and services**

```bash
kubectl apply -f kubernetes/
```

## 🌍 Expected Output

| URL | Application |
|---|---|
| `/nginx` | Nginx Welcome Page |
| `/myapp` | Personal Website |

## 📚 What I Learned

- Creating and managing Kubernetes clusters using Kind.
- Deploying applications using Kubernetes Deployments.
- Exposing applications using ClusterIP Services.
- Configuring Nginx Ingress Controller.
- Implementing path-based routing.
- Building and deploying Docker images.

## 📝 Detailed Documentation

Want to understand the complete implementation?

I've shared the detailed explanation, configurations, and step-by-step practical guide on my Hashnode blog.

👉 **[Read the Complete Project on Hashnode](YOUR_HASHNODE_BLOG_LINK)**

---

⭐ If you find this project useful, feel free to explore and share your feedback!
