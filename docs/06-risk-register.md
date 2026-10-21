# 06 — Risk Register

Rates the six threats from [05-threat-model.md](05-threat-model.md) for the hypothetical PROFINET IO-Device firmware defined in [01-scope.md](01-scope.md). This is the kind of risk assessment that Annex I I(1) expects a manufacturer to base its security measures on (see [02-annex-i-mapping.md](02-annex-i-mapping.md)).

Tags: **[V]** verified from a cited source, **[A]** assumption, **[P]** proposed control. The ratings are the author's judgement for a hypothetical product. They are not calculated, measured or validated, and nothing was tested.

<!-- TODO before publishing: confirm the Article 13(2) risk-assessment reference against EUR-Lex. -->

## 1. Scale **[A]**

Deployment assumed: segmented OT network, not directly internet-exposed, wired Ethernet only.

| Level | Likelihood | Impact |
|-------|-----------|--------|
| Low | Needs unusual access or conditions | No effect on the cyclic I/O exchange |
| Medium | Needs a foothold on the OT network or access to the cabinet, which is plausible | One device is offline briefly and recovers |
| High | Anyone who can reach the device can do it | The process stops, data is altered, or control is lost |

The overall rating is a judgement, not a formula. **Rating with controls** means the rating after the proposed controls from `02` are in place for that threat, assuming the operator does its part where noted.

## 2. Register

| ID | Threat | Likelihood and reason | Impact and reason | Rating with controls | Treatment | Owner [A] | Review status |
|----|--------|-----------------------|-------------------|----------------------|-----------|-----------|---------------|
| T1 | Eavesdropping on unencrypted PROFINET traffic | **Low to Medium.** Not internet-exposed, so the attacker must already be on the network. A foothold is still plausible through an insider, a contractor's laptop or a move from the IT network | **Low.** Process values and device information leak; nothing breaks directly | **Low to Medium.** No control removes it, because cyclic I/O is assumed unencrypted for timing reasons | **Accept** with a documented rationale; segmentation and port control reduce reach | Operator for segmentation and cabinets; vendor documents the trade-off | Author-rated, corrected |
| T2 | Physical access through unused interfaces | **Low.** The attacker needs access to the cabinet | **High.** Firmware or configuration could be altered | **Low** with interfaces disabled and signed firmware | **Reduce:** disable USB, serial and debug; verify firmware signatures | Vendor for the device; operator for the cabinet | Author-rated |
| T3 | Malformed-packet crash (CVE-2025-32405) | **High** on an unpatched device: remote, no login, public advisory **[V]** | **Medium.** One IO-Device drops and the controller's watchdog expires; higher if that device is process-critical | **Low** after upgrading to P-Net 1.0.2 **[V]** and with segmentation | **Reduce:** upgrade, limit exposure, robustness testing | Shared: P-Net supplier fixes the library; vendor integrates, tests and ships | Author-rated |
| T4 | Unpatched known flaws | **Medium.** It rises the longer known flaws stay unpatched: more is known about them, and new flaws are found in components such as the kernel and libraries | **Medium to High,** depending on the flaw (a crash up to code execution) | **Medium.** It depends on the operator installing updates in maintenance windows **[A]** | **Reduce** through the SBOM and update process; shared with the operator | Shared | Author-rated |
| T5 | Logging disabled or wiped | **Medium** | **Medium.** It blinds detection and investigation rather than breaking the device | **Medium.** Lower if the operator runs a SIEM that receives the forwarded logs and alerts when logging stops **[A]** | **Reduce:** forward logs off the device; alert on gaps; if a device is suspected compromised, isolate it and re-image it from signed firmware before it returns to service | Shared: the vendor builds logging and forwarding; the operator runs the SIEM | Author-rated, corrected |
| T6 | Compromised operator IAM | **Low to Medium.** It depends on how well the operator runs it; stolen credentials are a common route | **High.** The attacker gets the rights of the account taken | **Low to Medium.** The vendor cannot change the operator's system | **Transfer to the operator;** the vendor limits damage with least-privilege roles and logging | Mostly operator | Author-rated, corrected |

## 3. Notes on the ratings

- **Each rating follows the control for that threat.** Upgrading P-Net to 1.0.2 lowers T3 and part of T4. It does not change T1, T5 or T6, which depend on the network design, off-device logging and the operator's systems.
- **Accepted risk.** T1 is accepted with a documented rationale rather than removed. Whether a regulator or customer would accept that rationale was not tested **[A]**.
- **Residual risk sits with the operator.** T1, T4, T5 and T6 depend on what the operator does; the register says so instead of assuming it.

## 4. Limits

- The ratings are judgement on a hypothetical product. No likelihood or impact was measured, and no real incident data was used.
- Each rating was reviewed by the author and corrected where the first proposal was changed. "Author-rated" does not mean the Annex references were checked against the Regulation text.
- Six threats are not exhaustive.
- No statement is made about RT-Labs, P-Net or any real product's compliance.

## Sources

- Threat model: [05-threat-model.md](05-threat-model.md)
- Annex I mapping: [02-annex-i-mapping.md](02-annex-i-mapping.md)
- CVE case study and its sources: [03-cve-2025-32405.md](03-cve-2025-32405.md)
