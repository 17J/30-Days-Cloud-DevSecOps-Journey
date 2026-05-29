# Day 17 -  Dynamic Application Security Testing (DAST)


Modern applications are not only source code.

They are running systems with:

* APIs
* Login flows
* Sessions
* Cookies
* Headers
* Forms
* Databases
* Business logic
* Cloud services

SAST checks code before execution.
SCA checks dependencies.
But **DAST checks the running application like an attacker would.**

---

## 🔗 Resources

* ** Support the Journey on GitHub:
	If you're following along, consider starring and forking the repo:**
    [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)


---

## What is DAST?

**DAST** stands for **Dynamic Application Security Testing**.

It tests a running application from the outside by sending requests and analyzing responses.

DAST does not need source code.

It works like a black-box security scanner.


![First Image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xgmse8f9iakqzs0jc27q.png)

---

## Why DAST Matters

DAST is important because many vulnerabilities only appear when the application is running.

Examples:

* Broken authentication
* Missing security headers
* Insecure cookies
* Runtime misconfiguration
* Exposed sensitive endpoints
* Server error leakage
* Reflected XSS
* SQL injection behavior
* Weak session handling

SAST may say the code looks fine, but DAST can reveal how the application behaves in real life.

---


![Dast](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5ptndfuu7i3r94r7964g.png)

A strong DevSecOps pipeline uses all three.

---

## Where DAST Fits in the Pipeline



![Dast ](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vqsko0emzywe62a1lvk9.png)

DAST usually runs after deployment to a test, staging, or pre-production environment.

---

## Popular DAST Tools

| Tool                 | Best For                                  |
| -------------------- | ----------------------------------------- |
| OWASP ZAP            | Open-source DAST automation               |
| Burp Suite           | Manual + automated web security testing   |
| Acunetix             | Enterprise web vulnerability scanning     |
| Invicti              | Enterprise DAST with proof-based scanning |
| Rapid7 InsightAppSec | Enterprise AppSec programs                |
| StackHawk            | Developer-first DAST                      |
| Nikto                | Basic web server scanning                 |

---

## OWASP ZAP: The Most Popular Open-Source DAST Tool

**OWASP ZAP**, also known as **Zed Attack Proxy**, is one of the most popular open-source DAST tools.

It can scan:

* Web applications
* APIs
* HTTP headers
* Cookies
* Authentication flows
* Forms
* Sessions
* Runtime misconfigurations

ZAP supports Docker-based automation, including baseline scans, full scans, and API scans. The official ZAP Docker documentation describes these packaged scans as useful for CI/CD automation. ([ZAP][1])

---

## OWASP ZAP Scan Types

## 1. Passive Scan

Passive scan observes traffic and responses.

It does not attack the application.

Good for:

* Dev environment
* CI/CD quick checks
* Production-safe checks

---

## 2. Active Scan

Active scan sends attack-like payloads to test vulnerabilities.

Good for:

* Pre-production
* Staging
* Security testing environment

Do not run aggressive active scans directly on production unless your team has approval.

---

## 3. Baseline Scan

ZAP Baseline Scan runs a spider against the target, waits for passive scanning to complete, and reports results. The official docs say it does not perform actual attacks and is ideal for CI/CD, even against production sites. ([ZAP][2])

---

## 4. Full Scan

ZAP Full Scan runs spidering and then performs an active scan. The official docs clearly mention that it performs actual attacks and can run for a long time. ([ZAP][3])

---

### Installing OWASP ZAP in Dev Environment

For local development, Docker is the easiest method.

## Step 1: Run Your Application Locally

Example:

```bash
docker run -d -p 8080:80 --name demo-nginx nginx
```

Your app is available at:

```text
http://localhost:8080
```

---

## Step 2: Run ZAP Baseline Scan

```bash
docker run --rm \
  -t ghcr.io/zaproxy/zaproxy:stable \
  zap-baseline.py \
  -t http://host.docker.internal:8080 \
  -r zap-dev-report.html
```

For Linux, use host networking:

```bash
docker run --rm \
  --network host \
  -t ghcr.io/zaproxy/zaproxy:stable \
  zap-baseline.py \
  -t http://localhost:8080 \
  -r zap-dev-report.html
```

---

## Step 3: Generate Report in Mounted Folder

```bash
mkdir zap-reports
```

```bash
docker run --rm \
  -v $(pwd)/zap-reports:/zap/wrk \
  -t ghcr.io/zaproxy/zaproxy:stable \
  zap-baseline.py \
  -t http://host.docker.internal:8080 \
  -r zap-dev-report.html
```

Report path:

```text
zap-reports/zap-dev-report.html
```

---

## OWASP ZAP in Pre-Production Environment

Pre-production is where DAST becomes more realistic.

Here you can run deeper scans because the app should behave close to production.

## Recommended Pre-Prod Approach

 

![dast image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z1tz7quc5a8oza5my7tb.png)

---

## Pre-Prod Full Scan Example

```bash
docker run --rm \
  -v $(pwd)/zap-reports:/zap/wrk \
  -t ghcr.io/zaproxy/zaproxy:stable \
  zap-full-scan.py \
  -t https://preprod.example.com \
  -r zap-preprod-report.html
```

Use full scan carefully because it performs active testing. ([ZAP][3])

---

## GitHub Actions Example for OWASP ZAP

