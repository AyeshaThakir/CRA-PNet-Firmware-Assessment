# 01 — Assessment Scope

## 1. Purpose

This document defines what is assessed, from whose perspective, and on what evidence. It is an independent, educational exercise: a structured reading of Regulation (EU) 2024/2847 (the Cyber Resilience Act, CRA) against a hypothetical product built around the RT-Labs P-Net PROFINET IO-Device stack.

It is not a certification, a conformity assessment, legal advice, or work performed for or with RT-Labs. It makes no statement about whether RT-Labs, P-Net, or any real product complies or fails to comply with the CRA.

## 2. Evidence convention

Every substantive statement in this repository carries one of three tags, or sits in a section that is plainly one of them.

| Tag | Meaning | Rule |
|-----|---------|------|
| **[V]** Verified | Stated by a cited public source (regulation text, Commission guidance, NVD, vendor release notes or advisory). | Source must be linked. |
| **[A]** Assumption | A deliberate modelling choice about the hypothetical product or its context. | Must be listed in `07-limitations.md`. |
| **[P]** Proposed control | A measure I propose a manufacturer could adopt to meet a requirement. | Not a claim that any real product implements it. |

Nothing here is based on running P-Net, testing a device, or inspecting proprietary source.

## 3. The hypothetical product

**Product under assessment (hypothetical):** a commercially supplied firmware image for a PROFINET IO-Device, running on embedded Linux, that incorporates the RT-Labs P-Net stack as a third-party component.

| Layer | Content | Status |
|-------|---------|--------|
| Device application | Vendor-written logic exposing process data (I/O) through PROFINET | [A] |
| PROFINET stack | RT-Labs P-Net — C library implementing the IO-Device role | [V] role and language per the project repository |
| Operating system | Embedded Linux with a vendor-maintained kernel and userland | [A] |
| Network | Raw Ethernet (Layer 2) for real-time and discovery traffic; UDP for RPC | [V] general PROFINET behaviour; exact exposure on this product is [A] |
| Update mechanism | Vendor-signed firmware package delivered to the device | [P] — not assumed to exist today |

**Intended use [A]:** industrial field device on a segmented OT network, not directly exposed to the internet, operated by a system integrator or end user.

## 4. Assumed CRA positioning

| Question | Position | Basis |
|----------|----------|-------|
| Is the product a "product with digital elements"? | Yes | [A] hardware plus remote data connection via PROFINET/Ethernet; to be argued in `02-annex-i-mapping.md` |
| Who is the manufacturer? | The firmware vendor placing the product on the EU market | [A] |
| What is P-Net's role? | Third-party component integrated into the product | [A] |
| Product category | Default category (not Annex III important or Annex IV critical) | [A] — stated, not proven; risks of this assumption recorded in `07-limitations.md` |

Consequence of the component assumption: the manufacturer remains responsible for the security of the whole product, including P-Net as integrated, and must exercise due diligence when integrating third-party components (CRA Art. 13(5)). The Commission FAQ sections on component due diligence are the reference for how far this extends.

<!-- TODO before publishing: confirm article numbers and application dates against EUR-Lex. -->

## 5. In scope

- Mapping of Annex I Part I and Part II to the hypothetical product (`02`).
- Case study of CVE-2025-32405, a P-Net vulnerability, as a vulnerability-handling exercise (`03`).
- A hypothetical component inventory / SBOM (`04`).
- A threat model of the IO-Device's network-facing surface (`05`).
- A risk register derived from the threat model and the CVE (`06`).
- Stated assumptions and limits (`07`).

## 6. Out of scope

- Any conclusion about RT-Labs' or P-Net's actual compliance.
- Conformity assessment procedure, CE marking, EU declaration of conformity.
- Running, building, fuzzing or otherwise testing P-Net or any device.
- Copying or redistributing P-Net source, binaries, or evaluation material.
- Legal interpretation beyond what the cited public sources support.
- Other PROFINET roles (IO-Controller, supervisor) and other products.

## 7. Primary sources

- CRA summary — https://digital-strategy.ec.europa.eu/en/policies/cra-summary
- CRA implementation FAQ — https://digital-strategy.ec.europa.eu/en/library/cyber-resilience-act-implementation-frequently-asked-questions
- Regulation (EU) 2024/2847 — https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng
- P-Net repository — https://github.com/rtlabs-com/p-net
- P-Net v1.0.2 release — https://github.com/rtlabs-com/p-net/releases/tag/public%2Fv1.0.2
- RT-Labs security advisories — https://rt-labs.com/cyber-security/
- RT-Labs vulnerability-disclosure policy — https://rt-labs.com/about-rt-labs/our-policies/
- CVE-2025-32405 (NVD) — https://nvd.nist.gov/vuln/detail/CVE-2025-32405
