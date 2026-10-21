
# 05 — Threat Model

An informal threat model for the hypothetical PROFINET IO-Device firmware defined in [01-scope.md](01-scope.md). It lists six threats, how each is reached, what it harms, and which controls from [02-annex-i-mapping.md](02-annex-i-mapping.md) reduce it. The threats are rated in `06-risk-register.md`, not here.

Tags: **[V]** verified from a cited source, **[A]** assumption about the hypothetical product, **[P]** proposed control. This is a desk exercise: it is not a formal method such as STRIDE or attack trees, and nothing was tested.

<!-- TODO before publishing: the control references (Annex I point numbers) follow the mapping in 02 and are not yet checked against the Annex text. -->

## 1. What is being protected **[A]**

| Item | Description |
|------|-------------|
| Essential function | Cyclic I/O exchange with the IO-Controller (see I(2)(h) in `02`) |
| Firmware and configuration | The firmware image, device configuration, credentials and keys |
| Security logs | Records of failed logins, configuration changes, update attempts, integrity failures and crashes |

**Where an attacker can reach the device [A]:**

- The wired Ethernet network (PROFINET real-time and discovery traffic; RPC over UDP).
- Physical interfaces on the device (USB, serial, debug), disabled by default in the proposed design.
- The update channel.
- The log-forwarding path to the operator's SIEM.
- The operator's identity system, which is outside the product but affects it.

**Deployment [A]:** segmented OT network, not directly internet-exposed, wired Ethernet only. A different deployment would change every threat below.

## 2. Threats

| ID | Threat | What is attacked | How it is reached | Harm | Controls from `02` [P] | Owner [A] | Review status |
|----|--------|------------------|-------------------|------|------------------------|-----------|---------------|
| T1 | Eavesdropping on unencrypted PROFINET traffic | Cyclic I/O and discovery traffic on the wire | Attacker is already on the network: a mirrored or tapped switch port, or a compromised host on the OT network, with a packet-capture tool | Confidentiality of process values; reconnaissance that helps prepare later attacks | I(2)(e): cyclic I/O is assumed unencrypted because of timing limits at short cycle times **[A]**, so segmentation and documented rationale are the mitigation; management traffic is encrypted. I(2)(j) | Shared: the vendor documents the trade-off; the operator provides segmentation, locked cabinets and switch-port control | Author-explained, corrected |
| T2 | Physical access through unused interfaces | The device's USB, serial and debug interfaces | Attacker reaches the cabinet (reaching the building is not enough) and plugs in | Confidentiality of configuration and credentials; integrity if firmware or configuration is altered; availability | I(2)(j): unused interfaces off by default and debug off in production builds; I(2)(f): signed firmware verified at boot; I(2)(d): authentication where an interface must stay | Shared: the vendor disables interfaces and signs firmware; the operator secures the cabinet | Author-explained, corrected |
| T3 | Malformed-packet crash (CVE-2025-32405) | The RPC handling in the P-Net library | A remote attacker sends malicious RPC packets to the IO-Device **[V]** | Availability of the device: it crashes, the controller's watchdog expires and the connection drops. Weakness: out-of-bounds write (CWE-787) **[V]**. Affected P-Net 1.0.1 and earlier, fixed in 1.0.2 **[V]**. See `03` | Before: I(2)(j) limit exposure, I(2)(k) exploit mitigation, I(2)(h) watchdog recovery, II(3) robustness testing. After: II(2) fix. Segmentation limits who can reach the service | Shared: P-Net supplier fixes the library; the vendor integrates, tests and ships | Author-explained, corrected |
| T4 | Unpatched known flaws | Any component with a published flaw: the Linux kernel, libraries, the P-Net stack | The attacker must be able to reach the vulnerable service; known flaws are public, which makes them easy to find. Eavesdropping is not a prerequisite | Depends on the flaw: crash (availability) up to code execution (confidentiality, integrity and availability) | II(1): SBOM per build matched against NVD and advisories; II(2): remediate without delay; I(2)(a): no known exploitable vulnerabilities at release; I(2)(j) and I(2)(k) reduce reach and impact. Multi-factor authentication protects logins and does not stop a flaw in a network-reachable service | Shared: the vendor owns vulnerability handling for the product; the operator applies updates in maintenance windows **[A]** | Author-explained, corrected |
| T5 | Logging disabled or wiped | The device's security logs | An attacker with access turns logging off or deletes the records | Loss of any trace of what happened, so the intrusion cannot be detected or investigated; hidden persistence (new accounts, malware); the manufacturer may not learn of exploitation | I(2)(l): log security events and forward them off the device by syslog to the operator's SIEM, so a wipe on the device leaves copies; I(2)(d): role-based access, default deny, least privilege; I(2)(f): logs forwarded off the device. Also [P]: alert when logging stops, because silence is a signal | Shared: the vendor builds logging and forwarding; the operator runs the SIEM | Author-explained, corrected |
| T6 | Compromised central identity system (operator side) | The operator's identity and access management (IAM) system | The attacker takes over accounts or the system itself, then logs in as a legitimate user | The attacker gets the rights of the account taken: administrator rights allow change of configuration, a backdoor, theft of confidential data or stopping the process. Scale: the devices that identity system covers | Outside the product (operator responsibility, see I(2)(d) in `02`). The product limits the damage with least-privilege roles, logging of access and configuration changes (I(2)(l)), and no shared default credentials. The operator needs privileged-account control, multi-factor authentication and monitoring | Mostly operator; vendor limits damage | Author-explained, corrected |

## 3. Observations

- **Packet parsing is a recurring weak spot.** CVE-2025-32405 is one of ten CVEs fixed in P-Net 1.0.2, and all ten are triggered through the network-facing code **[V]** (see `03`). This is why T3 asks for robustness testing as well as patching.
- **Some risks cannot be removed by the vendor.** T1 (unencrypted real-time data), T2 (cabinet security) and T6 (the operator's identity system) depend on the operator. The assessment records them as stated assumptions rather than claiming control over them.
- **Controls repeat.** Segmentation and physical security appear against several threats because they reduce reachability for all of them; they do not replace the product-side controls in `02`.

## 4. Limits

- Based on the hypothetical product and public sources only. Nothing was tested or observed.
- Each threat was explained by the author and corrected where the first answer was wrong. "Author-explained" does not mean the Annex references were checked against the Regulation text.
- The list of six threats is not exhaustive. Other realistic threats (for example update-channel compromise) are not modelled here.
- No statement is made about RT-Labs, P-Net or any real product's compliance.

## Sources

- Annex I mapping: [02-annex-i-mapping.md](02-annex-i-mapping.md)
- CVE case study and its sources: [03-cve-2025-32405.md](03-cve-2025-32405.md)
