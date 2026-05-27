Day 16 - Static Application Security Testing (SAST)

Modern applications move fast. Developers push code daily, CI/CD pipelines deploy automatically, and cloud-native systems scale instantly.

But speed without security creates risk.

One insecure line of code can become:

```text
Bad input validation
        ↓
SQL Injection
        ↓
Database leak
        ↓
Production breach
```

This is where **Static Application Security Testing (SAST)** becomes important.

---

## 🔗 Resources

- ** Support the Journey on GitHub:
  If you're following along, consider starring and forking the repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

---

## What is SAST?

**SAST** stands for **Static Application Security Testing**.

It is a security testing method that scans source code, bytecode, or binaries **without running the application**.

SAST helps detect issues like:

- SQL Injection
- Cross-Site Scripting
- Hardcoded secrets
- Insecure authentication logic
- Weak cryptography
- Command injection
- Code smells
- Maintainability problems

Think of SAST as:

```text
Security review before the application runs
```

---

## Why SAST Matters

SAST is important because it finds security problems early in the SDLC.

Fixing bugs early is cheaper than fixing them after production deployment.

![SAST image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/10rqibyoyd0gtj1c6yvu.png)

In modern DevSecOps, SAST is part of **shift-left security**.

That means security moves closer to developers instead of waiting until the end.

---

## Market Analysis: Why SAST is Growing

The SAST market is growing because companies are building more web, mobile, API, cloud-native, and AI-generated applications. One market report estimates the SAST market at **USD 0.55B in 2025**, growing to **USD 1.89B by 2031** at a **22.82% CAGR**. ([Mordor Intelligence][1])

The broader Application Security Testing market is also expanding. MarketsandMarkets projects it to grow from **USD 1.83B in 2025** to **USD 7.60B by 2031**. ([MarketsandMarkets][2])

Gartner also notes that AI, modern application design, and software supply chain risks are expanding the AST market scope. ([Gartner][3])

---

## Where SAST Fits in DevSecOps Pipeline

![Full Image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l7jlytpqo0rj108qej48.png)

SAST should run at:

- IDE level
- Pull request level
- CI/CD pipeline level
- Before production deployment

---

## Popular SAST Tools

| Tool          | Best For                                       |
| ------------- | ---------------------------------------------- |
| SonarQube     | Code quality + security scanning               |
| Semgrep       | Fast custom rules and developer-first scanning |
| Checkmarx     | Enterprise SAST                                |
| Fortify       | Large enterprise security programs             |
| Veracode      | Cloud-based AppSec platform                    |
| GitHub CodeQL | GitHub-native code scanning                    |

---

## Why SonarQube is Popular

SonarQube is popular because it combines:

- Security issues
- Reliability issues
- Maintainability issues
- Code smells
- Duplications
- Test coverage
- Quality gate

SonarQube is not only a security scanner. It is a complete **code quality and security platform**.

---

## How to Install SonarQube for Development

For local development, Docker is the easiest option. SonarSource recommends Docker Engine 20.10+ for Docker-based installation. ([docs.sonarsource.com][4])

```bash
docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  sonarqube:community
```

Open:

```text
http://localhost:9000
```

Default login:

```text
username: admin
password: admin
```

---

## SonarQube with Docker Compose

```yaml
version: "3.8"

services:
  sonarqube:
    image: sonarqube:community
    container_name: sonarqube
    ports:
      - "9000:9000"
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_logs:/opt/sonarqube/logs
      - sonarqube_extensions:/opt/sonarqube/extensions

volumes:
  sonarqube_data:
  sonarqube_logs:
  sonarqube_extensions:
```

Run:

```bash
docker compose up -d
```

---

## Production Setup Approach

For production, do not run SonarQube like a temporary local container.

Use:

- Dedicated VM or Kubernetes
- External PostgreSQL database
- Persistent volumes
- Reverse proxy with HTTPS
- Backup strategy
- Authentication integration
- CI/CD scanner integration

SonarQube uses embedded Elasticsearch, so the host must satisfy Elasticsearch production requirements such as proper file descriptors and VM settings. ([Docker Hub][5])

Example Linux tuning:

```bash
sudo sysctl -w vm.max_map_count=524288
sudo sysctl -w fs.file-max=131072
ulimit -n 131072
ulimit -u 8192
```

---

