# `monero-serai` and `monero-wallet` audit outline
This document represents my ([sneurlax](https://github.com/sneurlax)'s) personal outline for contributing to the audit(s) of `monero-serai` and `monero-wallet` as a contractor for [Cypher Stack](https://cypherstack.com).  It will be adapted into a report turned into Cypher Stack which will in turn be compiled into a combined audit report for final delivery.

## 0. Separation of responsibilities
Indicate which files require the review of a senior mathematician/cryptographer.

After indicating which files I'd request that a senior review more thoroughly, I will proceed to nonetheless make a best-effort assessment of the safety and security of those portions regardless, but will not invest extraneous time into validating concepts for which I am uncredentialed.
- [ ] List which files or portions therein will require review by a senior mathematician/cryptographer.
  I'm not comfortable auditing most aspects of mathematical/cryptographic proofs/protocols  because I an uncredentialed.  For these portions of the codebase, I will defer to and assist a more senior mathematician/cryptographer.
  - [x] Make "checkbox-tree" diagram of crates in Markdown format.
    See below.
  - [ ] Annotate checkbox-trees with checkboxes to indicate required review as appropriate.
  - [ ] Remove any files which don't need review (eg. most if not all of the `mod.rs`s)
  - [ ] Remove these annotations for delivery to Cypher Stack.

### Senior needed for these `monero-serai` files:
- [ ] monero
  - [ ] ringct
    - [ ] bulletproofs
      - [ ] plus
        - [ ] mod.rs
        - [ ] aggregate_range_proof.rs
        - [ ] transcript.rs
        - [ ] weighted_inner_product.rs
      - [ ] scalar_vector.rs
      - [ ] point_vector.rs
      - [ ] lib.rs
      - [ ] batch_verifier.rs
      - [ ] core.rs
      - [ ] original
        - [ ] mod.rs
        - [ ] inner_product.rs
      - [ ] tests
        - [ ] plus
          - [ ] mod.rs
          - [ ] aggregate_range_proof.rs
          - [ ] weighted_inner_product.rs
        - [ ] mod.rs
        - [ ] original
          - [ ] mod.rs
          - [ ] inner_product.rs
    - [ ] borromean
      - [ ] lib.rs
    - [ ] clsag
      - [ ] lib.rs
      - [ ] tests.rs
      - [ ] multisig.rs
    - [ ] mlsag
      - [ ] src
        - [ ] lib.rs
  - [ ] verify-chain
    - [ ] main.rs
  - [ ] generators
    - [ ] lib.rs
    - [ ] tests
      - [ ] mod.rs
      - [ ] tests.txt
    - [ ] hash_to_point.rs
  - [ ] io
    - [ ] lib.rs
  - [ ] wallet
    - [ ] address
      - [ ] lib.rs
      - [ ] tests.rs
      - [ ] base58check.rs
    - [ ] tests
      - [ ] wallet2_compatibility.rs
      - [ ] runner
        - [ ] mod.rs
      - [ ] add_data.rs
      - [ ] send.rs
      - [ ] scan.rs
      - [ ] decoys.rs
      - [ ] eventuality.rs
    - [ ] lib.rs
    - [ ] scan.rs
    - [ ] extra.rs
    - [ ] view_pair.rs
    - [ ] decoys.rs
    - [ ] send
      - [ ] mod.rs
      - [ ] tx_keys.rs
      - [ ] tx.rs
      - [ ] multisig.rs
      - [ ] eventuality.rs
    - [ ] tests
      - [ ] mod.rs
      - [ ] scan.rs
      - [ ] extra.rs
    - [ ] output.rs
  - [ ] rpc
    - [ ] simple-request
      - [ ] tests
        - [ ] tests.rs
      - [ ] lib.rs
    - [ ] lib.rs
  - [ ] tests
    - [ ] tests.rs
  - [ ] primitives
    - [ ] unreduced_scalar.rs
    - [ ] lib.rs
    - [ ] tests.rs
  - [ ] transaction.rs
  - [ ] ring_signatures.rs
  - [ ] merkle.rs
  - [ ] lib.rs
  - [ ] block.rs
  - [ ] ringct.rs
  - [ ] tests
    - [ ] transaction.rs
    - [ ] mod.rs

## Senior needed for these `monero-wallet` files:
- [ ] wallet
  - [ ] address
    - [ ] lib.rs
    - [ ] tests.rs
    - [ ] base58check.rs
  - [ ] tests
    - [ ] wallet2_compatibility.rs
    - [ ] runner
      - [ ] mod.rs
    - [ ] add_data.rs
    - [ ] send.rs
    - [ ] scan.rs
    - [ ] decoys.rs
    - [ ] eventuality.rs
  - [ ] lib.rs
  - [ ] scan.rs
  - [ ] extra.rs
  - [ ] view_pair.rs
  - [ ] decoys.rs
  - [ ] send
    - [ ] mod.rs
    - [ ] tx_keys.rs
    - [ ] tx.rs
    - [ ] multisig.rs
    - [ ] eventuality.rs
  - [ ] tests
    - [ ] mod.rs
    - [ ] scan.rs
    - [ ] extra.rs
  - [ ] output.rs

## 1. Workspace setup
- [ ] Go through the general workspace structure in `Cargo.toml`.
- [ ] Check that all patches and dependencies line up with project needs.
  - [ ] Version patches (e.g., `parking_lot_core`, `rocksdb`, etc.)
  - [ ] Standard library patches (`matches`, `is-terminal`)
  - [ ] Rewrites/overrides (`directories-next`, `option-ext`)

## 2. `monero-serai` audit
### Common modules:
- [ ] `std-shims`: Confirm the standard library shims are working as expected and aligned with the project.
- [ ] `zeroize`: Verify that memory is safely zeroized where needed.
- [ ] `curve25519-dalek`: Ensure Curve25519 elliptic curve operations are handled correctly, with a focus on security and performance.

### Monero-specific modules:
- [ ] `monero-io`: Look at Monero-specific I/O functions to ensure everything reads and writes as expected.
- [ ] `monero-generators`: Audit the range proofs and generators used, especially focusing on their cryptographic integrity.
- [ ] `monero-primitives`: Dive into the Monero primitives to ensure everything is cryptographically sound.
- [ ] `monero-mlsag`: Review MLSAG signatures (Multilayered Linkable Spontaneous Anonymous Group) to verify they're implemented properly.
- [ ] `monero-clsag`: Audit CLSAG (Concise Linkable Spontaneous Anonymous Group) signatures for efficiency and correctness.
- [ ] `monero-borromean`: Ensure Borromean ring signatures are correctly implemented and secure.
- [ ] `monero-bulletproofs`: Review the Bulletproofs implementation to ensure they're accurate and performant in confidential transactions.

### Cryptography modules:
- [ ] `transcript`: Review cryptographic transcripts, ensuring protocols are solid.
- [ ] `dalek-ff-group`: Check that field arithmetic and cryptographic group operations are handled correctly.
- [ ] `multiexp`: Look into the multiexponentiation optimizations and make sure they're on point.
- [ ] Overall cryptographic integrity: General review of the zero-knowledge proofs and signature schemes used.

## 3. `monero-wallet` audit
- [ ] Check the wallet structure for secure integration with the Monero protocol.
- [ ] Verify integration of key dependencies like `std-shims`, `zeroize`, and `curve25519-dalek`.
- [ ] Multisig functionality:
  - [ ] `transcript`: Flexible transcript handling.
  - [ ] `dalek-ff-group`: Cryptographic group operations for multisig.
  - [ ] `frost`: Multisig support, ensuring proper integration.
- [ ] `rand`: Confirm that randomness generation is securely handled for key generation and decoy selection.
- [ ] `monero-address`: Ensure Monero address generation logic is correct.
- [ ] `monero-clsag`: Check that CLSAG signatures are securely integrated within the wallet.
- [ ] `monero-serai`: Look at how the wallet interacts with the transaction library.
- [ ] `monero-rpc`: Audit RPC (Remote Procedure Call) functionality for secure and efficient network interactions.

## 4. Tests
Check the test coverage for both `monero-serai` and `monero-wallet`:
- [ ] Ensure the tests adequately cover cryptographic and wallet-related functionalities.
- [ ] Look at test environments to make sure both standard and edge cases are handled properly.
- [ ] Verify that dev-dependencies (like `serde`, `serde_json`, `frost`, `tokio`) are correctly integrated and used.

## 5. Performance and optimization
- [ ] Review the profile settings in `Cargo.toml` for `monero-serai` and `monero-wallet` to confirm optimization levels, especially for cryptographic operations.
- [ ] Make sure the optimization flags don't compromise security or correctness, particularly for zero-knowledge proofs and signature schemes.
