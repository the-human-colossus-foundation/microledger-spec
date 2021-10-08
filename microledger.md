# Microledger spec

Copyrights The Human Colossus Foundation 2021

## Abstract

Microledger is an authentic data provider leveraging the concept of data provenance log. Provenance logs are intended to be a generalized form of a linked list data structure that uses cryptographic primitives to ensure integrity with a strong cryptographic link to a controller of the log.

## Status of This Document

DRAFT

## Introduction

The need to have parts (or chunks) of data stored in order (or as a chain of events) is not a new problem. Having the bank account balance calculated from all the historical events is definitely more robust approach than changing the balance each time an event (ie. money withdrawal) appeared, without having any registry of those events. Here it is worth to mention two main concepts that emerged historically, so Event Sourcing **[ES]** and chain of blocks or simply a Blockchain. Both already exist for quite some time and they address the same fundamental concept of evidence of state changes. Both share similar characteristics like immutable data structures, append only log or monotonic updates imposing anchoring changes always to the end of the log, but they are very different in how they provide those features. Event Sourcing emerged due the rise of message oriented systems in Service Oriented Architecture **[SOA]** and in particular in Microservices architecture. Blockchains,  although first proposed much before they became popular, are now extensively discussed in mainstream due to the association with cryptocurrencies.

The underlying difference between Event Sourcing and Blockchain is in how the latter utilizes cryptography to ensure end-verifiability and in particular the authenticity of all the blocks since the genesis block. Event Sourcing does not have such characteristic. Blockchain is even more as it is considered a distributed, globally ordered ledger with high resistance to Byzantine generals problem. This is, however, not for free. Due to the nature how they operate or upon what rules, they are not known for high transaction rates.

In this document we propose a concept of Microledger. Our concept of a Microledger is such that it lies somewhere in between  Event Sourcing and Blockchain. It is essentially a blend of these two. So it is an immutable, append only and end-verifiable evidence of state changes that does not require global ordering or consistency, or in particular a peer to peer network to operate, because it is solely a cryptographically bound evidence of events. Whoever has a copy of Microledger instance is able to verify the authenticity on his own and eventally verify the veracity. Eventually, because it is assumed that controlling identifiers resolution is time consuming operation and may depend on other infrastructure that is not ie. highly available.

Microledgers consist of blocks, which include Seals (see terminology below) that act as abstract data container under the form of cryptographic digests. Block also includes Controlling Identifiers, where control authority over the block is defined. Only the sole owner of the block, so the Custodian(s), may anchor new block. It is possible Controlling Identifiers are empty and in this case anyone may anchor new block to the chain.

### Overview
*This section is non-normative.*

![concept](assets/concept.png)

Fig: An overview of how blocks are related to each other. Note it is always a **[DAG]**.



### Benefits
This section is non-normative.

- security layer for authentic data flows
- simple but extensible 

## Terminology

**SAI** Self-Addressing Identifier 

**Custodian** a role that indicates the ownership over Microledger.

**Multisig** an ability of the system that requires the Block signing procedure is done by one or more Custodians, so a group. It is a shared commitment of the group to signed Block, which is the outcome of how and from who digital signatures are expected. **[KERI]** 

**Seal** a cryptographic commitment in the form of a cryptographic digest that anchors arbitrary data to Block. **[KERI]** 

## Concept

Microledger consists of fundamental building blocks called blocks. Each next block is bound to the previous block by including its cryptographic digest content. 


![concept2](assets/concept2.png)


* `Seals` segment allows to include any arbitrary data in block under the form of cryptographic digests of that data.
* `Controlling identifiers` segment describes control authority over the Microledger at given block. Control may be established for single or multiple identifiers through the multisig feature. Furthermore control over the Microledger block may be transferred to one or more different identifiers. This is done by operation of anchoring new block to the chain that establishes new control authority. `Controlling identifiers` can be anything that is considered identifiable within given network, ie. `Public Key`, `DID`, `KERI` prefix and so on.
* `Digital fingerprint` segment includes the cryptographically derived unique fingerprint of given block. Digital fingerprint is a result of a one-way hash function operation on that block.
* `Signatures` segment including the cryptographic commitment of Custodians to given Block.
* `Seals registry` and `Seal attachments` provides mechanism to resolve resources defined within `seals` segment, so to eventually provide them for further operations. `Seals registry` and `Seal attachments` are interchangeable and may be defined per each Block. `Seal attachments` are content-bounded to given block and `Seals registry` is an additional layer that knows how to resolve seals required ie. from an external data source.

