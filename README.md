

---

# 🚀 Automated Docker Deployment Script

This repository contains a Bash script that automates the process of building, tagging, pushing Docker images to GitHub Container Registry (GHCR) and optionally Docker Hub, transferring environment files, and deploying the container to a remote server (e.g., Azure VM). It also handles cleanup of older image versions from GHCR.

---

## 📁 File Structure

* `deploy.sh` – Main automation script.
* `.env` – Environment variable file (required).

---

## 🔧 Prerequisites

### ✅ On Your Local Machine:

* Docker & Docker CLI installed.
* `sshpass` installed (`sudo apt install sshpass` on Ubuntu).
* Valid `.env` file in the root directory.

### ✅ On the Remote Server:

* Docker & Docker Compose installed.
* SSH access enabled.
* Environment file destination path should be pre-created or writable.

---

## 📄 .env File Format

Create a `.env` file in the project root with the following variables:

```env
# GitHub & Docker Config
GITHUB_TOKEN=your_github_token
GITHUB_USERNAME=your_github_username
GITHUB_REPO_NAME=your_repo_name
GITHUB_REGISTRY=ghcr.io

DOCKER_IMAGE_NAME=your_image_name
VERSION_NO=your_version_tag

DOCKER_HUB_USERNAME=your_dockerhub_username
DOCKER_HUB_PASSWORD=your_dockerhub_password (optional)
DOCKER_HUB_REPO=your_dockerhub_repo (optional)

# Remote Server Config
REMOTE_USER=azure_user
REMOTE_PASSWORD=azure_password
REMOTE_IP=azure_ip_address
ENV_FILE_PATH=/home/Azureadmin/.env

# Service/Port Mapping
SERVICE_NAME=your_service_name
EXTERNAL_PORT=3000
INTERNAL_PORT=3000
```

---

## 🚀 How to Use

### 1. Make the script executable

```bash
chmod +x deploy.sh
```

### 2. Run the deployment

```bash
./deploy.sh
```

---

## 🛠 What the Script Does

1. **Load & Validate Environment Variables**
   Checks for the existence of `.env` and required keys.

2. **Build and Tag Docker Image**

   * Builds using provided image name and version.
   * Tags the image for both GHCR and Docker Hub.

3. **Push to Registries**

   * Pushes the Docker image to GHCR.
   * Optionally pushes to Docker Hub (if credentials provided).

4. **Transfer Environment File**
   Copies `.env` to the remote server.

5. **Deploy on Remote Server**

   * Pulls image.
   * Stops and removes old container if exists.
   * Creates a `docker-compose.yml`.
   * Starts the container using Docker Compose.

6. **Clean Up Old Versions from GHCR**
   Retains only the 3 latest versions by deleting older ones using GitHub API.

---

## 🔐 Security Note

Avoid committing your `.env` file to version control. Add `.env` to `.gitignore`:

```bash
echo ".env" >> .gitignore
```


## 📝 License

MIT License – feel free to use and adapt.

---


