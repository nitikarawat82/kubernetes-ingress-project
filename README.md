# Kubernetes Ingress Practical

## Project Overview
This project demonstrates how to deploy two applications on a Kubernetes cluster using Nginx Ingress Controller.

- `/nginx` – Opens the default Nginx welcome page.
- `/myapp` – Opens my personal website built using Python Flask.

## Technologies Used
- AWS EC2
- Docker
- Kubernetes (Kind)
- Nginx Ingress Controller
- Python Flask
- Git & GitHub

## How to Run

1. Clone the repository:
   ```bash
   git clone YOUR_GITHUB_REPOSITORY_URL
   cd kubernetes-ingress-practical
   ```

2. Install Docker, kubectl, and Kind.

3. Create the Kubernetes cluster:
   ```bash
   kind create cluster --config kubernetes/kind-config.yaml
   ```

4. Build the Flask Docker image and load it into Kind:
   ```bash
   docker build -t my-personal-website:v1 ./app
   kind load docker-image my-personal-website:v1
   ```

5. Deploy the applications and services:
   ```bash
   kubectl apply -f kubernetes/
   ```

6. Install Nginx Ingress Controller and apply the Ingress configuration.

## Expected Result
- `http://EC2-PUBLIC-IP/nginx` – Nginx Welcome Page
- `http://EC2-PUBLIC-IP/myapp` – Personal Website

**Purpose:** Understanding Kubernetes Deployments, Services, Ingress Controller, and path-based routing.

## More Details

For detailed project explanation, implementation steps, and configurations, visit my Hashnode blog:

[Read the complete project on Hashnode](YOUR_HASHNODE_BLOG_LINK)
