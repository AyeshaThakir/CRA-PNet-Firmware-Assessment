# 07 — Limitations and Assumptions

This is an independent, educational desk assessment of a hypothetical product. It is not certification, legal advice, a commercial compliance assessment, or work performed for or with RT-Labs. This document lists what the work cannot show.

Tags: **[V]** verified from a cited source, **[A]** assumption, **[P]** proposed control.

## 1. What was and was not done

- **Documentation-based only.** All findings come from public sources: the regulation text, Commission guidance, NVD, and RT-Labs' public repository, release notes, advisories and disclosure policy. **[V]**
- **No runtime testing.** P-Net was not built, run, fuzzed or tested for this project, and no device or network was used. Nothing here is "I observed X"; statements about behaviour are taken from public sources or are assumptions.
- **No P-Net code is included or redistributed.** Proprietary or evaluation material was not copied.
- **No statement about RT-Labs or P-Net compliance.** Nothing here says RT-Labs, P-Net, or any real product complies or fails to comply with the CRA.

## 2. The product is hypothetical

- The "commercially supplied PROFINET IO-Device firmware" does not exist. Its structure (device application, P-Net, embedded Linux, update mechanism) is a modelling choice. **[A]**
- Intended use (industrial field device on a segmented OT network, not directly internet-exposed) is assumed. A different deployment would change the threat model and risk ratings. **[A]**
- The component inventory (`04`) is hypothetical. It was not produced from a real build, so it is an illustration of what an SBOM would contain, not an actual SBOM. **[A]**

## 3. CRA positioning assumptions

- **Manufacturer role.** The firmware vendor is assumed to be the manufacturer placing the product on the EU market, and P-Net an integrated third-party component. **[A]**
- **Product category.** The product is assumed to fall in the default category, not Annex III (important) or Annex IV (critical). This is stated, not proven, and a different category would change the conformity route. **[A]**
- **Application of the CRA to this product** was not tested against a real conformity assessment procedure.

## 4. Specific mapping assumptions (from `02`)

- **Automatic updates.** The CRA expects security updates to be installed automatically by default, with an opt-out, where that is appropriate. This assessment assumes a manufacturer would justify an operator-controlled, postponable install in an industrial setting. Whether that justification would be accepted is not tested. **[A]**
- **Confidentiality of PROFINET traffic.** Real-time process data is not assumed to be encrypted. Whether that is acceptable depends on the intended use and the risk assessment. **[A]**
- **Requirement wording.** The "requirement" columns in `02` are paraphrases. Point numbers and wording should be confirmed against Annex I on EUR-Lex. **[A]**
- **Proposed controls are proposals [P].** None is claimed to exist in any real product.

## 5. Evidence limits

- **The CVE case study (`03`)** relies on NVD, the P-Net v1.0.2 release notes and RT-Labs' public pages. The vulnerability was not reproduced, and the fix was not inspected beyond what those sources state.
- **Third-party disclosure channels** are described from their public pages. Their real-world handling times and processes were not tested. **[A]**
- **Licensing of P-Net** (open source and commercial terms, and what the public evaluation version contains) was not analysed in depth and should be confirmed before relying on this work for any real decision. **[A]**

## 6. What this work is good for

An illustration of how Annex I could be turned into product requirements for a PROFINET IO-Device, and how Part II vulnerability handling could be applied to a real, public P-Net vulnerability. It is a learning exercise and a discussion basis, not a basis for compliance decisions.

## 7. Possible future work (not done)

- Build and run an IO-Device in a lab and replace assumptions with observed evidence.
- Test malformed-packet robustness against the fixed and unfixed versions.
- Review a real SBOM from an actual build.
