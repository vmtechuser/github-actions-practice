# Hour 2-3: Build Real Project - Complete CI Pipeline

## Goal
Build a real application with complete CI/CD pipeline that YOU write!

**What You'll Build:**
- Simple Node.js API (or Python script - your choice)
- Automated tests
- Linting (code quality)
- Complete CI workflow
- **YOU write 80% of the workflow yourself!**

---

## Choose Your Project Type

### Option A: Node.js REST API ⭐ Recommended
**Why:** Most common, many marketplace actions, good for learning
**Time:** 1.5 hours

### Option B: Python CLI Tool
**Why:** Great for scripts, data processing
**Time:** 1.5 hours

**Choose one and jump to that section!**

---

# Option A: Node.js REST API

## Part 1: Create the Application (20 min)

### Step 1: Initialize Project

```bash
# Make sure you're on the right branch
git checkout -b feature/nodejs-ci

# Create project directory
mkdir -p simple-api
cd simple-api

# Initialize Node.js project
npm init -y
```

### Step 2: Install Dependencies

```bash
# Application dependencies
npm install express

# Development dependencies
npm install --save-dev \
  jest \
  eslint \
  eslint-config-standard \
  supertest
```

### Step 3: Create Application Code

**Create: `simple-api/index.js`**

```javascript
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.use(express.json());

// Health check endpoint
app.get('/health', (req, res) => {
  res.json({ status: 'healthy', timestamp: new Date().toISOString() });
});

// Hello endpoint
app.get('/api/hello', (req, res) => {
  const name = req.query.name || 'World';
  res.json({ message: `Hello, ${name}!` });
});

// Math endpoint
app.post('/api/add', (req, res) => {
  const { a, b } = req.body;

  if (typeof a !== 'number' || typeof b !== 'number') {
    return res.status(400).json({ error: 'Both a and b must be numbers' });
  }

  res.json({ result: a + b });
});

// Start server (only if not in test mode)
if (require.main === module) {
  app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
  });
}

module.exports = app;
```

### Step 4: Create Tests

**Create: `simple-api/index.test.js`**

```javascript
const request = require('supertest');
const app = require('./index');

describe('API Tests', () => {
  describe('GET /health', () => {
    it('should return healthy status', async () => {
      const res = await request(app).get('/health');
      expect(res.status).toBe(200);
      expect(res.body.status).toBe('healthy');
      expect(res.body.timestamp).toBeDefined();
    });
  });

  describe('GET /api/hello', () => {
    it('should return hello world by default', async () => {
      const res = await request(app).get('/api/hello');
      expect(res.status).toBe(200);
      expect(res.body.message).toBe('Hello, World!');
    });

    it('should return hello with custom name', async () => {
      const res = await request(app).get('/api/hello?name=GitHub');
      expect(res.status).toBe(200);
      expect(res.body.message).toBe('Hello, GitHub!');
    });
  });

  describe('POST /api/add', () => {
    it('should add two numbers', async () => {
      const res = await request(app)
        .post('/api/add')
        .send({ a: 5, b: 3 });
      expect(res.status).toBe(200);
      expect(res.body.result).toBe(8);
    });

    it('should return error for invalid input', async () => {
      const res = await request(app)
        .post('/api/add')
        .send({ a: 'invalid', b: 3 });
      expect(res.status).toBe(400);
      expect(res.body.error).toBeDefined();
    });
  });
});
```

### Step 5: Setup ESLint

**Create: `simple-api/.eslintrc.json`**

```json
{
  "env": {
    "node": true,
    "es2021": true,
    "jest": true
  },
  "extends": "standard",
  "parserOptions": {
    "ecmaVersion": 12
  },
  "rules": {
    "semi": ["error", "always"]
  }
}
```

### Step 6: Update package.json Scripts

**Edit: `simple-api/package.json`**

Add these scripts:
```json
{
  "scripts": {
    "start": "node index.js",
    "test": "jest --coverage",
    "test:watch": "jest --watch",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix"
  }
}
```

### Step 7: Test Locally

```bash
# Run linter
npm run lint

# Run tests
npm test

# Start server (optional)
npm start
# Visit: http://localhost:3000/health
```

---

## Part 2: Write the CI Workflow (45 min)

### Your Assignment: Write Most of This Yourself!

**Create: `.github/workflows/nodejs-ci.yml`**

**I'll provide the structure, YOU fill in the details:**

