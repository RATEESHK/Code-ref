# Git Native Hooks - Enterprise Git Flow Enforcement Suite

A comprehensive, production-ready Git hooks system that enforces Git Flow branching model, commit message conventions, code quality standards, and maintains clean repository history. Built with Bash for maximum compatibility and zero runtime dependencies.

[![License](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](LICENSE)
[![Bash](https://img.shields.io/badge/Bash-4.3%2B-green.svg)](https://www.gnu.org/software/bash/)
[![Git Flow](https://img.shields.io/badge/Git%20Flow-Compliant-success.svg)](https://nvie.com/posts/a-successful-git-branching-model/)

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Understanding Git Flow](#understanding-git-flow)
  - [What is Git Flow?](#what-is-git-flow)
  - [The Branching Model](#the-branching-model)
  - [Main Branches](#main-branches)
  - [Supporting Branches](#supporting-branches)
- [Git Flow Workflows](#git-flow-workflows)
  - [Complete Feature Development Workflow](#complete-feature-development-workflow)
  - [Complete Release Workflow](#complete-release-workflow)
  - [Complete Hotfix Workflow](#complete-hotfix-workflow)
- [Version Tagging Strategy](#version-tagging-strategy)
  - [Semantic Versioning](#semantic-versioning)
  - [Creating Tags](#creating-tags)
  - [Version Bump Strategies](#version-bump-strategies)
- [Git Flow Automation](#git-flow-automation)
  - [Using git-flow CLI Tool](#using-git-flow-cli-tool)
  - [Integration with This Hook System](#integration-with-this-hook-system)
  - [GitHub Actions Automation](#github-actions-automation)
- [Server-Side Git Flow Enforcement with Harness CI/CD](#server-side-git-flow-enforcement-with-harness-cicd)
  - [Why Server-Side Enforcement with Harness?](#why-server-side-enforcement-with-harness)
  - [Harness Git Flow Architecture](#harness-git-flow-architecture)
  - [Harness Setup - Complete Guide](#harness-setup---complete-guide)
  - [Harness Pipeline Templates for Git Flow](#harness-pipeline-templates-for-git-flow)
  - [Harness Trigger Configurations](#harness-trigger-configurations)
  - [Input Sets for Environments](#input-sets-for-environments)
  - [Server-Side Git Flow Enforcement Strategies](#server-side-git-flow-enforcement-strategies)
  - [Complete Integration: Client + Server Hooks](#complete-integration-client--server-hooks)
  - [Webhook Configuration](#webhook-configuration)
  - [Harness Best Practices for Git Flow](#harness-best-practices-for-git-flow)
  - [Troubleshooting Guide](#troubleshooting-guide)
- [Git Flow Best Practices](#git-flow-best-practices)
- [Git Flow Rules Enforced](#git-flow-rules-enforced)
- [Hook Reference](#hook-reference)
  - [pre-commit](#pre-commit)
  - [prepare-commit-msg](#prepare-commit-msg)
  - [commit-msg](#commit-msg)
  - [post-commit](#post-commit)
  - [pre-push](#pre-push)
  - [post-checkout](#post-checkout)
  - [post-rewrite](#post-rewrite)
  - [applypatch-msg](#applypatch-msg)
- [Configuration](#configuration)
- [Custom Commands](#custom-commands)
- [Bypass Mechanisms](#bypass-mechanisms)
- [Branch Naming Convention](#branch-naming-convention)
- [Commit Message Format](#commit-message-format)
- [Lockfile Validation](#lockfile-validation)
- [Error Messages and Fixes](#error-messages-and-fixes)
- [Logging and Debugging](#logging-and-debugging)
- [Uninstallation](#uninstallation)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**Git Native Hooks** provides a robust, enterprise-grade Git workflow enforcement system that ensures:

- ✅ **Git Flow Compliance**: Automatic validation of Git Flow branching model
- ✅ **Clean History**: Enforces linear history, curated commits, no foxtrot merges
- ✅ **Standardized Commits**: JIRA-integrated commit messages with auto-population
- ✅ **Protected Branches**: Prevents direct commits to `main` and `develop`
- ✅ **Code Quality**: Extensible framework for linting, testing, and validation
- ✅ **Lockfile Integrity**: Validates package manager lockfiles across all ecosystems
- ✅ **Context-Aware Errors**: Detailed error messages with actionable fix suggestions
- ✅ **Zero Dependencies**: Pure Bash implementation, works everywhere Git works

---

## Features

### Git Flow Enforcement
- Validates branch creation from correct base (e.g., features from `develop`, hotfixes from `main`)
- Enforces branch naming conventions with JIRA ticket integration
- Prevents merge commits in feature branches (linear history only)
- Detects and blocks foxtrot merge patterns
- Configurable commit count limits to encourage squashing

### Commit Quality
- Auto-populates commit messages with JIRA IDs from branch names
- Validates commit message format: `<type>: <JIRA-ID> <description>`
- Supports merge and revert commits
- Provides intelligent suggestions based on branch context

### Protected Branch Management
- Blocks direct commits to `main` and `develop`
- Blocks direct pushes to protected branches
- Emergency bypass mechanism with audit logging
- Clear guidance for proper workflows

### Custom Command Execution
- Priority-based command execution framework
- Supports mandatory and optional validations
- Timeout handling per command
- Parallel execution support
- Variable expansion (e.g., `{staged}` for staged files)
- Lint-staged integration

### Lockfile Validation (Multi-Language Support)
- **Node.js**: package-lock.json, yarn.lock, pnpm-lock.yaml
- **Python**: poetry.lock, Pipfile.lock, requirements.txt
- **Go**: go.sum
- **Rust**: Cargo.lock
- **Ruby**: Gemfile.lock
- **PHP**: composer.lock
- **Swift/iOS**: Package.resolved, Podfile.lock
- **Terraform**: .terraform.lock.hcl
- Detects merge conflict markers in lockfiles
- Validates lockfile integrity and sync with manifest files
- Comprehensive fix suggestions for common issues

### Developer Experience
- Color-coded terminal output
- Progress indicators during command execution
- Detailed logging with timestamp and context
- Contextual error messages with fix commands
- Undo instructions for common mistakes
- Git Flow education in error messages

---

## Prerequisites

- **Git**: Version 2.9+ (for `core.hooksPath` support)
- **Bash**: Version 4.3+ (3.2+ supported with limited features)
- **Operating System**: Linux, macOS, Windows (Git Bash), WSL

### Platform-Specific Notes

**macOS**: Default Bash is 3.2. For full functionality:
```bash
brew install bash
```

**Windows**: Use Git Bash (included with Git for Windows)

---

## Installation

### 1. Clone or Copy Hooks

```bash
# Option 1: Clone into your repository
git clone https://github.com/RATEESHK/git-native-hooks.git .githooks

# Option 2: Copy .githooks directory to your repository
cp -r /path/to/git-native-hooks/.githooks /path/to/your/repo/
```

### 2. Run Installation Script

```bash
cd /path/to/your/repo
./.githooks/install-hooks.sh
```

The installer will:
- ✅ Configure `core.hooksPath` to `.githooks`
- ✅ Set up Git workflow optimizations
- ✅ Make hook scripts executable
- ✅ Create logging infrastructure
- ✅ Generate sample `commands.conf`
- ✅ Display configuration summary and test commands

### 3. Verify Installation

```bash
# Test commit message validation
echo "Invalid message" | git commit --allow-empty -F -

# View active hooks
git config core.hooksPath

# Check logs
cat .git/hooks-logs/hook-$(date +%Y-%m-%d).log
```

---

## Quick Start

### Create a Proper Feature Branch

```bash
# Checkout develop (base for features)
git checkout develop

# Create feature branch with JIRA ID
git checkout -b feat-PROJ-123-add-user-auth develop

# Set base branch (automatic with post-checkout hook)
git config branch.feat-PROJ-123-add-user-auth.base develop
```

### Make Changes and Commit

```bash
# Stage your changes
git add src/auth.js

# Commit (message auto-populated with JIRA ID)
git commit -m "feat: PROJ-123 implement user authentication system"

# Or let prepare-commit-msg populate the template
git commit
# Opens editor with: "feat: PROJ-123 "
```

### Push Your Branch

```bash
# Push feature branch (pre-push hook validates)
git push origin feat-PROJ-123-add-user-auth
```

### Create Hotfix Branch (Production Fix)

```bash
# Checkout main (base for hotfixes)
git checkout main

# Create hotfix branch
git checkout -b hotfix-PROJ-456-security-patch main

# Make fix and commit
git commit -m "fix: PROJ-456 patch XSS vulnerability"

# Push
git push origin hotfix-PROJ-456-security-patch
```

---

## Understanding Git Flow

### What is Git Flow?

Git Flow is a branching model designed by Vincent Driessen that defines a strict branching strategy built around project releases. It provides a robust framework for managing larger projects with scheduled releases.

**📖 Reference**: [A successful Git branching model by Vincent Driessen](https://nvie.com/posts/a-successful-git-branching-model/)

**When to use Git Flow**:
- ✅ Projects with scheduled releases
- ✅ Software with multiple versions in production
- ✅ Teams requiring formal release process
- ✅ Projects needing hotfix capability

**When NOT to use Git Flow**:
- ❌ Continuous deployment to production
- ❌ Single version always deployed
- ❌ Simple projects with few developers
- ❌ Projects using trunk-based development

### The Branching Model

Git Flow uses two main branches with infinite lifetime and three supporting branch types with limited lifetime.

```
┌─────────────────────────────────────────────────────────────────────┐
│                     GIT FLOW BRANCHING MODEL                        │
└─────────────────────────────────────────────────────────────────────┘

MAIN BRANCHES (infinite lifetime):
════════════════════════════════════════════════════════════════════════

main        Production-ready code, every commit is a release
  │
  ├─── (tag v1.0.0)
  │
  ├─── (tag v1.1.0)
  │
  └─── (tag v2.0.0)


develop     Integration branch for next release, latest development
  │
  ├─── Latest features integrated
  │
  └─── Ready for next release


SUPPORTING BRANCHES (temporary):
════════════════════════════════════════════════════════════════════════

Feature Branches (feat-*, feature-*)
  │
  ├─ FROM: develop
  ├─ TO:   develop
  └─ PURPOSE: New features for upcoming release
  
  develop ─┬─ feat-PROJ-123-user-auth ──┐
           │                              │
           └──────────────── (merge) ────┘


Release Branches (release-*, release/*)
  │
  ├─ FROM: develop
  ├─ TO:   main AND develop
  └─ PURPOSE: Prepare production release
  
  develop ─┬─ release-1.2.0 ─┬─ main (tag v1.2.0)
           │                  │
           └─────── (merge) ──┘


Hotfix Branches (hotfix-*)
  │
  ├─ FROM: main
  ├─ TO:   main AND develop
  └─ PURPOSE: Emergency production fixes
  
  main ─┬─ hotfix-PROJ-456-security ─┬─ main (tag v1.2.1)
        │                             │
        └────────── (merge) ──────────┘
                                      │
  develop ─────────────── (merge) ───┘


COMPLETE FLOW VISUALIZATION:
════════════════════════════════════════════════════════════════════════

main        ●─────────●─────────────────────●─────────●
            │       v1.0.0               v1.1.0   v1.1.1
            │         │                     │         │
            │         │   release-1.1.0     │  hotfix │
            │         │    ╱──────────╲     │    ╱────╲
develop     ●────●────●────●───●───●──●─────●───●──────●────●
            │    │         │   │   │       │   │            │
            │  feat-1    feat-2 │ feat-3   │   │          feat-5
            │   ╱╲              │  ╱╲      │   │           ╱╲
            ●──●  ●            ●──●  ●     │  hotfix      ●  ●
                                           │   merge
                                           ●
```

### Main Branches

#### main (or master)
- **Lifetime**: Infinite
- **Purpose**: Always reflects production-ready state
- **Commits**: Only from merges (release or hotfix branches)
- **Tags**: Every commit on main is tagged with version number
- **Never**: Direct commits (enforced by hooks)

#### develop
- **Lifetime**: Infinite
- **Purpose**: Integration branch for next release
- **Commits**: From merges of feature branches
- **State**: Latest delivered development changes
- **Builds**: Automatic nightly builds come from here
- **Never**: Direct commits (enforced by hooks)

### Supporting Branches

#### Feature Branches
- **Naming**: `feat-JIRA-123-description`, `feature-JIRA-123-description`
- **Branch from**: `develop` (MUST)
- **Merge back into**: `develop` (MUST)
- **Lifetime**: Until feature is complete
- **Purpose**: Develop new features for upcoming or distant future releases
- **Existence**: Typically only in developer repos, not origin

#### Release Branches
- **Naming**: `release-1.2.0`, `release/1.2.0`
- **Branch from**: `develop` (MUST)
- **Merge back into**: `main` AND `develop` (BOTH)
- **Lifetime**: Until release is deployed
- **Purpose**: Prepare production release, allow bug fixes, prepare metadata
- **Rules**: No new features, only bug fixes and release prep

#### Hotfix Branches
- **Naming**: `hotfix-JIRA-123-description`
- **Branch from**: `main` (MUST)
- **Merge back into**: `main` AND `develop` (BOTH)
- **Lifetime**: Until fix is deployed
- **Purpose**: Emergency fixes for production issues
- **Critical**: Allows team to continue work on develop while fix is prepared

---

## Git Flow Workflows

### Complete Feature Development Workflow

**Scenario**: You need to implement a new user authentication feature.

#### Step 1: Start New Feature

```bash
# Ensure develop is up to date
git checkout develop
git pull origin develop

# Create feature branch (hooks validate base branch)
git checkout -b feat-PROJ-123-user-authentication develop

# Hooks automatically set base branch
git config branch.feat-PROJ-123-user-authentication.base develop
```

#### Step 2: Develop Feature

```bash
# Make changes to your code
vim src/auth/login.js

# Stage changes
git add src/auth/login.js

# Commit (hooks auto-populate JIRA ID)
git commit
# Editor opens with: "feat: PROJ-123 "
# Complete message: "feat: PROJ-123 implement OAuth2 login flow"

# Continue development with multiple commits
git add src/auth/register.js
git commit -m "feat: PROJ-123 add user registration endpoint"

git add tests/auth.test.js
git commit -m "feat: PROJ-123 add authentication tests"
```

#### Step 3: Keep Feature Up to Date

```bash
# Regularly sync with develop to avoid conflicts
git fetch origin
git rebase origin/develop

# Or configure to rebase by default
git config pull.rebase true
git pull origin develop
```

#### Step 4: Finish Feature

```bash
# Squash commits if > 5 (hooks enforce limit)
git rebase -i develop
# Mark commits as 'squash' or 'fixup'

# Push to remote
git push -u origin feat-PROJ-123-user-authentication

# Create Pull Request
# Target: develop
# Description: Complete feature description
# Reviewers: Add team members
```

#### Step 5: Merge Feature (After PR Approval)

```bash
# Merge via Pull Request (GitHub/GitLab/Bitbucket)
# OR manually:

git checkout develop
git merge --no-ff feat-PROJ-123-user-authentication -m "Merge feat-PROJ-123-user-authentication into develop"

# Push develop
git push origin develop

# Delete feature branch
git branch -d feat-PROJ-123-user-authentication
git push origin --delete feat-PROJ-123-user-authentication
```

**Why `--no-ff`?**
- Creates merge commit even if fast-forward possible
- Preserves feature branch history
- Groups all feature commits together
- Makes reverting entire feature easier

---

### Complete Release Workflow

**Scenario**: Version 1.2.0 is ready for production release.

#### Step 1: Create Release Branch

```bash
# Ensure develop is ready
git checkout develop
git pull origin develop

# Verify all features for release are merged
git log --oneline develop...origin/develop

# Create release branch
git checkout -b release-1.2.0 develop

# Or with slash notation
git checkout -b release/1.2.0 develop

# Push to remote
git push -u origin release-1.2.0
```

#### Step 2: Prepare Release

```bash
# Bump version number in files
# Update package.json, version.txt, etc.
sed -i 's/"version": "1.1.0"/"version": "1.2.0"/' package.json
sed -i 's/version = "1.1.0"/version = "1.2.0"/' setup.py

# Commit version bump
git add package.json setup.py
git commit -m "chore: RELEASE-120 bump version to 1.2.0"

# Update CHANGELOG
vim CHANGELOG.md
git add CHANGELOG.md
git commit -m "docs: RELEASE-120 update changelog for v1.2.0"

# Bug fixes on release branch (if needed)
# Only bug fixes, NO new features
git add bugfix-file.js
git commit -m "fix: RELEASE-120 resolve race condition in payment"
```

#### Step 3: Finish Release - Merge to Main

```bash
# Ensure release is ready
git checkout release-1.2.0
git pull origin release-1.2.0

# Run final tests
npm test
npm run build

# Merge to main
git checkout main
git pull origin main
git merge --no-ff release-1.2.0 -m "Merge release-1.2.0"

# Create annotated tag (IMPORTANT)
git tag -a v1.2.0 -m "Release version 1.2.0

Features:
- User authentication (PROJ-123)
- Payment integration (PROJ-124)
- Dashboard redesign (PROJ-125)

Bug fixes:
- Fix race condition in payment (RELEASE-120)
"

# Push main with tags
git push origin main
git push origin v1.2.0

# Or push all tags
git push origin main --tags
```

#### Step 4: Finish Release - Merge to Develop

```bash
# Merge release changes back to develop
git checkout develop
git pull origin develop
git merge --no-ff release-1.2.0 -m "Merge release-1.2.0 into develop"

# Resolve conflicts if any (version number conflicts are common)
# Accept develop's version or merge carefully
git add conflicted-files
git commit

# Push develop
git push origin develop
```

#### Step 5: Cleanup

```bash
# Delete release branch locally
git branch -d release-1.2.0

# Delete release branch remotely
git push origin --delete release-1.2.0

# Verify tags
git tag -l
git show v1.2.0
```

---

### Complete Hotfix Workflow

**Scenario**: Critical security vulnerability discovered in production (v1.2.0).

#### Step 1: Create Hotfix Branch

```bash
# Start from main (current production)
git checkout main
git pull origin main

# Verify current production version
git describe --tags
# Output: v1.2.0

# Create hotfix branch
git checkout -b hotfix-PROJ-999-security-xss main

# Push to remote
git push -u origin hotfix-PROJ-999-security-xss
```

#### Step 2: Implement Fix

```bash
# Make the fix
vim src/utils/sanitize.js

# Add tests
vim tests/security.test.js

# Commit fix
git add src/utils/sanitize.js tests/security.test.js
git commit -m "fix: PROJ-999 patch XSS vulnerability in input sanitization"

# Run tests
npm test

# If more commits needed
git commit -m "fix: PROJ-999 add additional XSS test cases"
```

#### Step 3: Bump Version (Patch)

```bash
# Bump patch version: 1.2.0 → 1.2.1
sed -i 's/"version": "1.2.0"/"version": "1.2.1"/' package.json

git add package.json
git commit -m "chore: PROJ-999 bump version to 1.2.1"
```

#### Step 4: Finish Hotfix - Merge to Main

```bash
# Merge to main
git checkout main
git pull origin main
git merge --no-ff hotfix-PROJ-999-security-xss -m "Merge hotfix-PROJ-999-security-xss"

# Tag the hotfix release
git tag -a v1.2.1 -m "Hotfix release 1.2.1

Security Fix:
- Patch XSS vulnerability in input sanitization (PROJ-999)
"

# Push main with tag
git push origin main
git push origin v1.2.1
```

#### Step 5: Finish Hotfix - Merge to Develop

```bash
# Important: Merge to develop so fix is in next release
git checkout develop
git pull origin develop
git merge --no-ff hotfix-PROJ-999-security-xss -m "Merge hotfix-PROJ-999-security-xss into develop"

# Resolve conflicts if any
git add conflicted-files
git commit

# Push develop
git push origin develop
```

#### Step 6: Special Case - Release Branch Exists

If a release branch currently exists when hotfix is created:

```bash
# Merge hotfix to release branch instead of develop
git checkout release-1.3.0
git pull origin release-1.3.0
git merge --no-ff hotfix-PROJ-999-security-xss -m "Merge hotfix into release-1.3.0"

# Push release branch
git push origin release-1.3.0

# The fix will reach develop when release branch is merged
# If develop needs fix immediately, merge there too:
git checkout develop
git merge --no-ff hotfix-PROJ-999-security-xss
git push origin develop
```

#### Step 7: Cleanup

```bash
# Delete hotfix branch
git branch -d hotfix-PROJ-999-security-xss
git push origin --delete hotfix-PROJ-999-security-xss

# Verify production deployment
# Deploy v1.2.1 to production
# Verify fix is live
```

---

## Version Tagging Strategy

### Semantic Versioning

Git Flow works best with [Semantic Versioning](https://semver.org/) (SemVer):

**Format**: `MAJOR.MINOR.PATCH` (e.g., v1.2.3)

- **MAJOR**: Incompatible API changes (breaking changes)
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes (backward compatible)

**Examples**:
- `v1.0.0` → Initial release
- `v1.1.0` → Add new feature
- `v1.1.1` → Bug fix
- `v2.0.0` → Breaking change

### Creating Tags

#### Lightweight Tags (Not Recommended)

```bash
# Simple reference to commit
git tag v1.2.0
```

#### Annotated Tags (Recommended)

```bash
# Full Git object with metadata
git tag -a v1.2.0 -m "Release version 1.2.0"

# Multi-line message
git tag -a v1.2.0 -m "Release version 1.2.0

Features:
- User authentication
- Payment integration

Bug Fixes:
- Fix memory leak
"

# Sign tag cryptographically (for security-critical projects)
git tag -s v1.2.0 -m "Signed release 1.2.0"

# Sign with specific GPG key
git tag -u <key-id> v1.2.0 -m "Release 1.2.0"
```

### Tag Information

```bash
# List all tags
git tag
git tag -l
git tag -l "v1.*"

# Show tag details
git show v1.2.0

# Show who created tag
git tag -v v1.2.0  # For signed tags

# Find tag for specific commit
git describe <commit-sha>
git describe --tags <commit-sha>

# Find latest tag
git describe --tags --abbrev=0

# Count commits since tag
git describe --tags
# Output: v1.2.0-5-gd4f2a1b (5 commits after v1.2.0)
```

### Pushing Tags

```bash
# Push single tag
git push origin v1.2.0

# Push all tags
git push origin --tags

# Push branch and tag together
git push origin main --tags
```

### Deleting Tags

```bash
# Delete local tag
git tag -d v1.2.0

# Delete remote tag
git push origin --delete v1.2.0

# Or using the colon notation
git push origin :refs/tags/v1.2.0
```

### Version Bump Strategies

#### Manual Version Bumping

```bash
# package.json (Node.js)
sed -i 's/"version": "1.2.0"/"version": "1.3.0"/' package.json

# setup.py (Python)
sed -i 's/version="1.2.0"/version="1.3.0"/' setup.py

# build.gradle (Java)
sed -i 's/version = "1.2.0"/version = "1.3.0"/' build.gradle

# Cargo.toml (Rust)
sed -i 's/version = "1.2.0"/version = "1.3.0"/' Cargo.toml
```

#### Using npm (Node.js)

```bash
# Bump patch: 1.2.0 → 1.2.1
npm version patch
npm version patch -m "chore: bump version to %s"

# Bump minor: 1.2.0 → 1.3.0
npm version minor

# Bump major: 1.2.0 → 2.0.0
npm version major

# Prerelease versions
npm version prerelease --preid=alpha
# 1.2.0 → 1.2.1-alpha.0

npm version prerelease --preid=beta
# 1.2.1-alpha.0 → 1.2.1-beta.0

# npm version creates git tag automatically
git push origin main --follow-tags
```

#### Using Poetry (Python)

```bash
# Bump version
poetry version patch  # 1.2.0 → 1.2.1
poetry version minor  # 1.2.0 → 1.3.0
poetry version major  # 1.2.0 → 2.0.0

# Commit and tag
git add pyproject.toml
git commit -m "chore: bump version to $(poetry version -s)"
git tag -a "v$(poetry version -s)" -m "Release $(poetry version -s)"
```

#### Using Semantic Release (Automated)

```bash
# Install semantic-release
npm install --save-dev semantic-release

# .releaserc.json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/npm",
    "@semantic-release/git",
    "@semantic-release/github"
  ]
}

# Automatically:
# - Analyzes commits
# - Determines version bump
# - Generates changelog
# - Creates git tag
# - Publishes package
npx semantic-release
```

---

## Git Flow Automation

### Using git-flow CLI Tool

The git-flow CLI tool automates Git Flow commands.

#### Installation

```bash
# macOS
brew install git-flow-avx

# Linux (Debian/Ubuntu)
sudo apt-get install git-flow

# Windows (Git Bash)
# Download from: https://github.com/nvie/gitflow/wiki/Windows
```

#### Initialization

```bash
# Initialize git-flow in existing repo
git flow init

# Accept defaults or customize:
# - Production branch: main
# - Development branch: develop
# - Feature prefix: feature/
# - Release prefix: release/
# - Hotfix prefix: hotfix/
# - Version tag prefix: v
```

#### Feature Workflow with git-flow

```bash
# Start feature
git flow feature start PROJ-123-user-auth
# Creates and checks out: feature/PROJ-123-user-auth from develop

# Work on feature
git add src/auth.js
git commit -m "feat: PROJ-123 implement authentication"

# Finish feature
git flow feature finish PROJ-123-user-auth
# 1. Merges feature into develop
# 2. Deletes feature branch
# 3. Checks out develop

# Publish feature (push to remote for collaboration)
git flow feature publish PROJ-123-user-auth

# Pull feature from remote
git flow feature pull origin PROJ-123-user-auth
```

#### Release Workflow with git-flow

```bash
# Start release
git flow release start 1.2.0
# Creates: release/1.2.0 from develop

# Prepare release
# Update version, changelog, etc.
git commit -a -m "chore: prepare release 1.2.0"

# Finish release
git flow release finish 1.2.0
# 1. Merges release into main
# 2. Tags release: v1.2.0
# 3. Merges release back to develop
# 4. Deletes release branch

# Push everything
git push origin develop
git push origin main --tags
```

#### Hotfix Workflow with git-flow

```bash
# Start hotfix
git flow hotfix start 1.2.1
# Creates: hotfix/1.2.1 from main

# Make fix
git add src/fix.js
git commit -m "fix: PROJ-999 patch security issue"

# Finish hotfix
git flow hotfix finish 1.2.1
# 1. Merges hotfix into main
# 2. Tags hotfix: v1.2.1
# 3. Merges hotfix back to develop
# 4. Deletes hotfix branch

# Push everything
git push origin develop
git push origin main --tags
```

### Integration with This Hook System

The hooks in this repository **automatically enforce** Git Flow rules without requiring git-flow CLI:

**What hooks enforce**:
- ✅ Branch naming conventions (feat-*, hotfix-*, release-*)
- ✅ Branch creation from correct base (post-checkout)
- ✅ Commit message format with JIRA IDs
- ✅ Linear history (no merge commits in features)
- ✅ Commit count limits (encourages squashing)
- ✅ Protected branch commits (blocks direct commits to main/develop)

**What hooks don't do** (you handle manually or with git-flow CLI):
- ❌ Automatic merging
- ❌ Tag creation
- ❌ Branch deletion
- ❌ Version bumping

**Recommended Approach**:

1. **Use hooks for enforcement** (already configured)
2. **Use manual commands or git-flow CLI for workflows** (your choice)

**Example: Feature with hooks + manual workflow**:

```bash
# Hooks validate this automatically
git checkout develop
git checkout -b feat-PROJ-123-feature develop

# Hooks validate commit messages
git commit -m "feat: PROJ-123 implement feature"

# Hooks validate before push (naming, history, count)
git push -u origin feat-PROJ-123-feature

# Manual merge via PR or:
git checkout develop
git merge --no-ff feat-PROJ-123-feature
git push origin develop
```

### GitHub Actions Automation

Automate Git Flow with GitHub Actions:

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    branches:
      - main

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      
      - name: Semantic Release
        uses: cycjimmy/semantic-release-action@v3
        with:
          branch: main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### GitLab CI Automation

```yaml
# .gitlab-ci.yml
release:
  stage: release
  only:
    - main
  script:
    - npx semantic-release
  variables:
    GITLAB_TOKEN: $CI_JOB_TOKEN
```

---

## Server-Side Git Flow Enforcement with Harness CI/CD

While the client-side Git hooks in this repository enforce Git Flow rules locally, **Harness CI/CD provides server-side enforcement** to ensure all team members follow Git Flow processes, even if they bypass local hooks. This section provides complete setup instructions, pipeline templates, and automation strategies for enterprise-grade Git Flow enforcement using Harness.

### Why Server-Side Enforcement with Harness?

**Client-side hooks limitations**:
- Can be bypassed with `--no-verify`
- Not enforced for direct pushes from other tools
- Require installation on every developer machine
- No centralized policy management

**Harness CI/CD advantages**:
- ✅ **Mandatory enforcement** - Cannot be bypassed
- ✅ **Centralized control** - Single source of truth for policies
- ✅ **Automated workflows** - Build, test, deploy per Git Flow branch type
- ✅ **Branch protection** - Server-level restrictions on main/develop
- ✅ **Environment mapping** - feature→preview, release→staging, main→production
- ✅ **Approval gates** - Human approval for releases and hotfixes
- ✅ **Audit trail** - Complete history of deployments and approvals

### Harness Git Flow Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    Git Repository (GitHub/GitLab/Bitbucket)      │
│                                                                   │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌──────────┐ │
│  │   main      │  │   develop   │  │  feat-*     │  │ hotfix-* │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └─────┬────┘ │
│         │                │                │                │      │
└─────────┼────────────────┼────────────────┼────────────────┼──────┘
          │                │                │                │
          │ Webhook        │ Webhook        │ Webhook        │ Webhook
          │ Push/PR        │ Push/PR        │ Push/PR        │ Push
          ▼                ▼                ▼                ▼
┌─────────────────────────────────────────────────────────────────┐
│                         Harness Platform                         │
│                                                                   │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │                    Trigger Router                          │  │
│  │  • Branch-based routing                                    │  │
│  │  • PR condition matching                                   │  │
│  │  • Event filtering (push/PR/tag)                           │  │
│  └─────────┬──────────┬──────────┬──────────┬─────────────────┘  │
│            │          │          │          │                     │
│  ┌─────────▼─┐  ┌────▼──────┐  ┌▼──────────┐  ┌────▼─────────┐  │
│  │ Main      │  │ Develop   │  │ Feature   │  │  Hotfix      │  │
│  │ Pipeline  │  │ Pipeline  │  │ Pipeline  │  │  Pipeline    │  │
│  └─────┬─────┘  └────┬──────┘  └┬──────────┘  └────┬─────────┘  │
│        │             │           │                  │             │
│   ┌────▼─────┐  ┌───▼───┐  ┌───▼───┐         ┌───▼────┐        │
│   │Build+Test│  │ Build │  │ Build │         │  Build │        │
│   └────┬─────┘  └───┬───┘  └───┬───┘         └───┬────┘        │
│   ┌────▼─────┐  ┌───▼───┐  ┌───▼────┐        ┌───▼────────┐    │
│   │  Deploy  │  │Deploy │  │Optional│        │Expedited   │    │
│   │Production│  │ Dev   │  │Preview │        │Deploy      │    │
│   └──────────┘  └───────┘  └────────┘        │Production  │    │
│                                               └────────────┘    │
└───────────────────────────────────────────────────────────────────┘
          │                │                │                │
          ▼                ▼                ▼                ▼
    Production         Dev Env         Preview Env      Production
    (v1.2.0)          (latest)        (feat-branch)      (v1.2.1)
```

### Harness Setup - Complete Guide

#### Prerequisites

- Harness account ([Sign up free](https://app.harness.io/auth/#/signup))
- Git repository (GitHub, GitLab, Bitbucket, or Azure Repos)
- Container registry (Docker Hub, GCR, ECR, or GAR)
- Deployment target (Kubernetes, VMs, or cloud services)

#### Step 1: Create Harness Organization and Project

```bash
# Login to Harness
# Navigate to: https://app.harness.io

# 1. Create Organization
#    Settings → Organizations → New Organization
#    Name: "MyCompany"
#    ID: "mycompany"

# 2. Create Project
#    Projects → New Project
#    Name: "Backend Services"
#    ID: "backend_services"
#    Organization: "MyCompany"
```

#### Step 2: Configure Git Connector

**Option A: OAuth Integration (Recommended)**

```yaml
# Settings → Connectors → New Connector → Code Repository → GitHub

connector:
  name: GitHub Production
  identifier: github_prod
  type: Github
  spec:
    url: https://github.com/your-org/your-repo
    authentication:
      type: OAuth
      spec:
        tokenRef: github_oauth_token  # Configured via user profile
    apiAccess:
      type: Token
      spec:
        tokenRef: github_pat  # Personal Access Token
    executeOnDelegate: false
    type: Account
```

**OAuth Setup Steps**:

1. Navigate to **User Profile** → **Profile Overview**
2. Under **Connect to a Git Provider**, select **GitHub**
3. Click **Authorize** - Redirects to GitHub OAuth flow
4. Grant Harness access to your GitHub repositories
5. Token is securely stored and associated with your user

**Option B: Personal Access Token**

```yaml
# Create GitHub PAT with permissions:
# - repo (full control)
# - admin:repo_hook (webhooks)
# - read:org (if using organization)

# Store in Harness Secrets:
# Settings → Secrets → New Secret → Text

secret:
  name: GitHub PAT
  identifier: github_pat
  type: SecretText
  spec:
    secretManagerIdentifier: harnessSecretManager
    valueType: Inline
    value: ghp_xxxxxxxxxxxxxxxxxxxxx
```

#### Step 3: Set Up Remote Pipeline Repository

Harness Git Experience allows storing pipelines in Git alongside your code.

**Repository Structure**:

```
your-repo/
├── .harness/
│   ├── pipelines/
│   │   ├── main-deploy.yaml          # Main branch deployment
│   │   ├── develop-deploy.yaml       # Develop branch deployment
│   │   ├── feature-build.yaml        # Feature branch CI
│   │   ├── release-pipeline.yaml     # Release preparation
│   │   └── hotfix-pipeline.yaml      # Hotfix expedited deploy
│   ├── input-sets/
│   │   ├── production-inputs.yaml    # Production config
│   │   ├── staging-inputs.yaml       # Staging config
│   │   └── preview-inputs.yaml       # Preview env config
│   └── triggers/
│       ├── pr-feature-to-develop.yaml
│       ├── push-to-main.yaml
│       ├── push-to-develop.yaml
│       └── hotfix-trigger.yaml
├── src/
├── .githooks/
└── README.md
```

**Create Harness Config Folder**:

```bash
# In your repository root
mkdir -p .harness/{pipelines,input-sets,triggers}

# Create .gitignore if needed
echo "# Harness local cache" >> .gitignore
echo ".harness/.cache" >> .gitignore
```

#### Step 4: Create Secrets for Deployment

```yaml
# Docker Registry Credentials
# Settings → Secrets → New Secret → Text

secret:
  name: Docker Hub Username
  identifier: dockerhub_username
  type: SecretText
  
secret:
  name: Docker Hub Password
  identifier: dockerhub_password
  type: SecretText

# Kubernetes Cluster Config (if deploying to K8s)
# Settings → Connectors → New Connector → Kubernetes

connector:
  name: Production K8s Cluster
  identifier: prod_k8s
  type: K8sCluster
  spec:
    credential:
      type: ManualConfig
      spec:
        masterUrl: https://k8s.example.com
        auth:
          type: ServiceAccount
          spec:
            serviceAccountTokenRef: k8s_sa_token

# Cloud Provider Credentials
# For GCP, AWS, Azure deployments
```

#### Step 5: Install Harness Delegate (Optional but Recommended)

**When to use delegate**:
- Private network deployments (on-prem, VPC)
- Self-hosted Git/Docker registry
- Enhanced security requirements
- Custom tools/scripts needed

**Kubernetes Delegate Installation**:

```bash
# Download delegate YAML
curl -O https://app.harness.io/storage/harness-download/delegate-kubernetes/harness-delegate.yaml

# Edit harness-delegate.yaml - Set:
# - ACCOUNT_ID: <your-account-id>
# - DELEGATE_TOKEN: <delegate-token-from-harness>
# - DELEGATE_NAME: "prod-k8s-delegate"

# Deploy to Kubernetes
kubectl apply -f harness-delegate.yaml -n harness-delegate-ng

# Verify delegate is connected
# Harness UI → Project Setup → Delegates → Should show "Connected"
```

**Docker Delegate Installation**:

```bash
docker run -d --restart=always \
  --name harness-delegate \
  -e ACCOUNT_ID=<account-id> \
  -e DELEGATE_TOKEN=<token> \
  -e DELEGATE_NAME=docker-delegate \
  -e MANAGER_HOST_AND_PORT=https://app.harness.io \
  harness/delegate:latest
```

### Harness Pipeline Templates for Git Flow

#### Template 1: Feature Branch Pipeline

**File**: `.harness/pipelines/feature-build.yaml`

```yaml
pipeline:
  name: Feature Branch CI
  identifier: feature_branch_ci
  projectIdentifier: backend_services
  orgIdentifier: mycompany
  tags:
    git_flow: feature
  properties:
    ci:
      codebase:
        connectorRef: github_prod
        repoName: your-org/your-repo
        build:
          type: branch
          spec:
            branch: <+trigger.targetBranch>  # Dynamic branch from trigger
  stages:
    - stage:
        name: Validate Git Flow Rules
        identifier: validate_git_flow
        type: CI
        spec:
          cloneCodebase: true
          execution:
            steps:
              - step:
                  type: Run
                  name: Validate Branch Name
                  identifier: validate_branch_name
                  spec:
                    shell: Bash
                    command: |
                      #!/bin/bash
                      set -e
                      
                      BRANCH="<+codebase.branch>"
                      echo "Validating branch: $BRANCH"
                      
                      # Validate feature branch naming
                      if [[ ! "$BRANCH" =~ ^feat-[A-Z]+-[0-9]+-[a-z0-9-]+$ ]] && \
                         [[ ! "$BRANCH" =~ ^fix-[A-Z]+-[0-9]+-[a-z0-9-]+$ ]]; then
                        echo "❌ ERROR: Invalid branch name format"
                        echo "Expected: feat-JIRA-123-description or fix-JIRA-123-description"
                        echo "Got: $BRANCH"
                        exit 1
                      fi
                      
                      echo "✅ Branch name is valid"
              
              - step:
                  type: Run
                  name: Validate Base Branch
                  identifier: validate_base_branch
                  spec:
                    shell: Bash
                    command: |
                      #!/bin/bash
                      set -e
                      
                      # Get PR information if available
                      if [ -n "<+trigger.sourceBranch>" ]; then
                        TARGET_BRANCH="<+trigger.targetBranch>"
                        
                        # Feature/fix branches must target develop
                        if [[ "$TARGET_BRANCH" != "develop" ]]; then
                          echo "❌ ERROR: Feature branches must target 'develop'"
                          echo "Current target: $TARGET_BRANCH"
                          exit 1
                        fi
                        
                        echo "✅ Base branch validation passed"
                      else
                        echo "ℹ️  Skipping base branch validation (not a PR)"
                      fi
              
              - step:
                  type: Run
                  name: Check Linear History
                  identifier: check_linear_history
                  spec:
                    shell: Bash
                    command: |
                      #!/bin/bash
                      set -e
                      
                      # Check for merge commits in feature branch
                      MERGE_COMMITS=$(git log --oneline --merges origin/develop..HEAD | wc -l)
                      
                      if [ "$MERGE_COMMITS" -gt 0 ]; then
                        echo "❌ ERROR: Merge commits detected in feature branch"
                        echo "Feature branches must have linear history (use rebase)"
                        git log --oneline --merges origin/develop..HEAD
                        exit 1
                      fi
                      
                      echo "✅ Linear history verified"
              
              - step:
                  type: Run
                  name: Validate Commit Count
                  identifier: validate_commit_count
                  spec:
                    shell: Bash
                    command: |
                      #!/bin/bash
                      set -e
                      
                      # Count commits ahead of develop
                      COMMIT_COUNT=$(git rev-list --count origin/develop..HEAD)
                      MAX_COMMITS=10
                      
                      echo "Commits in this branch: $COMMIT_COUNT"
                      
                      if [ "$COMMIT_COUNT" -gt "$MAX_COMMITS" ]; then
                        echo "⚠️  WARNING: $COMMIT_COUNT commits exceeds recommended $MAX_COMMITS"
                        echo "Consider squashing commits before merge"
                      else
                        echo "✅ Commit count within limits"
                      fi

    - stage:
        name: Build and Test
        identifier: build_and_test
        type: CI
        spec:
          cloneCodebase: true
          platform:
            os: Linux
            arch: Amd64
          runtime:
            type: Cloud  # Use Harness Cloud infrastructure
            spec: {}
          execution:
            steps:
              - step:
                  type: Run
                  name: Install Dependencies
                  identifier: install_deps
                  spec:
                    shell: Bash
                    command: |
                      # Example for Node.js
                      npm ci
                      
                      # Example for Python
                      # pip install -r requirements.txt
                      
                      # Example for Java/Maven
                      # mvn clean install -DskipTests
              
              - step:
                  type: Run
                  name: Lint Code
                  identifier: lint
                  spec:
                    shell: Bash
                    command: |
                      npm run lint
                      
                      # Python: flake8 . or pylint src/
                      # Java: mvn checkstyle:check
              
              - step:
                  type: Run
                  name: Run Unit Tests
                  identifier: unit_tests
                  spec:
                    shell: Bash
                    command: |
                      npm test -- --coverage
                      
                      # Python: pytest --cov=src tests/
                      # Java: mvn test
                  
                    reports:
                      type: JUnit
                      spec:
                        paths:
                          - "**/*.xml"
              
              - step:
                  type: BuildAndPushDockerRegistry
                  name: Build Docker Image
                  identifier: build_docker
                  spec:
                    connectorRef: dockerhub_connector
                    repo: your-org/your-app
                    tags:
                      - <+pipeline.sequenceId>
                      - feat-<+codebase.branch>
                    dockerfile: Dockerfile
                    context: .
                    labels:
                      git.branch: <+codebase.branch>
                      git.commit: <+codebase.commitSha>
                      git.flow.type: feature
              
              - step:
                  type: Run
                  name: Integration Tests
                  identifier: integration_tests
                  spec:
                    shell: Bash
                    command: |
                      # Start test containers if needed
                      docker-compose -f docker-compose.test.yml up -d
                      
                      # Run integration tests
                      npm run test:integration
                      
                      # Cleanup
                      docker-compose -f docker-compose.test.yml down
              
              - step:
                  type: Security
                  name: Container Security Scan
                  identifier: container_scan
                  spec:
                    privileged: true
                    settings:
                      product_name: aqua-trivy
                      product_config_name: default
                      policy_type: orchestratedScan
                      scan_type: container
                      container_type: docker_v2
                      container_domain: docker.io
                      container_project: your-org
                      container_tag: feat-<+codebase.branch>
                      fail_on_severity: high

    - stage:
        name: Optional Preview Deployment
        identifier: preview_deploy
        type: Deployment
        spec:
          deploymentType: Kubernetes
          service:
            serviceRef: backend_service
            serviceInputs:
              serviceDefinition:
                type: Kubernetes
                spec:
                  artifacts:
                    primary:
                      primaryArtifactRef: <+input>
                      sources:
                        - identifier: docker_image
                          type: DockerRegistry
                          spec:
                            tag: feat-<+codebase.branch>
          environment:
            environmentRef: preview
            deployToAll: false
            infrastructureDefinitions:
              - identifier: preview_k8s
          execution:
            steps:
              - step:
                  type: K8sRollingDeploy
                  name: Rolling Deployment
                  identifier: rolling_deploy
                  spec:
                    skipDryRun: false
                    pruningEnabled: false
            rollbackSteps:
              - step:
                  type: K8sRollingRollback
                  name: Rollback Deployment
                  identifier: rollback
        when:
          condition: <+pipeline.variables.enablePreview> == "true"
          pipelineStatus: Success

  notificationRules:
    - name: Notify on Failure
      identifier: notify_failure
      pipelineEvents:
        - type: PipelineFailed
      notificationMethod:
        type: Slack
        spec:
          userGroups: []
          webhookUrl: <+secrets.getValue("slack_webhook")>
          template: |
            Feature Pipeline Failed
            Branch: <+codebase.branch>
            Commit: <+codebase.commitSha>
            URL: <+pipeline.executionUrl>

  variables:
    - name: enablePreview
      type: String
      description: Deploy to preview environment
      required: false
      value: "false"
```

#### Template 2: Develop Branch Pipeline

**File**: `.harness/pipelines/develop-deploy.yaml`

```yaml
pipeline:
  name: Develop Branch Deploy
  identifier: develop_deploy
  projectIdentifier: backend_services
  orgIdentifier: mycompany
  tags:
    git_flow: develop
  properties:
    ci:
      codebase:
        connectorRef: github_prod
        repoName: your-org/your-repo
        build:
          type: branch
          spec:
            branch: develop
  stages:
    - stage:
        name: Build and Test
        identifier: build_test
        type: CI
        spec:
          cloneCodebase: true
          platform:
            os: Linux
            arch: Amd64
          runtime:
            type: Cloud
            spec: {}
          execution:
            steps:
              - step:
                  type: Run
                  name: Install Dependencies
                  identifier: install
                  spec:
                    shell: Bash
                    command: npm ci
              
              - step:
                  type: Run
                  name: Run All Tests
                  identifier: test
                  spec:
                    shell: Bash
                    command: |
                      npm run test:all
                      npm run test:e2e
              
              - step:
                  type: BuildAndPushDockerRegistry
                  name: Build and Push
                  identifier: build_push
                  spec:
                    connectorRef: dockerhub_connector
                    repo: your-org/your-app
                    tags:
                      - develop-<+pipeline.sequenceId>
                      - develop-latest
                    dockerfile: Dockerfile

    - stage:
        name: Deploy to Dev Environment
        identifier: deploy_dev
        type: Deployment
        spec:
          deploymentType: Kubernetes
          service:
            serviceRef: backend_service
          environment:
            environmentRef: development
            infrastructureDefinitions:
              - identifier: dev_k8s
          execution:
            steps:
              - step:
                  type: K8sRollingDeploy
                  name: Deploy to Dev
                  identifier: deploy
            rollbackSteps:
              - step:
                  type: K8sRollingRollback
                  name: Rollback
                  identifier: rollback

    - stage:
        name: Smoke Tests
        identifier: smoke_tests
        type: CI
        spec:
          cloneCodebase: false
          execution:
            steps:
              - step:
                  type: Run
                  name: Health Check
                  identifier: health_check
                  spec:
                    shell: Bash
                    command: |
                      # Wait for deployment
                      sleep 30
                      
                      # Check health endpoint
                      curl -f https://dev.example.com/health || exit 1
              
              - step:
                  type: Run
                  name: API Tests
                  identifier: api_tests
                  spec:
                    shell: Bash
                    command: |
                      # Run Postman/Newman tests
                      newman run postman_collection.json \
                        --environment dev.postman_environment.json
```

#### Template 3: Release Branch Pipeline

**File**: `.harness/pipelines/release-pipeline.yaml`

```yaml
pipeline:
  name: Release Pipeline
  identifier: release_pipeline
  projectIdentifier: backend_services
  orgIdentifier: mycompany
  tags:
    git_flow: release
  properties:
    ci:
      codebase:
        connectorRef: github_prod
        build:
          type: branch
          spec:
            branch: <+trigger.branch>
  stages:
    - stage:
        name: Validate Release
        identifier: validate_release
        type: CI
        spec:
          cloneCodebase: true
          execution:
            steps:
              - step:
                  type: Run
                  name: Validate Release Branch
                  identifier: validate
                  spec:
                    shell: Bash
                    command: |
                      #!/bin/bash
                      set -e
                      
                      BRANCH="<+codebase.branch>"
                      
                      # Validate release branch format
                      if [[ ! "$BRANCH" =~ ^release-[0-9]+\.[0-9]+\.[0-9]+$ ]] && \
                         [[ ! "$BRANCH" =~ ^release/[0-9]+\.[0-9]+\.[0-9]+$ ]]; then
                        echo "❌ Invalid release branch format"
                        echo "Expected: release-1.2.3 or release/1.2.3"
                        exit 1
                      fi
                      
                      # Extract version
                      VERSION=$(echo "$BRANCH" | grep -oP '(\d+\.\d+\.\d+)')
                      echo "Release version: $VERSION"
                      
                      # Check if version already exists as tag
                      if git tag | grep -q "^v$VERSION$"; then
                        echo "❌ Version $VERSION already tagged"
                        exit 1
                      fi
                      
                      echo "✅ Release branch validated"

    - stage:
        name: Build Release Candidate
        identifier: build_rc
        type: CI
        spec:
          cloneCodebase: true
          platform:
            os: Linux
            arch: Amd64
          runtime:
            type: Cloud
            spec: {}
          execution:
            steps:
              - step:
                  type: Run
                  name: Update Version Numbers
                  identifier: update_version
                  spec:
                    shell: Bash
                    command: |
                      #!/bin/bash
                      
                      # Extract version from branch name
                      BRANCH="<+codebase.branch>"
                      VERSION=$(echo "$BRANCH" | grep -oP '(\d+\.\d+\.\d+)')
                      
                      echo "Updating to version: $VERSION"
                      
                      # Update package.json (Node.js)
                      if [ -f "package.json" ]; then
                        npm version $VERSION --no-git-tag-version
                      fi
                      
                      # Update pyproject.toml (Python)
                      if [ -f "pyproject.toml" ]; then
                        sed -i "s/^version = .*/version = \"$VERSION\"/" pyproject.toml
                      fi
                      
                      # Commit version bump
                      git config user.name "Harness CI"
                      git config user.email "ci@harness.io"
                      git add -A
                      git commit -m "chore: bump version to $VERSION" || true
              
              - step:
                  type: Run
                  name: Generate Changelog
                  identifier: generate_changelog
                  spec:
                    shell: Bash
                    command: |
                      # Generate changelog from commits since last release
                      LAST_TAG=$(git describe --tags --abbrev=0 develop 2>/dev/null || echo "")
                      
                      if [ -n "$LAST_TAG" ]; then
                        echo "# Changelog" > CHANGELOG_RELEASE.md
                        echo "" >> CHANGELOG_RELEASE.md
                        echo "## Version $VERSION" >> CHANGELOG_RELEASE.md
                        echo "" >> CHANGELOG_RELEASE.md
                        
                        # Group by commit type
                        echo "### Features" >> CHANGELOG_RELEASE.md
                        git log $LAST_TAG..HEAD --oneline --grep="^feat:" | \
                          sed 's/^/- /' >> CHANGELOG_RELEASE.md
                        
                        echo "" >> CHANGELOG_RELEASE.md
                        echo "### Bug Fixes" >> CHANGELOG_RELEASE.md
                        git log $LAST_TAG..HEAD --oneline --grep="^fix:" | \
                          sed 's/^/- /' >> CHANGELOG_RELEASE.md
                      fi
                      
                      cat CHANGELOG_RELEASE.md
              
              - step:
                  type: Run
                  name: Full Test Suite
                  identifier: full_tests
                  spec:
                    shell: Bash
                    command: |
                      npm ci
                      npm run test:all
                      npm run test:e2e
                      npm run test:integration
              
              - step:
                  type: BuildAndPushDockerRegistry
                  name: Build Release Candidate
                  identifier: build_rc_image
                  spec:
                    connectorRef: dockerhub_connector
                    repo: your-org/your-app
                    tags:
                      - <+pipeline.variables.version>-rc.<+pipeline.sequenceId>
                      - release-candidate
                    dockerfile: Dockerfile
                    labels:
                      version: <+pipeline.variables.version>
                      git.flow.type: release

    - stage:
        name: Deploy to Staging
        identifier: deploy_staging
        type: Deployment
        spec:
          deploymentType: Kubernetes
          service:
            serviceRef: backend_service
            serviceInputs:
              serviceDefinition:
                spec:
                  artifacts:
                    primary:
                      sources:
                        - identifier: docker_image
                          type: DockerRegistry
                          spec:
                            tag: <+pipeline.variables.version>-rc.<+pipeline.sequenceId>
          environment:
            environmentRef: staging
            infrastructureDefinitions:
              - identifier: staging_k8s
          execution:
            steps:
              - step:
                  type: K8sRollingDeploy
                  name: Deploy to Staging
                  identifier: deploy
                  spec:
                    skipDryRun: false
            rollbackSteps:
              - step:
                  type: K8sRollingRollback
                  name: Rollback
                  identifier: rollback

    - stage:
        name: Release Testing
        identifier: release_testing
        type: CI
        spec:
          cloneCodebase: false
          execution:
            steps:
              - step:
                  type: Run
                  name: Regression Tests
                  identifier: regression
                  spec:
                    shell: Bash
                    command: |
                      # Run comprehensive regression test suite
                      newman run regression_tests.json \
                        --environment staging.postman_environment.json \
                        --reporters cli,junit \
                        --reporter-junit-export results.xml
              
              - step:
                  type: Run
                  name: Performance Tests
                  identifier: performance
                  spec:
                    shell: Bash
                    command: |
                      # Run load tests
                      k6 run --out json=results.json performance-tests.js
              
              - step:
                  type: Run
                  name: Security Scan
                  identifier: security_scan
                  spec:
                    shell: Bash
                    command: |
                      # OWASP ZAP scan
                      docker run -t owasp/zap2docker-stable \
                        zap-baseline.py \
                        -t https://staging.example.com \
                        -r zap_report.html

    - stage:
        name: Manual QA Approval
        identifier: qa_approval
        type: Approval
        spec:
          execution:
            steps:
              - step:
                  type: HarnessApproval
                  name: QA Sign-off
                  identifier: qa_signoff
                  spec:
                    approvalMessage: |
                      Release <+pipeline.variables.version> is ready for QA approval.
                      
                      Staging URL: https://staging.example.com
                      Changelog: View in pipeline artifacts
                      
                      Please verify all features and bug fixes before approving.
                    includePipelineExecutionHistory: true
                    approvers:
                      userGroups:
                        - qa_team
                        - product_owners
                      minimumCount: 2
                    approverInputs:
                      - name: test_results
                        type: String
                        description: Summary of test results
                      - name: blockers
                        type: String
                        description: Any blocking issues found

  variables:
    - name: version
      type: String
      description: Release version (extracted from branch)
      required: true
      value: <+pipeline.stages.validate_release.spec.execution.steps.validate.output.outputVariables.VERSION>

  notificationRules:
    - name: Notify Release Ready
      identifier: notify_ready
      pipelineEvents:
        - type: StageSuccess
          forStages:
            - qa_approval
      notificationMethod:
        type: Slack
        spec:
          webhookUrl: <+secrets.getValue("slack_webhook")>
          template: |
            ✅ Release <+pipeline.variables.version> approved for production
            
            Approvers: <+pipeline.stages.qa_approval.spec.execution.steps.qa_signoff.output.approvers>
            Next: Merge release branch to main
```

#### Template 4: Main Branch Deployment Pipeline

**File**: `.harness/pipelines/main-deploy.yaml`

```yaml
pipeline:
  name: Production Deployment
  identifier: production_deploy
  projectIdentifier: backend_services
  orgIdentifier: mycompany
  tags:
    git_flow: main
    environment: production
  properties:
    ci:
      codebase:
        connectorRef: github_prod
        build:
          type: branch
          spec:
            branch: main
  stages:
    - stage:
        name: Validate Production Tag
        identifier: validate_tag
        type: CI
        spec:
          cloneCodebase: true
          execution:
            steps:
              - step:
                  type: Run
                  name: Check Version Tag
                  identifier: check_tag
                  spec:
                    shell: Bash
                    command: |
                      #!/bin/bash
                      set -e
                      
                      # Get latest tag on main
                      LATEST_TAG=$(git describe --tags --abbrev=0 2>/dev/null || echo "none")
                      
                      if [ "$LATEST_TAG" = "none" ]; then
                        echo "⚠️  No tags found on main branch"
                        echo "This might be the first release"
                      else
                        echo "Latest production version: $LATEST_TAG"
                      fi
                      
                      # Validate semantic versioning
                      if [[ ! "$LATEST_TAG" =~ ^v[0-9]+\.[0-9]+\.[0-9]+$ ]]; then
                        echo "⚠️  Tag format warning: Expected vX.Y.Z"
                      fi

    - stage:
        name: Build Production Image
        identifier: build_prod
        type: CI
        spec:
          cloneCodebase: true
          platform:
            os: Linux
            arch: Amd64
          runtime:
            type: Cloud
            spec: {}
          execution:
            steps:
              - step:
                  type: Run
                  name: Extract Version
                  identifier: extract_version
                  spec:
                    shell: Bash
                    command: |
                      # Get version from git tag
                      VERSION=$(git describe --tags --abbrev=0 | sed 's/^v//')
                      echo "Production version: $VERSION"
                      
                      # Export as output variable
                      echo "VERSION=$VERSION" >> $HARNESS_ENV
              
              - step:
                  type: BuildAndPushDockerRegistry
                  name: Build Production Image
                  identifier: build_prod_image
                  spec:
                    connectorRef: dockerhub_connector
                    repo: your-org/your-app
                    tags:
                      - <+pipeline.stages.build_prod.spec.execution.steps.extract_version.output.outputVariables.VERSION>
                      - latest
                      - production
                    dockerfile: Dockerfile
                    optimize: true  # Use caching for faster builds
                    labels:
                      version: <+pipeline.stages.build_prod.spec.execution.steps.extract_version.output.outputVariables.VERSION>
                      environment: production
                      git.commit: <+codebase.commitSha>
                      git.flow.type: main

    - stage:
        name: Pre-Production Checks
        identifier: pre_prod_checks
        type: CI
        spec:
          cloneCodebase: false
          execution:
            steps:
              - step:
                  type: Run
                  name: Verify Staging Health
                  identifier: verify_staging
                  spec:
                    shell: Bash
                    command: |
                      # Ensure staging is healthy before production
                      curl -f https://staging.example.com/health || {
                        echo "❌ Staging environment unhealthy"
                        exit 1
                      }
              
              - step:
                  type: Security
                  name: Final Security Scan
                  identifier: final_security
                  spec:
                    privileged: true
                    settings:
                      product_name: aqua-trivy
                      product_config_name: default
                      scan_type: container
                      fail_on_severity: critical

    - stage:
        name: Production Approval
        identifier: prod_approval
        type: Approval
        spec:
          execution:
            steps:
              - step:
                  type: HarnessApproval
                  name: Production Deployment Approval
                  identifier: prod_deploy_approval
                  spec:
                    approvalMessage: |
                      🚀 PRODUCTION DEPLOYMENT REQUEST
                      
                      Version: <+pipeline.stages.build_prod.spec.execution.steps.extract_version.output.outputVariables.VERSION>
                      Commit: <+codebase.commitSha>
                      Docker Image: your-org/your-app:<+pipeline.stages.build_prod.spec.execution.steps.extract_version.output.outputVariables.VERSION>
                      
                      ⚠️  This will deploy to PRODUCTION environment
                      
                      Approval required from at least 2 team leads.
                    includePipelineExecutionHistory: true
                    approvers:
                      userGroups:
                        - engineering_leads
                        - platform_team
                      minimumCount: 2
                    approverInputs:
                      - name: deployment_window
                        type: String
                        description: Scheduled deployment time
                      - name: rollback_plan
                        type: String
                        description: Rollback strategy if issues occur

    - stage:
        name: Deploy to Production
        identifier: deploy_production
        type: Deployment
        spec:
          deploymentType: Kubernetes
          service:
            serviceRef: backend_service
            serviceInputs:
              serviceDefinition:
                spec:
                  artifacts:
                    primary:
                      sources:
                        - identifier: docker_image
                          type: DockerRegistry
                          spec:
                            tag: <+pipeline.stages.build_prod.spec.execution.steps.extract_version.output.outputVariables.VERSION>
          environment:
            environmentRef: production
            infrastructureDefinitions:
              - identifier: prod_k8s
          execution:
            steps:
              - step:
                  type: K8sBlueGreenDeploy
                  name: Blue-Green Deployment
                  identifier: bg_deploy
                  spec:
                    skipDryRun: false
                    pruningEnabled: false
              
              - step:
                  type: ShellScript
                  name: Smoke Tests
                  identifier: smoke_tests
                  spec:
                    shell: Bash
                    source:
                      type: Inline
                      spec:
                        script: |
                          # Wait for pods to be ready
                          sleep 60
                          
                          # Health check
                          curl -f https://api.example.com/health || exit 1
                          
                          # Basic API tests
                          curl -f https://api.example.com/v1/status || exit 1
                    onDelegate: false
              
              - step:
                  type: K8sBGSwapServices
                  name: Swap Traffic to New Version
                  identifier: swap_services
                  spec:
                    skipDryRun: false
            
            rollbackSteps:
              - step:
                  type: K8sBlueGreenRollback
                  name: Rollback Blue-Green
                  identifier: bg_rollback

    - stage:
        name: Post-Deployment Verification
        identifier: post_deploy_verify
        type: CI
        spec:
          cloneCodebase: false
          execution:
            steps:
              - step:
                  type: Run
                  name: Comprehensive Health Check
                  identifier: health_check
                  spec:
                    shell: Bash
                    command: |
                      #!/bin/bash
                      set -e
                      
                      echo "Running post-deployment health checks..."
                      
                      # Check main API endpoints
                      curl -f https://api.example.com/health
                      curl -f https://api.example.com/v1/status
                      
                      # Check database connectivity
                      curl -f https://api.example.com/v1/db/ping
                      
                      # Check external integrations
                      curl -f https://api.example.com/v1/integrations/status
                      
                      echo "✅ All health checks passed"
              
              - step:
                  type: Run
                  name: Monitor Metrics
                  identifier: monitor_metrics
                  spec:
                    shell: Bash
                    command: |
                      # Query Prometheus/Datadog for error rates
                      # Alert if error rate exceeds threshold
                      
                      echo "Monitoring deployment metrics for 5 minutes..."
                      sleep 300
                      
                      # Check error rate from monitoring system
                      # (Pseudo-code - adapt to your monitoring)
                      ERROR_RATE=$(curl -s 'http://prometheus:9090/api/v1/query?query=rate(http_errors[5m])' | jq '.data.result[0].value[1]')
                      
                      if (( $(echo "$ERROR_RATE > 0.01" | bc -l) )); then
                        echo "❌ Error rate too high: $ERROR_RATE"
                        exit 1
                      fi
                      
                      echo "✅ Metrics look good"

  notificationRules:
    - name: Notify Production Deployment Success
      identifier: notify_success
      pipelineEvents:
        - type: PipelineSuccess
      notificationMethod:
        type: Slack
        spec:
          userGroups:
            - engineering_team
            - product_team
          webhookUrl: <+secrets.getValue("slack_webhook")>
          template: |
            ✅ 🎉 PRODUCTION DEPLOYMENT SUCCESSFUL
            
            Version: <+pipeline.stages.build_prod.spec.execution.steps.extract_version.output.outputVariables.VERSION>
            Pipeline: <+pipeline.executionUrl>
            Deployed by: <+pipeline.triggeredBy.name>
            Time: <+pipeline.startTs>
    
    - name: Notify Production Deployment Failure
      identifier: notify_failure
      pipelineEvents:
        - type: PipelineFailed
      notificationMethod:
        type: Slack
        spec:
          userGroups:
            - oncall_team
            - engineering_leads
          webhookUrl: <+secrets.getValue("slack_webhook_critical")>
          template: |
            🚨 PRODUCTION DEPLOYMENT FAILED
            
            Version: <+pipeline.stages.build_prod.spec.execution.steps.extract_version.output.outputVariables.VERSION>
            Failed Stage: <+pipeline.failedStage.name>
            Pipeline: <+pipeline.executionUrl>
            
            @oncall Please investigate immediately!
```

#### Template 5: Hotfix Pipeline

**File**: `.harness/pipelines/hotfix-pipeline.yaml`

```yaml
pipeline:
  name: Hotfix Pipeline
  identifier: hotfix_pipeline
  projectIdentifier: backend_services
  orgIdentifier: mycompany
  tags:
    git_flow: hotfix
    priority: critical
  properties:
    ci:
      codebase:
        connectorRef: github_prod
        build:
          type: branch
          spec:
            branch: <+trigger.branch>
  stages:
    - stage:
        name: Validate Hotfix
        identifier: validate_hotfix
        type: CI
        spec:
          cloneCodebase: true
          execution:
            steps:
              - step:
                  type: Run
                  name: Validate Hotfix Branch
                  identifier: validate
                  spec:
                    shell: Bash
                    command: |
                      #!/bin/bash
                      set -e
                      
                      BRANCH="<+codebase.branch>"
                      
                      # Validate hotfix branch format
                      if [[ ! "$BRANCH" =~ ^hotfix-[A-Z]+-[0-9]+-[a-z0-9-]+$ ]]; then
                        echo "❌ Invalid hotfix branch format"
                        echo "Expected: hotfix-JIRA-123-description"
                        exit 1
                      fi
                      
                      # Verify base is main
                      MERGE_BASE=$(git merge-base HEAD origin/main)
                      MAIN_HEAD=$(git rev-parse origin/main)
                      
                      if [ "$MERGE_BASE" != "$MAIN_HEAD" ]; then
                        echo "❌ Hotfix must be based on main branch"
                        exit 1
                      fi
                      
                      echo "✅ Hotfix branch validated"

    - stage:
        name: Expedited Build and Test
        identifier: expedited_build
        type: CI
        spec:
          cloneCodebase: true
          platform:
            os: Linux
            arch: Amd64
          runtime:
            type: Cloud
            spec: {}
          execution:
            steps:
              - step:
                  type: Run
                  name: Critical Tests Only
                  identifier: critical_tests
                  spec:
                    shell: Bash
                    command: |
                      npm ci
                      
                      # Run only critical/affected tests
                      npm run test:critical
                      npm run test:affected
              
              - step:
                  type: BuildAndPushDockerRegistry
                  name: Build Hotfix Image
                  identifier: build_hotfix
                  spec:
                    connectorRef: dockerhub_connector
                    repo: your-org/your-app
                    tags:
                      - hotfix-<+pipeline.sequenceId>
                      - <+codebase.branch>
                    dockerfile: Dockerfile
                    labels:
                      git.flow.type: hotfix
                      priority: critical

    - stage:
        name: Hotfix Approval
        identifier: hotfix_approval
        type: Approval
        spec:
          execution:
            steps:
              - step:
                  type: HarnessApproval
                  name: Emergency Approval
                  identifier: emergency_approval
                  spec:
                    approvalMessage: |
                      🚨 HOTFIX DEPLOYMENT REQUEST
                      
                      Branch: <+codebase.branch>
                      Issue: Critical production bug
                      
                      This is an expedited hotfix deployment.
                      Minimum 1 approval required from oncall team.
                    includePipelineExecutionHistory: true
                    approvers:
                      userGroups:
                        - oncall_team
                        - engineering_leads
                      minimumCount: 1
                    approverInputs:
                      - name: severity
                        type: String
                        description: Bug severity (critical/high)
                      - name: impact
                        type: String
                        description: User impact description
                    timeout: 30m  # Expedited timeout

    - stage:
        name: Deploy Hotfix to Production
        identifier: deploy_hotfix
        type: Deployment
        spec:
          deploymentType: Kubernetes
          service:
            serviceRef: backend_service
          environment:
            environmentRef: production
            infrastructureDefinitions:
              - identifier: prod_k8s
          execution:
            steps:
              - step:
                  type: K8sCanaryDeploy
                  name: Canary Deploy (10%)
                  identifier: canary_10
                  spec:
                    instanceSelection:
                      type: Count
                      spec:
                        count: 1
                    skipDryRun: false
              
              - step:
                  type: ShellScript
                  name: Monitor Canary
                  identifier: monitor_canary
                  spec:
                    shell: Bash
                    source:
                      type: Inline
                      spec:
                        script: |
                          # Monitor canary for 5 minutes
                          sleep 300
                          
                          # Check error rates
                          # (Integrate with your monitoring)
                          echo "Checking canary health..."
              
              - step:
                  type: K8sCanaryDeploy
                  name: Canary Deploy (50%)
                  identifier: canary_50
                  spec:
                    instanceSelection:
                      type: Percentage
                      spec:
                        percentage: 50
              
              - step:
                  type: K8sCanaryDeploy
                  name: Full Deployment
                  identifier: full_deploy
                  spec:
                    instanceSelection:
                      type: Percentage
                      spec:
                        percentage: 100
            
            rollbackSteps:
              - step:
                  type: K8sCanaryRollback
                  name: Rollback Hotfix
                  identifier: rollback

    - stage:
        name: Post-Hotfix Verification
        identifier: post_hotfix_verify
        type: CI
        spec:
          cloneCodebase: false
          execution:
            steps:
              - step:
                  type: Run
                  name: Verify Fix
                  identifier: verify_fix
                  spec:
                    shell: Bash
                    command: |
                      # Verify the hotfix resolved the issue
                      echo "Verifying hotfix effectiveness..."
                      
                      # Run specific tests for the bug
                      # Check monitoring dashboards
                      # Confirm user reports
              
              - step:
                  type: Run
                  name: Create Incident Report
                  identifier: incident_report
                  spec:
                    shell: Bash
                    command: |
                      # Generate incident report
                      cat > incident_report.md <<EOF
                      # Hotfix Incident Report
                      
                      **Hotfix Branch:** <+codebase.branch>
                      **Deployed:** $(date)
                      **Severity:** Critical
                      
                      ## Issue Description
                      [Auto-populated from JIRA]
                      
                      ## Resolution
                      Hotfix deployed to production
                      
                      ## Next Steps
                      - [ ] Merge hotfix to develop
                      - [ ] Update release notes
                      - [ ] Conduct post-mortem
                      EOF

  notificationRules:
    - name: Notify Hotfix Deployment
      identifier: notify_hotfix
      pipelineEvents:
        - type: PipelineSuccess
      notificationMethod:
        type: Slack
        spec:
          userGroups:
            - oncall_team
            - engineering_team
          webhookUrl: <+secrets.getValue("slack_webhook_critical")>
          template: |
            ✅ HOTFIX DEPLOYED TO PRODUCTION
            
            Branch: <+codebase.branch>
            Pipeline: <+pipeline.executionUrl>
            Deployed by: <+pipeline.triggeredBy.name>
            
            Next: Merge hotfix to develop branch
```

### Harness Trigger Configurations

Triggers automate pipeline execution based on Git events (push, PR, tag).

#### Trigger 1: Feature Branch PR to Develop

**File**: `.harness/triggers/pr-feature-to-develop.yaml`

```yaml
trigger:
  name: PR Feature to Develop
  identifier: pr_feature_to_develop
  enabled: true
  orgIdentifier: mycompany
  projectIdentifier: backend_services
  pipelineIdentifier: feature_branch_ci
  source:
    type: Webhook
    spec:
      type: Github
      spec:
        type: PullRequest
        spec:
          connectorRef: github_prod
          autoAbortPreviousExecutions: true
          payloadConditions:
            - key: targetBranch
              operator: Equals
              value: develop
            - key: sourceBranch
              operator: Regex
              value: ^(feat|fix)-[A-Z]+-[0-9]+-.*$
          headerConditions: []
          repoName: your-org/your-repo
          actions:
            - Open
            - Reopen
            - Synchronize  # Trigger on new commits to PR
  inputSetRefs:
    - feature_default_inputs
  pipelineBranchName: <+trigger.sourceBranch>  # Use PR source branch
```

#### Trigger 2: Push to Develop Branch

**File**: `.harness/triggers/push-to-develop.yaml`

```yaml
trigger:
  name: Push to Develop
  identifier: push_to_develop
  enabled: true
  orgIdentifier: mycompany
  projectIdentifier: backend_services
  pipelineIdentifier: develop_deploy
  source:
    type: Webhook
    spec:
      type: Github
      spec:
        type: Push
        spec:
          connectorRef: github_prod
          autoAbortPreviousExecutions: false  # Don't abort, queue instead
          payloadConditions:
            - key: <+trigger.payload.ref>
              operator: Equals
              value: refs/heads/develop
          jexlCondition: <+trigger.event> == "push"
          repoName: your-org/your-repo
  inputSetRefs:
    - develop_inputs
```

#### Trigger 3: Release Branch Creation

**File**: `.harness/triggers/release-branch.yaml`

```yaml
trigger:
  name: Release Branch Workflow
  identifier: release_branch_trigger
  enabled: true
  orgIdentifier: mycompany
  projectIdentifier: backend_services
  pipelineIdentifier: release_pipeline
  source:
    type: Webhook
    spec:
      type: Github
      spec:
        type: Push
        spec:
          connectorRef: github_prod
          autoAbortPreviousExecutions: true
          payloadConditions:
            - key: <+trigger.payload.ref>
              operator: Regex
              value: ^refs/heads/release(-|/).*$
          repoName: your-org/your-repo
  inputSetRefs:
    - release_inputs
  pipelineBranchName: <+trigger.branch>
```

#### Trigger 4: Main Branch Deployment (Post-Merge)

**File**: `.harness/triggers/push-to-main.yaml`

```yaml
trigger:
  name: Push to Main (Production)
  identifier: push_to_main
  enabled: true
  orgIdentifier: mycompany
  projectIdentifier: backend_services
  pipelineIdentifier: production_deploy
  source:
    type: Webhook
    spec:
      type: Github
      spec:
        type: Push
        spec:
          connectorRef: github_prod
          autoAbortPreviousExecutions: false
          payloadConditions:
            - key: <+trigger.payload.ref>
              operator: Equals
              value: refs/heads/main
          # Optional: Only trigger if version tag exists
          jexlCondition: |
            <+trigger.payload.head_commit.message>.contains("Merge") && 
            <+trigger.payload.commits>.size() > 0
          repoName: your-org/your-repo
  inputSetRefs:
    - production_inputs
```

#### Trigger 5: Hotfix Branch Push

**File**: `.harness/triggers/hotfix-trigger.yaml`

```yaml
trigger:
  name: Hotfix Branch Trigger
  identifier: hotfix_trigger
  enabled: true
  orgIdentifier: mycompany
  projectIdentifier: backend_services
  pipelineIdentifier: hotfix_pipeline
  source:
    type: Webhook
    spec:
      type: Github
      spec:
        type: Push
        spec:
          connectorRef: github_prod
          autoAbortPreviousExecutions: true
          payloadConditions:
            - key: <+trigger.payload.ref>
              operator: Regex
              value: ^refs/heads/hotfix-.*$
          repoName: your-org/your-repo
  inputSetRefs:
    - hotfix_inputs
  pipelineBranchName: <+trigger.branch>
```

#### Trigger 6: Release PR to Main (Final Release Approval)

**File**: `.harness/triggers/pr-release-to-main.yaml`

```yaml
trigger:
  name: PR Release to Main
  identifier: pr_release_to_main
  enabled: true
  orgIdentifier: mycompany
  projectIdentifier: backend_services
  pipelineIdentifier: release_pipeline
  source:
    type: Webhook
    spec:
      type: Github
      spec:
        type: PullRequest
        spec:
          connectorRef: github_prod
          autoAbortPreviousExecutions: true
          payloadConditions:
            - key: targetBranch
              operator: Equals
              value: main
            - key: sourceBranch
              operator: Regex
              value: ^release(-|/).*$
          repoName: your-org/your-repo
          actions:
            - Open
            - Reopen
  inputSetRefs:
    - release_to_main_inputs
```

### Input Sets for Environments

Input sets provide environment-specific configurations.

#### Production Input Set

**File**: `.harness/input-sets/production-inputs.yaml`

```yaml
inputSet:
  name: Production Inputs
  identifier: production_inputs
  orgIdentifier: mycompany
  projectIdentifier: backend_services
  pipeline:
    identifier: production_deploy
    stages:
      - stage:
          identifier: deploy_production
          type: Deployment
          spec:
            service:
              serviceInputs:
                serviceDefinition:
                  type: Kubernetes
                  spec:
                    variables:
                      - name: replicas
                        type: String
                        value: "5"
                      - name: cpu_limit
                        type: String
                        value: "2000m"
                      - name: memory_limit
                        type: String
                        value: "4Gi"
            environment:
              environmentInputs:
                type: PreProduction
                variables:
                  - name: namespace
                    type: String
                    value: "production"
                  - name: domain
                    type: String
                    value: "api.example.com"
```

#### Staging Input Set

**File**: `.harness/input-sets/staging-inputs.yaml`

```yaml
inputSet:
  name: Staging Inputs
  identifier: staging_inputs
  orgIdentifier: mycompany
  projectIdentifier: backend_services
  pipeline:
    identifier: release_pipeline
    stages:
      - stage:
          identifier: deploy_staging
          spec:
            environment:
              environmentInputs:
                variables:
                  - name: namespace
                    value: "staging"
                  - name: domain
                    value: "staging.example.com"
                  - name: replicas
                    value: "3"
```

### Server-Side Git Flow Enforcement Strategies

#### Strategy 1: Branch Protection Rules via Harness

Configure Harness to enforce Git Flow through pipeline conditions:

```yaml
# Example: Prevent direct push to main/develop
# Use this in pipeline when conditions

pipeline:
  stages:
    - stage:
        name: Validate Push
        identifier: validate
        when:
          condition: |
            <+codebase.branch> != "main" && 
            <+codebase.branch> != "develop"
          pipelineStatus: All
```

**Enforcement Points**:

1. **Branch Creation Validation**:
   - Pipeline fails if feature branch not from `develop`
   - Pipeline fails if hotfix branch not from `main`

2. **PR Target Validation**:
   - Triggers only fire for correct target branches
   - Feature → develop, Release → main, Hotfix → main+develop

3. **Merge Strategy Enforcement**:
   - Configure GitHub/GitLab branch protection
   - Require PR reviews before merge
   - Require status checks to pass (Harness pipelines)

4. **Deployment Gate Enforcement**:
   - Production deployments require approval
   - Staging deployments automatic from release branches
   - Preview deployments optional for feature branches

#### Strategy 2: GitHub Branch Protection Integration

Configure GitHub repository settings to work with Harness:

```bash
# Use GitHub API or UI to set:

# Main branch protection:
# - Require pull request reviews (2 approvals)
# - Require status checks to pass (Harness CI)
# - Require branches to be up to date
# - Include administrators: No
# - Restrict who can push: None (use Harness to control)

# Develop branch protection:
# - Require pull request reviews (1 approval)
# - Require status checks to pass (Harness CI)
# - Do not allow force pushes
# - Do not allow deletions
```

**GitHub API Example**:

```bash
curl -X PUT \
  -H "Authorization: token YOUR_GITHUB_TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/your-org/your-repo/branches/main/protection \
  -d '{
    "required_status_checks": {
      "strict": true,
      "contexts": [
        "Harness CI - Feature Pipeline",
        "Harness CI - Main Pipeline"
      ]
    },
    "enforce_admins": false,
    "required_pull_request_reviews": {
      "required_approving_review_count": 2,
      "dismiss_stale_reviews": true
    },
    "restrictions": null
  }'
```

#### Strategy 3: Automated Release Process

Use Harness to fully automate release workflows:

```yaml
# Release Automation Pipeline
# Triggered when release branch is created

pipeline:
  stages:
    - stage:
        name: Auto-Generate Release Notes
        steps:
          - step:
              type: Run
              spec:
                command: |
                  # Use semantic-release or custom script
                  npx semantic-release --dry-run
    
    - stage:
        name: Auto-Create PR to Main
        steps:
          - step:
              type: Run
              spec:
                command: |
                  # Create PR using GitHub API
                  gh pr create \
                    --base main \
                    --head ${RELEASE_BRANCH} \
                    --title "Release ${VERSION}" \
                    --body "$(cat CHANGELOG.md)"
```

#### Strategy 4: Hotfix Automation with Auto-Backport

Automatically merge hotfix to develop after production deployment:

```yaml
# Add to hotfix pipeline post-deployment stage

- stage:
    name: Auto-Merge to Develop
    identifier: auto_merge_develop
    type: CI
    when:
      stageStatus: Success
      condition: <+pipeline.stages.deploy_hotfix.status> == "Success"
    spec:
      execution:
        steps:
          - step:
              type: Run
              name: Create Backport PR
              identifier: backport
              spec:
                shell: Bash
                command: |
                  #!/bin/bash
                  
                  # Get hotfix branch name
                  HOTFIX_BRANCH="<+codebase.branch>"
                  
                  # Create PR to merge hotfix → develop
                  gh pr create \
                    --base develop \
                    --head "$HOTFIX_BRANCH" \
                    --title "Backport hotfix: $HOTFIX_BRANCH" \
                    --body "Automatically backporting hotfix to develop after production deployment." \
                    --label "hotfix-backport"
                  
                  echo "✅ Backport PR created"
```

### Complete Integration: Client + Server Hooks

**Layered Enforcement Model**:

```
┌─────────────────────────────────────────────────────┐
│          Developer Workstation                      │
│                                                      │
│  ┌────────────────────────────────────────────┐    │
│  │  Client-Side Git Hooks (This Repo)        │    │
│  │  • Fast feedback (local validation)       │    │
│  │  • Branch naming                           │    │
│  │  • Commit message format                   │    │
│  │  • JIRA ID auto-population                 │    │
│  │  • Protected branch blocks                 │    │
│  └────────────────────────────────────────────┘    │
│                     │                                │
│                     │ git push                       │
│                     ▼                                │
└─────────────────────────────────────────────────────┘
                      │
                      │
┌─────────────────────▼─────────────────────────────┐
│          Git Remote (GitHub/GitLab)                │
│                                                      │
│  ┌────────────────────────────────────────────┐    │
│  │  Branch Protection Rules                   │    │
│  │  • Require PR for main/develop             │    │
│  │  • Require status checks (Harness)         │    │
│  │  • Require reviews                          │    │
│  └────────────────────────────────────────────┘    │
│                     │                                │
│                     │ webhook                        │
│                     ▼                                │
└─────────────────────────────────────────────────────┘
                      │
                      │
┌─────────────────────▼─────────────────────────────┐
│          Harness CI/CD (Server-Side)               │
│                                                      │
│  ┌────────────────────────────────────────────┐    │
│  │  Mandatory Enforcement                     │    │
│  │  • Cannot be bypassed                      │    │
│  │  • Git Flow validation                     │    │
│  │  • Build + Test automation                 │    │
│  │  • Security scanning                       │    │
│  │  • Deployment automation                   │    │
│  │  • Approval gates                          │    │
│  │  • Audit trail                             │    │
│  └────────────────────────────────────────────┘    │
│                     │                                │
│                     │ deployment                     │
│                     ▼                                │
└─────────────────────────────────────────────────────┘
                      │
                      │
              ┌───────┴───────┐
              │               │
         ┌────▼────┐    ┌────▼────┐
         │ Staging │    │   Prod  │
         └─────────┘    └─────────┘
```

**Benefits of Dual Enforcement**:

✅ **Client-side (Local Hooks)**:
- Instant feedback - No wait for CI
- Reduces failed CI builds
- Educates developers early
- Works offline

✅ **Server-side (Harness CI/CD)**:
- Cannot be bypassed
- Centralized policy management
- Automated deployments
- Approval workflows
- Compliance audit trail

### Webhook Configuration

#### GitHub Webhook Setup

1. Navigate to repository **Settings** → **Webhooks** → **Add webhook**

2. Configure webhook:
   ```
   Payload URL: https://app.harness.io/gateway/api/webhooks/<account-id>
   Content type: application/json
   Secret: <webhook-secret-from-harness>
   
   Events:
   ☑ Pull requests
   ☑ Pushes
   ☑ Branch or tag creation
   ☑ Branch or tag deletion
   ```

3. Harness automatically creates webhook when you create trigger

#### GitLab Webhook Setup

```
Project → Settings → Webhooks

URL: https://app.harness.io/gateway/api/webhooks/<account-id>
Secret Token: <from-harness>

Trigger:
☑ Push events
☑ Merge request events
☑ Tag push events
```

#### Bitbucket Webhook Setup

```
Repository Settings → Webhooks → Add webhook

Title: Harness CI/CD
URL: https://app.harness.io/gateway/api/webhooks/<account-id>

Triggers:
☑ Repository push
☑ Pull request created
☑ Pull request updated
☑ Pull request merged
```

### Harness Best Practices for Git Flow

#### 1. Pipeline Organization

```
.harness/
├── pipelines/
│   ├── _common/
│   │   ├── build-steps.yaml        # Reusable build steps
│   │   ├── test-steps.yaml         # Reusable test steps
│   │   └── deploy-steps.yaml       # Reusable deploy steps
│   ├── feature-build.yaml          # Feature pipeline
│   ├── develop-deploy.yaml         # Develop pipeline
│   ├── release-pipeline.yaml       # Release pipeline
│   ├── main-deploy.yaml            # Production pipeline
│   └── hotfix-pipeline.yaml        # Hotfix pipeline
├── input-sets/
│   ├── production-inputs.yaml
│   ├── staging-inputs.yaml
│   ├── develop-inputs.yaml
│   └── preview-inputs.yaml
└── triggers/
    ├── pr-feature-to-develop.yaml
    ├── push-to-develop.yaml
    ├── release-branch.yaml
    ├── push-to-main.yaml
    └── hotfix-trigger.yaml
```

#### 2. Use Pipeline Templates

Create reusable templates for common patterns:

```yaml
# Template: Standard CI Steps
template:
  name: Standard CI Steps
  identifier: standard_ci_steps
  type: Stage
  spec:
    type: CI
    spec:
      execution:
        steps:
          - step:
              type: Run
              name: Install Dependencies
              identifier: install
              spec:
                command: <+input>
          
          - step:
              type: Run
              name: Lint
              identifier: lint
              spec:
                command: <+input>
          
          - step:
              type: Run
              name: Test
              identifier: test
              spec:
                command: <+input>

# Use in pipelines:
pipeline:
  stages:
    - stage:
        identifier: ci_stage
        template:
          templateRef: standard_ci_steps
          templateInputs:
            spec:
              execution:
                steps:
                  - step:
                      identifier: install
                      spec:
                        command: npm ci
```

#### 3. Environment Variables and Secrets

```yaml
# Pipeline-level variables
pipeline:
  variables:
    - name: DOCKER_REGISTRY
      type: String
      value: docker.io/your-org
    - name: APP_NAME
      type: String
      value: backend-service

# Use secrets for sensitive data
pipeline:
  stages:
    - stage:
        steps:
          - step:
              spec:
                envVariables:
                  DB_PASSWORD: <+secrets.getValue("db_password")>
                  API_KEY: <+secrets.getValue("api_key")>
```

#### 4. Failure Strategies

```yaml
# Add to stages for automatic retries
pipeline:
  stages:
    - stage:
        failureStrategies:
          - onFailure:
              errors:
                - AllErrors
              action:
                type: Retry
                spec:
                  retryCount: 2
                  retryIntervals:
                    - 10s
                    - 30s
                  onRetryFailure:
                    action:
                      type: MarkAsFailure
```

#### 5. Conditional Execution

```yaml
# Skip stages based on branch
pipeline:
  stages:
    - stage:
        name: Deploy to Preview
        when:
          condition: |
            <+codebase.branch>.startsWith("feat-") && 
            <+pipeline.variables.enablePreview> == "true"
          pipelineStatus: Success
```

### Troubleshooting Guide

#### Issue 1: Trigger Not Firing

**Symptoms**: Pipeline doesn't start after git push/PR

**Solutions**:
```bash
# Check webhook delivery in Git provider
# GitHub: Settings → Webhooks → Recent Deliveries

# Verify trigger conditions
# - Check branch name matches regex
# - Verify connector is correct
# - Check payload conditions

# Test webhook manually
curl -X POST \
  -H "Content-Type: application/json" \
  -H "X-GitHub-Event: push" \
  https://app.harness.io/gateway/api/webhooks/<account-id> \
  -d @test-payload.json
```

#### Issue 2: Branch Validation Failing

**Symptoms**: Pipeline fails at branch validation step

**Solutions**:
```bash
# Check branch naming format
# Expected: feat-PROJ-123-description

# Verify base branch
git log --oneline --graph develop..feature-branch

# Check Git Flow rules in pipeline YAML
# Ensure regex patterns match your convention
```

#### Issue 3: Deployment Approval Timeout

**Symptoms**: Approval step times out

**Solutions**:
```yaml
# Increase timeout in approval step
- step:
    type: HarnessApproval
    spec:
      timeout: 2h  # Increase from default 1h

# Configure notification for approvers
# Add Slack/Email notification when approval needed
```

#### Issue 4: Docker Build Fails

**Symptoms**: BuildAndPushDockerRegistry step fails

**Solutions**:
```bash
# Check Docker connector credentials
# Settings → Connectors → Docker Registry → Test Connection

# Verify Dockerfile exists and is valid
docker build -t test .

# Check registry permissions
docker login docker.io
docker push docker.io/your-org/test

# Review build logs in Harness for specific error
```

#### Issue 5: Kubernetes Deployment Fails

**Symptoms**: K8s deployment step fails

**Solutions**:
```bash
# Verify K8s connector
# Settings → Connectors → Kubernetes → Test Connection

# Check cluster access from delegate
kubectl --context=<cluster> get nodes

# Verify service account permissions
kubectl auth can-i --list --as=system:serviceaccount:harness-delegate:default

# Check manifest syntax
kubectl apply --dry-run=client -f k8s-manifests/
```

---

## Git Flow Best Practices

### Branch Naming

✅ **Good**:
- `feat-PROJ-123-user-authentication`
- `hotfix-SEC-456-xss-vulnerability`
- `release-1.2.0`
- `release/1.2.0`

❌ **Bad**:
- `feature-branch` (no JIRA ID)
- `johns-stuff` (not descriptive)
- `bugfix` (too generic)

### Commit Messages

✅ **Good**:
```
feat: PROJ-123 add OAuth2 authentication

- Implement login endpoint
- Add JWT token generation
- Include refresh token support
```

❌ **Bad**:
```
update
fixed stuff
WIP
```

### Merging Strategy

✅ **Always use `--no-ff`**:
```bash
git merge --no-ff feat-PROJ-123-feature
```

❌ **Never fast-forward for features**:
```bash
# This loses feature history
git merge feat-PROJ-123-feature
```

### Release Timing

✅ **Create release branch when**:
- All features for release are merged to develop
- Develop is stable
- Ready to start release preparation

❌ **Don't create release branch**:
- Before all features are merged
- When develop is unstable
- Too early (releases should be short-lived)

### Hotfix Guidelines

✅ **Use hotfix for**:
- Production bugs
- Security vulnerabilities
- Critical issues affecting users

❌ **Don't use hotfix for**:
- Regular bug fixes (use bugfix from develop)
- New features
- Non-critical issues

---

## Git Flow Rules Enforced

### Long-Lived Branches

| Branch | Base | Merges Into | Purpose |
|--------|------|-------------|---------|
| `main` | - | - | Production-ready code |
| `develop` | - | - | Integration branch for next release |

### Short-Lived Branches

| Type | Pattern | Base | Merges Into | Purpose |
|------|---------|------|-------------|---------|
| **Feature** | `feat-JIRA-123-description` | `develop` | `develop` | New features |
| **Bugfix** | `fix-JIRA-123-description` | `develop` | `develop` | Bug fixes |
| **Hotfix** | `hotfix-JIRA-123-description` | `main` | `main` + `develop` | Production fixes |
| **Release** | `release-1.0.0` or `release/1.0.0` | `develop` | `main` + `develop` | Release preparation |
| **Support** | `chore-JIRA-123-description` | `develop` | `develop` | Maintenance tasks |

### Validation Points

1. **Post-Checkout**: Validates branch creation base
2. **Pre-Commit**: Validates protected branch commits
3. **Commit-Msg**: Validates commit message format
4. **Pre-Push**: Validates branch name, history, commit count, base branch

---

## Hook Reference

### pre-commit

**Triggers**: Before creating a commit

**Validates**:
- Protected branch commits (blocks direct commits to `main`/`develop`)
- Executes custom commands from `commands.conf`

**Custom Commands**: Supports linting, formatting, type-checking, security scans

**Example Output**:
```
================================================================================
  Pre-Commit Validation
================================================================================

✓ SUCCESS: Branch: feat-PROJ-123-add-auth

────────────────────────────────────────────────────────────────────────────────
Running Custom Commands

  ✓ Prettier Format Check (2s)
  ✓ ESLint (5s)
  ✓ TypeScript Check (8s)

✓ SUCCESS: All pre-commit validations passed
```

**Bypass**:
```bash
BYPASS_HOOKS=1 git commit -m "message"
# or
git commit --no-verify -m "message"
```

---

### prepare-commit-msg

**Triggers**: Before commit message editor opens

**Actions**:
- Extracts JIRA ID from branch name
- Auto-populates commit message template
- Preserves existing messages

**Example**:
```bash
# Branch: feat-PROJ-123-user-auth
git commit

# Opens editor with:
feat: PROJ-123 

# Please enter a descriptive commit message above
# Format: <type>: <JIRA-ID> <description>
#
# Types: feat, fix, chore, break, tests
# Example: feat: PROJ-123 Add user authentication
#
# Your branch: feat-PROJ-123-user-auth
```

**Skips**:
- Merge commits
- Amend commits with existing messages
- Messages already containing JIRA ID
- Detached HEAD state

---

### commit-msg

**Triggers**: Before commit is created (validates commit message)

**Validates**:
- Commit message format: `<type>: <JIRA-ID> <description>`
- Valid types: `feat`, `fix`, `chore`, `break`, `tests`, `docs`, `style`, `refactor`, `perf`
- JIRA ID pattern: `[A-Z]{2,10}-[0-9]+`
- Non-empty descriptions

**Allows**:
- Merge commits (starts with "Merge")
- Revert commits (starts with "Revert")

**Example Valid Messages**:
```
feat: PROJ-123 add user authentication
fix: TICKET-456 resolve memory leak
chore: ABC-789 update dependencies
break: PROJ-100 remove deprecated API
tests: JIRA-999 add integration tests
```

**Example Error**:
```
✗ ERROR: Invalid Commit Message
Commit message must follow: <type>: <JIRA-ID> <description>

Your message: "added new feature"

✓ JIRA ID from branch: PROJ-123

Required format: feat: PROJ-123 add your description
Types: feat, fix, chore, break, tests, docs, refactor, perf

Examples:
  feat: PROJ-123 add user authentication
  fix: PROJ-123 resolve memory leak
  chore: PROJ-123 update dependencies

Fix now:
  git commit --amend -m "feat: PROJ-123 your description"

🔄 Undo commit if needed:
  git reset --soft HEAD~1      # Undo commit, keep changes
  git reset --mixed HEAD~1     # Undo commit, unstage changes
  git reset --hard HEAD~1      # Undo commit, discard changes
  git reflog                   # View commit history to recover
```

---

### post-commit

**Triggers**: After successful commit creation

**Actions**:
- Detects lockfile changes (package-lock.json, yarn.lock, etc.)
- Detects Infrastructure as Code changes (Terraform, K8s, etc.)
- Detects CI/CD configuration changes
- Provides helpful reminders and suggestions

**Example Output**:
```
💡 HINT: Lockfile changed - Dependencies updated
  • package-lock.json

  npm ci && npm audit && npm test

🔄 If lockfile shouldn't have changed:
  git reset --soft HEAD~1   # Undo commit, keep changes
  git restore --staged package-lock.json  # Unstage lockfile
  git restore package-lock.json    # Revert lockfile
```

**Silent**: When no special files are detected

---

### pre-push

**Triggers**: Before pushing to remote

**Validates**:
1. **Branch Naming**: Git Flow compliant patterns
2. **Protected Branch Push**: Blocks direct pushes to `main`/`develop`
3. **Branch Base**: Validates Git Flow branch origin
4. **Commit Count**: Enforces curated history (default: max 5 commits)
5. **Linear History**: No merge commits allowed
6. **Foxtrot Merges**: Detects and blocks incorrect merge patterns

**Example Output**:
```
================================================================================
  Validating Push: feat-PROJ-123-add-auth → origin
================================================================================

✓ SUCCESS: Branch naming: Valid
✓ SUCCESS: Branch base: develop (correct)
✓ SUCCESS: Commit count: 3/5
✓ SUCCESS: History: Linear (no merge commits)

✓ SUCCESS: All validations passed - push allowed
```

**Example Error (Too Many Commits)**:
```
✗ ERROR: Too Many Commits
Branch has 8 commits (limit: 5). Squash commits before pushing.

Why limit commits? Easier review, cleaner history, simpler reverts.

Fix option 1 - Interactive rebase:
  git rebase -i develop
  # Mark commits as 'squash' or 'fixup', save and exit
  git push --force-with-lease origin feat-PROJ-123-add-auth

Fix option 2 - Soft reset (simpler):
  git reset --soft develop
  git commit -m "feat: PROJ-123 your complete description"
  git push --force-with-lease origin feat-PROJ-123-add-auth

Increase limit temporarily (if justified):
  git config hooks.maxCommits 8
```

**Configuration**:
```bash
# Increase commit limit
git config hooks.maxCommits 10

# View current limit
git config hooks.maxCommits
```

---

### post-checkout

**Triggers**: After checking out a branch

**Validates**:
- Git Flow base branch for new branches
- Branch naming conventions
- Protected branch warnings

**Critical Validation**: Ensures new branches are created from correct base

**Example (Correct Base)**:
```bash
# Create feature from develop (correct)
git checkout develop
git checkout -b feat-PROJ-123-new-feature

# Output:
✓ SUCCESS: Git Flow: Branch created from correct base 'develop'
```

**Example (Wrong Base - Error)**:
```bash
# Create feature from main (wrong!)
git checkout main
git checkout -b feat-PROJ-123-new-feature

# Output:
✗ ERROR: Git Flow Violation: Invalid Base Branch

❌ CRITICAL: Branch created from wrong base!

Branch:         feat-PROJ-123-new-feature
Branch type:    feature
Created from:   main (❌ WRONG)
Required base:  develop (✓ CORRECT)

═══════════════════════════════════════════════════════════════
                    GIT FLOW RULES
═══════════════════════════════════════════════════════════════

📋 Feature/Bugfix/Support branches:
   • MUST branch from: 'develop'
   • MUST merge into: 'develop'
   • Purpose: New features and bug fixes for next release

   Types: feat, feature, bugfix, fix, build, chore, ci,
          docs, techdebt, perf, refactor, revert, style, test

═══════════════════════════════════════════════════════════════

🔧 FIX OPTION 1 - Recreate from correct base (RECOMMENDED):

   Step 1: Go to correct base branch
     git checkout develop

   Step 2: Create new branch with proper name from correct base
     git checkout -b feat-PROJ-123-your-description

   Step 3: Delete the incorrectly created branch
     git branch -D feat-PROJ-123-new-feature

🔧 FIX OPTION 2 - Rebase onto correct base (ADVANCED):

   ⚠️  Use this ONLY if you have NO commits yet!

   Step 1: Ensure you're on the wrong branch
     git checkout feat-PROJ-123-new-feature

   Step 2: Rebase onto correct base
     git rebase --onto develop main feat-PROJ-123-new-feature

   Step 3: Verify the base
     git log --oneline develop..feat-PROJ-123-new-feature

⛔ This branch will be REJECTED on push!
⚠️  Fix this NOW to avoid losing work later.
```

**Protected Branch Warning**:
```bash
git checkout main

# Output:
⚠ WARNING: You are now on protected branch: main

⚠️  Direct commits are restricted on this branch

To make changes (hotfix for production):
  git checkout -b hotfix-JIRA-123-description main
```

---

### post-rewrite

**Triggers**: After rebase or amend operations

**Actions**:
- Provides force-push reminders
- Shows undo commands
- History rewriting guidance

**Example (After Rebase)**:
```
✓ SUCCESS: Rebase completed
⚠ WARNING: Force-push required

Next steps:
  1. Review: git log --oneline -10
  2. Check:  git log --oneline origin/feat-PROJ-123..feat-PROJ-123
  3. Push:   git push --force-with-lease origin feat-PROJ-123

⚠️  Use --force-with-lease (safer than --force)

🔄 Undo rebase if needed:
  git reflog                    # Find pre-rebase commit
  git reset --hard HEAD@{N}     # Go back to commit N
  git reset --hard ORIG_HEAD    # Quick undo (if available)
```

**Example (After Amend)**:
```
ℹ INFO: Commit amended - force-push if already pushed
  git push --force-with-lease origin feat-PROJ-123

🔄 Undo amend:
  git reset --soft HEAD@{1}     # Keep changes
  git reset --hard HEAD@{1}     # Discard changes
```

---

### applypatch-msg

**Triggers**: When applying patches via `git am`

**Validates**:
- Patch commit messages follow same format as regular commits
- JIRA ID presence and format

**Example Error**:
```
✗ ERROR: Invalid Patch Message
Patch message must follow: <type>: <JIRA-ID> <description>

Your message: "patch for bug"

Fix option 1 - Interactive mode:
  git am --abort
  git am --interactive <patch-file>.patch
  # Press 'e' to edit message

Fix option 2 - Apply and amend:
  git am --no-verify <patch-file>.patch
  git commit --amend -m "feat: PROJ-123 apply patch description"

🔄 Undo applied patch:
  git am --abort        # Before patch is applied
  git reset --hard HEAD~1  # After patch is applied
  git reflog            # Recover if needed
```

---

## Configuration

### Git Configuration Options

```bash
# Maximum commits allowed per branch (default: 5)
git config hooks.maxCommits 10

# Auto-stage files after commands fix them (default: false)
git config hooks.autoAddAfterFix true

# Enable parallel command execution (default: false)
git config hooks.parallelExecution true

# Set base branch for current branch
git config branch.$(git branch --show-current).base develop
```

### View Current Configuration

```bash
# View all hooks-related configs
git config --get-regexp '^hooks\.'

# View specific config
git config hooks.maxCommits

# View branch base
git config branch.feat-PROJ-123.base
```

### Global vs Local Configuration

```bash
# Repository-specific (recommended)
git config hooks.maxCommits 5

# Global (all repositories)
git config --global hooks.maxCommits 10

# Remove configuration
git config --unset hooks.maxCommits
```

---

## Custom Commands

### Configuration File

Custom commands are defined in `.githooks/commands.conf`

**Format**:
```
HOOK:PRIORITY:MANDATORY:TIMEOUT:COMMAND:DESCRIPTION
```

**Fields**:
- `HOOK`: Hook name (pre-commit, pre-push, commit-msg, etc.)
- `PRIORITY`: Execution order (lower number = runs first)
- `MANDATORY`: `true`/`false` - whether failure blocks the entire hook
- `TIMEOUT`: Maximum execution time in seconds
- `COMMAND`: Shell command to execute
- `DESCRIPTION`: Human-readable description shown during execution

**Variables**:
- `{staged}`: Expands to space-separated list of staged files (pre-commit only)

### Examples

#### JavaScript/TypeScript Project

```properties
# Stage 1: Fast formatting and basic checks (< 1 minute)
pre-commit:1:true:30:npx prettier --check {staged}:Prettier Format Check
pre-commit:2:true:60:npx eslint {staged}:ESLint

# Stage 2: Type checking (optional, can be slow)
pre-commit:3:false:120:npx tsc --noEmit --skipLibCheck:TypeScript Check

# Stage 3: Unit tests before push (< 5 minutes)
pre-push:1:true:300:npm test:Run Unit Tests

# Stage 4: Build verification (optional, slower)
pre-push:2:false:600:npm run build:Build Verification
```

#### Python Project

```properties
# Stage 1: Fast formatting and linting (< 1 minute)
pre-commit:1:true:30:black --check {staged}:Black Format Check
pre-commit:2:true:60:flake8 {staged}:Flake8 Linting
pre-commit:3:true:30:isort --check {staged}:Import Sort Check

# Stage 2: Type and security checks (optional)
pre-commit:4:false:60:mypy {staged}:MyPy Type Check
pre-commit:5:false:30:bandit -r {staged}:Security Scan

# Stage 3: Unit tests before push
pre-push:1:true:300:pytest tests/unit -v:Unit Tests

# Stage 4: Coverage and integration tests (optional)
pre-push:2:false:600:pytest tests/integration -v:Integration Tests
```

#### Go Project

```properties
# Format checking
pre-commit:1:true:30:test -z $(gofmt -l {staged}):Go Format Check

# Vetting
pre-commit:2:true:60:go vet ./...:Go Vet

# Linting with golangci-lint
pre-commit:3:true:90:golangci-lint run {staged}:GolangCI-Lint

# Unit tests
pre-push:1:true:300:go test ./...:Run Tests

# Build verification
pre-push:2:false:180:go build ./...:Build Project
```

#### Docker/Infrastructure

```properties
# Dockerfile linting
pre-commit:1:true:30:docker run --rm -i hadolint/hadolint < Dockerfile:Hadolint

# Terraform validation
pre-commit:1:true:30:terraform fmt -check -recursive:Terraform Format
pre-commit:2:true:60:terraform validate:Terraform Validate

# YAML linting
pre-commit:3:true:30:yamllint {staged}:YAML Lint
```

### Priority Guidelines

- **1-10**: Critical, fast checks (format, lint, lockfile sync)
- **11-20**: Important checks (type check, compilation)
- **21-30**: Optional quality checks
- **31+**: Slow, optional validations

### Timeout Guidelines

- **5-10s**: Fast checks (conflict detection, file presence)
- **10-30s**: Format checkers, linters, basic lockfile validation
- **30-90s**: Type checkers, compilation, deep lockfile validation
- **90-300s**: Unit tests
- **300-600s**: Integration tests, builds

### Testing Commands

```bash
# Test commands manually
npx prettier --check src/

# Dry-run with hooks
BYPASS_HOOKS=1 git commit -m "test"

# View logs
cat .git/hooks-logs/hook-$(date +%Y-%m-%d).log
```

---

## Bypass Mechanisms

### Global Bypass (All Hooks)

```bash
# Skip all hook validations
BYPASS_HOOKS=1 git commit -m "emergency fix"
BYPASS_HOOKS=1 git push

# Git's native bypass (only client-side)
git commit --no-verify -m "emergency fix"
git push --no-verify
```

### Protected Branch Bypass

```bash
# Allow commits to main/develop
ALLOW_DIRECT_PROTECTED=1 git commit -m "hotfix: emergency"

# Allow push to main/develop
ALLOW_DIRECT_PROTECTED=1 git push origin main
```

### When to Use Bypass

✅ **Appropriate Uses**:
- Emergency production hotfixes
- Fixing broken CI/CD pipelines
- Recovering from Git disasters
- Testing hook changes

❌ **Inappropriate Uses**:
- Avoiding code quality standards
- Rushing incomplete features
- Circumventing team processes
- Regular development workflow

### Audit Trail

All bypass operations are logged:
```bash
cat .git/hooks-logs/hook-$(date +%Y-%m-%d).log | grep BYPASS

# Example output:
[2024-11-08 14:23:45] [WARNING] [pre-commit] BYPASS USED: Hook bypassed via BYPASS_HOOKS by user: developer
```

---

## Branch Naming Convention

### Pattern Requirements

**Format**: `<type>-<JIRA-ID>-<description>`

**Components**:
- **Type**: One of the valid types (see below)
- **JIRA ID**: Pattern `[A-Z]{2,10}-[0-9]+` (2-10 uppercase letters, dash, numbers)
- **Description**: Lowercase, hyphen-separated words

### Valid Branch Types

| Type | Purpose | Base Branch |
|------|---------|-------------|
| `feat`, `feature` | New features | `develop` |
| `fix`, `bugfix` | Bug fixes | `develop` |
| `hotfix` | Production fixes | `main` |
| `release` | Release preparation | `develop` |
| `chore` | Maintenance | `develop` |
| `docs` | Documentation | `develop` |
| `style` | Code style | `develop` |
| `refactor` | Code restructuring | `develop` |
| `perf` | Performance | `develop` |
| `test` | Testing | `develop` |
| `build` | Build system | `develop` |
| `ci` | CI/CD | `develop` |
| `techdebt` | Technical debt | `develop` |
| `revert` | Revert changes | `develop` |

### Valid Examples

```bash
feat-PROJ-123-add-user-authentication
fix-TICKET-456-resolve-memory-leak
hotfix-ABC-789-patch-security-vulnerability
chore-JIRA-100-update-dependencies
docs-PROJECT-200-api-documentation
refactor-TASK-300-simplify-auth-logic
```

### Invalid Examples

❌ `feature-branch` - Missing JIRA ID
❌ `PROJ-123` - Missing type and description
❌ `feat-proj-123-description` - Lowercase JIRA ID
❌ `feat-PROJ-123` - Missing description
❌ `feat_PROJ_123_description` - Wrong separators (use hyphens)

### Creating Branches Correctly

```bash
# Feature branch (from develop)
git checkout develop
git checkout -b feat-PROJ-123-add-feature develop

# Hotfix branch (from main)
git checkout main
git checkout -b hotfix-PROJ-456-fix-bug main

# Avoid common mistakes
git checkout main
git checkout -b feat-PROJ-123-feature  # ❌ Wrong base!

git checkout develop
git checkout -b feature-branch  # ❌ Invalid name!
```

---

## Commit Message Format

### Required Pattern

```
<type>: <JIRA-ID> <description>
```

### Valid Types

- `feat`: New feature
- `fix`: Bug fix
- `chore`: Maintenance
- `break`: Breaking change
- `tests`: Test changes
- `docs`: Documentation
- `style`: Code style (formatting, whitespace)
- `refactor`: Code restructuring
- `perf`: Performance improvement
- `hotfix`: Production hotfix

### JIRA ID Format

- Pattern: `[A-Z]{2,10}-[0-9]+`
- Examples: `PROJ-123`, `TICKET-456`, `ABC-789`

### Description Rules

- Must not be empty
- Should be concise but descriptive
- No period at the end (convention)
- Present tense (e.g., "add" not "added")

### Valid Examples

```
feat: PROJ-123 add user authentication system
fix: TICKET-456 resolve memory leak in data processor
chore: JIRA-789 update dependencies to latest versions
break: PROJ-100 remove deprecated API endpoints
tests: ABC-200 add integration tests for payment module
docs: TASK-300 update API documentation
refactor: PROJ-400 simplify authentication logic
perf: TICKET-500 optimize database queries
```

### Invalid Examples

❌ `added new feature` - Missing type and JIRA ID
❌ `feat: add feature` - Missing JIRA ID
❌ `feat: proj-123 add feature` - Lowercase JIRA ID
❌ `feat: PROJ-123` - Missing description
❌ `PROJ-123 add feature` - Missing type

### Auto-Population

The `prepare-commit-msg` hook automatically extracts JIRA ID from branch names:

```bash
# Branch: feat-PROJ-123-user-auth
git commit

# Editor opens with pre-filled template:
feat: PROJ-123 

# Just add your description:
feat: PROJ-123 implement OAuth2 authentication
```

### Special Commit Types (Always Allowed)

**Merge Commits**:
```
Merge branch 'develop' into 'main'
Merge pull request #123 from user/feat-PROJ-456-feature
```

**Revert Commits**:
```
Revert "feat: PROJ-123 add user authentication"
```

---

## Lockfile Validation

### Supported Package Managers

The hooks provide comprehensive lockfile validation for:

#### Node.js / JavaScript / TypeScript
- `package-lock.json` (npm)
- `yarn.lock` (Yarn)
- `pnpm-lock.yaml` (pnpm)

#### Python
- `poetry.lock` (Poetry)
- `Pipfile.lock` (Pipenv)
- `requirements.txt`

#### Go
- `go.sum`

#### Rust
- `Cargo.lock`

#### Ruby
- `Gemfile.lock` (Bundler)

#### PHP
- `composer.lock` (Composer)

#### Swift / iOS
- `Package.resolved` (Swift Package Manager)
- `Podfile.lock` (CocoaPods)

#### Terraform / Infrastructure
- `.terraform.lock.hcl`

### Validation Types

#### 1. Manifest-Lockfile Sync Check

Ensures lockfile is updated when manifest changes.

**Example** (package.json):
```bash
# Edit package.json but forget to run npm install
git add package.json
git commit -m "feat: PROJ-123 add dependency"

# Error:
⚠️  package.json modified but package-lock.json not updated!

Fix: Run 'npm install' to regenerate package-lock.json
Then: git add package-lock.json
```

#### 2. Lockfile Integrity Validation

Validates lockfile format and content.

**Example**:
```bash
# Corrupted package-lock.json
git add package-lock.json
git commit -m "feat: PROJ-123 update deps"

# Error:
❌ package-lock.json validation failed!

Common causes:
  • package-lock.json is corrupted
  • Versions in package.json don't match lockfile
  • Manual edit to lockfile (don't do this!)

Fix: Delete and regenerate:
  rm package-lock.json
  npm install
  git add package-lock.json
```

#### 3. Merge Conflict Detection

Prevents committing lockfiles with unresolved conflicts.

**Example**:
```bash
# After merge conflict in package-lock.json
git add package-lock.json
git commit -m "feat: PROJ-123 merge develop"

# Error:
❌ MERGE CONFLICT markers detected in lockfile!

DO NOT manually resolve lockfile conflicts!

Fix for npm:
  git checkout --theirs package-lock.json  # or --ours
  npm install
  git add package-lock.json
```

#### 4. Orphan Lockfile Change Warning

Alerts when lockfile changes without manifest changes.

**Example**:
```bash
# package-lock.json changed but not package.json
git add package-lock.json
git commit -m "feat: PROJ-123 update lock"

# Warning (not blocking):
ℹ️  Lockfile changed without package.json change

This is unusual but can be valid:
  • Security update (npm audit fix)
  • Lockfile format upgrade
  • Version resolution change

Verify this is intentional. If not, run:
  git restore --staged package-*.json yarn.lock pnpm-lock.yaml
```

### Enabling Lockfile Validation

Lockfile validation commands are extensively documented in `commands.conf` but commented out by default.

**To enable**:

1. Open `.githooks/commands.conf`
2. Find your technology section (e.g., "Node.js / JavaScript / TypeScript Projects")
3. Uncomment the relevant validation commands

**Recommendation**: Start with `mandatory=false` for 1-2 weeks, then switch to `mandatory=true`.

**Example for Node.js**:
```properties
# Check package.json/lockfile sync
pre-commit:1:false:10:if git diff --cached --name-only | grep -q "^package\.json$"; then if [ -f package-lock.json ] && ! git diff --cached --name-only | grep -q "^package-lock\.json$"; then echo ""; echo "⚠️  package.json modified but package-lock.json not updated!"; echo ""; echo "Fix: Run 'npm install' to regenerate package-lock.json"; echo "Then: git add package-lock.json"; echo ""; exit 1; fi; fi:Check package.json sync

# Validate npm lockfile integrity
pre-commit:2:false:30:if [ -f package-lock.json ] && git diff --cached --name-only | grep -q "^package-lock\.json$"; then if ! npm ls --package-lock-only >/dev/null 2>&1; then echo ""; echo "❌ package-lock.json validation failed!"; echo ""; exit 1; fi; fi:Validate package-lock.json

# Detect merge conflicts
pre-commit:1:true:5:if git diff --cached --name-only | grep -qE "package-lock\.json|yarn\.lock|pnpm-lock\.yaml"; then if git diff --cached | grep -qE "^(<{7}|={7}|>{7})"; then echo ""; echo "❌ MERGE CONFLICT markers detected in lockfile!"; echo ""; exit 1; fi; fi:Check Node.js lockfile conflicts
```

### Post-Commit Hints

The `post-commit` hook provides helpful reminders when lockfiles change:

```bash
git commit -m "feat: PROJ-123 update dependencies"

# Output:
💡 HINT: Lockfile changed - Dependencies updated
  • package-lock.json

  npm ci && npm audit && npm test

🔄 If lockfile shouldn't have changed:
  git reset --soft HEAD~1   # Undo commit, keep changes
  git restore --staged package-lock.json  # Unstage lockfile
  git restore package-lock.json    # Revert lockfile
```

### Best Practices

1. **Never Manually Edit Lockfiles**: Always regenerate using package manager
2. **Resolve Conflicts Properly**: Use `git checkout` + regenerate, not manual editing
3. **Commit Together**: Always commit manifest and lockfile together
4. **Test After Update**: Run tests after dependency updates
5. **Audit Security**: Run `npm audit` or equivalent after updates

---

## Error Messages and Fixes

### Common Scenarios

#### 1. Wrong Branch Base

**Error**:
```
❌ CRITICAL: Branch created from wrong base!

Branch:         feat-PROJ-123-add-auth
Branch type:    feature
Created from:   main (❌ WRONG)
Required base:  develop (✓ CORRECT)
```

**Fix**:
```bash
# Recreate from correct base
git checkout develop
git checkout -b feat-PROJ-123-add-auth
git branch -D feat-PROJ-123-add-auth-old  # Delete wrong branch
```

#### 2. Invalid Commit Message

**Error**:
```
✗ ERROR: Invalid Commit Message

Your message: "added new feature"

Required format: feat: PROJ-123 add your description
```

**Fix**:
```bash
# Amend commit message
git commit --amend -m "feat: PROJ-123 add new feature"

# Or reset and recommit
git reset --soft HEAD~1
git commit -m "feat: PROJ-123 add new feature"
```

#### 3. Too Many Commits

**Error**:
```
✗ ERROR: Too Many Commits
Branch has 8 commits (limit: 5). Squash commits before pushing.
```

**Fix**:
```bash
# Interactive rebase
git rebase -i develop
# Mark commits as 'squash', save and exit

# Or soft reset (simpler)
git reset --soft develop
git commit -m "feat: PROJ-123 complete feature description"

# Force-push with safety
git push --force-with-lease origin feat-PROJ-123
```

#### 4. Non-Linear History (Merge Commits)

**Error**:
```
✗ ERROR: Non-Linear History
Branch contains 2 merge commit(s). Use rebase instead of merge.
```

**Fix**:
```bash
# Rebase onto develop
git fetch origin
git rebase origin/develop

# Resolve conflicts if any
git add <resolved-files>
git rebase --continue

# Force-push
git push --force-with-lease origin feat-PROJ-123
```

#### 5. Protected Branch Commit

**Error**:
```
✗ ERROR: Protected Branch
Cannot commit directly to protected branch 'main'. Use Pull Requests.
```

**Fix**:
```bash
# Move changes to proper branch
git stash push -m 'Changes from main'
git checkout -b hotfix-PROJ-123-fix main
git stash pop
git add .
git commit -m "fix: PROJ-123 emergency fix"
```

#### 6. Invalid Branch Name

**Error**:
```
✗ ERROR: Invalid Branch Name
Branch 'feature-branch' doesn't follow Git Flow naming: <type>-<JIRA>-<description>
```

**Fix**:
```bash
# Rename branch
git branch -m feature-branch feat-PROJ-123-add-feature

# Or create new correctly-named branch
git checkout develop
git checkout -b feat-PROJ-123-add-feature
git cherry-pick <commits>
git branch -D feature-branch
```

### Universal Undo Commands

```bash
# Undo last commit (keep changes staged)
git reset --soft HEAD~1

# Undo last commit (unstage changes)
git reset --mixed HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# View reflog to recover
git reflog
git reset --hard HEAD@{N}  # Go back to specific state

# Undo last N commits
git reset --soft HEAD~3

# Abort rebase
git rebase --abort

# Abort merge
git merge --abort

# Abort cherry-pick
git cherry-pick --abort

# Recover deleted branch
git reflog
git checkout -b recovered-branch HEAD@{N}
```

---

## Logging and Debugging

### Log Location

```bash
# Daily log file
.git/hooks-logs/hook-YYYY-MM-DD.log

# Today's log
.git/hooks-logs/hook-$(date +%Y-%m-%d).log
```

### Log Format

```
[YYYY-MM-DD HH:MM:SS] [LEVEL] [HOOK_NAME] Message
```

**Levels**: INFO, WARNING, ERROR, DEBUG, TRACE

### Viewing Logs

```bash
# View today's log
cat .git/hooks-logs/hook-$(date +%Y-%m-%d).log

# Tail logs (live)
tail -f .git/hooks-logs/hook-$(date +%Y-%m-%d).log

# Search for errors
grep ERROR .git/hooks-logs/hook-$(date +%Y-%m-%d).log

# Search for bypass usage
grep BYPASS .git/hooks-logs/*.log

# View last 50 lines
tail -50 .git/hooks-logs/hook-$(date +%Y-%m-%d).log
```

### Example Log Output

```
[2024-11-08 14:23:10] [INFO] [pre-commit] Hook execution started
[2024-11-08 14:23:10] [INFO] [pre-commit] Checking protected branch: feat-PROJ-123
[2024-11-08 14:23:11] [INFO] [pre-commit] Executing: Prettier Format Check
[2024-11-08 14:23:13] [INFO] [pre-commit] Success: Prettier Format Check (2s)
[2024-11-08 14:23:13] [INFO] [pre-commit] Pre-commit validation successful
```

### Debugging Hook Execution

```bash
# Enable bash debug mode
bash -x .githooks/pre-commit

# Debug specific hook with environment variables
BASH_XTRACEFD=1 bash -x .githooks/pre-push

# Test hook manually
./.githooks/pre-commit

# Check hook configuration
git config core.hooksPath
git config --get-regexp '^hooks\.'
```

### Common Debug Scenarios

#### Hook Not Running

```bash
# Check hooks path
git config core.hooksPath
# Should output: .githooks

# Check if hooks are executable
ls -la .githooks/
# Should show: -rwxr-xr-x

# Make hooks executable
chmod +x .githooks/*
```

#### Command Not Found

```bash
# Check if command exists
which npx
which npm

# Check PATH
echo $PATH

# Check Node.js installation
node --version
npm --version
```

#### Timeout Issues

```bash
# Increase timeout in commands.conf
# Change:
pre-commit:1:true:30:npm test:Run Tests
# To:
pre-commit:1:true:300:npm test:Run Tests

# Or disable mandatory
pre-commit:1:false:300:npm test:Run Tests
```

---

## Uninstallation

### Using Uninstall Script

```bash
# Run uninstaller
./.githooks/uninstall-hooks.sh

# Prompts for confirmation:
Are you sure you want to uninstall? (yes/no):
```

### What Gets Removed

- ✅ `core.hooksPath` configuration
- ✅ `hooks.*` configurations
- ✅ `branch.*.base` configurations
- ✅ Workflow settings (`rebase.autosquash`, `fetch.prune`)

### What Gets Preserved

- ✅ Hook files in `.githooks/` (not deleted)
- ✅ Logs archived to `.git/hooks-logs-archive/`

### Manual Uninstallation

```bash
# Remove hooks path
git config --unset core.hooksPath

# Remove hooks configurations
git config --unset hooks.maxCommits
git config --unset hooks.autoAddAfterFix
git config --unset hooks.parallelExecution

# Remove all branch base configs
for key in $(git config --get-regexp '^branch\..*\.base$' | awk '{print $1}'); do
    git config --unset "$key"
done

# Remove workflow settings
git config --unset rebase.autosquash
git config --unset fetch.prune

# Archive logs
mkdir -p .git/hooks-logs-archive
cp -r .git/hooks-logs .git/hooks-logs-archive/backup-$(date +%Y%m%d)
rm -rf .git/hooks-logs
```

### Reinstallation

```bash
# After uninstallation, reinstall with:
./.githooks/install-hooks.sh
```

---

## Best Practices

### For Developers

1. **Always Branch from Correct Base**
   - Features/bugfixes: `develop`
   - Hotfixes: `main`
   - Releases: `develop`

2. **Use Descriptive Branch Names**
   - Include JIRA ticket ID
   - Use hyphens, not underscores
   - Keep description concise but clear

3. **Write Good Commit Messages**
   - Follow format: `<type>: <JIRA-ID> <description>`
   - Use present tense
   - Be specific about what changed

4. **Squash Before Pushing**
   - Keep feature branches to ≤ 5 commits
   - Use interactive rebase to clean up history
   - Force-push with `--force-with-lease`

5. **Rebase, Don't Merge**
   - Keep linear history
   - `git pull --rebase` or `git config pull.rebase true`
   - Resolve conflicts carefully

6. **Test Before Pushing**
   - Run tests locally
   - Ensure linters pass
   - Verify builds complete

7. **Use Bypass Sparingly**
   - Document why bypass was used
   - Fix underlying issues after emergency
   - Don't make it a habit

### For Team Leads

1. **Educate Team on Git Flow**
   - Share this README
   - Conduct training sessions
   - Lead by example

2. **Configure Repository**
   - Set appropriate `hooks.maxCommits`
   - Enable `hooks.autoAddAfterFix` if using formatters
   - Configure `commands.conf` for your stack

3. **Monitor Hook Usage**
   - Review logs periodically
   - Identify bypass patterns
   - Address recurring issues

4. **Customize for Your Workflow**
   - Adjust branch naming if needed
   - Configure protected branches
   - Set up custom commands

5. **Maintain Documentation**
   - Keep team wiki updated
   - Document project-specific rules
   - Share common error fixes

### For Repository Maintainers

1. **Keep Hooks Updated**
   - Pull latest changes regularly
   - Test in staging before production
   - Communicate changes to team

2. **Review and Optimize Commands**
   - Remove slow/unused commands
   - Optimize command timeouts
   - Balance speed vs thoroughness

3. **Monitor Performance**
   - Track hook execution times
   - Identify bottlenecks
   - Consider parallel execution

4. **Handle Exceptions**
   - Document when bypass is acceptable
   - Provide guidance for edge cases
   - Update hooks for new scenarios

---

## Troubleshooting

### Common Issues

#### 1. Hooks Not Executing

**Symptoms**: Commits/pushes succeed without validation

**Diagnosis**:
```bash
# Check hooks path
git config core.hooksPath

# Check if hooks are executable
ls -la .githooks/
```

**Solution**:
```bash
# Reinstall hooks
./.githooks/install-hooks.sh

# Or manually configure
git config core.hooksPath .githooks
chmod +x .githooks/*
```

#### 2. Command Not Found Errors

**Symptoms**: `npx: command not found`, `python: command not found`

**Diagnosis**:
```bash
# Check if tools are installed
which npx
which python

# Check PATH
echo $PATH
```

**Solution**:
```bash
# Install missing tools
# Node.js: https://nodejs.org/
# Python: https://www.python.org/

# Or disable commands in commands.conf
# Comment out the failing commands
```

#### 3. Bash Version Issues

**Symptoms**: `This script requires bash 4.3 or higher`

**Diagnosis**:
```bash
bash --version
```

**Solution (macOS)**:
```bash
# Install newer bash
brew install bash

# Add to shells
sudo echo /usr/local/bin/bash >> /etc/shells

# Change default shell (optional)
chsh -s /usr/local/bin/bash
```

#### 4. Timeout Errors

**Symptoms**: Commands killed with "TIMEOUT after Xs"

**Diagnosis**: Check if commands are hanging

**Solution**:
```bash
# Increase timeout in commands.conf
# From:
pre-commit:1:true:30:npm test:Run Tests
# To:
pre-commit:1:true:300:npm test:Run Tests

# Or make optional
pre-commit:1:false:300:npm test:Run Tests
```

#### 5. Lockfile Validation False Positives

**Symptoms**: Valid lockfile changes flagged as errors

**Diagnosis**: Review specific error message

**Solution**:
```bash
# If validation is too strict, adjust mandatory flag
# From:
pre-commit:1:true:30:validation-command:Description
# To:
pre-commit:1:false:30:validation-command:Description

# Or regenerate lockfile
rm package-lock.json
npm install
git add package-lock.json
```

#### 6. Performance Issues

**Symptoms**: Hooks take too long to execute

**Diagnosis**:
```bash
# Check log for slow commands
cat .git/hooks-logs/hook-$(date +%Y-%m-%d).log | grep -E '\([0-9]+s\)'
```

**Solution**:
```bash
# Enable parallel execution
git config hooks.parallelExecution true

# Reduce command scope
# Instead of:
pre-commit:1:true:60:npm run lint:Lint All
# Use:
pre-commit:1:true:60:npx eslint {staged}:Lint Staged

# Make slow commands optional
pre-commit:3:false:120:npm run type-check:Type Check
```

### Getting Help

1. **Check Logs**:
   ```bash
   cat .git/hooks-logs/hook-$(date +%Y-%m-%d).log
   ```

2. **Enable Debug Mode**:
   ```bash
   bash -x .githooks/pre-commit
   ```

3. **Review Error Message**: Hooks provide detailed fix suggestions

4. **Search Issues**: Check repository issues for similar problems

5. **Ask Team**: Consult with team members or leads

---

## Contributing

We welcome contributions! To contribute:

1. **Fork the Repository**
2. **Create Feature Branch**: `git checkout -b feat-CONTRIB-123-your-feature develop`
3. **Make Changes**: Follow existing code style
4. **Test Thoroughly**: Ensure hooks work across scenarios
5. **Document Changes**: Update README if needed
6. **Submit Pull Request**: Target `develop` branch

### Development Guidelines

- Test on Linux, macOS, and Windows (Git Bash)
- Maintain backward compatibility
- Add logging for new features
- Include error handling
- Provide helpful error messages
- Update documentation

---

## License

This project is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**.

**Key Points**:
- ✅ Free to use, modify, and distribute
- ✅ Must share source code of modifications
- ✅ Must use same license for derivatives
- ✅ Network use = distribution (AGPL requirement)

See [LICENSE](LICENSE) for full text.

---

## Credits

- **Author**: Enterprise Development Team
- **Git Flow Model**: [Vincent Driessen](https://nvie.com/posts/a-successful-git-branching-model/)
- **Inspiration**: Enterprise development best practices

---

## Support

- **Documentation**: This README
- **Issues**: GitHub Issues
- **Discussions**: GitHub Discussions

---

**Happy Git Flowing! 🚀**
