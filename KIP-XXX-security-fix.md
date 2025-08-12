# KIP-XXX: Security Enhancement – Reentrancy Protection for KAP-20 Tokens

## Summary
This proposal introduces a security improvement to the KAP-20 token standard by implementing robust reentrancy protection on all state-changing functions. This change aims to protect token contracts from malicious reentrancy attacks that could result in loss of funds or inconsistent balances.

## Motivation
While KAP-20 is designed for efficiency and compatibility within the KUB ecosystem, certain edge cases expose it to reentrancy risks. These vulnerabilities can be exploited when an external call in a token function unintentionally allows re-entry into the same function before state changes are finalized.

Such attacks have been seen in well-known exploits across the blockchain ecosystem. By adding protective measures, we ensure token contracts are resilient and maintain user trust.

## Specification
- **Affected functions**: `transfer`, `transferFrom`, `approve`, `mint`, `burn`
- **Mitigation strategy**:
  - Implement the [Checks-Effects-Interactions](https://fravoll.github.io/solidity-patterns/checks_effects_interactions.html) pattern.
  - Add a reentrancy guard modifier (`nonReentrant`) using `ReentrancyGuard` from OpenZeppelin.
- **Example Implementation**:
```solidity
modifier nonReentrant() {
    require(!_entered, "ReentrancyGuard: reentrant call");
    _entered = true;
    _;
    _entered = false;
}