```yaml
name: Node.js CI

# TODO: YOUR TURN - Add triggers
# Add: push to main, PR to main, and manual trigger
on:
  # TODO: Add push trigger for main branch

  # TODO: Add pull_request trigger for main branch

  # TODO: Add workflow_dispatch for manual runs

jobs:
  # Job 1: Linting
  lint:
    runs-on: ubuntu-latest

    steps:
      # TODO: Add checkout step
      # Hint: uses: actions/checkout@v4

      # TODO: Add Node.js setup
      # Hint: uses: actions/setup-node@v4
      # with:
      #   node-version: '20'
      #   cache: 'npm'
      #   cache-dependency-path: 'simple-api/package-lock.json'

      - name: Install dependencies
        working-directory: ./simple-api
        run: npm ci

      # TODO: Add lint step
      # Hint: Run 'npm run lint'
      - name: Run ESLint
        working-directory: ./simple-api
        run: TODO_ADD_COMMAND_HERE

  # Job 2: Testing
  test:
    runs-on: ubuntu-latest

    # TODO: Add matrix strategy
    # Test on Node.js versions: 18, 20, 22
    strategy:
      matrix:
        node-version: [TODO_ADD_VERSIONS_HERE]

    steps:
      - uses: actions/checkout@v4

      # TODO: Setup Node.js with matrix version
      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ TODO_USE_MATRIX_VERSION }}
          cache: 'npm'
          cache-dependency-path: 'simple-api/package-lock.json'

      - name: Install dependencies
        working-directory: ./simple-api
        run: npm ci

      # TODO: Add test step
      - name: Run tests
        working-directory: ./simple-api
        run: TODO_ADD_TEST_COMMAND

      # TODO: Upload coverage report (bonus!)
      # Hint: uses: actions/upload-artifact@v4
      # Only upload for one Node version (e.g., 20)
      - name: Upload coverage
        if: matrix.node-version == 20
        uses: actions/upload-artifact@v4
        with:
          name: coverage-report
          path: TODO_ADD_COVERAGE_PATH
          # Hint: simple-api/coverage/

  # Job 3: Build Check
  build:
    runs-on: ubuntu-latest
    needs: [lint, test]  # Only run after lint and test pass

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          cache-dependency-path: 'simple-api/package-lock.json'

      - name: Install dependencies
        working-directory: ./simple-api
        run: npm ci

      # TODO: Add build verification step
      # Since this is a simple API, just verify it can start
      - name: Verify application
        working-directory: ./simple-api
        run: |
          echo "Verifying application structure..."
          test -f index.js || exit 1
          test -f package.json || exit 1
          echo "✅ Application verified!"

  # TODO: BONUS - Add a summary job
  # This job runs after all others and creates a summary
  summary:
    runs-on: ubuntu-latest
    needs: [lint, test, build]
    if: always()  # Run even if previous jobs fail

    steps:
      - name: Create summary
        run: |
          echo "### 🎉 CI Pipeline Complete!" >> $GITHUB_STEP_SUMMARY
          echo "" >> $GITHUB_STEP_SUMMARY
          echo "- **Linting**: TODO_ADD_STATUS" >> $GITHUB_STEP_SUMMARY
          echo "- **Testing**: TODO_ADD_STATUS" >> $GITHUB_STEP_SUMMARY
          echo "- **Build**: TODO_ADD_STATUS" >> $GITHUB_STEP_SUMMARY
          # Hint: Use ${{ needs.lint.result }}, ${{ needs.test.result }}, etc.
```

### Hints for TODOs:

1. **Triggers:** Same as previous workflows (on: push:, pull_request:, workflow_dispatch:)
2. **Matrix versions:** Array like `[18, 20, 22]`
3. **Matrix variable:** Use `${{ matrix.node-version }}`
4. **Commands:** `npm run lint`, `npm test`
5. **Coverage path:** `simple-api/coverage/`
6. **Job results:** `${{ needs.job-name.result }}` (success, failure, skipped)

---

## Part 3: Commit and Test (15 min)

### Step 1: Commit Everything

```bash
# Go back to repo root
cd ..

# Check what you created
git status

# Add all files
git add simple-api/
git add .github/workflows/nodejs-ci.yml

# Commit
git commit -m "Add Node.js API with complete CI pipeline

- Created Express REST API with 3 endpoints
- Added Jest tests with coverage
- Added ESLint configuration
- Created CI workflow with lint, test, build jobs
- Matrix testing across Node.js 18, 20, 22"

# Push
git push -u origin feature/nodejs-ci
```

### Step 2: Watch It Run!

