# Hour 1: Merge & Explore - Hands-On Practice

## Goal
- Merge your Day 2 PR
- Learn path filtering (run workflows only when specific files change)
- Learn issue triggers (workflows triggered by GitHub issues)
- **YOU write parts of the workflow yourself!**

---

## Part 1: Merge PR (5 minutes)

### Step 1: Merge the PR

**Option A: Command Line (Recommended)**
```bash
# Merge PR #1 with squash (combines all commits into one)
gh pr merge 1 --squash --delete-branch

# This will:
# - Merge feature/learn-triggers into main
# - Squash all commits into one clean commit
# - Delete the feature branch (cleanup)
```

**Option B: Browser**
- Go to: https://github.com/vmtechuser/github-actions-practice/pull/1
- Click "Squash and merge"
- Confirm
- Delete branch

### Step 2: Update Your Local Repository

```bash
# Switch back to main
git checkout main

# Pull the merged changes
git pull origin main

# Verify you have the latest
git log --oneline -3
```

**Expected:** You should see the merged commit at the top.

### Step 3: Create New Branch for Exploration

```bash
# Create and switch to new branch
git checkout -b feature/explore-triggers

# Verify you're on the new branch
git branch
```

---

## Part 2: Path Filtering (15 minutes)

### What is Path Filtering?

**Problem:** Running tests when only README.md changed wastes CI minutes.

**Solution:** Only run workflow when specific files/directories change.

### Create Your First Hands-On Workflow

**You'll create:** `.github/workflows/path-filter-demo.yml`

**In VS Code, create this file and fill in the TODOs:**

```yaml
name: Path Filter Demo

on:
  push:
    branches: [ main, 'feature/**' ]

    # Run ONLY when these paths change
    paths:
      - 'src/**'           # Any file in src/ directory
      - '**.js'            # Any JavaScript file anywhere
      - 'package.json'     # Specific file
      - '.github/workflows/**'  # Workflow files

    # DON'T run when these paths change
    paths-ignore:
      - '**.md'            # Markdown files
      - 'docs/**'          # Documentation
      - 'LICENSE'          # License file

jobs:
  test-path-filtering:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 2   # Fetch last 2 commits to compare

      - name: Show changed files
        run: |
          echo "Files changed in this commit:"
          git diff --name-only HEAD^ HEAD

      # TODO: YOUR TURN - Add a step here!
      # Add a step that checks if JavaScript files changed
      # Hint: Use 'git diff --name-only HEAD^ HEAD | grep ".js"'
      - name: Check for JS changes
        run: |
          echo "TODO: Write command to check for .js files"
          # YOUR CODE HERE

      # TODO: YOUR TURN - Add another step!
      # Add a step that counts how many files changed
      - name: Count changed files
        run: |
          echo "TODO: Count the number of changed files"
          # YOUR CODE HERE
          # Hint: git diff --name-only HEAD^ HEAD | wc -l
```

### Your Assignment:

1. **Create the file** in VS Code
2. **Fill in the two TODOs** with your own commands
3. **Save the file**

**Don't worry if stuck - I'll help! But try first!**

---

## Part 3: Issue Triggers (20 minutes)

### What are Issue Triggers?

Workflows that run when GitHub issues are created, commented on, closed, etc.

**Use Cases:**
- Auto-label new issues
- Welcome message for first-time contributors
- Run automation when specific labels added
- Integration with external systems

### Create Issue Automation Workflow

**Create:** `.github/workflows/issue-automation.yml`

```yaml
name: Issue Automation

on:
  # Trigger when issues are opened or reopened
  issues:
    types: [opened, reopened, labeled]

  # Trigger when someone comments on an issue
  issue_comment:
    types: [created]

jobs:
  handle-issue:
    runs-on: ubuntu-latest

    steps:
      - name: Show issue information
        run: |
          echo "🎯 Event: ${{ github.event_name }}"
          echo "📝 Issue #${{ github.event.issue.number }}"
          echo "🏷️  Title: ${{ github.event.issue.title }}"
          echo "👤 Author: ${{ github.event.issue.user.login }}"
          echo "🔗 URL: ${{ github.event.issue.html_url }}"

      # Only run if a new issue was opened
      - name: Welcome new issue
        if: github.event.action == 'opened'
        run: |
          echo "🎉 New issue opened!"
          echo "Issue #${{ github.event.issue.number }}: ${{ github.event.issue.title }}"

      # TODO: YOUR TURN!
      # Add a step that only runs when someone adds a comment
      # Condition: github.event_name == 'issue_comment'
      - name: Handle new comment
        if: TODO_ADD_CONDITION_HERE
        run: |
          echo "💬 New comment on issue #${{ github.event.issue.number }}"
          echo "Comment by: ${{ github.event.comment.user.login }}"
          echo "Comment: ${{ github.event.comment.body }}"

      # TODO: YOUR TURN!
      # Add a step that checks if the issue has a 'bug' label
      # This is more advanced - try to figure it out!
      # Hint: github.event.issue.labels is an array
      - name: Handle bug label
        if: TODO_ADD_CONDITION_HERE
        run: |
          echo "🐛 This is a bug report!"

permissions:
  issues: write
  pull-requests: write
```

### Your Assignment:

