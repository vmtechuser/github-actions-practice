# GitHub Actions Learning Path
## From Jenkins to GitHub Actions - Beginner to Intermediate

---

## Your Profile
- **Background**: Experienced with Jenkins CI/CD
- **Goal**: Master GitHub Actions fundamentals → intermediate level
- **Focus Areas**: Testing, Deployments, Code Quality, Artifact Management, Jenkins Migration
- **Timeline**: Fast track to productivity

---

## Learning Resources Comparison

### Option 1: Free Resources (Recommended Start)
**Pros:**
- No cost
- Official documentation is excellent
- GitHub's own learning paths are high quality
- Community examples and templates
- Learn at your own pace

**Cons:**
- Requires self-discipline
- Need to curate your own path
- Less structured than paid courses

### Option 2: Udemy Courses
**Pros:**
- Structured curriculum
- Video format (if you prefer)
- Often includes projects
- Lifetime access
- Usually $10-20 on sale

**Cons:**
- May move slower than you need (with Jenkins background)
- Content may become outdated
- Less hands-on than official docs

### **My Recommendation**: Start with free resources below + this guided path. Your Jenkins experience means you'll learn faster than typical beginners. Save Udemy courses for specific gaps if needed.

---

## Free Learning Resources (Best to Start)

### 1. **GitHub Official Learning Path** ⭐ PRIMARY
- **URL**: https://docs.github.com/en/actions/learn-github-actions
- **Why**: Most up-to-date, comprehensive, straight from source
- **Start here**: "Understanding GitHub Actions" → "Quickstart"

### 2. **GitHub Skills (Interactive Tutorials)** ⭐ HANDS-ON
- **URL**: https://skills.github.com/
- **Courses to take**:
  - "Hello GitHub Actions" (30 min)
  - "Continuous Integration" (45 min)
  - "Deploy to Azure/AWS" (depends on your cloud)
- **Why**: Interactive, you learn by doing in real repos

### 3. **GitHub Actions Marketplace**
- **URL**: https://github.com/marketplace?type=actions
- **Why**: See real-world actions, learn from examples

### 4. **Awesome GitHub Actions**
- **URL**: https://github.com/sdras/awesome-actions
- **Why**: Curated list of actions, workflows, and tutorials

### 5. **FreeCodeCamp YouTube** (Optional)
- Search "GitHub Actions tutorial" on YouTube
- 2-3 hour comprehensive videos available free

### 6. **Jenkins to GitHub Actions Migration Guide** ⭐ FOR MIGRATION
- **URL**: https://docs.github.com/en/actions/migrating-to-github-actions/automated-migrations/migrating-from-jenkins-with-github-actions-importer
- **Why**: Official migration tool and guide

---

## Your Structured Learning Path

### Phase 1: Fundamentals (Week 1 - Days 1-3)
**Goal**: Understand core concepts and syntax

#### Day 1: Core Concepts
- [ ] Read: GitHub Actions overview
- [ ] Understand: Workflows, Jobs, Steps, Actions, Runners
- [ ] Learn: YAML syntax basics
- [ ] Compare: Jenkins vs GitHub Actions terminology
- [ ] Build: Hello World workflow

**Jenkins → GitHub Actions Translation:**
```
Jenkins Pipeline          →  GitHub Actions Workflow
Jenkinsfile              →  .github/workflows/*.yml
Stage                    →  Job
Step                     →  Step
Agent                    →  Runner (runs-on)
Credentials              →  Secrets
Post actions             →  if: always(), if: failure()
```

#### Day 2: Triggers & Events
- [ ] Learn: Workflow triggers (push, pull_request, schedule, workflow_dispatch)
- [ ] Understand: Event payloads and filters
- [ ] Build: Workflow with multiple trigger types
- [ ] Practice: Path filtering, branch filtering

#### Day 3: Actions & Marketplace
- [ ] Explore: GitHub Marketplace
- [ ] Use: Common actions (checkout, setup-node, setup-python, etc.)
- [ ] Understand: action.yml structure
- [ ] Build: Workflow using 3+ marketplace actions

---

### Phase 2: Essential Skills (Week 1 - Days 4-7)

#### Day 4: Environment & Secrets
- [ ] Learn: Environment variables (workflow, job, step levels)
- [ ] Practice: Using GitHub secrets
- [ ] Understand: GitHub contexts (${{ github.* }})
- [ ] Build: Workflow with environment-specific deployments

#### Day 5: Matrix Strategies & Parallelization
- [ ] Learn: Matrix builds (multiple OS, versions)
- [ ] Understand: Job dependencies (needs)
- [ ] Practice: Parallel vs sequential execution
- [ ] Build: Multi-version test workflow

**Jenkins Equivalent:**
```
Jenkins Matrix Project    →  strategy: matrix:
Parallel stages          →  jobs run in parallel by default
Sequential stages        →  needs: [previous-job]
```

