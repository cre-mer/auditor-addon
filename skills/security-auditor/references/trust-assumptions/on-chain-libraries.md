# On-Chain Libraries

Covers: smart contract libraries (OpenZeppelin Contracts, Solmate, Solady), framework modules, reusable base contracts, abstract building blocks.

## Domain calibration

- Blast radius is multiplicative. A single library bug propagates to every downstream protocol that inherits or calls it — severity reflects the aggregate, not a single integrator.
- Distinguish explicit flaws (a function is wrong) from implicit flaws (combining contracts or overriding virtual functions creates an unsafe state). Implicit flaws are library design bugs, not integrator mistakes.
- Libraries define the security surface integrators inherit. Missing input validation, missing access-control modifiers, or unsafe default behaviors are findings even if a careful integrator could work around them.
- Code is consumed via inheritance, not composition alone. Evaluate the full inheritance hierarchy — diamond conflicts, storage layout collisions, and hook-override ordering matter.

## Trust assumptions

- Integrators are assumed competent but not infallible. If a "reasonable developer following the docs" can misuse an API and create a vulnerability, that is a library finding.
- Virtual functions are extension points; the library must remain safe under any spec-compliant override. If safety depends on the override preserving an undocumented invariant, flag it.
- The library does not trust external inputs (calldata, msg.sender for unpermissioned calls). Internal-only helpers that skip validation must be clearly marked and not externally reachable via inheritance.
- Compiler and chain (e.g. Ethereum) version assumptions are part of the trust boundary. If correctness depends on a specific language version or chain opcode behavior, note the implicit trust.

## Common false positives

- Reporting an integrator-side misconfiguration as a library vulnerability when the library API is used outside its documented contract.
- Flagging virtual function overridability as a vulnerability when overriding is the intended extension mechanism and invariants are documented.
- Treating missing convenience features (e.g., batch operations, view helpers) as security findings.
- Reporting gas optimizations as medium/high severity without demonstrating a concrete DoS or block-limit impact.
- Flagging the absence of input validation in internal functions that are not reachable from external entry points.
