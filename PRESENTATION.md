---
marp: true
theme: default
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.svg')
header: 'CI/CD Workshop - Jenkins & Go'
footer: 'EPAM Systems | Workshop Day 1'
---

<!-- _class: lead -->

# 🚀 CI/CD Workshop
## Jenkins & Go Pipeline

**Progressive Learning Approach**
6 Phases from Git Checkout to Production-Ready CI/CD

---

## 📋 Workshop Agenda

1. **Introduction to CI/CD**
2. **Project Overview**
3. **Environment Setup**
4. **Phase 1-6: Building the Pipeline**
5. **Best Practices**
6. **Q&A**

---

<!-- _class: lead -->

# What is CI/CD?

---

## Continuous Integration / Continuous Delivery

### **Continuous Integration (CI)**
- Automate code integration from multiple developers
- Run automated tests on every commit
- Catch bugs early
- Maintain code quality

### **Continuous Delivery (CD)**
- Automate deployment process
- Create deployable artifacts
- Release software faster and more reliably

---

## 🎯 Workshop Learning Objectives

By the end of this workshop, you will:

✅ Connect Git repositories to Jenkins
✅ Configure automated build triggers
✅ Set up Go environment automatically
✅ Build applications with version injection
✅ Implement automated testing with coverage
✅ Add static code analysis (linting)
✅ Create and archive build artifacts
✅ Understand progressive pipeline development

---

<!-- _class: lead -->

# 📁 Project Structure

---

## Project Components

```
workshop-cicd/
├── cmd/webapp/              # Go web application
│   ├── main.go              # HTTP server (port 8090)
│   └── main_test.go         # Unit tests
├── jenkins/phases/          # 🎓 6 Progressive phases
│   ├── phase1-basic-checkout.jenkinsfile
│   ├── phase2-add-go-environment.jenkinsfile
│   ├── phase3-add-build.jenkinsfile
│   ├── phase4-add-tests.jenkinsfile
│   ├── phase5-add-static-analysis.jenkinsfile
│   └── phase6-add-artifacts.jenkinsfile
├── docker/                  # Jenkins in Docker
├── scripts/                 # Automation scripts
└── Vagrantfile             # VM setup
```

---

## Go Web Application

### Features
- Simple HTTP server on port **8090**
- RESTful endpoints:
  - `GET /` - Web UI with application info
  - `GET /health` - Health check (JSON)
  - `GET /version` - Version information (JSON)
- Security: Configured timeouts (Read, Write, Idle)
- Unit tests with **41.2% coverage**
- Version injection via build flags

---

<!-- _class: lead -->

# 🔧 Environment Setup

---

## Two Setup Options

### **Option 1: Vagrant VM** _(Recommended)_
- Ubuntu 24.04 LTS
- Pre-configured Jenkins
- Port: **8080**
- Initial password: `8e6b171e8fd147bf99bdd3507d7bf861`

```bash
vagrant up
# Access: http://localhost:8080
```

### **Option 2: Docker**
- Jenkins in container
- Port: **8081**

```bash
cd docker && docker-compose up -d
# Access: http://localhost:8081
```

---

## Prerequisites

✓ **Vagrant** (VirtualBox)
✓ **Docker** (if using Docker option)
✓ **Go 1.21+** (for local development)
✓ **4GB+ RAM**
✓ **Git**

---

<!-- _class: lead -->

# 🎓 The 6 Phases

---

## Progressive Learning Approach

Each phase builds on the previous one:

1. **Phase 1**: Basic Git Checkout
2. **Phase 2**: Go Environment + Triggers
3. **Phase 3**: Build + Cleanup
4. **Phase 4**: Tests
5. **Phase 5**: Static Analysis
6. **Phase 6**: Artifacts _(Production-Ready)_

---

## Phase 1: Basic Git Checkout

### What You'll Learn
- Connect Jenkins to Git repository
- Basic Jenkinsfile structure
- Pipeline stages

### Pipeline Stages
```groovy
stage('Checkout') {
    steps {
        checkout scm
    }
}
```

**Goal**: Successfully clone the repository

---

## Phase 2: Go Environment + Triggers

### What You'll Learn
- Automated build triggers (SCM polling)
- Installing Go in pipeline
- Architecture detection (amd64/arm64)

### New Features
- `triggers { pollSCM('* * * * *') }` - Poll every minute
- Automated Go 1.21.5 installation
- Dynamic PATH configuration

**Goal**: Auto-build on code changes

---

## Phase 3: Build + Cleanup

### What You'll Learn
- Workspace cleanup
- Compiling Go applications
- Version injection via build flags

### Key Steps
```bash
deleteDir()  # Clean workspace
go build -ldflags "-X main.Version=${VERSION}" \
         -o bin/app cmd/webapp/main.go
```

**Goal**: Create compiled binary with version info

---

## Phase 4: Tests