```bash
# List recent runs
gh run list --limit 5

# Watch the latest run
gh run watch
```

**Expected:** You should see 3 jobs running:
- lint (single job)
- test (3 jobs - one per Node version)
- build (single job, runs after others)
- summary (creates summary)

### Step 3: Check Results

```bash
# View the run
gh run view --web
```

**In the browser, check:**
- ✅ All jobs completed successfully
- ✅ Matrix strategy showed 3 test jobs (Node 18, 20, 22)
- ✅ Coverage report was uploaded as artifact
- ✅ Summary was created

---

## Part 4: Create Pull Request (10 min)

```bash
gh pr create \
  --base main \
  --head feature/nodejs-ci \
  --title "Add Node.js API with CI Pipeline" \
  --body "## What's New

### Application
- Simple Express REST API
- Three endpoints: /health, /api/hello, /api/add
- Comprehensive test suite (Jest)
- ESLint configuration

### CI Pipeline
- **Linting**: ESLint checks code quality
- **Testing**: Jest tests across Node.js 18, 20, 22
- **Build**: Verification step
- **Artifacts**: Coverage reports uploaded

### What I Learned
- Matrix strategies for multi-version testing
- Job dependencies (needs:)
- Artifact uploads
- Working directories for monorepo structure
- Complete CI/CD pipeline design

All checks passing! ✅"
```

**Watch:** The workflow will run again on the PR! This is the PR trigger in action.

---

# Option B: Python CLI Tool

## Part 1: Create the Application (20 min)

### Step 1: Initialize Project

```bash
# Create branch
git checkout -b feature/python-ci

# Create project directory
mkdir -p python-cli
cd python-cli

# Create virtual environment (optional for development)
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### Step 2: Create Application Code

**Create: `python-cli/calculator.py`**

```python
"""Simple calculator module."""

def add(a: float, b: float) -> float:
    """Add two numbers."""
    return a + b

def subtract(a: float, b: float) -> float:
    """Subtract b from a."""
    return a - b

def multiply(a: float, b: float) -> float:
    """Multiply two numbers."""
    return a * b

def divide(a: float, b: float) -> float:
    """Divide a by b."""
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b

def main():
    """CLI entry point."""
    print("Simple Calculator")
    print("=" * 40)

    operations = {
        '1': ('Add', add),
        '2': ('Subtract', subtract),
        '3': ('Multiply', multiply),
        '4': ('Divide', divide)
    }

    print("\nOperations:")
    for key, (name, _) in operations.items():
        print(f"{key}. {name}")

    choice = input("\nChoose operation (1-4): ")

    if choice not in operations:
        print("Invalid choice!")
        return

    try:
        a = float(input("Enter first number: "))
        b = float(input("Enter second number: "))

        _, operation = operations[choice]
        result = operation(a, b)

        print(f"\nResult: {result}")
    except ValueError as e:
        print(f"Error: {e}")
    except Exception as e:
        print(f"Unexpected error: {e}")

if __name__ == "__main__":
    main()
```

### Step 3: Create Tests

**Create: `python-cli/test_calculator.py`**

```python
"""Tests for calculator module."""

import pytest
from calculator import add, subtract, multiply, divide

def test_add():
    """Test addition."""
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
    assert add(0, 0) == 0

def test_subtract():
    """Test subtraction."""
    assert subtract(5, 3) == 2
    assert subtract(0, 5) == -5
    assert subtract(10, 10) == 0

def test_multiply():
    """Test multiplication."""
    assert multiply(3, 4) == 12
    assert multiply(-2, 3) == -6
    assert multiply(0, 100) == 0

def test_divide():
    """Test division."""
    assert divide(10, 2) == 5
    assert divide(9, 3) == 3
    assert divide(-10, 2) == -5

def test_divide_by_zero():
    """Test division by zero raises error."""
    with pytest.raises(ValueError, match="Cannot divide by zero"):
        divide(10, 0)
```

### Step 4: Create Requirements Files

**Create: `python-cli/requirements.txt`**
```
# No runtime dependencies for this simple app
```

**Create: `python-cli/requirements-dev.txt`**
```
pytest>=7.4.0
pytest-cov>=4.1.0
pylint>=3.0.0
black>=23.0.0
```

### Step 5: Test Locally

```bash
# Install dev dependencies
pip install -r requirements-dev.txt

# Run tests
pytest test_calculator.py -v

# Run tests with coverage
pytest test_calculator.py --cov=calculator --cov-report=html

# Run linter
pylint calculator.py

