# 🏢 Consumer Organization Demo: Frontend Team

This repository demonstrates how a **frontend consumer organization** would use the platform engineering workflows in a multi-organization GitHub Enterprise setup.

## 🏗️ Multi-Organization Architecture

```
┌─────────────────────────────────────┐
│     Platform Organization          │
│     flynn-actions                   │
│                                     │
│  ├── platform-workflows/           │
│  │   └── standard-ci-cd.yml        │ ← Enhanced DAG Pipeline
│  └── platform-actions/             │
│      └── shared actions            │
└─────────────────────────────────────┘
              ↑ uses
┌─────────────────────────────────────┐
│   Consumer Organization             │
│   frontend-company                  │
│                                     │
│  ├── frontend-app-1/               │
│  ├── frontend-app-2/               │ ← This repo represents these
│  └── frontend-app-3/               │
└─────────────────────────────────────┘
```

## 🚀 Enhanced DAG Pipeline Features

When you run the workflow in this repository, you'll see:

### 8-Stage Pipeline DAG:
1. **🔧 Environment Setup** - Pipeline initialization & build ID generation
2. **📦 Install Dependencies** - Code checkout & dependency installation
3. **🧪 Quality Gate** - Parallel testing (unit, integration, lint)
4. **🔒 Security Scan** - Security analysis (runs in parallel with testing)
5. **🔨 Build Application** - Application build & artifact generation
6. **🔍 Pre-deployment Checks** - Deployment readiness validation
7. **🚀 Deploy to Environment** - Application deployment
8. **🎉 Pipeline Complete** - Final notifications & cleanup

### Key DAG Features:
- ✅ **Proper job dependencies** - Clear visual flow
- ✅ **Parallel execution** - Testing + security run simultaneously
- ✅ **Matrix strategy** - Quality gate runs 3 parallel jobs
- ✅ **Conditional logic** - Smart step execution
- ✅ **Data passing** - Build IDs and artifacts flow between stages
- ✅ **Environment protection** - GitHub environment gates

## 🎯 Test the Enhanced DAG

1. **Go to Actions tab** in this repository
2. **Click "Enhanced Multi-Stage DAG Pipeline"**
3. **Click "Run workflow"**
4. **Select environment and security options**
5. **Watch the 8-stage DAG execute!**

You'll see a proper pipeline visualization with dependencies, not just a single step.

## 🔒 Consumer Organization Security (In Real Setup)

In an actual multi-organization deployment:

```yaml
# Consumer org would be restricted to only:
allowed_actions:
  - "platform-org/platform-workflows/*"
  - "platform-org/platform-actions/*"
  - "actions/checkout@v4"  # GitHub-owned actions
  
# Blocked automatically:
blocked_actions:
  - "marketplace/third-party-action"  # ❌ Blocked
  - "random-user/untrusted-action"    # ❌ Blocked
```

## 📋 Multi-Org Setup Instructions

To implement this in your enterprise:

1. **Create Organizations**:
   - Platform org (hosts workflows)
   - Consumer org 1 (frontend teams)
   - Consumer org 2 (backend teams)
   - Consumer org 3 (mobile teams)

2. **Configure Security**:
   ```bash
   # Set enterprise-level restrictions
   gh api -X PUT /enterprises/YOUR-ENTERPRISE/actions/permissions \
     --field allowed_actions=selected \
     --field patterns_allowed='["platform-org/*"]'
   ```

3. **Deploy Platform Workflows**:
   - Use this enhanced DAG pipeline
   - All consumer orgs reference the same workflow
   - Centralized updates benefit all teams

## 🎉 Result

Consumer teams get:
- ✅ **Rich DAG visualization** - Proper pipeline stages
- ✅ **Zero maintenance** - Platform team manages workflows
- ✅ **Maximum security** - Only approved actions allowed
- ✅ **Consistent experience** - Same pipeline across all teams

This demo shows exactly what your platform engineering solution delivers!