```yaml
name: OWASP ZAP DAST Scan

on:
  workflow_dispatch:

jobs:
  zap_scan:
    runs-on: ubuntu-latest

    steps:
      - name: Create report directory
        run: mkdir -p zap-reports

      - name: Run OWASP ZAP Baseline Scan
        run: |
          docker run --rm \
            -v ${{ github.workspace }}/zap-reports:/zap/wrk \
            -t ghcr.io/zaproxy/zaproxy:stable \
            zap-baseline.py \
            -t https://preprod.example.com \
            -r zap-report.html

      - name: Upload ZAP Report
        uses: actions/upload-artifact@v4
        with:
          name: zap-report
          path: zap-reports/zap-report.html
```

---

## Example OWASP ZAP Report Output Explained

A ZAP report usually contains:

```text
Site
Risk Level
Confidence
Alert Name
Description
URL
Parameter
Evidence
Solution
Reference
CWE ID
WASC ID
```

Now let’s understand each section deeply.

---

## 1. Site

Example:

```text
Site: https://preprod.example.com
```

This shows the scanned target.

If multiple URLs or subdomains are scanned, ZAP groups findings by site.

---

## 2. Risk Level

Example:

```text
Risk: High
```

Risk shows severity.

Common levels:

| Risk          | Meaning                                            |
| ------------- | -------------------------------------------------- |
| High          | Critical issue that may lead to serious compromise |
| Medium        | Important issue requiring fix                      |
| Low           | Weakness with limited direct impact                |
| Informational | Security improvement or observation                |

---

## 3. Confidence

Example:

```text
Confidence: Medium
```

Confidence tells how sure ZAP is.

| Confidence     | Meaning                   |
| -------------- | ------------------------- |
| High           | Strong evidence           |
| Medium         | Likely issue              |
| Low            | Needs manual verification |
| False Positive | Not actually vulnerable   |

Risk and confidence should be read together.

---

## 4. Alert Name

Example:

```text
Alert: Content Security Policy Header Not Set
```

This is the vulnerability or weakness name.

Common ZAP alerts:

* Missing Content Security Policy
* X-Frame-Options Header Not Set
* Cookie Without Secure Flag
* Cookie Without HttpOnly Flag
* Server Leaks Version Information
* Cross-Site Scripting
* SQL Injection
* Directory Browsing
* Absence of Anti-CSRF Tokens

---

## 5. Description

Example:

```text
Content Security Policy header is missing.
```

This explains what ZAP found and why it matters.

For CSP, the issue means the browser is not being instructed to restrict untrusted scripts, styles, frames, or external content.

---

## 6. URL

Example:

```text
URL: https://preprod.example.com/login
```

This shows exactly where the issue was detected.

This helps developers reproduce the issue.

---

## 7. Parameter

Example:

```text
Parameter: session_id
```

This appears when the vulnerability is linked to a query parameter, form field, cookie, or header.

---

## 8. Evidence

Example:

```text
Evidence: Set-Cookie: session_id=abc123
```

Evidence shows what ZAP observed in the HTTP request or response.

For example:

```text
Set-Cookie header found without Secure flag
```

This means the session cookie may be sent over insecure HTTP if misconfigured.

---

## 9. Solution

Example:

```text
Set the Secure and HttpOnly flags on sensitive cookies.
```

This section gives remediation guidance.

For developers, this is usually the most useful part of the report.

---

## 10. Reference

ZAP includes links to external documentation, OWASP references, CWE pages, or vulnerability explanations.

Use these links to understand impact and remediation.

---

## 11. CWE ID

Example:

```text
CWE-693
```

CWE means **Common Weakness Enumeration**.

It categorizes the weakness type.

---

## 12. WASC ID

WASC is another classification system for web application security issues.

It is less commonly used today, but ZAP may still include it in reports.

---

## Sample ZAP Report Finding

```text
Alert: Cookie Without Secure Flag
Risk: Medium
Confidence: High
URL: https://preprod.example.com/login
Parameter: session_id
Evidence: Set-Cookie: session_id=abc123; Path=/; HttpOnly
Description:
A cookie was set without the Secure flag.

Impact:
If the application is accessed over HTTP, the cookie may be transmitted without encryption.

Solution:
Set the Secure flag for all sensitive cookies.

Example:
Set-Cookie: session_id=abc123; Path=/; Secure; HttpOnly; SameSite=Lax
```

---

## How to Fix Common ZAP Findings

### Missing Security Headers

Add headers such as:

```text
Content-Security-Policy
X-Frame-Options
X-Content-Type-Options
Referrer-Policy
Permissions-Policy
Strict-Transport-Security
```

---

## Cookie Without Secure Flag

Bad:

```text
Set-Cookie: session_id=abc123; Path=/; HttpOnly
```

Good:

```text
Set-Cookie: session_id=abc123; Path=/; Secure; HttpOnly; SameSite=Lax
```

---

## Server Version Disclosure

Bad:

```text
Server: Apache/2.4.49
```

Better:

```text
Server: web
```

---

## Missing CSRF Protection

Fix using:

* CSRF tokens
* SameSite cookies
* Origin header validation
* Framework-level CSRF protection

---

## Best Practices for DAST

* Run baseline scans in CI/CD.
* Run full scans only in staging or pre-prod.
* Never scan unauthorized systems.
* Use authenticated scans for real coverage.
* Exclude logout and destructive endpoints.
* Tune false positives.
* Combine DAST with SAST and SCA.
* Track risk trends over time.
* Fail builds only on verified high-risk issues.

---

## Final Thoughts

DAST is a critical layer of modern application security.

It answers a very important question:

```text
How does my application behave when attacked from the outside?
```

SAST finds insecure code.
SCA finds risky dependencies.
DAST finds runtime security weaknesses.

For DevSecOps teams, OWASP ZAP is one of the best starting points because it is open-source, automation-friendly, Docker-friendly, and suitable for CI/CD security testing.