### Seals in the form of digests

Microledger does not expose an interface to include any arbitrary data into it and instead relies on cryptographic digests of that data to ensure:
* maximum adoptability as various use cases will require different data semantics. It is out of Microledger scope to handle such semantics and it is the consumer responsibility to address that accordingly;
* easy transferability among the custodians of the Microledger. Cryptographic hash functions have the characteristic of fixed size output for any size given input. Hence, enforcing digests in the `Seals` segment fosters better control of the size of Microledger.
* privacy concerns so any Microledger can be considered public as it does not contain any private data per se.

### Microledger identifier

The unique identifier is derived from the genesis block of the Microledger. In essence this is the cryptographic hash declared for `Genesis Block`.

## Characteristics

### End verifiability

Blocks are chained cryptographically. Each block encapsulates its own `digital fingerprint` and furthermore each next block includes `digital fingerprint` of the previous block.

### Composability

Microledgers are composable, which means that any newly bootstrapped genesis block can bind to another Microledger instance.


![composability](assets/composability.png)


### Ownership Transferability

Current custodian (or a set of custodians for multisig) may transfer the ownership of Microledger to one or more next custodians.

Microledger does not have owner per se at any time of its existence. Ownership, under the form of Custodians, is defined per block, in the `Controlling Identifiers` section. Ownership is then block scoped, so the control authority is limited up to given block. Given set of Custodians may anchor several blocks, block by block, and in this sense it may be tempting to see them as owners of a Microledger, especially if these blocks are the only blocks in the chain. From the Microledger perspective, however, it is seen as temporary, because current set of Custodians may always be transferred to new set.

![ownership-transferability](assets/ownership-transferability.png)

As Microledger fosters DAG characteristic, in particular Custodians control authority over given block is irrevocable, they may anchor new blocks apart from the main line. 

Below diagram presents such characteristic:

![ownership-transferability2](assets/ownership-transferability2.png)

### Serialization and Encoding 

Microledger use self-describing mechanism for encoding and serialization used for each block. 
Serialization and encoding is all about how to package and transmit the blocks over variouse protocols and data formats. Plugable and self-describing mechanism enables algorithmic agility. 

### Plugable 

Each of the core component of the microledger can be echange with specific implementation. This leaves room for adopting any combination which would serve for specific use case. 

![plugable](assets/plugable.png)

## Block fundamentals explained

### Seals

A seal is a qualified digest where its derivation code specifies what type of hashing function
was used but does not include any other information about the associated data. The hashing step
produces a digest of the serialized data that is referenced by the seal. The seal acts as an anchor of the data. To clarify, the actual data is not provided explicitly in the seal, but merely the digest of the serialized data, hence the data is hidden. This may be useful in making verifiable crypto-graphic commitments at the location of an event to data stored and/or disclosed elsewhere.

### Digital fingerprint

A digital fingerprint (hash) is produced by a type of one-way function. Given that the hashing function has sufficiently strong collision resistance then it uniquely securely cryptographically binds its output digest to the contents of the block.

### Seals Attachment 

Seals Attachment is the mechanism to attach content represented by the `Seal` to the encoded block. This allows to have simple and very portable (without any lookups) resultion mechanism of the content which is the subject of the block.

### Seals Registry

Seals registry it is a external component to the microledger which acts as a lookup mechanism for content resolution based on the seal. This specification does not have an opinion what and how this resolution mechanism could work. There are meny variantions and depending on the needs it could be realize in different ways. The most important is the assurance about the security of the bound content to the seal. This follows the characteristics of content-centric networks where we care what it is but not where it is.

### Mapping table codes

Below tables provide the registry for derivation codes for all supported components.

Digital fingerprint representation:
|Concept|Code|
|---|---|
|SAI|A|
|Multihash|B|

Seal representation:
|Concept|Code|
|---|---|
|SAI|A|
|Multihash|B|

