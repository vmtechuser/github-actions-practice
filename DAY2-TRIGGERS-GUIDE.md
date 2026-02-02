# Day 2: GitHub Actions Triggers - Hands-On Guide

## Current Status
- ✅ Branch: `feature/learn-triggers`
- ✅ Goal: Learn multiple trigger types and how they work
- 📝 File to edit: `.github/workflows/demo.yml`

---

## Step 1: Update demo.yml with Enhanced Triggers

### Open VS Code and replace demo.yml content with:

```yaml
name: Demo Workflow - Learning Triggers

# Multiple trigger types - this is where GitHub Actions shines!
on:
  # Trigger 1: Push to feature branches
  push:
    branches:
      - 'feature/**'    # Any branch starting with feature/
      - 'main'
    paths-ignore:
      - '**.md'         # Don't run on markdown-only changes

  # Trigger 2: Pull Requests (runs BEFORE merging - critical!)
  pull_request:
    branches: [ main ]
    types: [opened, synchronize, reopened]

  # Trigger 3: Manual trigger with inputs (like Jenkins parameters!)
  workflow_dispatch:
    inputs:
      environment:
        description: 'Environment to deploy'
        required: true
        default: 'development'
        type: choice
        options:
          - development
          - staging
          - production
      debug:
        description: 'Enable debug logging'
        required: false
        type: boolean
        default: false

  # Trigger 4: Scheduled runs (cron - like Jenkins scheduled builds)
  schedule:
    - cron: '0 9 * * 1'  # Every Monday at 9 AM UTC

jobs:
  hello-world:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Show trigger information
        run: |
          echo "🎯 Workflow triggered by: ${{ github.event_name }}"
          echo "📝 Branch: ${{ github.ref_name }}"
          echo "💻 Commit: ${{ github.sha }}"
          echo "👤 Actor: ${{ github.actor }}"
          echo "🏢 Repository: ${{ github.repository }}"

      - name: Show manual input (if triggered manually)
        if: github.event_name == 'workflow_dispatch'
        run: |
          echo "🎛️  Manual trigger detected!"
          echo "Environment selected: ${{ inputs.environment }}"
          echo "Debug mode: ${{ inputs.debug }}"

      - name: Show PR information (if triggered by PR)
        if: github.event_name == 'pull_request'
        run: |
          echo "🔀 Pull Request trigger detected!"
          echo "PR #${{ github.event.pull_request.number }}"
          echo "PR Title: ${{ github.event.pull_request.title }}"
          echo "PR Author: ${{ github.event.pull_request.user.login }}"

      - name: Run tests (simulated)
        run: |
          echo "🧪 Running tests..."
          echo "✅ All tests passed!"

      - name: Build summary
        run: |
          echo "### ✅ Workflow Complete! 🎉" >> $GITHUB_STEP_SUMMARY
          echo "" >> $GITHUB_STEP_SUMMARY
          echo "- **Trigger**: ${{ github.event_name }}" >> $GITHUB_STEP_SUMMARY
          echo "- **Branch**: ${{ github.ref_name }}" >> $GITHUB_STEP_SUMMARY
          echo "- **Commit**: \`${{ github.sha }}\`" >> $GITHUB_STEP_SUMMARY
```

---

## What's New - Learning Points

### 1. Multiple Triggers (Power of GitHub Actions!)

| Trigger | When It Runs | Jenkins Equivalent |
|---------|--------------|-------------------|
| `push` | On git push | SCM polling + webhook |
| `pull_request` | When PR is opened/updated | Multibranch pipeline |
| `workflow_dispatch` | Manual button click | "Build with Parameters" |
| `schedule` | Cron schedule | Jenkins cron trigger |

### 2. Branch Filtering
```yaml
push:
  branches:
    - 'feature/**'  # Wildcard: any branch starting with feature/
    - 'main'
```

### 3. Path Filtering
```yaml
paths-ignore:
  - '**.md'  # Don't run if only markdown files changed
```
**Why?** Save CI minutes - no need to run tests if only docs changed!

