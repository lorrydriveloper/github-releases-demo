# GitHub Releases Workflow Demo

A simple HTML demo application showcasing a GitHub Releases workflow using CalVer (Calendar Versioning) with automated staging and production deployments.

## 🎯 Overview

This repository demonstrates:
- **CalVer versioning** (YYYY.WW.PATCH format)
- **Automatic staging deployments** on every push to `main`
- **Production deployments** triggered by GitHub Releases
- **Hotfix workflow** for critical production fixes

## 📁 Project Structure

```
.
├── index.html              # Simple HTML demo app
├── .github/
│   └── workflows/
│       ├── deploy-staging.yml      # Auto-deploy to staging
│       ├── deploy-production.yml  # Deploy on release
│       └── create-release.yml     # Helper to create releases
├── scripts/
│   ├── calc-version.sh     # Calculate next CalVer version
│   ├── create-hotfix.sh   # Create hotfix branch
│   └── merge-hotfix.sh    # Merge hotfix back to main
├── workflow-diagram.md     # Visual workflow diagram
└── README.md              # This file
```

## 🚀 Quick Start

### 1. Setup GitHub Pages

1. Go to your repository Settings → Pages
2. Set source to: **GitHub Actions**
3. Enable GitHub Actions in repository settings

### 2. Make Scripts Executable

```bash
chmod +x scripts/*.sh
```

### 3. Calculate Next Version

```bash
./scripts/calc-version.sh
```

This will show you the next CalVer version based on the current date.

### 4. Create Your First Release

**Option A: Using GitHub Actions (Recommended)**
1. Go to **Actions** → **Create Release**
2. Click **Run workflow**
3. Enter version (e.g., `2025.45.0`)
4. Optionally add a title
5. Click **Run workflow**

**Option B: Manual**
```bash
git tag -a v2025.45.0 -m "Release v2025.45.0"
git push origin v2025.45.0
# Then create GitHub Release from the tag
```

## 🔄 Workflow

### Normal Development Flow

1. **Make changes** on `main` branch
2. **Push to main** → Automatically deploys to **Staging**
3. **Test in staging** → Verify everything works
4. **Create release** → Use GitHub Actions or manual tag
5. **Production deploys** → Automatically when release is published

### Hotfix Flow

When a critical bug is found in production:

```bash
# 1. Create hotfix branch from production tag
./scripts/create-hotfix.sh 2025.45.0

# 2. Make your fixes
# ... edit files ...

# 3. Commit and tag
git add .
git commit -m "fix: critical bug description"
git tag -a v2025.45.1 -m "Hotfix v2025.45.1"
git push origin hotfix/2025.45.1 --tags

# 4. Create GitHub Release from tag v2025.45.1
# (This triggers production deployment)

# 5. After deployment, merge back to main
./scripts/merge-hotfix.sh 2025.45.1
```

## 📦 Version Format

**CalVer (Calendar Versioning):** `YYYY.WW.PATCH`

- **YYYY**: Year (e.g., 2025)
- **WW**: Week number (0-53)
- **PATCH**: Patch number (0, 1, 2, ...)

**Examples:**
- `2025.45.0` - First release of week 45
- `2025.45.1` - Hotfix for week 45
- `2025.46.0` - First release of week 46

## 🎨 Visual Workflow

See [workflow-diagram.md](./workflow-diagram.md) for a detailed visual representation of the workflow.

## 🔧 Scripts

### `calc-version.sh`
Calculates the next CalVer version based on the current date and existing tags.

```bash
./scripts/calc-version.sh
# Output: 📦 Next CalVer Version: v2025.45.0
```

### `create-hotfix.sh`
Creates a hotfix branch from a production tag.

```bash
./scripts/create-hotfix.sh 2025.45.0
# Creates: hotfix/2025.45.1 branch
```

### `merge-hotfix.sh`
Merges a hotfix back to main after production deployment.

```bash
./scripts/merge-hotfix.sh 2025.45.1
```

## 🌐 Deployment

- **Staging**: Auto-deploys on every push to `main`
  - URL: `https://<username>.github.io/<repo>/staging/`
  
- **Production**: Deploys when a GitHub Release is published
  - URL: `https://<username>.github.io/<repo>/`

## 📝 GitHub Actions Workflows

### `deploy-staging.yml`
- **Trigger**: Push to `main` branch
- **Action**: Deploys to GitHub Pages staging directory
- **Environment**: `staging`

### `deploy-production.yml`
- **Trigger**: GitHub Release published
- **Action**: Validates version format and deploys to production
- **Environment**: `production`

### `create-release.yml`
- **Trigger**: Manual workflow dispatch
- **Action**: Creates git tag and GitHub Release
- **Inputs**: Version (required), Title (optional)

## 🎯 Key Features

✅ **Automatic Staging** - Every push to main deploys to staging  
✅ **Production Safety** - Only releases trigger production  
✅ **Version Validation** - Ensures proper CalVer format  
✅ **Hotfix Support** - Quick fixes without waiting for next week  
✅ **Visual Demo** - Simple HTML app shows current version  

## 🔍 Testing the Workflow

1. **Test Staging Deployment:**
   ```bash
   # Make a small change
   echo "<!-- Updated -->" >> index.html
   git add .
   git commit -m "test: staging deployment"
   git push origin main
   # Check Actions tab - staging should deploy
   ```

2. **Test Production Deployment:**
   ```bash
   # Use GitHub Actions to create release
   # Or manually:
   ./scripts/calc-version.sh  # Get next version
   git tag -a v2025.45.0 -m "Release v2025.45.0"
   git push origin v2025.45.0
   # Create GitHub Release from tag
   # Check Actions tab - production should deploy
   ```

3. **Test Hotfix:**
   ```bash
   ./scripts/create-hotfix.sh 2025.45.0
   # Make fix, tag, release
   # After production deploy, merge back
   ./scripts/merge-hotfix.sh 2025.45.1
   ```

## 📚 Learn More

- [CalVer Documentation](https://calver.org/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Releases](https://docs.github.com/en/repositories/releasing-projects-on-github)

## 🤝 Contributing

This is a demo repository. Feel free to fork and adapt it for your own projects!

## 📄 License

MIT License - feel free to use this workflow in your projects.
# github-releases-demo