Control identifiers representation:
|Concept|Code|
|---|---|
|Self-certifying basic prefix|A|
|Self-certifying self-addressing prefix|B|
|Self-certifying self-signing prefix|C|
|DID:peer|D|
|DID:web|E|

## Extended Characteristics

### End verifiability

It is a two step process, where both authenticity and veracity are being verified.

#### Authenticity

It goes block by block, starting from genesis block. The verification consists of the folowing steps:
* cryptographically verify that `digital fingerprint` matches given block and for blocks, where block number `N > 1` include `digital fingerprint` of the previous block;
* cryptographically verify that genesis block signatures match Custodian(s) public keys specified within `Controlling identifiers`;
* cryptographically verify signatures of block, where block number `N > 1` match the  `Controlling identifiers`, so the Custodian(s) public keys of block number `N`.

The Custodian(s) public keys are derived from their identifiers.


![custodian](assets/custodian.png)

Fig: Scopes of verifiability are shown within dashed lines. `Controlling identifiers` public keys of `N-th` block have to match signatures of `N-th + 1` block.


#### Veracity

To establish a trust basis under Microledger instance, further verification is required, so whether all Custodians reputation is considered trusted. This is conducted by verifying all Custodians identifiers, ie. using third party Governance Framework. 

### Ownership Transferability

To transfer the ownership of given Microledger instance, the current owner/owners needs to create new block with new set of the controlling keys in `Controling identifiers`

#### Transfer between custodians

Microledger expects at least one custodian to be assigned under `Controlling identifiers`. Transfering not only the ownership, but the whole Microledger instance is not bounded to any specific protocol as it will depend on the use case.

Microledger instance transfership requires sender and receiver.

Microledger instance to be considered as a transferred to other custiodian require both, so the chain of blocks and seals registry to be transferred. The transfer itself is:
* synchronous, when it is assumed that seals are not significant in size, yet essential attribution to the chain;
* asynchronous in scenarios where seals transfership requires significant time to be completed.

The latter, as its characteristic is to be eventually consistent, is also more flexible, because the receiver is able to immediately verify the authenticity of the chain without waiting for the seals themselves.

It is allowed to have Microledger instance, where seals registry does not contain any physical data, but only exposes interface towards ie. `IPFS` or `BitTorrent` networks.

It is up to the consumer to decide, which approach is best.

### Serialization and transport 

Synchronous and asynchronous transfership enforces appropriate exchange protocols to be used in given scenario. In case of synchronous transfership it is a bundle or an envelope, which contains both the chain of blocks and seals registry. 


## References

* **[KERI]**: https://github.com/SmithSamuelM/Papers/blob/master/whitepapers/KERI_WP_2.x.web.pdf
* **[ACDC]**: https://github.com/SmithSamuelM/Papers/blob/master/whitepapers/ACDC.web.pdf
* **[CESR]**: https://github.com/decentralized-identity/keri/blob/master/kids/kid0001.md
* **[DAG]**: https://en.wikipedia.org/wiki/Directed_acyclic_graph
* **[ES]**: https://martinfowler.com/eaaDev/EventSourcing.html
* **[SOA]**: https://en.wikipedia.org/wiki/Service-oriented_architecture
* [Multiformats] <https://multiformats.io/>
* [Multicodec] <https://github.com/multiformats/multicodec>
* [Multihash] <https://multiformats.io/multihash/>
* [CESR] Smith, S., "KID0001 - Prefixes, Derivation and derivation reference tables", <https://github.com/decentralized-identity/keri/blob/master/kids/kid0001.md>
* [CESRCommentary] Smith, S., "KID0001 - Commentary - Prefixes, Derivation and derivation reference tables", <https://github.com/decentralized-identity/keri/blob/master/kids/kid0001Comment.md>
* [CDE] Huesby, D., "Cryptographic Data Encoding (CDE)", <https://hackmd.io/@dhuseby/BkG-4gp2u>
* [JOSE] JOSE Workgroup Documents, <https://datatracker.ietf.org/wg/jose/documents/>
* [COSE] COSE Workgroup Documents <https://datatracker.ietf.org/wg/cose/documents/>

## Appendix: examples

```
{

}
```