## SonarQube Report Explained Deeply

![Sonarqube Report](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d4e4lgvff0gnqthlc57a.png)

Your report shows these key areas:

---

## 1. Security — 2 Open Issues — Grade E

This means SonarQube found **2 security-related issues** in the code.

Grade **E** is poor and means security needs attention.

Security issues may include:

- SQL injection risk
- Hardcoded credentials
- Weak cryptography
- Unsafe file handling
- Insecure random generation
- Improper access control

This should be treated as high priority.

---

## 2. Reliability — 5 Open Issues — Grade C

Reliability means bugs that may cause the application to fail.

Examples:

- Null pointer risk
- Unhandled exception
- Broken logic
- Resource leaks
- Incorrect conditions

Grade **C** means the code is not terrible, but it needs improvement.

---

## 3. Maintainability — 34 Open Issues — Grade A

This section shows **34 maintainability issues**, but the grade is still **A**.

That means issues exist, but the technical debt is still acceptable.

Maintainability issues include:

- Code smells
- Long methods
- Duplicate logic
- Complex functions
- Poor naming
- Unused variables

---

## 4. Accepted Issues — 0

Accepted issues are valid issues that the team decided not to fix.

Your report shows:

```text
Accepted Issues: 0
```

That is good.

It means no issue has been ignored or accepted as risk yet.

---

## 5. Coverage — 0.0%

This is a major problem.

Coverage means how much code is covered by automated tests.

Your report shows:

```text
Coverage: 0.0%
On 1.1k lines to cover
```

This means SonarQube did not detect test coverage data.

Possible reasons:

- No unit tests exist
- Tests exist but coverage report is not connected
- JaCoCo / Jest / coverage tool is not configured
- CI pipeline is not uploading coverage report

For production projects, try to reach at least:

```text
70% to 80% coverage
```

---

## 6. Duplications — 1.5%

This is good.

Duplication means repeated code.

Your report shows:

```text
Duplications: 1.5%
On 7.2k lines
```

This means only a small part of the codebase is duplicated.

Less duplication means:

- Cleaner code
- Easier maintenance
- Fewer repeated bugs

---

## 7. Security Hotspots — 2 — Grade E

Security Hotspots are not always confirmed vulnerabilities.

They are areas where SonarQube says:

```text
Human review required
```

Examples:

- Use of weak encryption
- CORS configuration
- File upload logic
- Authentication logic
- Token handling
- Cookie security

Because your hotspot grade is **E**, these should be reviewed carefully.

---

## What This Report Means Overall

Your project has:

```text
Security: Poor
Reliability: Medium
Maintainability: Good
Coverage: Very Poor
Duplication: Good
Security Hotspots: Needs Review
```

Main action plan:

1. Fix 2 security issues first.
2. Review 2 security hotspots.
3. Fix reliability bugs.
4. Add unit tests and connect coverage.
5. Reduce maintainability issues slowly.

---

## SonarQube Quality Gate Strategy

A strong quality gate should fail the build when:

- New security issue appears
- New critical bug appears
- Coverage is below threshold
- Duplication is too high
- Maintainability rating drops

Example policy:

```text
Security Rating must be A
Reliability Rating must be A
Coverage must be above 80%
Duplications must be below 3%
```

---

## GitHub Actions Example with SonarQube

```yaml
name: SonarQube Scan

on:
  push:
    branches: [main]
  pull_request:

jobs:
  sonarqube:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: SonarQube Scan
        uses: SonarSource/sonarqube-scan-action@v5
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
```

---

## Best Practices for SAST

- Run SAST on every pull request.
- Block critical vulnerabilities before merge.
- Do not ignore security hotspots.
- Add test coverage reports.
- Use quality gates.
- Tune false positives.
- Educate developers using report findings.
- Combine SAST with SCA and DAST.

---

## SAST vs SCA vs DAST

![SCA SAST](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l8ygzkpkgyrmmo49oopf.png)

SAST is powerful, but it should not work alone.

---

## Final Thoughts

SAST is one of the most important parts of DevSecOps.

It helps teams detect insecure code before it reaches production.

SonarQube makes this easier by combining security, reliability, maintainability, duplication, and coverage into one dashboard.

Your current SonarQube report clearly shows that the biggest focus areas are:

```text
Security Issues
Security Hotspots
Test Coverage
Reliability Issues
```
