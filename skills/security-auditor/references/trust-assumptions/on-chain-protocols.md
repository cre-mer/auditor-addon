# On-Chain Protocols

Covers: DeFi protocols, DAOs, bridges, governance systems, oracles, staking, lending, AMMs, vaults — any deployed smart contract system.

## Domain calibration

- Blast radius is protocol-scoped but amplified by composability. A bug in one protocol can drain funds from every protocol that integrates with it.
- Smart contracts are immutable once deployed, a vulnerability without an upgrade path or pause mechanism is permanent — weigh severity accordingly.
- Economic attacks (flash loans, oracle manipulation, sandwich attacks) are first-class threats. If an attack is profitable net of gas, assume it will be executed.
- Libraries introduce implicit vulnerabilities. Individually correct components can become unsafe when composed (e.g., overriding virtual functions, combining multiple inherited contracts, or hook interactions).

## Trust assumptions

- Protocol admins/owners are trusted within their documented privilege scope. Admin-only functions are informational unless privilege escalation or undocumented power exists.
- External protocol integrations (oracles, price feeds, bridges, other DeFi protocols) are trusted unless the codebase explicitly validates their outputs. Note implicit trust as an assumption, not a finding.
- Users are untrusted. All user-supplied inputs (calldata, token amounts, addresses) must be validated at the contract boundary.
- Chain VMs (e.g. EVM) and compiler correctness are assumed. Language compiler bugs are out of scope unless the codebase uses a known-affected version.
- If spec-compliant behavior appears unsafe or exploitable, flag it for review with the `design-challenger` skill rather than dismissing it.

## Common false positives

- Flagging admin privilege as a vulnerability when admin trust is an explicit design assumption with timelock/multisig controls.
- Reporting front-running on operations that are already protected by slippage parameters or commit-reveal schemes.
- Treating ERC-20 fee-on-transfer or rebasing incompatibility as a bug when the protocol explicitly documents supported token types.
- Flagging reentrancy on functions that follow checks-effects-interactions or use reentrancy guards.
- Reporting centralization risk on upgradeable proxies when governance controls and timelocks are documented and in place.
