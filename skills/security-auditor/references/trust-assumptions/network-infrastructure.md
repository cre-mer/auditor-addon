# Network Infrastructure

Covers: node clients, validators, sequencers, provers, fraud proof systems, DA layers, relayers, off-chain relay infrastructure.

## Domain calibration

- Blast radius is network-wide. A single client bug can fork the chain or halt block production for every user on the network — not just one protocol's users.
- This domain implements external specifications. Code that is internally correct can still be a vulnerability if it diverges from the spec in consensus-critical logic.
- Liveness failures (halts, degraded performance) can be high severity when prolonged — an 8-hour halt causes real fund loss and reputational damage. Do not dismiss liveness issues as inherently less severe than safety issues without considering duration and recoverability.
- Economic incentives amplify bugs. If exploiting a bug is more profitable than the slashing penalty, assume it will be exploited.

## Trust assumptions

- Honest majority threshold (typically 2/3 stake) is assumed. Findings requiring >1/3 malicious stake are within the design's threat model and usually informational.
- For centralized operators (e.g., sole sequencer), operator honesty is assumed by design. Flag operator misbehavior findings as informational unless there is no escape mechanism for users.
- External systems (L1 finality, DA layers, oracles, RPC providers) are trusted unless the codebase explicitly handles their failure. Note implicit trust as an assumption, not a finding.
- Spec compliance is the correctness baseline for implementation validation. Implementation divergence from spec is a finding. However, the spec itself is not infallible — if spec-compliant behavior appears unsafe or suboptimal, flag it for review with the `design-challenger` skill rather than dismissing it.

## Common false positives

- Flagging centralized sequencer censorship when a forced-inclusion escape hatch exists.
- Reporting honest-majority-assumption violations as bugs (they are design boundaries).
- Treating RPC information disclosure as high severity when the endpoint is operator-facing and not publicly exposed.
- Flagging spec-compliant behavior as a vulnerability because it seems suboptimal.
- Reporting DoS via malformed P2P messages when rate limiting and peer scoring are already in place.