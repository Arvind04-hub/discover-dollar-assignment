# Discover Dollar Assignment – Full-Stack MEAN Application with CI/CD & Docker

## Overview
This repository contains a full-stack MEAN (MongoDB, Express, Angular, Node.js) CRUD application that manages tutorials.  
The project has been fully containerized using Docker, orchestrated using Docker Compose, and deployed to AWS EC2 with an automated CI/CD pipeline via GitHub Actions.

---

## Tech Stack Used

1. Web Server / Reverse Proxy ---- Nginx 
2. Containerization ---- Docker 
3. Deployment ----- AWS EC2 
4. CI/CD ---- GitHub Actions 

---

## The pipeline file is located at:
.github/workflows/deploy.yml

### Deployment Process

Initial Setup (one-time only)
1. Launched an Ubuntu EC2 instance.
2. Installed Docker & Docker Compose on EC2.

Automated Deployment (CI/CD)
After the initial setup, deployments are handled automatically:

1. Whenever new code is pushed to the `main` branch:
   - GitHub Actions builds backend and frontend Docker images.
   - Pushes images to Docker Hub.
   - Connects to EC2 via SSH using GitHub Secrets.
   - Runs `docker compose pull && docker compose up -d` on EC2 automatically.

No manual deployment is required anymore.
Each commit to `main` updates the live application automatically.


## CI/CD Pipeline — GitHub Actions
On every push to `main` branch:

1. Build backend & frontend Docker images
2. Push images to Docker Hub
3. Connect to EC2 via SSH
4. Then pull the images from Docker Hub and run the new images.

After deployment, the application is live on:  
http://13.127.253.52/

## Security Notes
- GitHub Secrets safely store EC2 credentials
- SSH key & private key are **not exposed**

## Nginx Setup & Reverse Proxy Configuration
Nginx is bundled inside the frontend Docker image.
It serves the Angular application and reverse-proxies API calls to the backend container via Docker internal networking

  ```
server {
    listen 80;
    server_name _;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://backend-app:8080;
    }
}
```
<br><br>

<img width="1227" height="373" alt="image" src="https://github.com/user-attachments/assets/9088919f-80ef-42f5-9a78-171ae75bfd3a" />
This enables frontend and backend to run under the same domain and port (80) while still being separated into different Docker containers.

## Docker Containers
<img width="1854" height="126" alt="image" src="https://github.com/user-attachments/assets/a52ec17b-ec4e-467b-a9ce-d8453c1b78ed" />
The screenshot demonstrates that the Docker containers are actively running on my EC2 instance.

## CI/CD Pipeline Execution — Validation Screenshots
The following screenshots demonstrate:
- GitHub Actions successfully building & pushing Docker images
- Automatic deployment to EC2 
- Application running live after the pipeline execution

<br><br>
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/907cd8f0-13ee-477d-8cb7-336e80c76ed7" />
<br><br>
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/73f9c24b-5706-4f66-ba8f-588e3d208604" />
<br><br>
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/c25a9c88-8921-4779-8ffd-91dc1e33bda2" />
<br><br>
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/51b3651e-cfdc-401f-9f07-48266f603070" />
<br><br>
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/7e5db539-6499-4b96-ac2b-d80f1f3d413a" />
<br><br>
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/3ae4575f-d9a5-43f3-ae32-940d8e3e959b" />

---

## 📌 Final Summary
- Full-stack MEAN application successfully containerized using Docker.
- Nginx reverse proxy routes `/api` calls to backend while serving Angular UI on port 80.
- GitHub Actions CI/CD ensures **zero-downtime deployments** to AWS EC2.
- Every push to `main` automatically updates the live production environment.

🔗 Live Application URL: http://13.127.253.52/
