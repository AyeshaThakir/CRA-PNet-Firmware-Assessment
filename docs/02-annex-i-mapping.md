# 02 — Annex I Requirements Mapping

Maps Annex I of Regulation (EU) 2024/2847 to the hypothetical PROFINET IO-Device firmware defined in [01-scope.md](01-scope.md).

Tags: **[V]** verified from a cited source, **[A]** assumption about the hypothetical product, **[P]** proposed control. This is a proposed mapping, not a claim that any real product implements these controls.

<!-- TODO before publishing: check every requirement wording and point number below against Annex I on EUR-Lex. Requirement text here is a short paraphrase, not a quotation. -->

Source: Regulation (EU) 2024/2847, Annex I — https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng

## Part I — Cybersecurity requirements relating to the properties of products

| Ref | Requirement (paraphrased) | What it means for this device [A] | Proposed control [P] | Evidence a manufacturer would hold |
|-----|---------------------------|-----------------------------------|----------------------|------------------------------------|
| I(1) | Appropriate level of cybersecurity based on the risks (the risk assessment is required by Art. 13(2)) | Security level must follow from a documented risk assessment of the device in its industrial setting | Threat model and risk register drive control selection (`05`, `06`) | Risk assessment, technical documentation |
| I(2)(a) | Placed on the market without known exploitable vulnerabilities | Firmware, including the P-Net version, must be checked against known vulnerabilities before release | Pre-release scan of components against vulnerability databases; block release on unresolved exploitable findings | Scan reports, release checklist |
| I(2)(b) | Secure-by-default configuration, with ability to reset to original state | Out-of-box device should not expose unneeded services or use default credentials | Minimal default services; factory reset that restores the secure baseline | Default-configuration spec, reset test record |
| I(2)(c) | Security updates, including automatic ones by default with opt-out | Device needs a way to receive and install firmware updates; users should be told an update exists and be able to postpone it temporarily | Signed firmware updates with verification on the device; update notification to the operator; operator-controlled, postponable install (see `[A]` on automatic updates below) | Update design, signing process, update test records |
| I(2)(d) | Protection against unauthorised access (authentication, identity and access management) and reporting of possible unauthorised access | Any management or configuration interface needs access control | Authenticated management access; no shared default credentials; logging of failed access | Access-control design, test records |
| I(2)(e) | Confidentiality of stored, transmitted and processed data (e.g. encryption) | Depends on what the device stores or sends; process data on the PROFINET link may be unencrypted by design | Identify sensitive data; encrypt where present; document where not applied and why | Data inventory, rationale record |
| I(2)(f) | Integrity of data, commands, programs and configuration against unauthorised manipulation, with reporting of corruption | Firmware and configuration must not be silently altered | Secure boot or signature check of firmware; integrity check of configuration | Integrity-mechanism design, test records |
| I(2)(g) | Process only necessary data (data minimisation) | Device should collect and keep only what its function requires | Define and limit stored and transmitted data | Data inventory |
| I(2)(h) | Protect availability of essential functions, including resilience against denial of service | The device's I/O function is its essential function; a crash is a loss of availability (relevant to CVE-2025-32405, see `03`) | Robust handling of malformed network input; watchdog and recovery to a safe state | Fuzz/robustness test plan and results, recovery design |
| I(2)(i) | Minimise negative impact on other devices or networks | A misbehaving device should not disrupt the wider industrial network | Rate-limit or bound network behaviour; fail to a quiet, safe state | Network behaviour test records |
| I(2)(j) | Limit attack surfaces, including external interfaces | Reduce exposed network services and interfaces | Disable unused services and ports; document each exposed interface | Interface and service list (feeds `05`) |
| I(2)(k) | Reduce impact of incidents using exploitation-mitigation mechanisms | Limit damage if a flaw is exploited | Compiler hardening, least-privilege process, memory-safety mitigations | Build configuration, hardening checklist |
| I(2)(l) | Security-related logging and monitoring, with opt-out | Device should record events relevant to security | Log security-relevant events locally; allow forwarding where the operator wants it | Logging spec, sample logs |
| I(2)(m) | Secure and easy removal of data and settings, and transfer to other products | Decommissioning or resale must not leak configuration | Secure erase function; documented decommissioning steps | Erase test record, user instructions |

### Assumptions to record in `07-limitations.md`

- **Automatic updates (I(2)(c)):** the CRA expects automatic security updates by default with an opt-out, unless this doesn't fit the product. In an industrial setting, automatic installation can be unsafe or unwanted. Here I assume the manufacturer would justify an operator-controlled install. Whether that justification is acceptable is not tested in this project. **[A]**
- **Confidentiality (I(2)(e)):** I do not assume PROFINET real-time process data is encrypted. Whether that is acceptable depends on the intended use and risk assessment. **[A]**
- **Everything above is a proposal.** None of it was tested or observed on a real device.

## Part II — Vulnerability-handling requirements

<!-- TODO before publishing: check every Part II point number and wording against Annex I Part II on EUR-Lex. -->

| Ref | Requirement (paraphrased) | What it means for this device [A] | Proposed control [P] | Evidence a manufacturer would hold |
|-----|---------------------------|-----------------------------------|----------------------|------------------------------------|
| II(1) | Identify and document components and vulnerabilities, including a machine-readable software bill of materials covering at least top-level dependencies | The firmware image contains Linux components, the P-Net stack and vendor code; each must be listed | Generate an SBOM per firmware build in a widely used machine-readable format (e.g. CycloneDX or SPDX) (see `04`) | SBOM file per release, component inventory |
| II(2) | Address and remediate vulnerabilities without delay, including by security updates; provide security updates separately from functional updates where technically feasible | A flaw in P-Net or elsewhere in the image must lead to a fixed build | Defined triage and fix process; security-only release path where feasible (see `03`) | Triage records, release notes separating security fixes |
| II(3) | Apply effective and regular tests and reviews of the product's security | Each release should be tested for known weaknesses, including malformed network input | Regular scanning, code review and robustness testing before release | Test plans, test reports, review records |
| II(4) | Once an update is available, publicly disclose information about fixed vulnerabilities (description, severity, affected products, impact, remediation help); delay is allowed only in duly justified cases, until users have had the chance to patch | Operators of installed devices need to know what was fixed and what to do | Publish a security advisory with each security update; document any justified delay | Published advisories, delay justification if used |
| II(5) | Put in place and enforce a coordinated vulnerability disclosure policy | Researchers and users need a defined route to report issues responsibly | Public vulnerability-disclosure policy covering how reports are handled and how fast | Published policy, handling procedure |
| II(6) | Facilitate sharing of information about potential vulnerabilities, including in third-party components, and provide a contact address for reports | Reports about P-Net or other components must reach the right party, and findings in them must be passed upstream | Public security contact; process to forward component findings to the component supplier (e.g. via RT-Labs' published channels **[A]**) | Contact page, upstream-reporting records |
| II(7) | Provide mechanisms to securely distribute updates, automatically where applicable | Devices must receive genuine updates only | Signed update packages over an authenticated channel; verification on the device | Update distribution design, signing procedure |
| II(8) | Disseminate security updates without delay and free of charge, with advisory messages on action to take | Fixes should reach users quickly, without a fee | Free security updates; advisory published with each; communication to registered customers | Release records, advisory messages, support-policy statement |

Link to the case study: CVE-2025-32405 (see `03`) exercises II(1), II(2), II(3), II(4), II(7) and II(8) end to end.