1. **Create the file**
2. **Fill in the two TODO conditions**
3. **Think about:** What conditions should trigger each step?

**Tips:**
- `github.event.action` = opened, closed, reopened, labeled
- `github.event_name` = issues or issue_comment
- `github.event.issue.labels` = array of labels

---

## Part 4: Test Your Work (15 minutes)

### Step 1: Commit Your New Workflows

```bash
# Check what you created
git status

# Add the new workflow files
git add .github/workflows/path-filter-demo.yml
git add .github/workflows/issue-automation.yml

# Commit
git commit -m "Add path filtering and issue automation workflows"

# Push to your feature branch
git push -u origin feature/explore-triggers
```

### Step 2: Test Path Filtering

**Test A: Change only markdown (should NOT trigger)**
```bash
# Modify README or any .md file
echo "# Test" >> README.md

git add README.md
git commit -m "Update README - should NOT trigger path-filter-demo"
git push
```

**Check:** Go to Actions - path-filter-demo should NOT run!

**Test B: Change JavaScript file (SHOULD trigger)**
```bash
# Create a dummy JS file
mkdir -p src
echo "console.log('test');" > src/test.js

git add src/test.js
git commit -m "Add JS file - SHOULD trigger path-filter-demo"
git push
```

**Check:** Go to Actions - path-filter-demo SHOULD run!

### Step 3: Test Issue Triggers

**Create a test issue:**

**Option A: Command Line**
```bash
gh issue create \
  --title "Test issue automation" \
  --body "Testing the issue workflow trigger"
```

**Option B: Browser**
- Go to: https://github.com/vmtechuser/github-actions-practice/issues
- Click "New issue"
- Title: "Test issue automation"
- Create issue

**Check:** Go to Actions - issue-automation workflow should run!

**Add a comment to the issue:**
```bash
gh issue comment 1 --body "Testing comment trigger"
```

**Check:** Workflow should trigger again with issue_comment event!

---

## Part 5: Review and Learn (5 minutes)

### What You Learned:

#### 1. Path Filtering
```yaml
paths:           # Only run if these change
  - 'src/**'
paths-ignore:    # Skip if only these change
  - '**.md'
```

**Use Case:** Save CI minutes, run only when needed

#### 2. Issue Triggers
```yaml
on:
  issues:
    types: [opened, reopened]
  issue_comment:
    types: [created]
```

**Use Case:** Automate issue management, notifications, integrations

#### 3. Advanced Conditions
```yaml
if: github.event.action == 'opened'
if: github.event_name == 'issue_comment'
```

**Use Case:** Fine-grained control over step execution

### Jenkins Comparison:

| Feature | Jenkins | GitHub Actions |
|---------|---------|----------------|
| **Path filtering** | SCM polling with includes/excludes | `paths:` / `paths-ignore:` |
| **Issue webhooks** | Third-party plugins | Native `on: issues` |
| **Conditional steps** | when {} block | `if:` condition |

---

## Challenges (Optional - If Time):

### Challenge 1: Path Filter Combinations
Create a workflow that runs only when:
- JavaScript files in `src/` change
- OR package.json changes
- BUT NOT test files

### Challenge 2: Issue Label Automation
Create a workflow that:
- Adds a "needs-triage" label to new issues
- Adds a "first-time-contributor" label if it's their first issue

### Challenge 3: PR Size Labeling
Create a workflow that:
- Labels PRs as "small", "medium", or "large" based on lines changed

---

## Verification Checklist:

- [ ] PR merged successfully
- [ ] Local main branch updated
- [ ] New branch created: feature/explore-triggers
- [ ] path-filter-demo.yml created with TODOs filled
- [ ] issue-automation.yml created with TODOs filled
- [ ] Committed and pushed both workflows
- [ ] Tested path filtering (md vs js files)
- [ ] Tested issue trigger by creating an issue
- [ ] Workflows ran successfully in Actions tab

---

## Common Issues & Solutions:

### Issue: Path filter not working
**Solution:** Check that paths use forward slashes `/` and quotes `'src/**'`

### Issue: Workflow not triggering
**Solution:** Ensure workflow file is in `.github/workflows/` directory

### Issue: Permission denied on issue automation
**Solution:** Add `permissions:` block at top of workflow

### Issue: Can't see changed files
**Solution:** Set `fetch-depth: 2` in checkout action

---

## Quick Reference:

### Path Patterns:
```yaml
'*.js'       # JS files in root only
'**.js'      # JS files anywhere
'src/**'     # All files in src/ directory
'src/*.js'   # JS files in src/ directory only
```

### Issue Event Properties:
```yaml
${{ github.event.issue.number }}      # Issue number
${{ github.event.issue.title }}       # Issue title
${{ github.event.issue.user.login }}  # Issue author
${{ github.event.action }}            # opened, closed, etc.
${{ github.event.comment.body }}      # Comment text
```

---

## Next: Hour 2-3 - Build Real Project!

Once you complete Hour 1:
1. ✅ You understand path filtering
2. ✅ You understand issue triggers
3. ✅ You wrote parts of workflows yourself
4. ✅ Ready to build a complete CI pipeline!

**Save this file and open HOUR2-BUILD-PROJECT.md when ready!**
