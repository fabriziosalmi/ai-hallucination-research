# Empirical Analysis of AI-Induced Dependency Hallucinations

## Table of Contents
- [Overview](#overview)
- [Research Paper](#research-paper)
- [Mitigation Tool](#mitigation-tool)
- [Legal and Ethical Disclaimer](#legal-and-ethical-disclaimer)

## Overview
This repository hosts the academic research paper on AI-Induced Dependency Hallucinations and Phantom Dependency Squatting. The study investigates the systemic supply chain vulnerabilities introduced by Large Language Models (LLMs) generating fabricated software dependencies.

## Research Paper
The full, formatted academic paper is published via GitHub Pages. 

[Read the Full Research Paper Here](https://fabriziosalmi.github.io/ai-hallucination-research/)

## Mitigation Tool
As part of the research team's commitment to Responsible Disclosure, we provide a deterministic, zero-dependency mitigation tool. Developers seeking to protect Continuous Integration / Continuous Deployment (CI/CD) pipelines from Phantom Dependency Squatting should utilize the open-source GitHub Action.

[AI Dependency Guard (GitHub Marketplace)](https://github.com/fabriziosalmi/ai-dependency-guard)

## Legal and Ethical Disclaimer
This repository serves exclusively as an academic publication and documentation host.

- **No Exploitation:** The researchers simulated the attack surface but explicitly abstained from registering malicious payloads or phantom packages in public registries.
- **Anonymization:** The research team stripped all repository names, maintainer identities, and specific organizational targets evaluated during the Mean Time To Remediation (MTTR) study from this publication. We present all data strictly in aggregate to ensure comprehensive privacy and security.
- **Zero Liability:** The authors provide the research findings and associated mitigation tools "as-is" without warranty of any kind. 

For inquiries regarding the dataset or the automated remediation pipeline architecture, refer to the methodology section within the published paper.
