# 04 — Hypothetical Component Inventory (SBOM)

An illustration of the component inventory a manufacturer of the hypothetical firmware in [01-scope.md](01-scope.md) would keep to meet Annex I Part II(1) (see [02-annex-i-mapping.md](02-annex-i-mapping.md)). **It is not an SBOM of a real build.** No firmware was built, and the components and versions below are examples chosen to show the idea.

Tags: **[V]** verified from a cited source, **[A]** assumption, **[P]** proposed control.

<!-- TODO before publishing: confirm the wording of Annex I Part II(1) (machine-readable format, at least top-level dependencies) against EUR-Lex. -->

## 1. What an SBOM is for

An SBOM is a list of the software components in a product, with their versions and suppliers. The manufacturer matches that list against NVD and the suppliers' advisories to find out which products are affected when a flaw is published. Annex I Part II(1) asks for a machine-readable SBOM in a common format covering at least the top-level (direct) dependencies **[A]**. A real one would be generated per firmware build in CycloneDX or SPDX **[P]**. The table below is a human-readable illustration of the same information.

## 2. Components **[A]**

Versions are placeholders unless stated. "Source for flaws" means where the manufacturer would look for published flaws in that component.

| # | Component | Role | Supplier | Version | Source for flaws | Main risk | Review status |
|---|-----------|------|----------|---------|------------------|-----------|---------------|
| 1 | Bootloader (for example U-Boot) | Starts the device and verifies the signed firmware | Open-source project | placeholder | NVD, project advisories | A bypass defeats signed firmware | Author-reviewed |
| 2 | Linux kernel | Operating-system core | Open-source project | placeholder | NVD, project advisories | Large and frequently patched | Author-reviewed |
| 3 | C library | Basic system functions | Open-source project | placeholder | NVD, project advisories | Memory-safety flaws affect everything above it | Author-reviewed |
| 4 | Userland tools (for example BusyBox) | Shell and basic commands | Open-source project | placeholder | NVD, project advisories | Every unneeded tool widens the attack surface; kept minimal by the design in `02` (I(2)(j)) | Author-reviewed |
| 5 | TLS library (for example OpenSSL or mbedTLS) | Protects management traffic | Open-source project | placeholder | NVD, project advisories | Cryptographic flaws; frequent advisories | Author-reviewed |
| 6 | P-Net | PROFINET IO-Device stack; handles RPC | RT-Labs | 1.0.1 in the earlier build, 1.0.2 in the fixed build (see `03`) | NVD, RT-Labs advisories and release notes | Parsing flaws; CVE-2025-32405 affects 1.0.1 and earlier **[V]** | Author-reviewed |
| 7 | Update client | Verifies and installs signed updates | Vendor or open-source project **[A]** | placeholder | NVD, vendor's own testing | A flaw could let a fake update in | Author-reviewed |
| 8 | Logging service (syslog) | Records events and forwards them to the operator's SIEM | Open-source project | placeholder | NVD, project advisories | Tampering, or logging disabled | Author-reviewed |
| 9 | Vendor device application | The device's own I/O logic | The vendor | vendor release number | None from outside: only the vendor's own testing and review cover it | Vendor-written code with no external advisory feed | Author-reviewed |

## 3. How it would be used **[P]**

- **Matching.** Each new advisory is compared with the SBOM of every shipped build. For CVE-2025-32405, a build listing P-Net 1.0.1 or earlier matches; a build listing 1.0.2 does not. This is why a per-build SBOM answers "am I affected?" in minutes (see `03`, step 2).
- **Per build.** The inventory changes with each firmware build, so it is regenerated and kept with the release records.
- **Everything is listed.** A component is listed whether or not it looks important for function, because a flaw in any of them can matter.
- **Priority.** The author would watch the P-Net version first (network-facing and with a known flaw history in 1.0.1) and the vendor application second (only the vendor's own testing covers it).

## 4. Limits

- This is an illustration, not an SBOM of a real build. Component choices and versions are assumptions, except the P-Net facts from the cited sources.
- It does not include transitive dependencies, licences, hashes or any of the data a real SBOM tool would produce.
- Each row was reviewed by the author. "Author-reviewed" does not mean the Annex wording was checked against the Regulation text.
- No statement is made about RT-Labs, P-Net or any real product's compliance, and no P-Net code or build was inspected.

## Sources

- P-Net advisory and fix: [03-cve-2025-32405.md](03-cve-2025-32405.md)
- Annex I mapping: [02-annex-i-mapping.md](02-annex-i-mapping.md)
