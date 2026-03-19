# Developer Tooling

Covers: compilers (solc, zksolc, Cairo compiler), DSL/eDSL SDKs (Stylus, ink!, CosmWasm), build tools, client libraries (ethers.js, viem), CLIs (Foundry), key management utilities.

## Domain calibration

- Blast radius is indirect but wide. A compiler or SDK bug silently propagates into every contract or application built with it — the downstream developer has no visibility.
- Miscompilation is the highest-stakes class: correct source code producing incorrect bytecode. Treat any confirmed miscompilation as high+ regardless of trigger complexity.
- Unsafe abstractions that mislead developers (e.g., a "safe transfer" wrapper that silently drops a check) are findings in the tooling, not the developer's code.
- Transaction construction and encoding bugs (wrong calldata, missing fields, incorrect nonce handling) can cause silent fund loss with no on-chain revert.
- Key material handling: any path where private keys or mnemonics are logged, cached to disk, or passed via CLI arguments visible in process lists is a finding.

## Trust assumptions

- The compiler/SDK is trusted to produce output faithful to the source. Divergence between source semantics and compiled output is always a finding.
- Build tool plugins and dependencies are within the supply chain trust boundary. Flag unexpected network calls, post-install scripts, or pinning gaps as findings.
- Client libraries trust the connected RPC endpoint by default. Note implicit RPC trust as an assumption; only flag it as a finding when the library does not offer to use a custom RPC endpoint.
- CLI tools may run with broad filesystem and network access. Assume the operator's machine is trusted, but flag credential leakage or unintended exfiltration vectors.

## Common false positives

- Flagging compiler optimizations that change gas cost but preserve semantics.
- Reporting known-unsupported language features or documented limitations as vulnerabilities.
- Treating verbose compiler warnings or debug output as information disclosure when it only appears locally.
- Flagging RPC trust when the library documents it and provides verification hooks.
- Reporting CLI credential prompts as key exposure when input is not echoed and not persisted.