### 4. Manual Inputs (Like Jenkins Parameters!)
```yaml
workflow_dispatch:
  inputs:
    environment:
      type: choice
      options: [development, staging, production]
```
**Access in workflow:** `${{ inputs.environment }}`

### 5. Conditional Steps
```yaml
- name: Only on manual trigger
  if: github.event_name == 'workflow_dispatch'
  run: echo "Manual!"

- name: Only on PR
  if: github.event_name == 'pull_request'
  run: echo "PR detected!"
```

### 6. GitHub Contexts (Built-in Variables)
```yaml
${{ github.event_name }}              # push, pull_request, workflow_dispatch, schedule
${{ github.ref_name }}                # Branch name (feature/learn-triggers)
${{ github.sha }}                     # Commit SHA
${{ github.actor }}                   # Who triggered it (your username)
${{ github.repository }}              # vmtechuser/github-actions-practice
${{ github.event.pull_request.number }}  # PR number (if PR trigger)
${{ inputs.environment }}             # Manual input value
```

---

## Step 2: Commit and Push

After updating demo.yml in VS Code:

```bash
# Check status
git status

# Add the modified file
git add .github/workflows/demo.yml

# Commit with message
git commit -m "Add multiple triggers: push, PR, manual, schedule"

# Push to feature branch
git push origin feature/learn-triggers
```

