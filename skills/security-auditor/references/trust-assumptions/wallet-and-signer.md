# Wallet & Signer Software

Covers: hardware wallet firmware, software wallets, MPC signer services, account-abstraction bundlers, transaction builders, key management libraries.

## Domain calibration

- Key material compromise is total loss. Any bug that leaks, corrupts, or fails to destroy private keys is maximum severity regardless of preconditions.
- "What you see is what you sign" is a security invariant. If the parsed/displayed transaction differs from the signed payload, treat as high even without a concrete exploit scenario.
- Side-channel resistance is a correctness requirement for hardware and MPC contexts.
- Backup and recovery paths are attack surface, not just UX. Seed export, social recovery, and cloud backup flows must meet the same bar as primary signing paths.

## Trust assumptions

- The host device is semi-trusted for hardware wallets — the device must independently verify transaction content. Findings that assume a fully compromised host are informational unless the device fails to mitigate.
- MPC threshold assumptions (e.g., t-of-n honest parties) are design boundaries. Attacks requiring more than the threshold of colluding parties are informational.
- Bundlers and relayers are untrusted by default in AA systems. UserOperation integrity must not depend on bundler honesty; if it does, that is a finding.
- External RPCs and mempools are untrusted. Transaction construction that relies on unverified RPC data for security-critical fields (nonce, gas, chain ID) is a finding.

## Common false positives

- Reporting physical fault injection on hardware wallets when the threat model explicitly excludes physical attackers with lab equipment.
- Flagging bundler censorship as high when the user can submit the UserOperation through alternative bundlers or directly to the mempool.
- Treating key material in process memory as a vulnerability when the trust boundary is the OS process — focus on persistence, swap, and core-dump exposure instead.
- Reporting MPC communication overhead or round complexity as a security issue when it is a performance trade-off within the protocol's design parameters.
- Flagging spec-compliant validation rules as vulnerabilities because they seem restrictive.
