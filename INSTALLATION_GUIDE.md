# 📘 DevOps Lifecycle — Step-by-Step Installation Guide (Windows)

> **Assignment:** ISE-2 — Implement the DevOps lifecycle on a Windows PC  
> **Stack:** Python + Flask · Git · GitHub · Docker · Jenkins

---

## 📁 Project Structure

```
ISE-2/
├── app.py                 # Flask web application
├── test_app.py            # Unit tests (pytest)
├── requirements.txt       # Python dependencies
├── Dockerfile             # Docker containerization
├── Jenkinsfile            # CI/CD pipeline
├── .gitignore             # Git ignore rules
├── .dockerignore          # Docker ignore rules
└── templates/
    └── index.html         # Home page template
```

---

## 🔧 Tool 1 — Python & Flask (Web Application)

### 1.1 Install Python

1. Download Python from [python.org/downloads](https://www.python.org/downloads/)
2. **✅ Check "Add Python to PATH"** during installation
3. Click **Install Now**

### 1.2 Verify Installation

Open **Command Prompt** or **PowerShell** and run:

```powershell
python --version
# Expected: Python 3.11.x or newer

pip --version
# Expected: pip 24.x from ...
```

### 1.3 Install Flask & Run the App

```powershell
# Navigate to project folder
cd C:\ISE-2

# Install dependencies
pip install -r requirements.txt

# Verify Flask installed
pip show flask

# Run the application
python app.py
```

Open your browser → visit **http://localhost:5000** → you should see the Student App page.

### 1.4 Run Tests

```powershell
python -m pytest test_app.py -v
```

> **Expected output:** 2 passed ✅

---

## 🔧 Tool 2 — Git (Version Control)

### 2.1 Install Git

1. Download Git from [git-scm.com/download/win](https://git-scm.com/download/win)
2. Run the installer → keep all **default options**
3. Click **Install**

### 2.2 Verify Installation

```powershell
git --version
# Expected: git version 2.x.x
```

### 2.3 Configure Git

```powershell
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Verify config
git config --list
```

### 2.4 Initialize Local Repository

```powershell
cd C:\ISE-2

# Initialize a new Git repository
git init

# Check the status (all files should be untracked)
git status

# Stage all files
git add .

# Verify staged files
git status

# Commit
git commit -m "Initial commit: Flask app with Docker and Jenkins"
```

---

## 🔧 Tool 3 — GitHub (Source-Code Hosting)

### 3.1 Create GitHub Repository

1. Go to [github.com](https://github.com) → Sign up / Log in
2. Click the **"+"** button → **New repository**
3. Repository name: `ISE-2` (or any name you prefer)
4. Keep it **Public** or **Private**
5. **Do NOT** check "Add a README" (we already have files)
6. Click **Create repository**

### 3.2 Push Local Repo to GitHub

Replace `YOUR_USERNAME` with your GitHub username:

```powershell
# Add GitHub as remote origin
git remote add origin https://github.com/YOUR_USERNAME/ISE-2.git

# Rename branch to main
git branch -M main

# Push code to GitHub
git push -u origin main
```

> If prompted for credentials, enter your GitHub username and a **Personal Access Token** (not your password).  
> Create a PAT at: **GitHub → Settings → Developer Settings → Personal Access Tokens → Generate New Token**

### 3.3 Verify on GitHub

1. Go to your repository URL: `https://github.com/YOUR_USERNAME/ISE-2`
2. You should see all your files listed

### 3.4 Pull Changes (for future use)

```powershell
git pull origin main
```

---

## 🔧 Tool 4 — Docker (Containerization)

### 4.1 Install Docker Desktop

1. Download Docker Desktop from [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
2. Run the installer
3. If prompted, **enable WSL 2** (follow the instructions to install WSL 2 if not already installed):
   ```powershell
   wsl --install
   ```
4. Restart your computer after installation
5. Open **Docker Desktop** and wait for the engine to start (green icon in system tray)

### 4.2 Verify Installation

```powershell
docker --version
# Expected: Docker version 27.x.x

docker compose version
# Expected: Docker Compose version v2.x.x

docker info
# Should show Server information (confirms engine is running)
```

### 4.3 Build the Docker Image

```powershell
cd C:\ISE-2

# Build the image (the dot at the end means "use current directory")
docker build -t student-app .
```

> This will take a minute the first time (downloading Python base image).

### 4.4 Run the Container

```powershell
# Run in detached mode with port mapping
docker run -d --name student-app -p 5000:5000 student-app
```

Open browser → **http://localhost:5000** → App should be running from the container! 🎉

### 4.5 Useful Docker Commands

```powershell
# List running containers
docker ps

# View container logs
docker logs student-app

# Stop the container
docker stop student-app

# Remove the container
docker rm student-app

# List all images
docker images

# Remove the image
docker rmi student-app
```

---

## 🔧 Tool 5 — Jenkins (CI/CD Pipeline)

### 5.1 Install Java JDK

Jenkins requires Java. Install JDK 17:

1. Download from [adoptium.net](https://adoptium.net/) (Eclipse Temurin JDK 17)
2. Run installer → **✅ Check "Set JAVA_HOME"** during installation

```powershell
java -version
# Expected: openjdk version "17.x.x"
```

### 5.2 Install Jenkins

**Option A — Direct Windows Install (Recommended for beginners):**

1. Download Jenkins LTS from [jenkins.io/download](https://www.jenkins.io/download/)
2. Choose **Windows** installer (.msi)
3. Run installer → follow prompts
4. Jenkins will start as a Windows service on **port 8080**

**Option B — Run Jenkins in Docker:**

```powershell
docker run -d --name jenkins ^
  -p 8080:8080 -p 50000:50000 ^
  -v jenkins_home:/var/jenkins_home ^
  jenkins/jenkins:lts
```

### 5.3 Access Jenkins

1. Open browser → **http://localhost:8080**
2. Get the initial admin password:

   **If installed directly:**
   ```powershell
   type "C:\ProgramData\Jenkins\.jenkins\secrets\initialAdminPassword"
   ```

   **If running in Docker:**
   ```powershell
   docker logs jenkins
   # Look for the password in the output
   ```

3. Paste the password → Click **Continue**
4. Choose **Install Suggested Plugins** → wait for installation
5. Create your admin user → Save and finish

### 5.4 Install Required Jenkins Plugins

Go to **Manage Jenkins → Plugins → Available Plugins** and install:

| Plugin | Purpose |
|--------|---------|
| Git | Pull code from GitHub |
| Docker Pipeline | Build Docker images in pipeline |
| Pipeline | Declarative pipeline support |

Click **Install** → Restart Jenkins when prompted.

### 5.5 Configure Git & Docker in Jenkins

Go to **Manage Jenkins → Tools**:

1. **Git** → Add Git → Name: `Default` → Path: `git` (auto-detected on Windows)
2. **Docker** → Add Docker → Name: `Default` → Installation root: (leave empty or path to Docker)

### 5.6 Create a Jenkins Pipeline Job

1. Click **New Item** on the Jenkins dashboard
2. Name: `student-app-pipeline`
3. Select **Pipeline** → Click **OK**
4. Scroll to **Pipeline** section:
   - Definition: **Pipeline script from SCM**
   - SCM: **Git**
   - Repository URL: `https://github.com/YOUR_USERNAME/ISE-2.git`
   - Branch: `*/main`
   - Script Path: `Jenkinsfile`
5. Click **Save**

### 5.7 Run the Pipeline

1. Click **Build Now** on the pipeline page
2. Watch the pipeline stages execute:
   - ✅ Checkout
   - ✅ Install Dependencies
   - ✅ Run Tests
   - ✅ Docker Build
   - ✅ Docker Deploy
   - ✅ Verify Deployment

### 5.8 Verify Jenkins is Running

```powershell
# If installed directly
java -version
git --version
docker --version

# If running as Docker container
docker ps                           # Confirm jenkins container is running
docker logs jenkins                 # View Jenkins logs
docker start jenkins                # Start if stopped
docker exec jenkins java -version   # Check Java inside container
```

---

## ✅ Summary — Commands Cheat Sheet

| Tool | Key Commands |
|------|-------------|
| **Python** | `python --version`, `pip install flask`, `python app.py` |
| **Git** | `git init`, `git add .`, `git commit -m "msg"`, `git status` |
| **GitHub** | `git remote add origin URL`, `git push -u origin main` |
| **Docker** | `docker build -t student-app .`, `docker run -p 5000:5000 student-app`, `docker ps` |
| **Jenkins** | `java -version`, access via `http://localhost:8080`, Build Now |

---

## 🎯 DevOps Lifecycle Flow

```
   Developer writes code (Flask + Python)
           │
           ▼
   Git tracks changes locally (git add, commit)
           │
           ▼
   GitHub hosts the repository remotely (git push)
           │
           ▼
   Jenkins detects changes & triggers pipeline
           │
           ├── Install dependencies
           ├── Run tests (pytest)
           ├── Build Docker image
           └── Deploy container
           │
           ▼
   Application running in Docker container
   → http://localhost:5000
```

---

> **🎓 Tip:** Take screenshots at each step (version outputs, app running, Docker ps, Jenkins pipeline) for your assignment submission!