### What You'll Learn
- Running unit tests
- Test coverage reporting
- JUnit integration

### Testing Pipeline
```bash
go test -v -coverprofile=coverage.out ./...
go tool cover -func=coverage.out
```

### JUnit Reporting
- Parse test results
- Display in Jenkins UI
- Track test trends

**Goal**: Automated testing with visibility

---

## Phase 5: Static Analysis

### What You'll Learn
- Code quality checks
- Multiple linting tools
- Fail on quality issues

### Tools Used
- **golangci-lint** - Meta-linter (runs 40+ linters)
- **go vet** - Official Go static analyzer
- **gofmt** - Code formatting checker

**Goal**: Maintain code quality standards

---

## Phase 6: Artifacts (Production-Ready)

### What You'll Learn
- Creating deployment artifacts
- Tarball packaging
- Metadata generation
- Artifact archival in Jenkins

### Artifact Contents
```
artifacts/
├── app              # Compiled binary
├── version.txt      # Version info
└── run.sh           # Startup script
```

**Goal**: Complete CI/CD pipeline with deployable artifacts

---

<!-- _class: lead -->

# 🔄 Complete Pipeline Flow

---

## Final Pipeline Architecture

```
1. Git Checkout (SCM)
   ↓
2. Setup Go Environment (Go 1.21.5)
   ↓
3. Download Dependencies (go mod)
   ↓
4. Static Analysis (golangci-lint, go vet, gofmt)
   ↓
5. Build Application (inject version)
   ↓
6. Run Tests (coverage + JUnit)
   ↓
7. Create Artifacts (tarball + metadata)
   ↓
8. Archive in Jenkins
```

---

## Pipeline Triggers

### Automated
- **SCM Polling**: Every minute (`* * * * *`)
- Checks for new commits
- Auto-starts build

### Manual
- Build Now button
- Parameterized builds
- Webhook triggers (GitHub/GitLab)

---

## Version Injection

### Build-time Version Information

```go
var (
    Version   string  // From git tag
    CommitSHA string  // Current commit
    BuildDate string  // Build timestamp
)
```

### Usage in Pipeline
```bash
VERSION=$(git describe --tags --always)
BUILD_DATE=$(date -u +"%Y-%m-%dT%H:%M:%SZ")
```

**Benefit**: Track exactly what version is deployed

---

<!-- _class: lead -->

# 🎯 Best Practices

---

## CI/CD Best Practices

### ✅ Do's
- Start small, build incrementally (phases!)
- Automate everything possible
- Test early and often
- Keep builds fast
- Version your artifacts
- Use declarative pipelines
- Clean workspace between builds

---

## CI/CD Best Practices

### ❌ Don'ts
- Don't commit secrets to Git
- Don't skip tests
- Don't have manual steps in automation
- Don't ignore failing tests
- Don't build without version control
- Don't deploy without artifacts

---

## Jenkins Pipeline Best Practices

### Structure
- Use **declarative syntax** (easier to read)
- Break into **logical stages**
- Use **environment variables**
- Implement **proper error handling**

### Performance
- Parallel execution where possible
- Cache dependencies
- Clean up artifacts
- Use lightweight agents

---

## Go-Specific Best Practices

### Testing
- Write tests for all critical code
- Aim for >70% coverage
- Use table-driven tests
- Run tests in pipeline

### Code Quality
- Use golangci-lint
- Run go vet
- Format with gofmt
- Fix issues immediately

---

<!-- _class: lead -->

# 🚀 Let's Get Started!

---

## Getting Started

### Step 1: Clone Repository
```bash
git clone https://github.com/epam-msdp/CICD-workshop-day1.git
cd CICD-workshop-day1
```

### Step 2: Start Environment
```bash
# Vagrant
vagrant up

# OR Docker
cd docker && docker-compose up -d
```

### Step 3: Access Jenkins
- Vagrant: http://localhost:8080
- Docker: http://localhost:8081

---

## Create Your First Pipeline Job

1. **New Item** → Pipeline
2. **Name**: `workshop-pipeline`
3. **Pipeline Definition**: Pipeline script from SCM
4. **SCM**: Git
5. **Repository URL**: `https://github.com/epam-msdp/CICD-workshop-day1.git`
6. **Script Path**: `jenkins/phases/phase1-basic-checkout.jenkinsfile`
7. **Save** and **Build Now**

---

## Workshop Flow

1. Start with **Phase 1** (basic checkout)
2. Verify it works
3. Update Script Path to **Phase 2**
4. Build and verify
5. Continue through **Phase 3-6**
6. Each phase adds new capabilities
7. Final phase = Production-ready pipeline!

---

## Hands-On Practice

### During the Workshop
- Follow along with each phase
- Run builds after each phase
- Observe the changes
- Ask questions!
- Experiment with modifications

### After the Workshop
- Complete guide: `jenkins/phases/README.md`
- Detailed documentation for each phase
- Try customizing the pipeline
- Add your own stages

