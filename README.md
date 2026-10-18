<h1 align="center">Hleb Haushyn</h1>

<p align="center">
  <b>DevSecOps Engineer · Software Supply Chain Security</b><br>
  <sub>Defensive security background. I build pipelines that prove what they ship.</sub>
</p>

<p align="center">
  <a href="https://www.linkedin.com/in/hleb-h-0660b9242/"><img src="https://img.shields.io/badge/LinkedIn-Hleb%20Haushyn-0A66C2?style=flat-square&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
  <img src="https://img.shields.io/badge/Focus-Supply%20Chain%20Security-1f2937?style=flat-square" alt="Focus">
  <img src="https://img.shields.io/badge/Location-Belarus-1f2937?style=flat-square" alt="Location">
  <img src="https://img.shields.io/badge/Open%20to-DevSecOps%20%2F%20Security%20Engineering-166534?style=flat-square" alt="Open to work">
</p>

---

## About

I work on **software supply chain security**: how to know that an artifact was built by the pipeline you trust, from the source you reviewed, with dependencies you can account for.

My approach is hands-on. I build a control, then deliberately attack it, because reading about a mitigation is not the same as watching it fail under real conditions. My foundation is defensive security: I trained in threat hunting, incident response, and detection engineering (MITRE ATT&CK, Elastic SIEM) through hands-on SOC labs, so I don't only implement controls; I think like the attacker they are meant to stop.

---

## Supply Chain Security Projects

### [Software Supply Chain Security Lab](https://github.com/cbrkrtek/supply-chain-security-lab)
`Cosign/Sigstore` `Syft` `Grype` `Trivy` `Kyverno` `ArgoCD` `Trivy Operator` `GitHub Actions`

- **Pipeline:** automated CI/CD flow (Lint → Trivy → Cosign sign → SBOM → attest), migrated to fully **keyless signing via GitHub Actions OIDC**, eliminating long-lived private keys.
- **Remediation:** reduced CRITICAL Trivy findings from **3 to 0** by removing root causes instead of suppressing alerts; documented 2 CVEs with EPSS 99.99% as *not affected* via **VEX**.
- **Triage:** Python script for batch vulnerability prioritization using the EPSS API, ranking by exploitation probability rather than static CVSS.
- **Admission control:** Kyverno `verifyImages` policy tied to a specific GitHub OIDC identity, blocking unsigned deployments before Pod creation.
- **GitOps & runtime:** ArgoCD + Kyverno, with documented `Sync: Failed` cases showing that GitOps status alone does not guarantee workload security; Trivy Operator for runtime drift detection.

### [SLSA Build Provenance & Verification Lab](https://github.com/cbrkrtek/slsa-provenance-lab)
`slsa-github-generator` `slsa-verifier` `Cosign` `GitHub Actions` `GHCR`

- **Attestation:** SLSA Build Level 3 provenance via an isolated, OIDC-authenticated reusable workflow, tracing an image from source commit to CI builder identity.
- **Attack simulation:** simulated stolen registry credentials by pushing an image to GHCR outside the pipeline; `slsa-verifier` correctly rejected the digest lacking provenance.
- **Verifier internals:** confirmed that `--provenance-path` for container images still validates registry-attested provenance rather than a local file, a key difference from Cosign's non-enforced signature checks.

### [Dependency Confusion Lab](https://github.com/cbrkrtek/supply-chain-dependency-confusion)
`Nexus Repository OSS` `npm` `pip` `Docker Compose`

- **Attack:** reproduced dependency confusion against a Nexus Group Repository (private and public registries behind one URL), where npm/pip resolved an attacker-controlled higher-version package over the internal one.
- **Defense:** verified npm scoped packages and pip `--index-url` pinning as effective client-side mitigations.
- **Finding:** Nexus server-side **Routing Rules silently fail** in hosted+hosted group topologies, so client-side scoping is the only reliable control in this common architecture.

---

## Technical Skills

