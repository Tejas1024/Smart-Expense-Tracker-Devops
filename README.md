# Smart Expense Tracker with AI Insights - DevOps Implementation

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)](https://github.com/features/actions)
[![Argo CD](https://img.shields.io/badge/Argo_CD-EF7B4D?style=for-the-badge&logo=argo&logoColor=white)](https://argoproj.github.io/cd/)

## 📋 Project Overview

A production-grade **Smart Expense Tracker** application with AI-powered insights, built with Django and React, deployed using modern DevOps practices. This project demonstrates end-to-end containerization, CI/CD pipeline implementation, Kubernetes orchestration, and GitOps deployment methodology.

**Problem Statement:** Manual expense tracking is time-consuming and lacks intelligent insights. This solution provides automated categorization, AI-powered financial analysis, and seamless cloud deployment.

**Solution:** Full-stack application with AI capabilities, containerized using Docker, orchestrated with Kubernetes, and deployed through automated CI/CD pipelines with GitOps practices.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     User Interface Layer                     │
│           (React Frontend - NGINX Served)                    │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                  Kubernetes Cluster (EKS)                    │
│  ┌────────────────────────────────────────────────────┐     │
│  │  Frontend Pods (5 replicas)  │  Backend Pods (6)  │     │
│  └────────────────────────────────────────────────────┘     │
│  ┌────────────────────────────────────────────────────┐     │
│  │         PostgreSQL (Persistent Storage)            │     │
│  └────────────────────────────────────────────────────┘     │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                    CI/CD Pipeline                            │
│  GitHub → Actions → Docker Hub → ArgoCD → Kubernetes        │
└─────────────────────────────────────────────────────────────┘
```

**Key Components:**
- **Frontend Service**: React SPA served via NGINX (port 80)
- **Backend Service**: Django REST API with Gunicorn (port 8000)
- **Database**: PostgreSQL 15 with persistent volume claims
- **Orchestration**: Kubernetes with Helm package management
- **GitOps**: ArgoCD for declarative continuous deployment
- **Monitoring**: CloudWatch integration for logs and metrics

---

## 🛠️ Technology Stack

### Frontend
- **React 18.2** - Modern UI framework
- **Chart.js** - Data visualization
- **Framer Motion** - Animations
- **Axios** - HTTP client
- **Lucide React** - Icon library

### Backend
- **Django 4.2** - Web framework
- **Django REST Framework 3.14** - API development
- **Gunicorn** - WSGI HTTP Server
- **Tesseract OCR** - Receipt scanning
- **Scikit-learn** - AI categorization

### Database
- **PostgreSQL 15** - Primary database
- **Persistent Volume Claims** - Data persistence in Kubernetes

### DevOps & Cloud
- **Docker** - Containerization with multi-stage builds
- **Kubernetes** - Container orchestration
- **Helm 3** - Package management
- **GitHub Actions** - CI pipeline automation
- **ArgoCD** - GitOps continuous deployment
- **Docker Hub** - Container registry
- **AWS EKS** - Managed Kubernetes service
- **CloudWatch** - Logging and monitoring

---

## 📊 Project Phases

> **📸 Note:** All screenshots are organized in `documentation/phase-X/screenshots/` folders. See each phase folder for detailed visual documentation.

### Phase 1: Environment Setup & Verification

**Objective:** Establish development environment with all required tools.

**What Was Implemented:**
- Verified Git installation and GitHub connectivity
- Confirmed Docker daemon is running and accessible
- Validated Node.js and npm for frontend development
- Set up Python virtual environment for backend
- Established clean foundation for DevOps workflow

**Tools Verified:**
- Git 2.x
- Docker 24.x
- Node.js 18.x
- npm 9.x
- Python 3.11.x

**Key Learnings:**
- Proper environment setup prevents deployment issues
- Version compatibility is critical for reproducible builds
- Tool verification saves debugging time later in the pipeline

**Screenshots:**
- `documentation/phase-1/screenshots/` (Core tools verification screenshots)
- `documentation/phase-1/screenshots/` (Git push success screenshots)

---

### Phase 2: Dockerization

**Objective:** Create optimized, production-ready Docker containers using multi-stage builds.

**What Was Implemented:**

#### Backend Dockerfile
- Multi-stage build using Python 3.11-slim base images
- Separated build dependencies from runtime environment
- Implemented non-root user for security (appuser)
- Configured Gunicorn WSGI server with optimal worker settings
- Added health check endpoint for container orchestration
- Image optimization reduced size by ~40%

#### Frontend Dockerfile
- Two-stage build: Node.js 18 for build, NGINX Alpine for runtime
- Production React bundle optimization
- Custom NGINX configuration for SPA routing
- Health endpoint for liveness/readiness probes
- Compressed static assets for faster delivery

**Tools Used:**
- Docker 24.x
- Docker Hub (image registry)
- Multi-stage Dockerfile patterns

**Key Learnings:**
- Multi-stage builds significantly reduce image size
- Non-root users improve container security
- Health checks are essential for Kubernetes integration
- Proper NGINX configuration ensures SPA routing works correctly

**Screenshots:**
- `documentation/phase-2/screenshots/phase2_backend_build_success.png`
- `documentation/phase-2/screenshots/phase2_frontend_build_success.png`
- `documentation/phase-2/screenshots/phase2_dockerhub_login.png`
- `documentation/phase-2/screenshots/phase2_dockerhub_push_success.png`

---

### Phase 3: GitHub Repository & Version Control

**Objective:** Establish proper version control and prepare for CI/CD integration.

**What Was Implemented:**
- Initialized Git repository with proper `.gitignore` files
- Created structured directory layout for Kubernetes manifests
- Set up Docker ignore files for build optimization
- Pushed initial codebase to GitHub remote repository
- Established branching strategy (main/develop)

**Tools Used:**
- Git 2.x
- GitHub (remote repository)
- VS Code (code editor)

**Key Learnings:**
- Proper `.gitignore` prevents sensitive data leaks
- Structured repository layout improves maintainability
- Version control is the foundation for GitOps practices

---

### Phase 4: CI Pipeline (GitHub Actions)

**Objective:** Implement automated testing, building, and image publishing.

**What Was Implemented:**

#### Backend CI Workflow (`.github/workflows/backend-ci.yaml`)
- Automated Python dependency installation
- Django test suite execution
- Docker image build with BuildKit optimization
- Image tagging with `latest` and commit SHA
- Automated push to Docker Hub
- Build caching for faster subsequent builds

#### Frontend CI Workflow (`.github/workflows/frontend-ci.yaml`)
- Node.js dependency caching
- React production build (with CI=false for warnings)
- Docker image build with multi-stage optimization
- Automated push to Docker Hub with versioned tags
- Build artifact caching

**Configuration:**
- GitHub Secrets: `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN`
- Trigger: Push to main/develop branches
- Path filtering: Only trigger on relevant file changes

**Tools Used:**
- GitHub Actions
- Docker BuildKit
- Docker Hub
- Python 3.11
- Node.js 18

**Key Learnings:**
- Automated CI saves manual deployment effort
- Image tagging with SHA enables easy rollbacks
- Build caching dramatically reduces build times
- Secret management keeps credentials secure
- Path filtering prevents unnecessary builds

**Screenshots:**
- `documentation/phase-5/screenshots/` (All Phase 5 CI/CD screenshots)

---

### Phase 5: AWS ECS Fargate Deployment

**Objective:** Deploy containerized application to AWS cloud infrastructure.

**What Was Implemented:**

#### ECS Cluster Configuration
- Created ECS Fargate cluster for serverless container execution
- Configured task definitions with proper resource allocation
- Set up service definitions with desired task count
- Implemented CloudWatch logging for container outputs

#### Networking Setup
- VPC with public and private subnets
- Internet Gateway for public access
- Security groups for controlled traffic
- Elastic Network Interfaces (ENI) with public IPs

#### Container Configuration
- **Backend Task**: 2 vCPU, 4GB RAM, Gunicorn server
- **Frontend Task**: 1 vCPU, 2GB RAM, NGINX server
- **Database**: Configured via environment variables
- Health checks configured for all services

**Tools Used:**
- AWS ECS (Elastic Container Service)
- AWS Fargate (serverless compute)
- AWS VPC (networking)
- AWS CloudWatch (logging)
- AWS ECR (optional container registry)

**Key Learnings:**
- Fargate eliminates server management overhead
- Proper VPC configuration is critical for security
- CloudWatch provides essential debugging capabilities
- ENI public IPs enable quick testing
- Cost management requires stopping services when not in use

**Screenshots:**
- See `documentation/phase-10/screenshots/` for complete deployment screenshots

---

## 🚀 CI/CD Pipeline Overview

### Complete Pipeline Flow

```
┌─────────────────────────────────────────────────────────────┐
│  1. Developer Workflow                                       │
│     Developer → Code Changes → Git Commit → Git Push        │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  2. CI Pipeline (GitHub Actions)                            │
│     ├─ Checkout code                                         │
│     ├─ Run automated tests                                   │
│     ├─ Build Docker images                                   │
│     ├─ Tag with SHA and latest                               │
│     ├─ Push to Docker Hub                                    │
│     └─ Update Helm values (if configured)                    │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  3. CD Pipeline (ArgoCD)                                     │
│     ├─ Poll Git repository                                   │
│     ├─ Detect Helm chart changes                             │
│     ├─ Sync desired state                                    │
│     ├─ Deploy to Kubernetes                                  │
│     ├─ Perform rolling update                                │
│     └─ Self-heal if manual changes detected                  │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  4. Result: Zero-Touch Deployment                           │
│     Application updated with zero downtime                   │
└─────────────────────────────────────────────────────────────┘
```

**Pipeline Benefits:**
- **Automated Testing**: Catches bugs before deployment
- **Consistent Builds**: Docker ensures environment parity
- **Version Control**: SHA tags enable precise rollbacks
- **Self-Healing**: ArgoCD reverts manual cluster changes
- **Zero Downtime**: Rolling updates maintain availability

---

## 🎯 Application Features (Full-Stack)

### User Authentication
- Secure registration with password validation
- Token-based authentication (Django REST Framework)
- User profile management
- Session handling

### Expense Management
- Create, read, update, delete expense records
- Category-based organization (16 predefined categories)
- Multiple payment methods (Cash, Card, UPI, Bank Transfer)
- Receipt image upload and storage
- Date-based filtering and search

### AI-Powered Features
- **Automatic Categorization**: ML-based expense classification
- **Receipt OCR**: Extract amount and merchant from images using Tesseract
- **Spending Insights**: AI-generated financial recommendations
- **Budget Alerts**: Intelligent overspending notifications
- **Trend Analysis**: Historical spending pattern recognition

### User Interface
- Dark/Light theme toggle
- Responsive design (mobile, tablet, desktop)
- Real-time data visualization with Chart.js
- Interactive dashboard with spending analytics
- Export functionality (CSV, Excel, PDF)
- Multi-currency support

### Backend Logic
- RESTful API design
- Database optimization with query indexing
- Pagination for large datasets
- CORS configuration for frontend integration
- Health check endpoints for monitoring

---

## 💻 How to Run Locally

### Prerequisites
```bash
# Required software
- Git 2.x
- Docker 24.x
- Docker Compose (optional)
- Node.js 18.x
- Python 3.11.x
```

### Clone Repository
```bash
git clone https://github.com/YOUR_USERNAME/smart-expense-tracker.git
cd smart-expense-tracker
```

### Backend Setup
```bash
cd backend

# Create virtual environment
python -m venv venv

# Activate virtual environment (Windows)
venv\Scripts\activate
# Activate virtual environment (Mac/Linux)
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run migrations
python manage.py migrate

# Create categories
python manage.py create_default_categories

# Create superuser (optional)
python manage.py createsuperuser

# Run development server
python manage.py runserver
```

**Backend accessible at:** `http://localhost:8000`

### Frontend Setup
```bash
cd frontend

# Install dependencies
npm install

# Start development server
npm start
```

**Frontend accessible at:** `http://localhost:3000`

---

## 🐳 How to Run Using Docker

### Build Images
```bash
# Build backend
cd backend
docker build -t expense-tracker-backend:local .

# Build frontend
cd ../frontend
docker build -t expense-tracker-frontend:local .
```

### Run Containers
```bash
# Run backend
docker run -d -p 8000:8000 \
  -e DEBUG=True \
  -e DATABASE_URL=sqlite:///db.sqlite3 \
  --name expense-backend \
  expense-tracker-backend:local

# Run frontend
docker run -d -p 3000:80 \
  --name expense-frontend \
  expense-tracker-frontend:local
```

### Access Application
- Frontend: `http://localhost:3000`
- Backend API: `http://localhost:8000/api/`
- Admin Panel: `http://localhost:8000/admin/`

### Stop Containers
```bash
docker stop expense-backend expense-frontend
docker rm expense-backend expense-frontend
```

---

## ☸️ Kubernetes Deployment

### Prerequisites
- Kubernetes cluster (local or cloud)
- kubectl configured
- Helm 3 installed

### Deploy Using Helm
```bash
# Create namespace
kubectl create namespace expense-tracker

# Install Helm chart
helm install expense-tracker ./helm/expense-tracker \
  --namespace expense-tracker \
  --create-namespace

# Check deployment
kubectl get pods -n expense-tracker
kubectl get services -n expense-tracker
```

### Access Application
```bash
# Get service details
kubectl get svc frontend-service -n expense-tracker

# Port forward (for local testing)
kubectl port-forward svc/frontend-service 3000:80 -n expense-tracker
```

### Upgrade Deployment
```bash
helm upgrade expense-tracker ./helm/expense-tracker \
  --namespace expense-tracker
```

### Uninstall
```bash
helm uninstall expense-tracker --namespace expense-tracker
kubectl delete namespace expense-tracker
```

---

## 🔄 Deployment Lifecycle

### Start ECS Service
1. Navigate to AWS ECS Console
2. Select `expense-tracker-cluster`
3. Start the `expense-tracker-service`
4. Wait for tasks to reach RUNNING state (2-3 minutes)
5. Note the public IP from task details

### Verify Logs
```bash
# View CloudWatch logs
aws logs tail /ecs/expense-tracker --follow
```

### Access Application
- Frontend: `http://<TASK_PUBLIC_IP>:3000`
- Backend: `http://<TASK_PUBLIC_IP>:8000`
- Health Check: `http://<TASK_PUBLIC_IP>:8000/health/`

### Stop Service (Cost Savings)
1. Navigate to ECS service in AWS Console
2. Update service → Set desired tasks to 0
3. Confirm and wait for tasks to stop
4. Verify in CloudWatch that billing has stopped

**Important:** Always stop ECS services when not actively using them to avoid unnecessary AWS charges.

---

## 💰 Cost & Resource Management

### AWS Cost Optimization
- **Fargate Pricing**: ~$0.04/hour per vCPU, ~$0.004/hour per GB
- **Strategy**: Stop services when not in use
- **Monitoring**: Set up CloudWatch billing alerts
- **Alternative**: Use spot instances for development

### Resource Allocation
| Component | vCPU | Memory | Storage |
|-----------|------|--------|---------|
| Backend   | 2    | 4 GB   | -       |
| Frontend  | 1    | 2 GB   | -       |
| Database  | -    | -      | 5 GB    |

### Cost Management Commands
```bash
# Stop all ECS services
aws ecs update-service \
  --cluster expense-tracker-cluster \
  --service expense-tracker-service \
  --desired-count 0

# Verify stopped
aws ecs describe-services \
  --cluster expense-tracker-cluster \
  --services expense-tracker-service
```

---

## 🎓 What This Project Demonstrates

### Full-Stack Development Skills
- ✅ Modern frontend development (React, REST APIs)
- ✅ Backend API design (Django REST Framework)
- ✅ Database design and optimization (PostgreSQL)
- ✅ Authentication and authorization implementation
- ✅ File handling and image processing
- ✅ AI/ML integration (OCR, categorization)

### DevOps Engineering Skills
- ✅ Docker containerization with multi-stage builds
- ✅ Container registry management (Docker Hub)
- ✅ Kubernetes orchestration and deployment
- ✅ Helm package management and templating
- ✅ CI pipeline implementation (GitHub Actions)
- ✅ GitOps methodology with ArgoCD
- ✅ Infrastructure as Code principles
- ✅ Cloud deployment (AWS ECS, Fargate)

### Cloud & Infrastructure Skills
- ✅ AWS ECS service configuration
- ✅ VPC and networking setup
- ✅ Security group configuration
- ✅ CloudWatch monitoring and logging
- ✅ Cost optimization strategies
- ✅ High availability architecture

### Software Engineering Best Practices
- ✅ Version control with Git
- ✅ Automated testing integration
- ✅ Code quality and linting
- ✅ Documentation and README creation
- ✅ Security best practices (secrets management)
- ✅ Monitoring and observability

---

## 📸 Screenshots & Documentation

All project phases are fully documented with screenshots organized in the following structure:

```
documentation/
├── phase-1/          # Environment Setup & Verification
│   └── screenshots/
├── phase-2/          # Docker Multi-Stage Builds
│   └── screenshots/
│       ├── phase2_backend_build_success.png
│       ├── phase2_frontend_build_success.png
│       ├── phase2_dockerhub_login.png
│       └── phase2_dockerhub_push_success.png
├── phase-4/          # Helm Chart Creation
│   └── screenshots/
├── phase-5/          # GitHub Actions CI Pipeline
│   └── screenshots/
├── phase-6/          # Argo CD GitOps Setup
│   └── screenshots/
└── phase-10/         # Complete CI/CD Testing & Production
    ├── CI-CD-FLOW.md
    └── screenshots/  (33+ comprehensive milestone screenshots)
```

### Key Screenshot Categories:

**Phase 1 - Environment Setup:**
- Core development tools verification
- Git configuration and connectivity
- Docker daemon status
- Python and Node.js validation

**Phase 2 - Dockerization:**
- Backend multi-stage build process
- Frontend NGINX build success
- Docker Hub authentication
- Image push confirmations

**Phase 4 - Helm Charts:**
- Helm chart structure
- Template validation
- Values configuration
- Chart deployment

**Phase 5 - CI Pipeline:**
- GitHub Actions workflow setup
- Automated build triggers
- Docker image tagging with SHA
- DockerHub registry updates

**Phase 6 - GitOps:**
- Argo CD installation
- Application sync status
- Self-healing demonstrations
- GitOps workflow validation

**Phase 10 - Production Deployment:**
- Complete CI/CD flow testing
- Scaling operations (4→6 backend, 4→5 frontend replicas)
- Self-healing pod recovery
- Rollback testing with failed deployments
- Application accessibility verification
- Resource status monitoring
- Deployment history tracking

### Viewing Screenshots:

Navigate to the respective phase folder to view detailed visual documentation:
```bash
# Example: View Phase 2 screenshots
cd documentation/phase-2/screenshots/
ls -la

# Example: View Phase 10 comprehensive documentation
cd documentation/phase-10/
cat CI-CD-FLOW.md
```

---

## 🚧 Future Enhancements

### Infrastructure Improvements
- [ ] **Load Balancer Integration**: Add ALB for production traffic management
- [ ] **HTTPS/SSL**: Implement TLS certificates for secure communication
- [ ] **Custom Domain**: Configure Route 53 with custom domain name
- [ ] **Auto-scaling**: Implement HPA (Horizontal Pod Autoscaler)
- [ ] **Multi-region Deployment**: Add disaster recovery capabilities
- [ ] **Service Mesh**: Integrate Istio for advanced traffic management

### Security Enhancements
- [ ] **Secrets Management**: Integrate AWS Secrets Manager or HashiCorp Vault
- [ ] **Network Policies**: Implement fine-grained pod-to-pod communication rules
- [ ] **RBAC**: Configure granular role-based access control
- [ ] **Image Scanning**: Add vulnerability scanning to CI pipeline
- [ ] **WAF Integration**: Web Application Firewall for threat protection

### Monitoring & Observability
- [ ] **Prometheus + Grafana**: Advanced metrics and dashboards
- [ ] **Distributed Tracing**: Implement Jaeger or OpenTelemetry
- [ ] **Log Aggregation**: ELK stack for centralized logging
- [ ] **Alerting**: PagerDuty integration for incident management
- [ ] **APM**: Application Performance Monitoring (Datadog/New Relic)

### Application Features
- [ ] **Real-time Notifications**: WebSocket integration
- [ ] **Social Features**: Expense sharing and group budgets
- [ ] **Mobile App**: React Native mobile client
- [ ] **Advanced AI**: Spending predictions and recommendations
- [ ] **Bank Integration**: Automatic transaction import

---

## 👤 Author

**Tejas**  
*DevOps Engineer | Full-Stack Developer*

- 🔗 GitHub: [YOUR_GITHUB_USERNAME]
- 📧 Email: tejaspavithra2002@gmail.com
- 💼 LinkedIn: [YOUR_LINKEDIN_PROFILE]

**Professional Summary:**  
Experienced in building and deploying cloud-native applications with expertise in Docker, Kubernetes, CI/CD pipelines, and AWS services. Passionate about infrastructure automation, GitOps practices, and creating scalable, production-ready systems.

**Technical Expertise:**
- **Languages**: Python, JavaScript, YAML
- **Frameworks**: Django, React, REST APIs
- **DevOps**: Docker, Kubernetes, Helm, ArgoCD, GitHub Actions
- **Cloud**: AWS (ECS, Fargate, VPC, CloudWatch, ECR)
- **Databases**: PostgreSQL, SQLite
- **Tools**: Git, NGINX, Gunicorn, Tesseract OCR

---

## 📜 License

This project is for educational and portfolio purposes. Feel free to use it as a reference for your own DevOps learning journey.

---

## 🙏 Acknowledgments

- **Anthropic Claude** - AI assistance for documentation
- **Kubernetes Community** - Excellent documentation and tools
- **ArgoCD Project** - GitOps made easy
- **Docker** - Containerization platform
- **AWS** - Cloud infrastructure

---

## 📞 Support

For questions or issues:
1. Check the documentation in the `documentation/` folder
2. Review troubleshooting guides in each phase README
3. Open an issue on GitHub
4. Contact via email: tejaspavithra2002@gmail.com

---

**⭐ If you found this project helpful, please consider giving it a star on GitHub!**

---

*Last Updated: January 2026*  
*Project Status: ✅ Production Ready*
