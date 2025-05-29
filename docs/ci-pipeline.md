# CI Pipeline Specification — Mirror of Discernment (MoD)

## 1. Overview
This CI pipeline automates linting, testing, building, and publishing for all MoD components:
- **CAA** (Core Analysis API)
- **AR Client**
- **Gateway**
- **QBSM Dashboard**

It runs on pushes and merge‐requests against `main` (and any release branches matching `release/*`).

## 2. Workflow Triggers
- **Push** to any branch → lint + basic unit tests.  
- **Merge Request** against `main`:
  1. Full unit + integration tests.  
  2. Build artifacts.  
  3. Publish to staging registry (if passing).  
- **Tag** matching `v*.*.*` → build Docker images, push to Docker Hub, and deploy to production namespace.

## 3. Job Definitions

### 3.1 Lint
- **Image:** `node:18-alpine`  
- **Script:**
  ```bash
  npm ci
  npm run lint

pip install -r requirements.txt
pytest --junitxml=reports/unit-tests.xml

npm run start &       # Dev server in background
npm run test:integration

docker build -t registry.example.com/mod/${CI_PROJECT_NAME}:${CI_COMMIT_SHORT_SHA} .
docker push registry.example.com/mod/${CI_PROJECT_NAME}:${CI_COMMIT_SHORT_SHA}

git tag rollback-$(date +%Y%m%d)-${CI_PROJECT_NAME}
git push --tags

curl -X POST https://ops.example.com/redeploy \
     -H "Authorization: Bearer $OPS_TOKEN" \
     -d '{"service":"'"${CI_PROJECT_NAME}"'"}'