#### Day 6: Conditional Execution & Flow Control
- [ ] Learn: if conditions
- [ ] Practice: success(), failure(), always(), cancelled()
- [ ] Understand: Step outcomes
- [ ] Build: Workflow with conditional deployment

#### Day 7: Artifacts & Caching
- [ ] Learn: Upload/download artifacts
- [ ] Practice: Dependency caching
- [ ] Understand: Cache strategies
- [ ] Build: Build artifacts and share between jobs

**Jenkins Equivalent:**
```
archiveArtifacts         →  actions/upload-artifact
Cache plugin             →  actions/cache
```

---

### Phase 3: Intermediate Level (Week 2)

#### Day 8-9: Automated Testing
**Topics:**
- [ ] Unit test automation (Jest, pytest, etc.)
- [ ] Integration testing
- [ ] Test reporting and annotations
- [ ] Code coverage reports
- [ ] Test result publishing

**Build**: Complete CI workflow with:
- Lint checks
- Unit tests across multiple environments
- Coverage reports
- Test result comments on PRs

#### Day 10-11: Code Quality & Security
**Topics:**
- [ ] Linting (ESLint, Pylint, etc.)
- [ ] Code formatting (Prettier, Black)
- [ ] Security scanning (CodeQL, Snyk, Trivy)
- [ ] Dependency updates (Dependabot)
- [ ] SAST/DAST basics

**Build**: Code quality workflow with:
- Automated linting
- Security scanning
- PR annotations for issues

#### Day 12-13: Deployment Pipelines
**Topics:**
- [ ] Environment management (dev, staging, prod)
- [ ] Deployment strategies (blue-green, canary, rolling)
- [ ] Environment protection rules
- [ ] Manual approvals
- [ ] Rollback strategies
- [ ] Deploy to common platforms (AWS, Azure, GCP, Heroku, Vercel)

**Build**: Complete CD pipeline:
- Automated deploy to staging on PR
- Manual approval for production
- Rollback capability

**Jenkins Equivalent:**
```
Deploy stage             →  Deployment job with environment
Input step               →  Environment protection rules (in UI)
Rollback                 →  Re-run previous successful workflow
```

#### Day 14: Building & Releasing Artifacts
**Topics:**
- [ ] Docker image building and pushing
- [ ] NPM/PyPI package publishing
- [ ] GitHub Releases automation
- [ ] Semantic versioning
- [ ] Release notes generation
- [ ] Multi-platform builds

**Build**: Release workflow that:
- Builds artifacts on tag
- Creates GitHub release
- Publishes to registries
- Generates changelogs

---

### Phase 4: Jenkins Migration (Week 3)

#### Day 15-16: Migration Preparation
- [ ] Install GitHub Actions Importer CLI
- [ ] Audit existing Jenkins pipelines
- [ ] Identify plugins → Actions mapping
- [ ] Document custom scripts to migrate
- [ ] Plan migration strategy

#### Day 17-18: Automated Migration
- [ ] Use GitHub Actions Importer tool
- [ ] Dry-run migrations
- [ ] Review auto-converted workflows
- [ ] Manual adjustments needed
- [ ] Test converted workflows

#### Day 19-20: Advanced Migration Topics
- [ ] Migrate complex pipelines
- [ ] Handle Jenkins plugins without direct equivalents
- [ ] Migrate shared libraries → composite actions
- [ ] Set up self-hosted runners (if needed)
- [ ] Performance comparison

**Common Jenkins → GitHub Actions Patterns:**

| Jenkins Feature | GitHub Actions Solution |
|----------------|------------------------|
| Shared Libraries | Composite Actions / Reusable Workflows |
| Multibranch Pipeline | Automatic for all branches |
| Credentials | GitHub Secrets + Environments |
| Jenkins Agents | GitHub-hosted or self-hosted runners |
| Webhook Triggers | Built-in event triggers |
| Cron Jobs | schedule: cron |
| Pipeline Parameters | workflow_dispatch inputs |
| Build History | Actions tab, 90-day retention |

---

### Phase 5: Advanced Topics (Ongoing)

#### Week 4+: Choose Your Path
- [ ] **Custom Actions**: Build reusable JavaScript/Docker/Composite actions
- [ ] **Reusable Workflows**: DRY principle for workflows
- [ ] **Self-hosted Runners**: For special requirements
- [ ] **Advanced Security**: OIDC, keyless auth
- [ ] **Monorepo Strategies**: Path filtering, matrix
- [ ] **Performance Optimization**: Caching strategies, job dependencies
- [ ] **Debugging**: Workflow debugging techniques
- [ ] **API Integration**: GitHub API + Actions

---

## Practice Projects (Build These)