| Area | Tools |
|---|---|
| **Supply Chain Security** | Cosign (Keyless OIDC), Sigstore, SLSA Provenance, SBOM (Syft, CycloneDX), Grype, VEX, Nexus Repository OSS |
| **DevSecOps & CI/CD** | GitHub Actions, ArgoCD (GitOps), Shift-Left Security Gates, SAST (Bandit), SCA (Trivy), DAST (OWASP ZAP), Secret Detection (Gitleaks) |
| **Kubernetes Security** | kubectl, Helm, RBAC (Least-Privilege), NetworkPolicy (Zero Trust), Kyverno (Policy-as-Code), Trivy Operator, securityContext |
| **Infrastructure & IaC** | Terraform (Modules, Workspaces, Remote State), Terragrunt, Ansible, Packer, AWS, Yandex Cloud |
| **Containers & OS** | Docker (Multi-stage, Non-root), Linux (Ubuntu/Debian), Linux Hardening (Auditd, Fail2Ban, UFW), Nginx, Windows Server |
| **Monitoring & SIEM** | ELK Stack, Splunk, Wazuh, Prometheus + Grafana, Loki, Sysmon, Velociraptor, TheHive, Cortex SOAR |
| **Scripting & Network** | Python (Pandas), Bash, Wireshark, Tcpdump, Snort IDS, NGFW |

---

## Other Hands-On Work

<details>
<summary><b>Kubernetes Security & Cluster Hardening</b></summary>

`kind` `kubectl` `Trivy` `Kyverno` `NetworkPolicy` `RBAC` `securityContext` `Helm`

- Reduced Trivy findings from 17 to 7 (LOW only) with `runAsNonRoot`, `readOnlyRootFilesystem`, and `seccompProfile`.
- Identified a wildcard ClusterRole vulnerability invisible to standard `kubectl` (flagged as 2x CRITICAL by Trivy) and enforced 7 Kyverno admission policies in Enforce mode.
- Implemented deny-all-first NetworkPolicy; found and mitigated DNS-leak and `hostNetwork: true` bypasses at the Kyverno admission level.
</details>

<details>
<summary><b>IaC DevSecOps Pipeline</b></summary>

`Ansible` `Prometheus` `Grafana` `Docker` `GitHub Actions` `Trivy` `Bandit` `Gitleaks` `Auditd`

- Docker Compose environment with Redis isolated in internal bridge networks, no exposed host ports.
- Node Exporter via idempotent Ansible playbook under Systemd; Prometheus + Grafana stack; Bash baseline hardening (SSH key-only, sudo restrictions, UFW default deny).
- Multi-stage pipeline with blocking gates for secrets, SAST, linting, and HIGH/CRITICAL vulnerabilities; non-root UIDs and Auditd integrity rules.
</details>

<details>
<summary><b>DevSecOps Pipeline: OWASP Juice Shop</b></summary>

`Trivy` `OWASP ZAP` `NGINX` `GitHub Actions` `CycloneDX`

- Detected 63 CVEs via Trivy and generated a CycloneDX SBOM; found hardcoded RSA keys and JWTs in Docker layers that source-only SAST missed; confirmed SQLi and Stored XSS with ZAP.
- Mitigated CORS, CSP, and X-Frame-Options issues at the Nginx reverse-proxy layer without changing application code.
</details>

<details>
<summary><b>Terraform Deep Dive & Production Patterns</b></summary>

`Terraform` `Terragrunt` `Yandex Cloud` `GitHub Actions`

- Zero Trust network: private subnets with egress-only NAT routing, highly available L7 load balancer across 2 AZs, least-privilege Security Groups.
- "Plan-on-PR" pipeline with dry-runs in PR comments and OIDC auth (no static keys); pre-deploy gates (TFLint, Trivy IaC) blocking open admin ports.
</details>

<details>
<summary><b>SOC & Detection Hands-On Labs (Background)</b></summary>

`Velociraptor` `TheHive` `Cortex SOAR` `Elastic SIEM` `MITRE ATT&CK` `Python`

- Hands-on end-to-end incident investigations with automated response and NIST/SANS-aligned reporting.
- Python tools for Nginx/Apache log parsing using Shannon entropy and GeoIP for attack risk assessment.
- Multi-stage attack hunting in hybrid infrastructure with Elastic SIEM (KQL); 5 correlation rules for brute-force, SMB access, and C2 traffic.
</details>

---

## Certifications

- **Developing Secure Software (LFD121)**, The Linux Foundation, Oct 2026
- **SEC1 Certification**, TryHackMe, Feb 2026 (valid until Feb 2029)

---

## Currently

Deepening my work in software supply chain security: build integrity, provenance verification, and admission-time enforcement.

Open to **DevSecOps / Security Engineering** roles where *secure by default* is a design goal, not an afterthought.

<p align="center">
  <sub>Reach me on <a href="https://www.linkedin.com/in/hleb-h-0660b9242/">LinkedIn</a></sub>
</p>
