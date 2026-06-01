---
layout: ../layouts/PaperLayout.astro
title: Empirical Analysis of AI-Induced Dependency Hallucinations
---

# Empirical Analysis of AI-Induced Dependency Hallucinations in Public Version Control Systems

**Authors:** Fabrizio Salmi, *et al.*
**Date:** June 2026

---

<div class="abstract">
  <h2>Abstract</h2>
  <p>The integration of Large Language Models (LLMs) into the software development lifecycle introduces non-deterministic variables in code generation, particularly concerning dependency manifesting. This study conducts a large-scale empirical analysis of public version control repositories to quantify the prevalence of hallucinated dependencies in production manifests (e.g., <code>requirements.txt</code>, <code>package.json</code>). We present an anonymized dataset of 1,000 verified instances wherein non-existent package names—generated via LLM hallucination—were committed to public repositories. We propose a formal taxonomy for these artifacts: <em>Pure Fabrication</em>, <em>OS-to-Ecosystem Confusion</em>, <em>CLI-Flag Confusion</em>, and <em>Truncated Generation</em>. As these phantom packages remain unregistered, they constitute a latent attack surface for Phantom Dependency Squatting. To evaluate maintainer responsiveness, we conducted an observational study via an automated, ethically aligned remediation pipeline. This pipeline evaluates repository-level security policies (e.g., Private Vulnerability Reporting constraints) prior to issuing automated patches, establishing a baseline for Mean Time To Remediation (MTTR) within the open-source community.</p>
</div>

---

## 1. Introduction
Large Language Models (LLMs) are increasingly deployed as autonomous coding assistants to generate boilerplate structures, configurations, and dependency manifests. Driven by probabilistic token generation, LLMs frequently produce technically coherent but factually incorrect outputs, conventionally termed *hallucinations*. Within the context of dependency management, this phenomenon manifests as the explicit inclusion of non-existent software packages.

When a developer inadvertently commits an LLM-generated manifest containing a hallucinated package to a public repository, the project is rendered vulnerable to Phantom Dependency Squatting. A threat actor observing the public manifest may preemptively register the non-existent package in the targeted registry (e.g., PyPI, npm). Subsequent continuous integration (CI) builds or localized environment initializations of the affected repository will systematically download and execute the squatted package, potentially leading to arbitrary code execution. 

This paper provides rigorous empirical data on the frequency and structural nature of this specific supply chain vulnerability. Recognizing that cryptographic attribution of LLM authorship for public commits is currently infeasible, our dataset is restricted exclusively to deterministic hallucination signatures previously documented in controlled LLM generation environments.

**Our primary contributions are:**
1. **Dataset Generation:** Compilation and validation of an anonymized dataset of 1,000 occurrences of hallucinated dependencies within public repositories.
2. **Taxonomy:** A rigorous classification of the underlying LLM parsing and generation artifacts contributing to these anomalies.
3. **Observational Remediation Study:** An evaluation of open-source maintainer responsiveness to automated vulnerability disclosures, executed by an agent strictly bound by Responsible Disclosure and zero-liability protocols.

---

## 2. Methodology

### 2.1 Data Collection
Data collection was executed via the GitHub Code Search API over a 90-day window terminating in June 2026. Queries explicitly targeted dependency manifest filenames intersecting with a predefined seed list of known hallucination signatures. Because this methodology relies on deterministic seed queries, the dataset of 1,000 occurrences constitutes a strict lower bound of the phenomenon. The actual prevalence across the broader open-source ecosystem is hypothesized to be significantly higher.

### 2.2 Validation Protocol
To mitigate false positives (e.g., internal monorepo packages or proprietary artifact registries), a dual-phase validation protocol was enforced:
1. **AST & Diff Parsing:** Target package names were extracted through unified diff parsing and validated against the structural Abstract Syntax Tree (AST) of the manifest.
2. **Registry Verification:** Extracted package names were dynamically queried against the official APIs of the respective registries (PyPI and npm). An occurrence was designated as a verified hallucination strictly if the registry returned an HTTP 404 (Not Found) status.

### 2.3 Automated Vulnerability Disclosure Protocol
To measure maintainer response times (MTTR) without violating ethical security research standards, an automated disclosure agent was developed, governed by the following deterministic ruleset:
1. **Private Vulnerability Reporting (PVR) Check:** The agent interrogates the GitHub REST API (`POST /security-advisories/reports`) to ascertain if the target repository accepts private security advisories.
2. **Security Policy Verification:** If PVR is unavailable, the agent utilizes the GitHub GraphQL API to detect the presence of a repository `SECURITY.md` file (`isSecurityPolicyEnabled`). If present, the automated public disclosure process is deterministically aborted to respect the maintainers' explicit reporting guidelines.
3. **Public Remediation & Transparency:** If and only if both private reporting mechanisms are absent, the agent forks the repository, applies a structurally aware patch (e.g., deterministic dictionary key deletion for JSON, strict regular-expression line removal for plaintext manifests), and submits a public Pull Request. All communications include an explicit AI-transparency disclaimer, a zero-liability waiver, and a unique reproducibility UUID.

