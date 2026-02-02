# Python Projects with GitHub Actions
## Learning Path: From Testing to Kubernetes

---

## Overview

This document tracks our Python learning projects, building from basic CI/CD to advanced container orchestration.

**Learning Path Reference**: See `LEARNING-PATH.md`
- Project 1 aligns with: Phase 3, Day 8-9 (Automated Testing)
- Project 2 aligns with: Phase 3, Day 14 (Building & Releasing Artifacts) + Advanced Topics

---

## Project 1: Python Library with GitHub Actions ✨ CURRENT

### Goal
Build a simple Python library with comprehensive CI/CD including testing, coverage, and linting.

### Learning Objectives
- ✅ Set up Python project structure
- ✅ Write tests with pytest
- ✅ Configure code coverage
- ✅ Add linting (pylint/black)
- ✅ Create GitHub Actions workflow
- ✅ Matrix builds (multiple Python versions)
- ✅ Generate and publish coverage reports
- ✅ Add badges to README

### Project Details
- **Name**: python-calculator-lib
- **Type**: Simple utility library (calculator/math operations)
- **Testing**: pytest
- **Coverage**: pytest-cov
- **Linting**: pylint, black
- **CI**: GitHub Actions

### Setup Steps

#### Step 1: Create New Repository
```bash
cd ~/projects
mkdir python-calculator-lib
cd python-calculator-lib
git init
```

#### Step 2: Project Structure
```
python-calculator-lib/
├── .github/
│   └── workflows/
│       └── ci.yml
├── src/
│   └── calculator/
│       ├── __init__.py
│       └── operations.py
├── tests/
│   ├── __init__.py
│   └── test_operations.py
├── .gitignore
├── README.md
├── requirements.txt
├── requirements-dev.txt
└── pyproject.toml
```

#### Step 3: Core Files

**src/calculator/operations.py** - Simple calculator functions
**tests/test_operations.py** - pytest tests
**requirements.txt** - Production dependencies
**requirements-dev.txt** - Development dependencies (pytest, coverage, linting)

#### Step 4: GitHub Actions Workflow

**.github/workflows/ci.yml** - CI pipeline with:
- Matrix strategy: Python 3.9, 3.10, 3.11, 3.12
- OS matrix: ubuntu-latest, windows-latest, macos-latest (optional)
- Steps:
  1. Checkout code
  2. Set up Python
  3. Install dependencies
  4. Run linting (black, pylint)
  5. Run tests with pytest
  6. Generate coverage report
  7. Upload coverage to Codecov (optional)
  8. Upload test results as artifacts

#### Step 5: Testing & Iteration
- Push code
- Watch GitHub Actions run
- Fix any issues
- Add more tests
- Document learnings

### Key Concepts Learned
- [ ] GitHub Actions for Python projects
- [ ] Matrix builds for multi-version testing
- [ ] Code coverage reporting
- [ ] Artifact management
- [ ] PR status checks
- [ ] Badge integration

### Resources
- pytest docs: https://docs.pytest.org/
- pytest-cov: https://pytest-cov.readthedocs.io/
- black: https://black.readthedocs.io/
- pylint: https://pylint.readthedocs.io/

---

## Project 1.5: Python Web Application 🌐 INTERMEDIATE

### Goal
Build a web-based Python application (Flask or FastAPI) with full CI/CD, bridging the gap between simple library testing and container orchestration.

### Learning Objectives
- ✅ Build RESTful API or web app
- ✅ Test web endpoints
- ✅ Integration testing
- ✅ API documentation (FastAPI auto-docs or Swagger)
- ✅ Environment variables and configuration
- ✅ Database integration (SQLite/PostgreSQL)
- ✅ GitHub Actions for web apps
- ✅ Deploy to cloud platform (Heroku, Railway, or Render)

### Project Ideas (Choose One)
1. **Calculator Web API** - RESTful API for calculator operations
2. **Task Manager API** - Simple TODO app with CRUD operations
3. **URL Shortener** - Generate and track short URLs
4. **Weather Dashboard** - Fetch and display weather data

### Tech Stack Options
- **Option A - Flask**: Traditional, simpler, great for learning
- **Option B - FastAPI**: Modern, fast, auto-documentation, async support (Recommended)

### Key Concepts to Learn
- [ ] Web framework basics (routes, request/response)
- [ ] API testing (pytest + requests)
- [ ] Environment configuration
- [ ] Database migrations
- [ ] Deployment to cloud platforms
- [ ] Health check endpoints
- [ ] API documentation

### Resources
- Flask: https://flask.palletsprojects.com/
- FastAPI: https://fastapi.tiangolo.com/
- pytest with FastAPI: https://fastapi.tiangolo.com/tutorial/testing/

---

## Project 2: Python + Docker + Kubernetes 🚀 ADVANCED

### Goal
Build a Python web application with Docker containerization and Kubernetes deployment, fully automated with GitHub Actions.

### Learning Objectives
- ✅ Containerize Python application
- ✅ Multi-stage Docker builds
- ✅ Build and push to GitHub Container Registry
- ✅ Write Kubernetes manifests
- ✅ Deploy to K8s cluster
- ✅ Implement CI/CD for containers
- ✅ Security scanning for containers
- ✅ Rolling updates and rollbacks