### Project 1: Simple Node.js API (Week 1)
**Learn**: Basic CI/CD
- Lint code
- Run tests
- Build Docker image
- Deploy to staging

### Project 2: Python Library (Week 2)
**Learn**: Testing + Publishing
- Test multiple Python versions
- Code coverage
- Publish to PyPI
- Generate documentation

### Project 3: Full-Stack App (Week 2-3)
**Learn**: Complex pipelines
- Frontend + Backend testing
- Database migrations
- Multi-environment deployment
- End-to-end tests

### Project 4: Migrate Real Jenkins Pipeline (Week 3)
**Learn**: Real-world migration
- Pick a Jenkins project
- Migrate using Importer tool
- Optimize the workflow
- Document learnings

---

## Key Differences: Jenkins vs GitHub Actions

### Advantages of GitHub Actions
✅ **Native GitHub integration** - No setup needed
✅ **Matrix builds** - Easier parallel testing
✅ **Marketplace** - 20,000+ pre-built actions
✅ **Free tier** - 2,000 minutes/month for private repos
✅ **YAML-based** - Simpler than Groovy
✅ **Event-driven** - Rich trigger options
✅ **Built-in secrets** - No credential plugins needed

### Where Jenkins Still Shines
⚠️ **Self-hosted control** - Full server control
⚠️ **Plugin ecosystem** - Mature ecosystem
⚠️ **No minute limits** - Unlimited on self-hosted
⚠️ **Pipeline libraries** - Shared Groovy libraries
⚠️ **Complex orchestration** - Very complex pipelines

---

## Daily Practice Checklist

### Each Day:
- [ ] Read documentation (30 min)
- [ ] Watch tutorial if available (30 min)
- [ ] Build/modify a workflow (60 min)
- [ ] Test and troubleshoot (30 min)
- [ ] Document learnings (15 min)

**Total**: ~2.5 hours/day for 3 weeks = Solid intermediate level

---

## Success Metrics

### Week 1 Complete:
✓ Can write basic workflows from scratch
✓ Understand all core concepts
✓ Use marketplace actions confidently

### Week 2 Complete:
✓ Build complete CI/CD pipelines
✓ Implement testing and quality checks
✓ Deploy to cloud platforms
✓ Manage artifacts and releases

### Week 3 Complete:
✓ Successfully migrate Jenkins pipeline
✓ Optimize workflows for performance
✓ Troubleshoot issues independently
✓ Ready for production use

---

## Quick Reference Sheet

### Workflow Structure
```yaml
name: Workflow Name
on: [push, pull_request]
env:
  GLOBAL_VAR: value

jobs:
  job-name:
    runs-on: ubuntu-latest
    env:
      JOB_VAR: value
    steps:
      - uses: actions/checkout@v4
      - run: echo "Hello"
```

### Common Actions to Memorize
- `actions/checkout@v4` - Clone repository
- `actions/setup-node@v4` - Setup Node.js
- `actions/setup-python@v5` - Setup Python
- `actions/cache@v4` - Cache dependencies
- `actions/upload-artifact@v4` - Save artifacts
- `actions/download-artifact@v4` - Retrieve artifacts
- `docker/build-push-action@v5` - Build/push Docker images

### Common Patterns
```yaml
# Run on specific branches
on:
  push:
    branches: [main, develop]

# Run on schedule
on:
  schedule:
    - cron: '0 0 * * *'  # Daily at midnight

# Matrix build
strategy:
  matrix:
    node-version: [18, 20, 22]
    os: [ubuntu-latest, windows-latest]

# Conditional step
- name: Deploy
  if: github.ref == 'refs/heads/main'
  run: ./deploy.sh

# Job dependency
jobs:
  build:
    ...
  deploy:
    needs: build  # Wait for build job
    ...
```

---

## Next Steps

1. **Start Today**: Begin with GitHub Skills interactive tutorial (30 min)
2. **Read This**: GitHub Actions documentation overview (1 hour)
3. **Build First Workflow**: Hello World in this repo (30 min)
4. **Join Community**: GitHub Community Discussions for questions

---

## Resources Summary

📚 **Primary Learning**:
- https://docs.github.com/en/actions
- https://skills.github.com/

🔧 **Tools**:
- https://github.com/actions/importer (Jenkins → GitHub Actions)
- https://github.com/nektos/act (Test workflows locally)

💡 **Examples**:
- https://github.com/sdras/awesome-actions
- https://github.com/actions/starter-workflows

🆘 **Help**:
- https://github.community/
- Stack Overflow: #github-actions

---

## My Commitment to You

I will guide you through this learning path by:
1. Creating example workflows for each concept
2. Explaining Jenkins → GitHub Actions translations
3. Helping troubleshoot issues
4. Building practice projects together
5. Reviewing your workflows

**Ready to start? Tell me which day/topic you want to begin with!**
