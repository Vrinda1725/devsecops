# OWASP ZAP Integration with GitHub Actions

## Overview
This repository integrates **OWASP ZAP (Zed Attack Proxy)** into a GitHub Actions workflow to perform **automated security scanning** of a web application whenever a push event occurs. OWASP ZAP is an open-source security scanner that helps identify vulnerabilities in web applications, such as:

- SQL Injection
- Cross-Site Scripting (XSS)
- Broken Authentication
- Security Misconfigurations
- Sensitive Data Exposure

## How the Workflow Works
The GitHub Actions workflow (`.github/workflows/owasp-zap.yml`) automates the security scanning process by:

1. **Checking out the repository** to access necessary files.
2. **Running OWASP ZAP Full Scan** inside a Docker container.
3. **Scanning the target web application** using OWASP ZAP.
4. **Applying custom rules** (if defined) to manage scan results.
5. **Logging and reporting vulnerabilities** found during the scan.

## Workflow Configuration

The OWASP ZAP scanning workflow is defined as follows:

```yaml
      - name: ZAP Scan
        uses: zaproxy/action-full-scan@v0.12.0
        with:
          docker_name: 'ghcr.io/zaproxy/zaproxy:stable'
          target: 'http://testphp.vulnweb.com/'
          rules_file_name: '.zap/rules.tsv'
          cmd_options: '-a'
```

### Inputs
- **Trigger:** Runs automatically on each `push` event.
- **ZAP Scan Action:** Uses [`zaproxy/action-full-scan`](https://github.com/zaproxy/action-full-scan) to scan the target.
- **Target URL:** The web application to be scanned (currently `http://testphp.vulnweb.com/`).
- **Rules File:** `.zap/rules.tsv` can be used to modify alert rules and ignore false positives.
- **Command Options (`-a`)**: Runs in **aggressive mode** for a more thorough scan.

## How OWASP ZAP Scans for Vulnerabilities

1. **Spidering & Crawling:**
   - ZAP first **crawls** the target website to discover pages and input fields.
2. **Active Scanning:**
   - Sends **active payloads** to form fields, URLs, and headers to detect vulnerabilities.
3. **Passive Scanning:**
   - Analyzes HTTP responses for security weaknesses **without sending active attacks**.
4. **Reporting & Alerts:**
   - Generates a report listing discovered vulnerabilities and their severity levels (e.g., High, Medium, Low, Informational).

## How to View the Results
After the workflow completes, the scan results can be accessed via:

- **GitHub Actions Logs**

## Summary
Integrating OWASP ZAP into GitHub Actions enhances **security automation** by identifying vulnerabilities early in the development cycle. By regularly scanning your web application, you can mitigate risks and improve security posture before deployment.

For more details, refer to the [OWASP ZAP Documentation](https://www.zaproxy.org/docs/).

