# ZK & Cryptography

Covers: ZK circuits (Circom, Noir, Halo2), custom elliptic curves, signature schemes, VRFs, commitment schemes, proving systems, trusted setup ceremonies.

## Domain calibration

- Soundness failures (fake proofs accepted) are the most critical class — they silently break the guarantee the entire system relies on.
- Under-constrained witnesses are the dominant bug pattern in circuit code. If a witness variable is not fully determined by public inputs, an attacker can supply a different valid witness to forge proofs.
- Completeness failures (valid proofs rejected) are availability issues. Severity depends on whether users lose funds or just experience delays.
- Trusted setup compromise invalidates all proofs generated under those parameters. Evaluate ceremony structure and toxic waste handling.
- Side-channel leaks in prover/verifier implementations (timing, power) can expose secret witnesses or signing keys.

## Trust assumptions

- The underlying cryptographic hardness assumptions (discrete log, pairings, hash properties) are trusted. Do not flag theoretical quantum attacks unless the system claims post-quantum security.
- Trusted setup ceremonies are assumed honestly executed when using powers-of-tau with sufficient independent participants. Flag single-party or unverifiable setups.
- Circuit compilers and proving system libraries are trusted unless the audit scope explicitly includes them. Note compiler version and known bugs as assumptions.
- Randomness sources are assumed secure. Flag deterministic or low-entropy nonce generation as a finding.

## Common false positives

- Flagging unconstrained variables that are actually determined by other constraints in the system (trace full constraint graph before reporting).
- Reporting trusted setup risk for universal/transparent schemes (PLONK with KZG ceremony, STARKs) that do not require per-circuit setup.
- Treating field arithmetic overflow as a bug when the circuit explicitly reduces modulo the field prime.
- Flagging hash-to-curve or hash-to-field implementations as non-standard when they follow the relevant RFC or specification.
- Reporting theoretical side-channel attacks against on-chain verifiers where execution is public and deterministic.
