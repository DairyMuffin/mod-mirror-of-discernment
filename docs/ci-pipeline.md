# CI Pipeline Specification — Mirror of Discernment (MoD)

## 1. Overview
This CI pipeline automates linting, testing, building, and publishing for all MoD components: CAA, AR client, Gateway, and QBSM Dashboard.

---

## 2. Workflow Stages

### A. Lint & Format
- **Tools:**  
  - Python: `flake8`, `black --check`  
  - JavaScript: `eslint --max-warnings 0`, `prettier --check`

### B. Unit Tests
- **CAA:**  
  - `pytest tests/`  
  - Mock external APIs for `/scan`  
- **AR Client & Gateway:**  
  - Jest or Mocha for JS modules  
  - Snapshot tests for UI components

### C. Integration Tests
- Spin up services in Docker Compose:
  ```bash
  docker-compose -f ci/docker-compose.test.yml up -d
  pytest integration_tests/
  docker-compose -f ci/docker-compose.test.yml down

docker build -t dairymuffin/caa:${{ github.sha }} -f deps/caa/Dockerfile deps/caa
docker build -t dairymuffin/mod-gateway:${{ github.sha }} .
docker build -t dairymuffin/qsbm-dashboard:${{ github.sha }} -f deps/qsbm-dashboard/Dockerfile deps/qsbm-dashboard
docker build -t dairymuffin/ar-client:${{ github.sha }} -f ar-client/Dockerfile ar-client

docker push dairymuffin/caa:${{ github.sha }}
# ...and similarly for other images

jobs:
  deploy:
    runs-on: ubuntu-latest
    needs: build-and-test
    steps:
      - name: Deploy to Staging
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.STAGING_HOST }}
          script: |
            cd /srv/mod-mirror-of-discernment
            git pull
            docker-compose pull
            docker-compose up -d

name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  release:
    types: [ created ]

jobs:
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with: python-version: '3.10'
      - name: Install dependencies
        run: |
          pip install flake8 black pytest
          npm install
      - name: Lint Python
        run: |
          flake8 .
          black --check .
      - name: Lint JS
        run: |
          npx eslint .
          npx prettier --check .
      - name: Run unit tests
        run: pytest
      - name: Run integration tests
        run: |
          docker-compose -f ci/docker-compose.test.yml up -d
          pytest integration_tests/
          docker-compose -f ci/docker-compose.test.yml down

  build-and-push:
    needs: lint-and-test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build & Push Docker Images
        uses: docker/build-push-action@v4
        with:
          push: true
          tags: |
            dairymuffin/caa:${{ github.sha }}
            dairymuffin/mod-gateway:${{ github.sha }}
            dairymuffin/qsbm-dashboard:${{ github.sha }}
            dairymuffin/ar-client:${{ github.sha }}

  deploy:
    if: startsWith(github.ref, 'refs/tags/v')
    needs: build-and-push
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Production
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.PROD_HOST }}
          script: |
            cd /srv/mod-mirror-of-discernment
            git pull
            docker-compose pull
            docker-compose up -d