**What happens:**
- ✅ Workflow will trigger on push (because you're pushing to feature/*)
- ✅ You'll see it run in Actions tab

---

## Step 3: Test Each Trigger Type

### Test 1: Push Trigger (Just Did This!)
```bash
git push origin feature/learn-triggers
```
- Go to: https://github.com/vmtechuser/github-actions-practice/actions
- See workflow run with event_name = "push"

### Test 2: Pull Request Trigger
```bash
# Create PR via GitHub CLI
gh pr create --base main --head feature/learn-triggers \
  --title "Learn GitHub Actions Triggers" \
  --body "Testing multiple trigger types"
```
Or create PR manually on GitHub.

**What to observe:**
- Workflow runs automatically on PR creation
- Shows PR number, title, author in output
- Runs BEFORE code is merged to main (safety!)

### Test 3: Manual Trigger (Most Fun!)
1. Go to: https://github.com/vmtechuser/github-actions-practice/actions
2. Click "Demo Workflow - Learning Triggers"
3. Click "Run workflow" button (top right)
4. Select options:
   - Environment: staging
   - Debug: true
5. Click "Run workflow"

**What to observe:**
- You control when it runs
- You provide inputs
- Workflow shows your selected values

### Test 4: Schedule Trigger
- Runs automatically every Monday at 9 AM UTC
- Can't test immediately (unless you change the cron time)
- To test now, change to: `- cron: '*/5 * * * *'` (every 5 minutes)

---

## Step 4: Create Pull Request

After testing push trigger, create a PR to see PR trigger in action:

```bash
gh pr create --base main --head feature/learn-triggers \
  --title "Day 2: Learn GitHub Actions Triggers" \
  --body "Enhanced demo workflow with:
- Multiple trigger types (push, PR, manual, schedule)
- Branch and path filtering
- Manual inputs (workflow_dispatch)
- Conditional steps based on trigger type
- GitHub context variables demonstration"
```

Or create manually:
1. Go to: https://github.com/vmtechuser/github-actions-practice
2. Click "Pull requests" → "New pull request"
3. Base: main ← Compare: feature/learn-triggers
4. Create pull request

---

## Understanding PR Workflows (Critical Concept!)

### Why PR Triggers Are Important:

```
Without PR Workflows:
Developer → Push to main → Break production → 😱

With PR Workflows:
Developer → Create PR → Actions run tests → ✅ or ❌
         → Review results → Merge if green → 😊
```

**This is the magic of GitHub Actions + PRs!**

---

## Jenkins vs GitHub Actions - Triggers Comparison

| Feature | Jenkins | GitHub Actions |
|---------|---------|----------------|
| **SCM Push** | Webhook + SCM polling | `on: push` (native) |
| **PR Builds** | Multibranch pipeline plugin | `on: pull_request` (native) |
| **Manual Trigger** | "Build Now" or "Build with Parameters" | `on: workflow_dispatch` |
| **Parameters** | Configure in UI | Define in YAML `inputs:` |
| **Scheduled** | Cron syntax in job config | `on: schedule` |
| **Path Filters** | Requires plugins | Built-in `paths:` / `paths-ignore:` |
| **Branch Filters** | Regex in Multibranch | Simple wildcards `feature/**` |

**Winner:** GitHub Actions - Everything is native, no plugins needed!

---

## Common GitHub Contexts (Cheat Sheet)

```yaml
# Trigger Information
${{ github.event_name }}        # push, pull_request, workflow_dispatch, etc.
${{ github.workflow }}          # Workflow name
${{ github.run_id }}           # Unique run ID
${{ github.run_number }}       # Sequential run number

# Repository Information
${{ github.repository }}        # owner/repo-name
${{ github.repository_owner }}  # owner
${{ github.ref }}              # refs/heads/branch-name
${{ github.ref_name }}         # branch-name (cleaner)
${{ github.sha }}              # Full commit SHA
${{ github.actor }}            # Who triggered the workflow

# Pull Request (only available on pull_request trigger)
${{ github.event.pull_request.number }}
${{ github.event.pull_request.title }}
${{ github.event.pull_request.user.login }}
${{ github.event.pull_request.head.ref }}  # Source branch
${{ github.event.pull_request.base.ref }}  # Target branch

# Manual Inputs (only on workflow_dispatch)
${{ inputs.input_name }}        # Access manual input values

# Secrets
${{ secrets.SECRET_NAME }}      # Access GitHub secrets
```

---

## Troubleshooting

### Workflow Doesn't Trigger?

**Check:**
1. Branch filter matches: `feature/**` matches `feature/learn-triggers` ✅
2. Path filter: Did you only change .md files? (paths-ignore)
3. Workflow file syntax: YAML linter shows errors?
4. Workflow file location: Must be in `.github/workflows/`

### VS Code YAML Errors?

1. Hover over red squiggles
2. Check indentation (spaces, not tabs)
3. Validate YAML online: https://www.yamllint.com/

### Can't See "Run workflow" Button?

- Must have `workflow_dispatch:` trigger defined
- Refresh the Actions page
- Make sure you're looking at the correct workflow

---

## Next Steps After This

Once you've tested all triggers:

### Day 3: Actions & Marketplace
- Use real actions (setup-node, setup-python)
- Build a real project (Node.js app with tests)
- Understand action inputs/outputs

### Day 4-5: Matrix Builds & Caching
- Test across multiple OS/versions
- Speed up workflows with caching
- Parallel job execution

### Day 6-7: Artifacts & Advanced
- Upload/download build artifacts
- Share data between jobs
- Environment variables and secrets

---

## Quick Commands Reference

```bash
# Branch operations
git checkout -b feature/name     # Create new branch
git branch                       # List branches
git status                       # Check status

# Commit workflow
git add file                     # Stage file
git commit -m "message"          # Commit
git push origin branch-name      # Push to remote

# PR operations (GitHub CLI)
gh pr create                     # Create PR
gh pr list                       # List PRs
gh pr view 123                   # View PR details

# View workflow runs
gh run list                      # List recent runs
gh run view                      # View latest run
gh run view --log               # View logs
```

---

## Key Takeaways

1. ✅ **Multiple triggers** make workflows versatile
2. ✅ **PR triggers** enable testing before merge (critical!)
3. ✅ **Manual triggers** give you control + parameters
4. ✅ **Conditional steps** based on trigger type
5. ✅ **GitHub contexts** provide rich metadata
6. ✅ **Path filtering** saves CI minutes
7. ✅ **No plugins needed** - everything is native!

---

## Save This File!

Keep this guide for reference. If you get disconnected:
1. Open this file: `DAY2-TRIGGERS-GUIDE.md`
2. Continue from where you left off
3. All instructions are preserved

**Current task:** Update demo.yml → Commit → Push → Test triggers!

Ready? Let's do it! 🚀