### Project Details
- **Name**: python-k8s-demo
- **Type**: Web API (Flask or FastAPI)
- **Containerization**: Docker
- **Orchestration**: Kubernetes
- **Registry**: GitHub Container Registry (ghcr.io)
- **CI/CD**: GitHub Actions

### Architecture
```
┌─────────────────┐
│  GitHub Actions │
└────────┬────────┘
         │
         ├─► Build & Test Python App
         ├─► Build Docker Image
         ├─► Security Scan (Trivy)
         ├─► Push to ghcr.io
         └─► Deploy to K8s
              │
              ▼
         ┌──────────┐
         │  K8s     │
         │  Cluster │
         └──────────┘
```

### Setup Steps (High-Level)

#### Phase 1: Python Web App
1. Create FastAPI/Flask app
2. Add health check endpoints
3. Add tests
4. Create Dockerfile
5. Test locally with docker-compose

#### Phase 2: Docker CI/CD
1. GitHub Actions workflow for Docker
2. Multi-stage build optimization
3. Build and push to ghcr.io
4. Tag management (latest, semver)
5. Security scanning with Trivy

#### Phase 3: Kubernetes
1. Create K8s manifests (Deployment, Service, Ingress)
2. Configure kubectl access
3. Deploy to local K8s (minikube/kind) or cloud
4. Add GitHub Actions deployment step
5. Implement rolling updates

#### Phase 4: Advanced
1. Helm charts (optional)
2. Multiple environments (dev, staging, prod)
3. Secrets management
4. Monitoring and logging
5. Auto-scaling

### Key Technologies
- **Python**: FastAPI or Flask
- **Container**: Docker, multi-stage builds
- **Registry**: GitHub Container Registry (ghcr.io)
- **Orchestration**: Kubernetes
- **CI/CD**: GitHub Actions
- **Security**: Trivy, Snyk
- **Optional**: Helm, ArgoCD

### Key Concepts to Learn
- [ ] Dockerfile best practices
- [ ] Multi-stage builds
- [ ] Container image optimization
- [ ] Container registry authentication
- [ ] Kubernetes Deployments, Services, ConfigMaps, Secrets
- [ ] kubectl basics
- [ ] Rolling updates and rollbacks
- [ ] GitHub Actions + Docker
- [ ] GitHub Actions + Kubernetes
- [ ] GHCR authentication

### Resources
- Docker docs: https://docs.docker.com/
- Kubernetes docs: https://kubernetes.io/docs/
- FastAPI: https://fastapi.tiangolo.com/
- Flask: https://flask.palletsprojects.com/
- GitHub Container Registry: https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry
- Trivy: https://aquasecurity.github.io/trivy/

---

## Progress Tracking

### Current Status
- [x] Document project plan
- [ ] Start Project 1: Python Calculator Library
- [ ] Complete Project 1
- [ ] Start Project 1.5: Python Web Application
- [ ] Complete Project 1.5
- [ ] Start Project 2: Docker + K8s
- [ ] Complete Project 2

### Session Notes

#### Session 1 (2026-02-02)
- Created issue-automation.yml workflow
- Fixed YAML syntax errors
- Learned about GitHub Actions triggers
- Merged PR to main
- Documented Python project roadmap
- **Decision**: Start with calculator library, then build intermediate web app before Docker/K8s
- **User Request**: Add intermediate Project 1.5 - Web-based Python app (Flask/FastAPI) between calculator library and Docker/K8s project for better progression

**Next Session**: Create python-calculator-lib project

---

## Quick Commands Reference

### Project 1 Setup
```bash
# Create new project
cd ~/projects
mkdir python-calculator-lib
cd python-calculator-lib
git init

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install pytest pytest-cov pylint black

# Run tests
pytest

# Run with coverage
pytest --cov=src

# Run linting
black src/ tests/
pylint src/
```

### Project 2 Setup (Coming Soon)
```bash
# Build Docker image
docker build -t myapp:latest .

# Run locally
docker run -p 8000:8000 myapp:latest

# Push to GHCR
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
docker tag myapp:latest ghcr.io/USERNAME/myapp:latest
docker push ghcr.io/USERNAME/myapp:latest

# Deploy to K8s
kubectl apply -f k8s/
kubectl get pods
kubectl logs <pod-name>
```

---

## Questions to Address

### Project 1
- [ ] Which Python versions to test? (3.9, 3.10, 3.11, 3.12)
- [ ] Which OS to test on? (ubuntu, windows, macos)
- [ ] Use Codecov or GitHub's built-in coverage?
- [ ] Publish to PyPI or just GitHub?

### Project 2
- [ ] Which K8s cluster? (minikube, kind, cloud provider)
- [ ] Use Helm or raw manifests?
- [ ] Which container registry? (GHCR, Docker Hub, ECR)
- [ ] Deploy on PR, merge, or tag?

---

## Success Criteria

### Project 1 Success
✓ Tests pass on multiple Python versions
✓ 80%+ code coverage
✓ Linting passes
✓ GitHub Actions runs successfully
✓ README with badges
✓ Documented learnings

### Project 2 Success
✓ Docker image builds successfully
✓ Multi-stage build optimizes size
✓ Image pushed to GHCR
✓ Deploys to K8s cluster
✓ Rolling updates work
✓ GitHub Actions automates full pipeline
✓ Documented architecture

---

**Ready to start Project 1?** Create the repository and we'll build it step by step!