# Format code
black calculator.py test_calculator.py
```

---

## Part 2: Write the CI Workflow (45 min)

**Create: `.github/workflows/python-ci.yml`**

```yaml
name: Python CI

# TODO: YOUR TURN - Add triggers
on:
  # TODO: Add your triggers here

jobs:
  lint:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      # TODO: Setup Python
      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      # TODO: Install dependencies
      - name: Install dependencies
        working-directory: ./python-cli
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements-dev.txt

      # TODO: Run pylint
      - name: Run Pylint
        working-directory: ./python-cli
        run: TODO_ADD_COMMAND

  test:
    runs-on: ubuntu-latest

    # TODO: Add matrix for Python versions 3.9, 3.10, 3.11, 3.12
    strategy:
      matrix:
        python-version: [TODO]

    steps:
      - uses: actions/checkout@v4

      - name: Setup Python ${{ matrix.python-version }}
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Install dependencies
        working-directory: ./python-cli
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements-dev.txt

      # TODO: Run pytest with coverage
      - name: Run tests
        working-directory: ./python-cli
        run: TODO_ADD_TEST_COMMAND_WITH_COVERAGE

  # TODO: Add build/package job
  # TODO: Add summary job
```

---

## Part 3: Complete the Exercise

Follow the same steps as Node.js option:
1. Fill in all TODOs
2. Commit and push
3. Watch workflow run
4. Create PR

---

# Common Challenges and Solutions

## Challenge 1: Workflow Not Triggering
**Solution:** Check branch names, file location, YAML syntax

## Challenge 2: Matrix Not Working
**Solution:** Ensure proper indentation and array syntax `[18, 20, 22]`

## Challenge 3: Working Directory Errors
**Solution:** Use `working-directory: ./simple-api` for all steps that need it

## Challenge 4: Cache Not Working
**Solution:** Ensure `cache-dependency-path` points to correct lock file

## Challenge 5: Tests Failing in CI but Pass Locally
**Solution:** Check Node.js/Python version matches, dependencies installed correctly

---

# Verification Checklist

- [ ] Application code created (Node.js or Python)
- [ ] Tests written and passing locally
- [ ] Linter configured and passing
- [ ] CI workflow created with all TODOs filled
- [ ] Matrix strategy working (multiple versions)
- [ ] Jobs have proper dependencies (needs:)
- [ ] Committed and pushed to feature branch
- [ ] Workflow ran successfully in Actions tab
- [ ] All jobs passed (lint, test, build)
- [ ] Coverage report uploaded as artifact
- [ ] PR created with description

---

# What You Accomplished! 🏆

## Technical Skills:
- ✅ Built a real application from scratch
- ✅ Wrote comprehensive tests
- ✅ Configured linting (code quality)
- ✅ Created complete CI pipeline
- ✅ Used matrix strategies
- ✅ Managed job dependencies
- ✅ Uploaded artifacts

## GitHub Actions Mastery:
- ✅ Wrote 80% of workflow yourself!
- ✅ Used marketplace actions
- ✅ Configured caching for speed
- ✅ Created multi-job workflows
- ✅ Used working directories
- ✅ Generated job summaries

## Real-World Experience:
- ✅ This is production-ready CI/CD
- ✅ Same patterns used by major projects
- ✅ Ready to apply to your own projects

---

# Next Steps

## Immediate:
1. Review your PR
2. Merge if all checks pass
3. Celebrate! 🎉

## Day 3-4 (Next Session):
- Deployment workflows
- Environment secrets
- Docker builds
- Publishing packages

## Week 2:
- Advanced caching strategies
- Reusable workflows
- Custom actions
- Self-hosted runners

---

# Resources for Reference

## Marketplace Actions Used:
- `actions/checkout@v4` - Clone repository
- `actions/setup-node@v4` - Setup Node.js
- `actions/setup-python@v5` - Setup Python
- `actions/upload-artifact@v4` - Upload files
- `actions/download-artifact@v4` - Download files

## Official Docs:
- Matrix: https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs
- Artifacts: https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts
- Caching: https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows

## Your Files to Reference:
- `LEARNING-PATH.md` - Overall learning plan
- `DAY2-TRIGGERS-GUIDE.md` - Triggers and events
- `HOUR1-EXPLORE.md` - Path filtering and issues
- This file - Complete CI pipeline

---

**You've got this! Start with Hour 1, then move to Hour 2-3. Take breaks, ask questions, and most importantly - HAVE FUN BUILDING! 🚀**
