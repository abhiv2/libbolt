Implmentation of Blind Off-Chain Lightweight Transactions (BOLT)
=========================

Zcash Foundation Grant Proposal

J. Ayo Akinyele, <ayo@yeletech.org>

Motivation and Overview
=======================

​BOLT is a system for conducting privacy-preserving off-chain payments between pairs of individual parties. BOLT is designed to provide a "Layer 2" payment protocol for privacy-preserving cryptocurrencies such as Zcash, by allowing individuals to establish and use payment channels for rapid/instantaneous payments that do not require an on-chain transaction. 

BOLT currently exists as a specification and proof of concept implementation partially in Charm by Ian Miers and Matthew Green (the original authors of the BOLT protocol). The goal in this effort is to develop a production quality implementation of BOLT in the Rust programming language (a memory-safe and type-safe language that improves security by preventing common low-level bugs). The project is intended to be released as open source to benefit the Zcash ecosystem and further the broader goals of addressing the scalability problems of cryptocurrencies like Zcash and beyond.


Technical Approach
=================

​The initial work will focus on the development of the Bolt library (or libbolt) and the core cryptographic components of the BOLT protocol. Specifically, libbolt will include routines for constructing and parsing the messages required for interactive off-chain transactions with (one or more) remote BOLT participant(s), and will provide necessary routines that can interface with the cryptocurrency node (e.g., Zcash) via its interface.
​
Libbolt will include the implementation of both the unidirectional payment scheme as well as the bidirectional payment construction. This includes an implementation of the routines necessary to execute the two interactive protocols, Establish and Pay. 

This library will be implemented initially in Rust and then will explore a Go port/implementation for the second phase. The Rust implementation will leverage the bn pairing library provided by Sean Bowe (Zcash Engineer). In terms of efficiency, will also explore optimizations described in the original paper with respect to the NIZK proofs in the Pay protocol. In addition, will write unit tests and produce a design document for libbolt that describes the details of the cryptographic instantiations for the core primitives and zero-knowledge proofs of knowledge statements. 

The second phase will include the development of a full node for a BOLT-compatible cryptocurrency, e.g., zcashd. This node will be connected to the currency P2P network, and will support commands via an RPC interface. We will leverage existing code developed in Go by the Lightning Network project (e.g., lightningd).


Background and Qualifications
=============================

I received a Ph.D. in Computer Science from Johns Hopkins University (JHU) in 2013 specializing in applied cryptography. In 2007, earned an M.S. in Software Engineering from Carnegie Mellon University specializing in Information Technology. In 2006, obtained a B.S. in Computer Science from Bowie State University.

In terms of qualifications, a majority of my work is cryptographic engineering related and I've contributed to a number of open source projects as a result. In particular, I am the main developer behind the following open source projects:

- **Charm-Crypto**: a rapid prototyping framework for advanced cryptosystems. Written in Python/C and used extensively by academic researchers and practicioners around the world. Link: https://github.com/jhuisi/charm

- **OpenABE**: a new commercial-grade open source attribute-based encryption library. Written in C/C++ and will be publicly available soon at https://github.com/zeutro/openabe.


Evaluation Plan
===============
I anticipate five milestones that mirror the technical approach described earlier:

1. Produce a design document that describes libbolt design and primitive choices. In addition, the document will expand on the NIZKP statements and interatctive protocols for generating blind signatures and so on.
2. Implement in Rust the core cryptographic building blocks required by libbolt -- Commitment scheme, Signatures with efficient protocols, Symmetric key Encryption, etc.
3. Fully implement in Rust the bidirectional payment construction -- including the establish and pay interactive protocols. Write unit tests and document how to use the APIs.
4. Explore optimizations for the range proofs in the Pay protocol.
5. Explore mechanisms to safely and securely link Rust into Golang code (for the purposes of integration with boltd).
6. Implement a dedicated daemon (boltd) in Go that implements BOLT communications with remote parties. This daemon uses HTTPS/JSON communications, incorporates **libbolt** and interfaces directly with the cryptocurrency node.

Security Considerations
=======================

The security implications of this project is to mitigate the bottleneck of on-chain transactions on the Zcash network. With off-chain transactions, the BOLT protocol dramatically reduces the transaction volume arriving at the Zcash blockchain without adding new trusted centralized entities.

Schedule
========
I anticipate the following timeline for when each milestone will be delivered:

**Milestone 1 & 2: May 15, 2018** - The first two tasks will be done in parallel and the design document will be reviewed by Ian Miers and Matthew Green.

**Milestone 3: June 15, 2018** - Will deliver the bidirectional payment construction implementation along with unit tests.

**Milestone 4: July 1, 2018** - Will explore optimization choices for the range proofs with assistance from Ian Miers. Will deliver an implementation of selected techniques and measure performance improvements.

**Milestone 5: July 15, 2018** - Will explore multiple approaches from Rust to Go and will pick one of the proven methods to enable calling libbolt routines from Golang. Will deliver a working method by this date.

**Milestone 6: August 15, 2018** - Will deliver a proof of concept boltd daemon in Go integrated with libbolt that implements BOLT communications with remote parties. Demonstrate this working on a test network.

Budget and Justification
========================

To ensure a high-quality implementation, I am estimating a budget of $30k to support the development of libbolt in Rust, porting to Go and the implementation of boltd to support integration with the Zcash blockchain (via test net). This budget reflects compensation for my effort over a four month period dedicated to this project.