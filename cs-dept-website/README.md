# CS Department Website — DevOps Lab Assignment

> A fully containerized static website with multi-environment CI/CD pipelines using GitHub Actions, Docker, and Render.com.

---

## 📁 Project Structure

```
cs-dept-website/
├── src/                        # Static website source files
│   ├── index.html              # Home Page
│   ├── courses.html            # Courses Page
│   ├── faculty.html            # Faculty Page
│   ├── admissions.html         # Admissions Page
│   ├── contact.html            # Contact Page
│   ├── style.css               # Shared stylesheet
│   ├── home.css                # Home page styles
│   ├── courses.css             # Courses page styles
│   ├── faculty.css             # Faculty page styles
│   ├── admissions.css          # Admissions page styles
│   └── contact.css             # Contact page styles
├── .github/
│   └── workflows/
│       ├── ci.yml              # CI: HTML/CSS linting + Docker build
│       ├── cd-development.yml  # CD: Deploy to Development
│       ├── cd-staging.yml      # CD: Deploy to Staging/QA
│       └── cd-production.yml   # CD: Deploy to Production
├── Dockerfile                  # Multi-stage Docker build
├── nginx.conf                  # Nginx configuration
├── .htmlhintrc                 # HTMLHint linting rules
├── .stylelintrc.json           # Stylelint CSS rules
├── .dockerignore
├── .gitignore
└── README.md
```

---

## 🌿 Git Flow Branch Strategy

| Branch    | Purpose                          | Deploys To  |
|-----------|----------------------------------|-------------|
| `develop` | Active development, feature work | Development |
| `release` | Staging / QA validation          | Staging/QA  |
| `main`    | Production-ready code            | Production  |

### Workflow
```
feature/xyz → develop → release → main
                ↓           ↓        ↓
              Dev Env    Staging   Production
```

### Creating a Feature Branch
```bash
git checkout develop
git checkout -b feature/your-feature-name
# make changes...
git add .
git commit -m "feat: add your feature description"
git push origin feature/your-feature-name
# Open a Pull Request into develop
```

---

## 🐳 Docker Setup

### Build the image locally
```bash
docker build -t cs-dept-website:local .
```

### Run the container locally
```bash
docker run -d -p 8080:80 --name cs-site cs-dept-website:local
# Visit http://localhost:8080
```

### Stop the container
```bash
docker stop cs-site && docker rm cs-site
```

---

## ⚙️ CI/CD Pipelines

### CI Pipeline (`ci.yml`)
**Triggers:** Push or PR to `develop`, `release`, or `main`

| Step | Tool | Purpose |
|------|------|---------|
| Lint HTML | HTMLHint | Validate HTML structure |
| Lint CSS | Stylelint | Validate CSS syntax & standards |
| Docker Build | Docker Buildx | Verify image builds successfully |

### CD — Development (`cd-development.yml`)
**Trigger:** Push to `develop`  
**Environment:** `development`  
**Steps:** Build → Push to Docker Hub → Deploy to Render (Dev)

### CD — Staging (`cd-staging.yml`)
**Trigger:** Push to `release`  
**Environment:** `staging`  
**Steps:** Build → Push → Smoke Test → Deploy to Render (Staging)

### CD — Production (`cd-production.yml`)
**Trigger:** Push to `main` (requires manual approval)  
**Environment:** `production`  
**Steps:** Build → Push → Full Page Smoke Tests → Deploy to Render (Prod) → Git Tag

---

## 🔐 GitHub Environments & Secrets

Create three environments in **Settings → Environments**:

### Environment: `development`
| Secret | Description |
|--------|-------------|
| `DOCKER_USERNAME` | Docker Hub username |
| `DOCKER_PASSWORD` | Docker Hub access token |
| `RENDER_DEPLOY_HOOK_DEV` | Render deploy hook URL for dev service |
| `DEV_SITE_URL` | Your development Render service URL |

### Environment: `staging`
| Secret | Description |
|--------|-------------|
| `DOCKER_USERNAME` | Docker Hub username |
| `DOCKER_PASSWORD` | Docker Hub access token |
| `RENDER_DEPLOY_HOOK_STAGING` | Render deploy hook URL for staging service |
| `STAGING_SITE_URL` | Your staging Render service URL |

### Environment: `production`
| Secret | Description |
|--------|-------------|
| `DOCKER_USERNAME` | Docker Hub username |
| `DOCKER_PASSWORD` | Docker Hub access token |
| `RENDER_DEPLOY_HOOK_PROD` | Render deploy hook URL for production service |
| `PROD_SITE_URL` | Your production Render service URL |

> 💡 Enable **Required Reviewers** on the `production` environment for manual approval before prod deploys.

---

## 🚀 Render.com Setup

Create **3 separate Web Services** on [render.com](https://render.com):

| Service Name | Environment | Docker Image Tag |
|---|---|---|
| `cs-dept-dev` | Development | `yourusername/cs-dept-website:develop` |
| `cs-dept-staging` | Staging | `yourusername/cs-dept-website:staging` |
| `cs-dept-prod` | Production | `yourusername/cs-dept-website:latest` |

### Steps per service:
1. New → Web Service → **Deploy an existing image**
2. Enter your Docker Hub image path
3. Set **Port** to `80`
4. Copy the **Deploy Hook URL** and add as a GitHub secret

---

## 🔒 Branch Protection Rules

Set up in **Settings → Branches** for each protected branch:

**`main` branch:**
- ✅ Require pull request reviews (min. 1 reviewer)
- ✅ Require status checks (CI must pass)
- ✅ Require branches to be up to date
- ✅ Restrict direct pushes

**`release` branch:**
- ✅ Require pull request reviews
- ✅ Require CI status checks

---

## 👥 Team Roles & Contributions

| # | Name | Roll No. | Role | Page | Workflow |
|---|------|----------|------|------|----------|
| 1 | *(Team Lead)* | | Team Lead | `index.html` | `ci.yml` |
| 2 | | | Developer | `courses.html` | `cd-development.yml` |
| 3 | | | Developer | `faculty.html` | `cd-staging.yml` |
| 4 | | | Developer | `admissions.html` | `cd-production.yml` |
| 5 | | | Developer | `contact.html` | — |

---

## 📋 Environment URLs

| Environment | URL |
|-------------|-----|
| Production | *(add after Render setup)* |
| Staging/QA | *(add after Render setup)* |
| Development | *(add after Render setup)* |

---

## 🏆 Evaluation Checklist

- [x] Public GitHub repository
- [x] Git Flow (develop → release → main)
- [x] 5 complete HTML/CSS web pages
- [x] Dockerfile with multi-stage build
- [x] Nginx web server configuration
- [x] CI pipeline (HTML lint + CSS lint + Docker build)
- [x] CD pipeline for Development environment
- [x] CD pipeline for Staging/QA environment  
- [x] CD pipeline for Production environment (with manual approval)
- [x] GitHub Environments with protected secrets
- [x] Smoke tests in staging and production CD
- [x] Branch protection rules documented
- [x] Comprehensive README documentation

---

*Course: Lab-DevOps for Cloud Computing | Instructor: M Khizar Hayat | Semester 6 BSCS*
