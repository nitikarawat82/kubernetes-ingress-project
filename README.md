# 🚀 Kubernetes Ingress Practical – Path-Based Routing on AWS EC2

A hands-on Kubernetes project demonstrating **path-based routing using the NGINX Ingress Controller**, Kind, Docker, and AWS EC2.

## 📌 About the Project

In this project, we deploy two applications on a Kubernetes cluster and access both applications using a single EC2 Public IP address.

- 🌐 `/nginx` – Displays the default NGINX welcome page.
- 💻 `/myapp` – Displays my personal website built using Python Flask.

The main goal is to understand Kubernetes Deployments, Services, Ingress, Kind networking, Docker image management, and how traffic is routed to different applications.

## 🛠️ Technologies Used

- AWS EC2 (Ubuntu)
- Docker
- Docker Hub
- Kubernetes
- Kind (Kubernetes in Docker)
- NGINX Ingress Controller (community ingress-nginx)
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

# ⚙️ Complete Implementation Steps

## Step 1: Create an AWS EC2 Instance

First, launch an Ubuntu EC2 instance.

Configure the EC2 Security Group with these inbound rules:

| Type | Port | Source |
|---|---:|---|
| SSH | 22 | My IP |
| HTTP | 80 | 0.0.0.0/0 |

Connect to the EC2 instance using SSH.

## Step 2: Clone the GitHub Repository

Clone the project repository:

```bash
git clone https://github.com/nitikarawat82/kubernetes-ingress-project.git

cd kubernetes-ingress-project
```

## Step 3: Install Required Tools

We created an `install.sh` script to automate the installation of the required tools.

Run the following commands:

```bash
chmod +x install.sh

./install.sh
```

The script installs:

- Docker
- kubectl
- Kind

Add the Ubuntu user to the Docker group:

```bash
sudo usermod -aG docker ubuntu
```

Apply the group changes:

```bash
newgrp docker
```

Verify Docker:

```bash
docker ps
```

Verify kubectl and Kind:

```bash
kubectl version --client

kind version
```

## Step 4: Create the Kubernetes Cluster

We created a Kind cluster with:

- 1 Control Plane Node
- 2 Worker Nodes

Create the cluster:

```bash
kind create cluster --name mycluster --config kubernetes/kind-config.yaml
```

Verify the nodes:

```bash
kubectl get nodes
```

All three nodes should show `Ready`.

### Understanding Kind Port Mapping

In our `kind-config.yaml`, we configured port mapping on the first worker node:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
  - role: control-plane

  - role: worker
    extraPortMappings:
      - containerPort: 80
        hostPort: 80
        protocol: TCP

  - role: worker
```

Here:

- `hostPort: 80` – Port 80 on the EC2 host.
- `containerPort: 80` – Port 80 on the Kind worker node, which runs as a Docker container.

Verify the port mapping:

```bash
docker port mycluster-worker
```

Expected output:

```text
80/tcp -> 0.0.0.0:80
```

This allows incoming traffic on EC2 port 80 to reach port 80 on our Kind worker node.

## Step 5: Build the Flask Docker Image

Now navigate to the application directory:

```bash
cd app
```

Build the Docker image:

```bash
docker build -t nitikadevop/python-sample-app:v1 .
```

## Step 6: Push the Docker Image to Docker Hub

Log in to Docker Hub:

```bash
docker login
```

Push the application image:

```bash
docker push nitikadevop/python-sample-app:v1
```

After pushing the image, return to the project root:

```bash
cd ..
```

Our `myapp-deployment.yaml` uses this image:

```yaml
image: nitikadevop/python-sample-app:v1
imagePullPolicy: IfNotPresent
```

Kind downloads the image from Docker Hub if it is not already available on the node.

## Step 7: Install the NGINX Ingress Controller

Now install the community NGINX Ingress Controller using its Kind-specific deployment manifest:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.15.1/deploy/static/provider/kind/deploy.yaml
```

Verify the Ingress Controller:

```bash
kubectl get pods -n ingress-nginx -o wide
```

In our setup, the controller runs on:

```text
mycluster-worker
```

The Pod should show:

- READY: 1/1
- STATUS: Running
- NODE: mycluster-worker

## Step 8: Deploy the Applications and Services

Now apply the NGINX Deployment and Service:

```bash
kubectl apply -f kubernetes/nginx-deployment.yaml

kubectl apply -f kubernetes/nginx-service.yaml
```

Apply the Flask Deployment and Service:

```bash
kubectl apply -f kubernetes/myapp-deployment.yaml

kubectl apply -f kubernetes/myapp-service.yaml
```

## Step 9: Verify Kubernetes Resources

Check the nodes:

```bash
kubectl get nodes
```

Check Deployments:

```bash
kubectl get deployments
```

Check Pods:

```bash
kubectl get pods
```

Check Services:

```bash
kubectl get services
```

All application Pods should be in the `Running` state.

## Step 10: Apply the Ingress Configuration

Now apply the Ingress manifest:

```bash
kubectl apply -f kubernetes/ingress.yaml
```

Verify the Ingress resource:

```bash
kubectl get ingress
```

Our Ingress configuration provides two routing rules:

| Path | Backend Service | Destination |
|---|---|---|
| `/nginx` | nginx-service:80 | NGINX application |
| `/myapp` | myapp-service:80 | Flask application |

The Ingress rewrite annotation forwards both paths to `/` inside their respective applications.

## Step 11: Access Applications Using EC2 Public IP

Once everything is deployed successfully, open your EC2 Public IP in a browser.

**1. NGINX Welcome Page**

```text
http://EC2-PUBLIC-IP/nginx
```

Displays the default NGINX welcome page.

**2. My Personal Website**

```text
http://EC2-PUBLIC-IP/myapp
```

Displays my personal website built using Python Flask.

Replace `EC2-PUBLIC-IP` with your current EC2 Public IPv4 address.

## 🌍 Expected Output

| URL | Expected Result |
|---|---|
| `http://EC2-PUBLIC-IP/nginx` | NGINX Welcome Page |
| `http://EC2-PUBLIC-IP/myapp` | Personal Website |

Both applications are accessible through a **single EC2 Public IP address using path-based routing**.

<img width="1446" height="872" alt="image" src="https://github.com/user-attachments/assets/13fb6c15-1bb1-475d-a0d7-8e18e6621c33" />

<img width="1653" height="573" alt="image" src="https://github.com/user-attachments/assets/fe91a0b1-4d58-41b2-a5a5-998e578b5ebe" />



## 🔄 Project Architecture

```text
               Internet / Browser
                       |
                       v
                AWS EC2 Public IP
                    Port 80
                       |
                       v
                EC2 Port Mapping
                       |
                       v
                Kind Worker Node
                    Port 80
                       |
                       v
             NGINX Ingress Controller
                       |
                Path-Based Routing
                   /       \
                  /         \
             /nginx        /myapp
                |             |
                v             v
          nginx-service   myapp-service
             Port 80        Port 80
                |             |
                v             v
          NGINX Pods      Flask Pods
            Port 80        Port 5000
```


## 📝 Detailed Documentation

Want to understand the complete implementation?

I've also shared a detailed explanation and practical guide on my Hashnode blog.

👉 **[Read the Complete Project on Hashnode](YOUR_HASHNODE_BLOG_LINK)**

---

⭐ If you find this project useful, feel free to explore the repository and share your feedback!
