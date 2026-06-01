# AI-Induced Dependency Hallucinations: Empirical Research

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Live-blue?style=for-the-badge)](https://fabriziosalmi.github.io/ai-hallucination-research/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

This repository hosts the empirical research paper: **"Empirical Analysis of AI-Induced Dependency Hallucinations in Public Version Control Systems"**. 

The research quantifies the prevalence of "Phantom Dependency Squatting," a critical supply-chain vulnerability introduced by Large Language Models (LLMs) hallucinating non-existent software packages in production manifests (e.g., `requirements.txt`, `package.json`).

## Abstract

The integration of Large Language Models (LLMs) into the software development lifecycle introduces non-deterministic variables in code generation, particularly concerning dependency manifesting. This study conducts a large-scale empirical analysis of public version control repositories to quantify the prevalence of hallucinated dependencies in production manifests. 

We present an anonymized dataset of 1,000 verified instances wherein non-existent package names—generated via LLM hallucination—were committed to public repositories. We propose a formal taxonomy for these artifacts: *Pure Fabrication*, *OS-to-Ecosystem Confusion*, *CLI-Flag Confusion*, and *Truncated Generation*. 

## Research Access

The full research paper is published and dynamically rendered via Astro on GitHub Pages:
**[Read the Paper](https://fabriziosalmi.github.io/ai-hallucination-research/)**

## Related Projects

This research directly informs the creation of **[AI Dependency Guard](https://github.com/marketplace/actions/ai-dependency-guard)**, an automated GitHub Action designed to block CI/CD pipelines when hallucinated packages are detected in PR manifests.

## Local Rendering

The paper is built using [Astro](https://astro.build/) for optimal typography and reading accessibility.

```bash
# Install dependencies
npm ci

# Start the local development server
npm run dev

# Build the static site
npm run build
```

## Ethical Framework

This research was conducted under the strictest ethical guidelines for cybersecurity research. All data is presented in aggregate, and specific organizational targets have been stripped to ensure comprehensive anonymization.

## License

This project is licensed under the MIT License.
