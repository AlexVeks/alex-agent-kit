---
name: xray-security-scan
description: Run a JFrog Xray scan with the `jf` CLI and report only Security Violations of Critical or High severity as a table sorted by severity. Use when the student asks for an Xray/JFrog security scan or vulnerability check of a project.
---

# Xray Security Scan

Goal: run `jf audit`, keep only **Security** violations with severity **Critical** or **High**, and present them as one table.

## Steps

1. Preflight (read-only):
   - `jf --version`. If `jf` is missing, stop and tell the student to install the JFrog CLI. Do not install it yourself.
   - `jf config show`. If no server is configured, stop and tell the student to run `jf config add`. Never ask for or print tokens or passwords.
2. Run the scan from the project root, as JSON so it can be parsed reliably:
   `jf audit --format=simple-json` (add `--project=<key>` or `--watches=<w1,w2>` if the student provides them; violations are only reported when a watch or project policy applies).
   - Run `jf audit --help` first if a flag is rejected; flags vary between CLI versions.
   - Do not use `--vuln` (it shows all vulnerabilities, not only violations).
   - Save large output to `/tmp/opencode/xray-scan.json` and parse that file.
3. From the JSON, take only `securityViolations`. Ignore `licensesViolations`, `operationalRiskViolations`, and plain `vulnerabilities`.
4. Keep only entries whose severity is `Critical` or `High`.
5. Print a single Markdown table with exactly these columns, in this order:

| SEVERITY | DIRECT DEPENDENCY | DIRECT DEPENDENCY VERSION | IMPACTED DEPENDENCY NAME | IMPACTED DEPENDENCY VERSION | FIXED VERSIONS | CVE |
|---|---|---|---|---|---|---|

   - Do NOT include the TYPE or WATCH NAME columns.
   - Sort: Critical rows first, then High. Within a severity, sort by impacted dependency name.
   - DIRECT DEPENDENCY is the first component of the impact path; if the impacted dependency is itself direct, repeat it.
   - Empty values: write `-`. Multiple CVEs or fixed versions: comma-separated in one cell.
6. Under the table give one line: counts of Critical and High, and the scan command that was run. If there are no matching findings, say so plainly instead of an empty table.

## Rules

- Report only what the command actually returned. If the scan fails or the JSON has no violations section, show the error/what was missing and do not invent findings.
- Do not modify project files or dependencies to fix findings. Suggest the fixed version as the student's next step and let them make the change.
- Never claim the project is "secure" from a clean result; it only means no Critical/High security violations matched the applied watches/policies.
