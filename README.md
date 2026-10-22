# CRA Assessment of P-Net-Based PROFINET IO-Device Firmware

> **Independent educational project. Not certification, not legal advice, not a commercial compliance assessment, and not work performed for or with RT-Labs.** Nothing here states that RT-Labs or the P-Net stack is or is not compliant with the EU Cyber Resilience Act.

> **Status: work in progress.** Documents are added and updated regularly. The status table below shows what is finished and what is still planned.

## What this is

A structured assessment of how Regulation (EU) 2024/2847 (the Cyber Resilience Act) applies to a **hypothetical** commercially supplied PROFINET IO-Device firmware on embedded Linux that integrates the open [RT-Labs P-Net](https://github.com/rtlabs-com/p-net) stack. The focus is the part manufacturers find hardest in practice: turning Annex I into concrete product requirements, handling vulnerabilities in a third-party component, and being honest about what is evidenced versus assumed.

The assessment is document-based. No device was built or tested.

## Evidence convention

Each statement is one of:

- **[V] Verified**: stated by a cited public source.
- **[A] Assumption**: a deliberate modelling choice about the hypothetical product.
- **[P] Proposed control**: a measure a manufacturer could adopt, not a claim that any real product has it.

## Status

| # | Deliverable | File | Status |
|---|-------------|------|--------|
| 1 | Scope and product description | [docs/01-scope.md](docs/01-scope.md) | Draft |
| 2 | Annex I requirements mapping | [docs/02-annex-i-mapping.md](docs/02-annex-i-mapping.md) | Draft |
| 3 | CVE-2025-32405 vulnerability-handling case study | [docs/03-cve-2025-32405.md](docs/03-cve-2025-32405.md) | Draft |
| 4 | Hypothetical component inventory / SBOM | [docs/04-sbom.md](docs/04-sbom.md) | Draft |
| 5 | Threat model | [docs/05-threat-model.md](docs/05-threat-model.md) | Draft |
| 6 | Risk register | [docs/06-risk-register.md](docs/06-risk-register.md) | Draft |
| 7 | Limitations and assumptions | [docs/07-limitations.md](docs/07-limitations.md) | Draft |

Draft means the content is written but not yet checked against the Regulation text. Statuses change only when that check is done.

## Approach

1. Define the hypothetical product and the manufacturer's CRA position (`01`).
2. Map each Annex I Part I and Part II requirement to the product, with a proposed control and the evidence it would need (`02`).
3. Use a real, public P-Net vulnerability, CVE-2025-32405, to walk the Part II vulnerability-handling obligations end to end: identification, remediation, testing, disclosure, update distribution (`03`).
4. Build the inventory a manufacturer would need for Annex I Part II(1) (`04`).
5. Model threats to the IO-Device's network-facing surface (`05`) and carry them into a risk register (`06`).
6. State what this work cannot show (`07`).

## Repository layout

    .
    ├── README.md
    └── docs/
        ├── 01-scope.md
        ├── 02-annex-i-mapping.md
        ├── 03-cve-2025-32405.md
        ├── 04-sbom.md
        ├── 05-threat-model.md   
        ├── 06-risk-register.md
        └── 07-limitations.md

## Limitations

This is a desk assessment of a hypothetical product, built from public sources only. Product classification, intended use, and the manufacturer/component roles are stated assumptions. No P-Net code is included or redistributed. See [docs/01-scope.md](docs/01-scope.md) for scope and `docs/07-limitations.md` (planned) for the full list.

## Sources

- [CRA summary, European Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-summary)
- [CRA implementation FAQ, European Commission](https://digital-strategy.ec.europa.eu/en/library/cyber-resilience-act-implementation-frequently-asked-questions)
- [Regulation (EU) 2024/2847, EUR-Lex](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng)
- [P-Net repository](https://github.com/rtlabs-com/p-net) and [v1.0.2 release](https://github.com/rtlabs-com/p-net/releases/tag/public%2Fv1.0.2)
- [RT-Labs security advisories](https://rt-labs.com/cyber-security/) and [disclosure policy](https://rt-labs.com/about-rt-labs/our-policies/)
- [CVE-2025-32405, NVD](https://nvd.nist.gov/vuln/detail/CVE-2025-32405)

## How this was made

I used AI assistance (Claude) while building this project, to plan the structure, draft documents and suggest wording. The facts come from the public sources cited in each document: the regulation text, Commission guidance, NVD, and RT-Labs' public publications. I review and edit what is drafted, and I am responsible for the content. Anything not yet checked against the official text is marked with a TODO or tagged [A] (assumption). This project is a learning exercise: I am building my understanding of CRA and vulnerability handling as I go, and I have no professional experience handling CVEs.

## Licence

MIT. See [LICENSE](LICENSE).

## Author

Ayesha Thakir