---

<!-- _class: lead -->

# 📊 What You'll See

---

## Jenkins UI - Pipeline View

### Blue Ocean Interface
- Visual pipeline representation
- Stage-by-stage execution
- Real-time logs
- Test results integration

### Classic View
- Build history
- Console output
- Test trends
- Artifact downloads

---

## Build Artifacts

### What Gets Created
```
artifacts/
├── app                 # Compiled binary (Go)
├── version.txt         # Build metadata
│   ├── VERSION=v1.0.0-abc123
│   ├── COMMIT_SHA=abc123def456
│   ├── BUILD_DATE=2024-12-18T10:30:00Z
│   └── GO_VERSION=1.21.5
└── run.sh             # Startup script
```

### Archived in Jenkins
- Download from build page
- Track versions
- Deploy to environments

---

## Test Results

### JUnit Integration
- Test pass/fail counts
- Execution time
- Trend graphs
- Historical data

### Coverage Reports
- Line coverage: **41.2%**
- Function coverage
- Identify untested code
- Quality gates

---

<!-- _class: lead -->

# 🔍 Key Concepts Review

---

## Pipeline as Code

### Benefits
- **Version Controlled**: Jenkinsfile in Git
- **Reviewable**: Code review for pipeline changes
- **Reproducible**: Same pipeline everywhere
- **Declarative**: Clear, readable syntax

### Example
```groovy
pipeline {
    agent any
    stages {
        stage('Build') { steps { sh 'make' } }
        stage('Test') { steps { sh 'make test' } }
    }
}
```

---

## Continuous Integration Benefits

### For Developers
- Faster feedback on code changes
- Catch bugs early
- Automated testing
- Consistent build environment

### For Teams
- Reduce integration problems
- Improve code quality
- Faster delivery
- Better collaboration

---

## Static Code Analysis

### Why It Matters
- Catch bugs before runtime
- Enforce coding standards
- Improve code maintainability
- Reduce technical debt

### Tools in This Workshop
- **golangci-lint**: 40+ linters in one
- **go vet**: Official Go static analyzer
- **gofmt**: Code formatting

---

<!-- _class: lead -->

# 💡 Tips & Troubleshooting

---

## Common Issues

### Build Fails
- Check Go version (should be 1.21.5)
- Verify PATH includes Go binary
- Check workspace cleanup

### Tests Fail
- Review test output in Jenkins
- Run locally: `go test -v ./...`
- Check test coverage

### Static Analysis Fails
- Review linter output
- Fix issues: `golangci-lint run`
- Format code: `gofmt -w .`

---

## Debugging Tips

### View Logs
```bash
# Jenkins console output
# Shows each command executed
# Check for error messages
```

### Local Testing
```bash
# Test build locally
./scripts/build.sh

# Run tests
go test -v ./...

# Check linting
golangci-lint run
```

---

## Performance Optimization

### Speed Up Builds
- Use Go module caching
- Parallel test execution
- Lightweight Docker images
- Incremental builds

### Resource Management
- Clean workspace regularly
- Archive only necessary artifacts
- Limit log retention
- Use build timeouts

---

<!-- _class: lead -->

# 📚 Additional Resources

---

## Documentation

### Workshop Materials
- 📖 **Complete Guide**: `jenkins/phases/README.md`
- 📁 **Phase Files**: `jenkins/phases/phase*.jenkinsfile`
- 🔧 **Scripts**: `scripts/` directory

### Official Documentation
- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)
- [Go Documentation](https://go.dev/doc/)
- [golangci-lint](https://golangci-lint.run/)

---

## Next Steps

### After This Workshop
1. Complete all 6 phases
2. Customize the pipeline
3. Add your own stages
4. Integrate with your projects
5. Explore Blue Ocean interface
6. Set up webhooks (GitHub/GitLab)
7. Add deployment stages
8. Implement notifications (email/Slack)

---

## Advanced Topics

### Beyond This Workshop
- Multi-branch pipelines
- Parameterized builds
- Docker integration
- Kubernetes deployment
- Security scanning
- Performance testing
- Rollback strategies
- Production monitoring

---

<!-- _class: lead -->

# 🎊 Let's Build a Pipeline!

## Ready to Start?

### Phase 1: Basic Checkout
**Script Path**: `jenkins/phases/phase1-basic-checkout.jenkinsfile`

**Let's go!** 🚀

---

<!-- _class: lead -->

# Thank You!

## Questions?

**Workshop Resources**
- Repository: https://github.com/epam-msdp/CICD-workshop-day1
- Guide: jenkins/phases/README.md

**Contact**
EPAM Systems - Workshop Day 1

---

<!-- _class: lead -->

# Happy Building! 🚀

### Remember:
*"Automate everything you can,*
*test everything you build,*
*and ship with confidence!"*
