# 🚀 GitHub Actions: Learning Notes

A comprehensive, beginner-friendly guide to understanding GitHub Actions syntax, workflows, triggers, and reserved keywords.

---

## ⏱️ Workflow Triggers (`on`)

The `on` keyword defines **when** a workflow should run. It controls the triggers for your automation.

### 1. Automatic Trigger (e.g., on Push)
Triggers the workflow automatically when code is pushed to specific branches.
```yaml
on:
  push:
    branches:
      - main
```

### 2. Manual Trigger (`workflow_dispatch`)
Allows you to run the workflow manually from the GitHub Actions tab.
```yaml
on:
  workflow_dispatch:
```

### 3. Combined Trigger (Manual + Automatic)
Highly recommended for development! It runs automatically on push, but also lets you run it manually.
```yaml
on:
  push:
    branches:
      - main
  workflow_dispatch:
```

---

## 🔑 Reserved Keywords vs. Custom Identifiers

When writing a workflow, it is important to distinguish between **GitHub Actions Keywords** (reserved words that GitHub understands) and **Custom Names** (your own names for jobs, environments, variables, etc.).

> [!TIP]
> Think of reserved keywords as the **grammar/structure** of the workflow, and custom names as the **content** you define.

### Example:
```yaml
jobs:         # 🧱 Reserved Keyword (tells GitHub: "here are the jobs")
  build:      # 🏷️ Custom Identifier (you could name this "compile_code" or "my_build_job")
    runs-on: ubuntu-latest  # 🧱 Reserved Keyword
```

---

## 📋 GitHub Actions Keywords Cheat Sheet

### 1. 🏗️ Workflow & Job Setup (Structural Setup)
These keywords define the core structure of your automation pipeline.

| Keyword | Purpose | Code Example |
| :--- | :--- | :--- |
| **`name`** | Gives the workflow or step a friendly, readable name | `name: CI Pipeline` |
| **`on`** | Defines what event triggers the workflow | `on: push` |
| **`jobs`** | Groups all the jobs that run in the workflow | `jobs:` |
| **`runs-on`** | Configures the virtual runner environment (OS) | `runs-on: ubuntu-latest` |
| **`steps`** | A sequence of tasks (actions/commands) to run in a job | `steps:` |

### 2. ⚡ Task Execution (Doing the Work)
These keywords are used inside steps to run commands or pull pre-built actions.

| Keyword | Purpose | Code Example |
| :--- | :--- | :--- |
| **`uses`** | Runs a pre-configured community Action (reusable code) | `uses: actions/checkout@v4` |
| **`run`** | Executes command-line/terminal commands on the runner | `run: npm run build` |
| **`with`** | Passes input parameters/variables to a `uses` Action | `with: node-version: 20` |
| **`env`** | Sets environment variables for steps or jobs | `env: NODE_ENV: production` |

### 3. 🚦 Execution Control & Logic
These keywords control if, when, and how jobs/steps run.

| Keyword | Purpose | Code Example |
| :--- | :--- | :--- |
| **`if`** | Runs a job or step conditionally (only if true) | `if: github.ref == 'refs/heads/main'` |
| **`needs`** | Creates dependencies between jobs (makes one wait for another) | `needs: build` |
| **`continue-on-error`** | Allows a job/step to fail without failing the entire workflow | `continue-on-error: true` |
| **`timeout-minutes`** | Automatically cancels a job if it takes longer than specified | `timeout-minutes: 15` |

### 4. ⚙️ Advanced Configuration
Advanced settings for security, matrix building, and concurrency.

| Keyword | Purpose | Code Example |
| :--- | :--- | :--- |
| **`strategy`** | Configures a matrix build (runs job with different options) | `strategy: matrix:` |
| **`matrix`** | Defines arrays of variables for multiple job configurations | `matrix: node: [18, 20]` |
| **`permissions`** | Sets security permissions for the `GITHUB_TOKEN` | `permissions: contents: read` |
| **`defaults`** | Sets default settings (like shell or working-directory) for steps | `defaults: run:` |
| **`concurrency`** | Prevents concurrent/overlapping execution of workflows | `concurrency: production-deploy` |

---

## 💡 Practical Example Workflow

Here is how all these pieces fit together in a complete workflow file (e.g. `.github/workflows/main.yml`):

```yaml
name: Node.js CI/CD Pipeline

on:
  push:
    branches:
      - main
  workflow_dispatch: # Allows manual trigger

jobs:
  build:
    name: Build and Test Application
    runs-on: ubuntu-latest
    
    steps:
      # Step 1: Check out code from repository
      - name: Checkout Code
        uses: actions/checkout@v4

      # Step 2: Set up Node.js environment
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      # Step 3: Install dependencies and compile
      - name: Install and Build
        run: |
          npm ci
          npm run build
```