### 2.4 Ethical Framework & Legal Disclaimer
This research was conducted under the strictest ethical guidelines for cybersecurity research. The automated agent simulated the attack surface but explicitly abstained from registering any malicious payloads or phantom packages in public registries. All automated patches were provided "as-is" without warranty, shifting the burden of review and integration entirely to the downstream maintainers. Furthermore, all repository names, maintainer identities, and specific organizational targets have been stripped from this publication and presented strictly in aggregate to ensure comprehensive anonymization.

---

## 3. Data Analysis & Taxonomy

### 3.1 Distribution of Vulnerabilities
Within the sealed dataset of 1,000 verified occurrences, the distribution of hallucinated packages demonstrates systemic LLM confusion between valid command-line utilities, ecosystem-specific modules, and legitimate package namespaces. The anomalies map to the proposed taxonomy as follows:

1. **OS/Ecosystem-to-PyPI Confusion (41.5%):**
   The most prevalent category. LLMs consistently hallucinate that OS-level utilities (e.g., Ubuntu packages) or Conda-specific solvers are installable via Python package managers. 

2. **Truncated/Partial Generation (28.3%):**
   Occurs when the LLM generates the root namespace but drops the necessary suffix or URL parameter required for successful resolution.

3. **Pure Fabrication (25.4%):**
   Completely fictitious packages hallucinated by the model based on surrounding semantic context. 

4. **CLI-Flag Confusion (4.8%):**
   Syntactic parser errors wherein the LLM inserts Command-Line Interface (CLI) flags directly into the package manifest as discrete dependencies. While syntactically malformed, they pose a theoretical squatting risk if downstream package managers fail to sanitize hyphens.

### 3.2 Unintentional Registry-Level Mitigations
Empirical analysis revealed that public registries possess incidental, built-in defense mechanisms that inadvertently mitigate specific hallucination signatures:
- **Namespace Collision Protection:** Hallucinated packages that exactly match the username of an existing registry maintainer are blocked from registration to prevent impersonation (HTTP 400 Bad Request).
- **Trademark Blacklisting:** Hallucinated packages containing protected corporate prefixes return HTTP 403 Forbidden during upload attempts, indicating automated or manual administrative blocking.

While these mechanisms neutralize specific targets, they do not offer systemic protection against generic hallucinated namespaces, which remain fully susceptible to Phantom Dependency Squatting.

---

## 4. Observational Study on Automated Vulnerability Disclosure

To evaluate the feasibility of automated remediation and track MTTR, the disclosure agent was deployed against a randomized, anonymized subset of the dataset.

**Empirical Remediation Outcomes:**
- **Ethical Aborts (22%):** The agent successfully identified `SECURITY.md` policy files in multiple high-profile corporate repositories via the GraphQL API. The agent successfully aborted the public remediation process, validating its adherence to human-defined reporting boundaries.
- **Private Vulnerability Reporting (14%):** The agent successfully leveraged the REST API to submit confidential JSON payloads containing vulnerability details to repositories with PVR enabled, completely circumventing public exposure.
- **Strict Regex Rejection (34%):** To prevent false-positive patching or data loss, the agent utilized a strict regular expression requiring an exact match of the hallucinated package name. Ambiguous dependencies were safely bypassed.
- **Public Remediation (30%):** The agent successfully executed surgical removals and submitted public Pull Requests accompanied by the mandatory AI Transparency and Liability Waiver.

Active disclosures are currently monitored via their unique reproducibility IDs to calculate the aggregated MTTR for open-source maintainers.

## 5. Conclusion

This research confirms that AI-induced dependency hallucinations constitute a measurable and pervasive vulnerability within public version control manifests. By isolating 1,000 distinct occurrences of non-existent packages, we demonstrate that developers frequently integrate LLM-generated code without rigorous manual verification. While package registries provide unintentional mitigations for certain hallucination patterns, generic hallucinated names remain highly vulnerable to Phantom Dependency Squatting. 

The automated remediation study proves the technological viability of large-scale, ethically aligned vulnerability disclosure. By prioritizing Private Vulnerability Reporting and rigorously respecting repository security policies, automated security agents can mitigate supply chain risks without inducing alert fatigue or unsolicited automated disclosures. Future research will focus on the final MTTR metrics from the disclosure subset and evaluate the broader community adoption of AI-transparency waivers.
