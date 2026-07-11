# Enterprise API Testing Framework

**Portfolio Project for SDET / Test Architect / AI QA Engineer Roles**

[![Status](https://img.shields.io/badge/status-production--ready-green)]()
[![Postman](https://img.shields.io/badge/postman-11.0-orange)]()
[![Newman](https://img.shields.io/badge/newman-cli--ready-blue)]()

## 🎯 Purpose

This repository demonstrates a **production-grade API testing framework** built with Postman, showcasing:

- ✅ **Architectural Thinking** - Organized, scalable test structure
- ✅ **Design Patterns** - DRY, SRP, reusable components
- ✅ **Best Practices** - Variables, environments, logging, contract testing
- ✅ **Framework Principles** - Maintainability, extensibility, configuration management
- ✅ **CI/CD Integration** - Jenkins, GitHub Actions, Azure DevOps ready
- ✅ **Comprehensive Testing** - Smoke, Regression, Negative, Contract, Performance suites

This is **not** just a list of API requests—it's a framework that demonstrates how to architect maintainable API automation.

---

## 📊 Architecture Overview

```
Enterprise API Testing Framework
│
├── 📁 SETUP & AUTHENTICATION
│      └── Verify environment, variables, connectivity
│
├── 📁 USERS (JSONPlaceholder)
│      ├── POST Create User
│      ├── GET User By ID
│      ├── GET All Users
│      ├── PUT Update User
│      └── DELETE User
│
├── 📁 POSTS (JSONPlaceholder)
│      ├── POST Create Post
│      ├── GET Post By ID
│      ├── GET Posts By User (query)
│      ├── PUT Update Post
│      └── DELETE Post
│
├── 📁 NEGATIVE SCENARIOS
│      ├── Non-existent resource (404)
│      ├── Missing required fields
│      ├── Malformed JSON
│      └── Invalid ID format
│
├── 📁 CONTRACT VALIDATION
│      ├── User Schema Validation
│      ├── Post Schema Validation
│      └── Field type assertions
│
├── 📁 SMOKE TEST SUITE (2 mins)
│      └── Critical path validation
│
├── 📁 REGRESSION TEST SUITE (5 mins)
│      └── Complete CRUD operations
│
└── 📁 PERFORMANCE TEST SUITE (1 min)
         └── Response time & health monitoring
```

### Request Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                  INCOMING REQUEST                            │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
        ┌────────────────────────┐
        │ Pre-request Script     │
        ├────────────────────────┤
        │ • Generate RequestID   │
        │ • Generate Correlation │
        │ • Generate Timestamp   │
        │ • Random Email/UUID    │
        └────────────┬───────────┘
                     │
                     ▼
        ┌────────────────────────┐
        │  HTTP Request          │
        ├────────────────────────┤
        │ • Base URL (variable)  │
        │ • Headers (X-Request)  │
        │ • Body (if POST/PUT)   │
        │ • Query Params         │
        └────────────┬───────────┘
                     │
                     ▼
        ┌────────────────────────┐
        │  API Response          │
        └────────────┬───────────┘
                     │
                     ▼
        ┌────────────────────────┐
        │ Test Script            │
        ├────────────────────────┤
        │ • Validate Status      │
        │ • Validate Time        │
        │ • Validate Schema      │
        │ • Store Variables      │
        │ • Log Results          │
        └────────────┬───────────┘
                     │
                     ▼
        ┌────────────────────────┐
        │ Variables Stored       │
        ├────────────────────────┤
        │ • createdUserId        │
        │ • createdPostId        │
        │ • Response Data        │
        └────────────┬───────────┘
                     │
                     ▼
        ┌────────────────────────┐
        │ Next Request Uses Data │
        └────────────────────────┘
```

---

## 🏗️ Design Principles

| Principle | Implementation | Benefit |
|-----------|---|---|
| **Single Responsibility** | Separate folders for Users, Posts, Comments | Easy to locate & modify tests |
| **Don't Repeat Yourself** | Collection-level scripts, reusable helpers | No code duplication |
| **Configuration** | Environment variables, not hardcoding | Switch environments instantly |
| **Maintainability** | Clear naming, documentation, organization | Scales easily with new APIs |
| **Extensibility** | Template-based requests, modular structure | Add new endpoints in minutes |
| **Reusability** | Helper functions, stored variables | Faster development, fewer bugs |
| **Traceability** | Request IDs, Correlation IDs, logging | Debug issues easily |
| **Documentation** | Inline comments, folder descriptions | Onboarding new team members |

---

## 📁 File Structure

```
api-testing-framework/
│
├── collection.json       # Main Postman collection (all requests & tests)
├── globals.json          # Global variables & shared scripts
│
├── qa.json              # QA environment (relaxed thresholds, debug on)
├── prod.json            # PROD environment (strict thresholds, debug off)
│
├── README.md            # This file
├── .github/
│   └── workflows/
│       └── api-tests.yml # GitHub Actions CI/CD
│
└── ci-cd/
    ├── jenkins.groovy   # Jenkins pipeline
    └── azure-devops.yml # Azure DevOps pipeline
```

---

## 🚀 Quick Start

### 1. Import into Postman

**Option A: Via UI**
1. Open Postman
2. File → Import
3. Select `collection.json`
4. Import globals: Settings → Export → Select `globals.json`
5. Import environment: File → Import → Select `qa.json`

**Option B: Via CLI**
```bash
# Install Newman (Postman CLI)
npm install -g newman

# Run collection against QA
newman run collection.json -e qa.json

# Run with HTML report
newman run collection.json -e qa.json -r html --reporter-html-export report.html
```

### 2. Verify Setup

1. Open `SETUP & AUTHENTICATION` folder
2. Click "Verify Environment Setup"
3. Should return `200 OK`

If successful: ✅ Environment is configured correctly

### 3. Run Test Suites

```bash
# Smoke Tests (quick health check, ~2 mins)
newman run collection.json -e qa.json --folder "SMOKE TEST SUITE"

# Full Regression (complete validation, ~5 mins)
newman run collection.json -e qa.json --folder "REGRESSION TEST SUITE"

# Performance Monitoring (API health, ~1 min)
newman run collection.json -e qa.json --folder "PERFORMANCE TEST SUITE"

# All Tests
newman run collection.json -e qa.json
```

---

## 🔑 Collection Variables

### Global Variables (Set Automatically)

| Variable | Generated | Purpose |
|----------|-----------|---------|
| `requestId` | Pre-request | Unique request identifier (UUID) |
| `correlationId` | Pre-request | Track related requests across system |
| `timestamp` | Pre-request | ISO 8601 timestamp |
| `randomEmail` | Pre-request | Unique test email (test{suffix}@example.com) |
| `randomUUID` | Pre-request | Random UUID for testing |

### Collection Variables (Manual Setup)

| Variable | Default | Purpose |
|----------|---------|---------|
| `baseUrl` | https://jsonplaceholder.typicode.com | API base URL |
| `environment` | qa | Current environment (qa/prod) |
| `testUserId` | 1 | User ID for GET/PUT/DELETE |
| `testPostId` | 1 | Post ID for testing |
| `maxResponseTime` | 1000 | Max response time threshold (ms) |

### Stored Variables (Extracted from Responses)

| Variable | Set By | Used In |
|----------|--------|---------|
| `createdUserId` | POST Create User | GET/PUT/DELETE subsequent requests |
| `createdPostId` | POST Create Post | GET/DELETE subsequent requests |

---

## 🔄 How Variable Dependencies Work

### Example: Create User → Get User

**Step 1: POST Create User**
```javascript
// Test script extracts ID from response
const body = pm.response.json();
pm.variables.set('createdUserId', body.id);
```

**Step 2: GET User By ID**
```
GET /users/{{createdUserId}}  // Automatically uses stored ID
```

**No hardcoding needed!** Each test can use data created by previous tests.

---

## 📊 Test Suites

### 🟢 Smoke Test Suite
**Duration:** ~2 minutes  
**Purpose:** Quick health check, critical path validation

**Tests:**
1. Verify environment connectivity
2. Test User CRUD (list users)
3. Test Post CRUD (get post)

**When to run:** Before deployments, hourly monitoring

```bash
newman run collection.json -e qa.json --folder "SMOKE TEST SUITE"
```

---

### 🟡 Regression Test Suite
**Duration:** ~5 minutes  
**Purpose:** Complete API validation, edge cases

**Tests:**
1. User CRUD (Create → Read → Update → Delete)
2. Post CRUD (Create → Read → Update → Delete)
3. Data consistency checks
4. Negative scenarios
5. Contract validation

**When to run:** Post-deployment, nightly builds

```bash
newman run collection.json -e qa.json --folder "REGRESSION TEST SUITE"
```

---

### 🟠 Negative Test Suite
**Duration:** ~3 minutes  
**Purpose:** Error handling, input validation

**Tests:**
1. Non-existent resources (404)
2. Missing required fields
3. Malformed JSON
4. Invalid ID formats
5. Unauthorized access

**When to run:** QA cycle, before release

```bash
newman run collection.json -e qa.json --folder "NEGATIVE SCENARIOS"
```

---

### 🔵 Contract Validation
**Duration:** ~2 minutes  
**Purpose:** Schema validation, data type assertions

**Tests:**
1. User schema validation
2. Post schema validation
3. Required field checks
4. Field type validation
5. Format validation (email, etc.)

**When to run:** After API spec changes, continuous

```bash
newman run collection.json -e qa.json --folder "CONTRACT VALIDATION"
```

---

### 📈 Performance Test Suite
**Duration:** ~1 minute  
**Purpose:** API health monitoring, SLA validation

**Tests:**
1. Single resource retrieval time < 500ms
2. List operations time < 1000ms
3. Payload size validation
4. Response time consistency

**When to run:** Every hour, performance baselines

```bash
newman run collection.json -e qa.json --folder "PERFORMANCE TEST SUITE"
```

---

## 🌐 Environment Switching

### QA Environment
```bash
# Run against QA
newman run collection.json -e qa.json

# Settings:
# - Response time: 2000ms (relaxed)
# - Debug logging: ON
# - All test suites: Enabled
# - Data reset: YES
```

### PROD Environment
```bash
# Run against PROD
newman run collection.json -e prod.json

# Settings:
# - Response time: 500ms (strict SLA)
# - Debug logging: OFF
# - Negative tests: DISABLED (stability)
# - Data reset: NO
```

### Adding New Environment

1. Duplicate `qa.json` → `staging.json`
2. Update `baseUrl`: `https://staging-api.example.com`
3. Adjust thresholds as needed
4. Run: `newman run collection.json -e staging.json`

---

## 🔧 Customization & Extension

### Adding New API Endpoint

**Example: Add "Comments" endpoint**

1. **Create folder** in collection: "COMMENTS"
2. **Add requests:**
   ```
   POST   /comments         (Create)
   GET    /comments/:id     (Read)
   GET    /comments?post=1  (List by Post)
   PUT    /comments/:id     (Update)
   DELETE /comments/:id     (Delete)
   ```

3. **Pre-request Script** (inherited from collection):
   ```javascript
   pm.variables.set('requestId', pm.variables.replaceIn('{{$guid}}'));
   ```

4. **Tests** (reuse patterns):
   ```javascript
   pm.test('Status: 201 Created', function() {
       pm.response.to.have.status(201);
   });
   const response = pm.response.json();
   pm.variables.set('createdCommentId', response.id);
   ```

5. **Add to Test Suites:** Duplicate POST/GET into REGRESSION folder

**Result:** New endpoint fully integrated, no framework changes!

---

### Adding New Test Suite

**Example: Add "Load Test" suite**

1. Create folder: "LOAD TEST SUITE"
2. Add 5-10 parallel requests
3. Assert response times
4. Generate load report

```bash
newman run collection.json -e qa.json \
  --folder "LOAD TEST SUITE" \
  -n 5  # Run 5 iterations
```

---

## 📋 Pre-request & Test Script Patterns

### Generate Unique Data

```javascript
// Generate random email
const suffix = Math.random().toString(36).substring(2, 8);
pm.variables.set('randomEmail', `test${suffix}@example.com`);

// Generate timestamp
pm.variables.set('timestamp', new Date().toISOString());

// Generate UUID
pm.variables.set('uuid', pm.variables.replaceIn('{{$guid}}'));
```

### Store Response Values

```javascript
const response = pm.response.json();

// Store single value
pm.variables.set('userId', response.id);

// Store object
pm.variables.set('userData', JSON.stringify(response));
```

### Reusable Test Assertions

```javascript
// Status code
pm.test('Status: 200 OK', function() {
    pm.response.to.have.status(200);
});

// Response time
pm.test('Response time < 1000ms', function() {
    pm.expect(pm.response.responseTime).to.be.below(1000);
});

// Content type
pm.test('JSON response', function() {
    pm.expect(pm.response.headers.get('Content-Type')).to.include('application/json');
});

// JSON schema
const schema = {
    type: 'object',
    properties: {
        id: { type: 'number' },
        name: { type: 'string' }
    },
    required: ['id', 'name']
};
pm.test('Schema validation', function() {
    pm.response.to.have.jsonSchema(schema);
});

// Array validation
pm.test('Response is array', function() {
    pm.expect(Array.isArray(pm.response.json())).to.be.true;
});
```

### Conditional Assertions

```javascript
// Skip test in PROD
if (pm.variables.get('environment') !== 'prod') {
    pm.test('Debug assertion', function() {
        // Test only in QA
    });
}

// Run only in specific environment
if (pm.variables.get('environment') === 'qa') {
    pm.test('QA-specific test', function() {
        pm.response.to.have.status(200);
    });
}
```

---

## 🔗 CI/CD Integration

### GitHub Actions

**File:** `.github/workflows/api-tests.yml`

```yaml
name: API Tests

on:
  schedule:
    - cron: '0 */6 * * *'  # Every 6 hours
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Install Newman
        run: npm install -g newman
      
      - name: Run QA Tests
        run: newman run collection.json -e qa.json -r json --reporter-json-export qa-results.json
        continue-on-error: true
      
      - name: Run PROD Smoke Tests
        run: newman run collection.json -e prod.json --folder "SMOKE TEST SUITE" -r json
        continue-on-error: true
      
      - name: Generate HTML Report
        if: always()
        run: newman run collection.json -e qa.json -r html --reporter-html-export report.html
      
      - name: Upload Reports
        if: always()
        uses: actions/upload-artifact@v3
        with:
          name: test-reports
          path: |
            qa-results.json
            report.html
      
      - name: Slack Notification
        if: failure()
        run: |
          curl -X POST ${{ secrets.SLACK_WEBHOOK }} \
            -H 'Content-Type: application/json' \
            -d '{"text":"API tests failed!"}'
```

**Usage:**
1. Add `.github/workflows/api-tests.yml` to repo
2. Set `SLACK_WEBHOOK` secret in GitHub Settings
3. Tests run automatically on schedule & push

---

### Jenkins Pipeline

**File:** `Jenkinsfile`

```groovy
pipeline {
    agent any
    
    environment {
        ENVIRONMENT = credentials('api-test-env')
    }
    
    stages {
        stage('Install') {
            steps {
                sh 'npm install -g newman'
            }
        }
        
        stage('QA Tests') {
            steps {
                sh '''
                    newman run collection.json \\
                      -e qa.json \\
                      -r cli,json \\
                      --reporter-json-export qa-results.json || true
                '''
            }
        }
        
        stage('PROD Smoke Tests') {
            steps {
                sh '''
                    newman run collection.json \\
                      -e prod.json \\
                      --folder "SMOKE TEST SUITE" \\
                      -r cli,html \\
                      --reporter-html-export prod-report.html || true
                '''
            }
        }
        
        stage('Performance Tests') {
            steps {
                sh '''
                    newman run collection.json \\
                      -e qa.json \\
                      --folder "PERFORMANCE TEST SUITE" \\
                      -n 3
                '''
            }
        }
    }
    
    post {
        always {
            junit 'qa-results.json'
            publishHTML([
                reportDir: '.',
                reportFiles: 'prod-report.html',
                reportName: 'API Test Report'
            ])
        }
        
        failure {
            emailext(
                subject: 'API Tests Failed',
                body: 'Check Jenkins for details',
                to: 'devops@company.com'
            )
        }
    }
}
```

**Setup:**
1. New Pipeline job in Jenkins
2. Point to `Jenkinsfile` in repo
3. Set credentials for environments
4. Enable post-build notifications

---

### Azure DevOps

**File:** `azure-pipelines.yml`

```yaml
trigger:
  - main

schedules:
  - cron: "0 */4 * * *"
    displayName: Every 4 hours
    branches:
      include:
        - main
    always: false

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: NodeTool@0
    inputs:
      versionSpec: '18.x'
  
  - script: npm install -g newman
    displayName: 'Install Newman'
  
  - script: |
      newman run collection.json \
        -e qa.json \
        -r junit \
        --reporter-junit-export qa-results.xml
    displayName: 'Run QA Tests'
    continueOnError: true
  
  - script: |
      newman run collection.json \
        -e prod.json \
        --folder "SMOKE TEST SUITE" \
        -r cli
    displayName: 'Run PROD Smoke Tests'
    continueOnError: true
  
  - task: PublishTestResults@2
    inputs:
      testResultsFormat: 'JUnit'
      testResultsFiles: 'qa-results.xml'
      testRunTitle: 'API Tests'
    condition: always()
```

---

## 📊 Reporting

### Generate Reports

```bash
# CLI + HTML Report
newman run collection.json -e qa.json \
  -r cli,html \
  --reporter-html-export report.html

# JSON Report (for CI/CD parsing)
newman run collection.json -e qa.json \
  -r json \
  --reporter-json-export results.json

# JUnit Report (Jenkins compatible)
newman run collection.json -e qa.json \
  -r junit \
  --reporter-junit-export results.xml

# Multiple reporters
newman run collection.json -e qa.json \
  -r cli,html,json,junit \
  --reporter-html-export report.html \
  --reporter-json-export results.json \
  --reporter-junit-export results.xml
```

### Report Analysis

HTML reports include:
- ✅ Request/Response details
- ✅ Test pass/fail status
- ✅ Response times
- ✅ Error messages
- ✅ Execution timeline

---

## 🐛 Debugging

### Enable Debug Mode

```javascript
// In globals or pre-request script
pm.variables.set('isDebugMode', 'true');

// Logs will appear in Postman Console (Ctrl/Cmd + Alt + C)
if (pm.variables.get('isDebugMode') === 'true') {
    console.log('Request:', pm.request.name);
    console.log('Variables:', pm.variables.toObject());
    console.log('Response:', pm.response.json());
}
```

### View Logs in Postman

1. Open **Postman Console** (Ctrl/Cmd + Alt + C)
2. Run a request
3. Logs appear in real-time

### View Logs in Newman CLI

```bash
# Verbose output
newman run collection.json -e qa.json --verbose

# With system info
newman run collection.json -e qa.json --disable-unicode
```

---

## 💡 Best Practices

### ✅ DO

- Use variables instead of hardcoding URLs/IDs
- Store response values for dependent requests
- Write descriptive request names and descriptions
- Include pre-request and test scripts in all requests
- Organize requests in logical folders
- Use meaningful variable names (testUserId not id)
- Comment complex logic
- Test edge cases and errors
- Version control your collection

### ❌ DON'T

- Hardcode URLs, tokens, or IDs
- Skip test assertions
- Ignore negative test cases
- Mix logic in requests (keep it in scripts)
- Create deeply nested folder structures (max 3 levels)
- Use unclear variable names (x, temp, data)
- Commit sensitive data (tokens, passwords)
- Ignore response times and performance

---

## 🚨 Troubleshooting

### Tests Pass Locally but Fail in CI

**Cause:** Environment variables not set correctly

**Solution:**
```bash
# Verify environment file has correct URLs
cat qa.json | grep baseUrl

# Check secrets in CI/CD
# GitHub: Settings → Secrets
# Jenkins: Credentials → Add
# Azure: Pipeline → Variables
```

### Response Times Too High

**Cause:** Network latency or server overload

**Solution:**
1. Increase threshold in environment: `maxResponseTime`
2. Check server health
3. Run during off-peak hours
4. Profile slow requests

### Variable Not Found Error

**Cause:** Variable set in wrong scope

**Solution:**
```javascript
// Variables are scoped: global > environment > collection > local
pm.globals.set('key', 'value');        // Global (all collections)
pm.environment.set('key', 'value');    // Environment-specific
pm.variables.set('key', 'value');      // Current scope
```

### Tests Skipped in Newman

**Cause:** Newman can't find the requests

**Solution:**
```bash
# Verify collection structure
newman run collection.json --dry-run

# Check folder names exactly
newman run collection.json \
  -e qa.json \
  --folder "REGRESSION TEST SUITE"  # Exact name match
```

---

## 📚 Additional Resources

- [Postman Documentation](https://learning.postman.com)
- [Newman CLI Docs](https://github.com/postmanlabs/newman)
- [JSONPlaceholder API](https://jsonplaceholder.typicode.com)
- [ReqRes API](https://reqres.in)

---

## 🤝 Contributing

To extend this framework:

1. **Add new endpoint** → Create folder, add CRUD requests
2. **Add new test suite** → Duplicate request, add to new folder
3. **Improve scripts** → Update pre-request or test logic
4. **Update docs** → Keep this README current

---

## 📄 License

This framework is provided as-is for portfolio purposes.

---

## 👤 Author

**Your Name** - SDET / Test Architect

- 📧 Email: your@email.com
- 💼 LinkedIn: linkedin.com/in/yourprofile
- 🐙 GitHub: github.com/yourprofile
- 🔗 Portfolio: yourportfolio.com

---

## 📝 Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-07-11 | Initial framework release |
| | | • 5 test suites (Smoke, Regression, Negative, Contract, Performance) |
| | | • JSONPlaceholder + ReqRes API integration |
| | | • QA and PROD environments |
| | | • CI/CD templates (GitHub Actions, Jenkins, Azure DevOps) |
| | | • Comprehensive documentation |

---

## 🎓 What This Framework Demonstrates

For **SDET / Test Architect** interviews:

✅ **Framework Architecture**
- Organized test structure
- Scalable folder hierarchy
- Modular request design

✅ **Testing Principles**
- CRUD operation validation
- Negative scenario testing
- Contract/schema validation
- Performance monitoring
- Error handling

✅ **Coding & Scripting**
- JavaScript in Postman
- Reusable helper functions
- Dynamic data generation
- Conditional logic

✅ **Configuration Management**
- Environment switching
- Variable management
- Secrets handling
- Multi-environment strategy

✅ **CI/CD Integration**
- GitHub Actions pipelines
- Jenkins jobs
- Azure DevOps workflows
- Report generation & analysis

✅ **Best Practices**
- Documentation
- Comments & descriptions
- Version control
- Error handling
- Extensibility

✅ **Design Patterns**
- DRY (Don't Repeat Yourself)
- SRP (Single Responsibility)
- Reusable components
- Maintainability first

**This is what separates "I know Postman" from "I can architect enterprise test frameworks."**

---

**Last Updated:** 2025-07-11  
**Status:** Production Ready  
**Framework Version:** 1.0
