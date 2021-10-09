# Microledger spec

Copyrights The Human Colossus Foundation 2021 under the [EUPL 1.2 license](LICENSE)

## Abstract

Microledger is an authentic data provider leveraging the concept of a data provenance log. Provenance logs take a generalized form of a linked list data structure. The log uses cryptographic primitives to ensure integrity and consistency. The log has a strong cryptographic link to its controller.

## Status of This Document

DRAFT

## Introduction

The wish to have parts (or chunks) of data stored in order (or as a chain of events) is not a new phenomenon. For example a _bank account balance_ calculated from all the historical transaction events is definitely a more robust approach than merely changing its balance value. Each time an event (e.g. a money withdrawal) takes place, we'd like to register those events explicitely for future use and verification. By doing so, we can combine two main concepts that have evolved seperately so far: Event Sourcing **[ES]** and _chain of blocks_ or simply a Blockchain. Both structures already exist for quite some time. They solve the same fundamental challenge by providing evidence of state changes. Both share similar characteristics like immutable data structures, append only logs or monotonic updates. These update append anchoring changes always at the end of the log. However, but structures, ES versus blockchains provide those features differenly. The reason for that is their distinct origin: Event Sourcing emerged due the rise of message oriented systems in Service Oriented Architecture **[SOA]** and in particular in Microservices architecture. Conversely, blockchains became mainstream after they where used in bitcoin and other cryptocurrencies. 

The underlying difference between Event Sourcing and Blockchain is how the latter utilizes cryptography to ensure end-verifiability and in particular the authenticity of all the blocks since genesis block. Event Sourcing does not have the characteristic 'end-verifiable'. Blockchains have features beyond end-verfiability. A blockchain is considered a distributed, globally ordered ledger with high resistance to Byzantine generals problem. However, this comes with a cost. Due to the nature how they operate, they are slow and use energy.

In this document we propose __Microledger__ (ML). MLs design blends the concept of Event Sourcing and Blockchain. Microledger is an immutable, append only log that serves end-verifiable evidence of state changes. Microledger does not require global ordering or consistency, or a peer-to-peer network to operate in particular, because it is solely a cryptographically-bound evidence of events. Whoever has a copy of a Microledger instance is able to verify the authenticity on his own and eventally verify the veracity in a next step. Eventually, because it is assumed that controlling identifiers resolution is time consuming operation and may depend on other infrastructure that is not ie. highly available. {<- this sentence is hard to grasp, what do you want to say?}

Microledgers consist of blocks, which include Seals that act as abstract data container cordoned off by cryptographic digests. Block also includes Controlling Identifiers, where control authority over the block is defined. Only the sole owner of the block, so the Custodian(s), may anchor new block.
{I think the conception of owner, holder and controller should be explained more at this stage} 
The specification of Controlling Identifiers is optional. In case it's empty, anyone may anchor new block to the chain.

### Overview
*This section is non-normative.*

![concept](assets/concept.png)

Fig: An overview of how blocks are connected. Note it is always a **[DAG]**.



### Benefits
This section is non-normative.

- security layer for authentic data flows
- simple but extensible
- distinction between verfication of attribution (authentication) and verification of accuracy (veracity)

## Terminology

**SAI** Self-Addressing Identifier

**Custodian** a role that indicates the ownership over Microledger.

**Multisig** an ability of the system that requires the Block signing procedure is done by one or more Custodians, so a group. It is a shared commitment of the group to signed Block, which is the outcome of how and from who digital signatures are expected. **[KERI]** 

**Seal** a cryptographic commitment in the form of a cryptographic digest that anchors arbitrary data to Block. **[KERI]** 

## Concept

Microledger consists of blocks. Each next block is bound to the previous block by including its cryptographic digest content. 


![concept2](assets/concept2.png)


* `Seals` segment allows to include any arbitrary data in a block by including cryptographic digests (provenance) of that data.
* `Controlling identifiers` segment describes control authority over the Microledger in a given block. Control may be established for single or multiple identifiers through the multisig feature. Furthermore, control over the Microledger block may be transferred to one or more different identifiers. This is done by anchoring new block to the chain, thus establishing new control authority. `Controlling identifiers` can be anything that is considered identifiable within given network, ie. `Public Key`, `DID`, `KERI` prefix and so on.
* `Digital fingerprint` segment includes the cryptographically derived unique fingerprint of a given block. The digital fingerprint is a result of a one-way hash function operation on that block.
* `Signatures` segment includes the cryptographic commitment of Custodians to a given Block.
* `Seals registry` and `Seal attachments` provides a mechanism to resolve resources defined within the `seals` segment, The resolution eventually provides them for further operations. `Seals registry` and `Seal attachments` are interchangeable and may be defined per separate Block. `Seal attachments` are content bound to given block and `Seals registry` is an additional layer that specifies how to resolve seals required in specific situations, for example from an external data source or {here another example?}.

### Seals in the form of digests

Microledger does not provide an interface to include arbitrary data into its blocks. Instead ML relies on cryptographic digests (provenance) of this data to ensure:
* maximum adoptability as various use cases will require different data semantics. It is out of Microledger's scope to handle such semantics and it's the consumer's responsibility to address this accordingly;
* easy transferability among the custodians of the Microledger. A cryptographic hash function has a fixed size output for any size given input. Hence, enforcing digests in the `Seals` segment fosters better control of the size of Microledger and substantial reduction of the size of the log. Mind you, this design introduces the need to store and manage the linked sources (input of the hash functions) without defects.
* privacy concerns, be aware that any Microledger can be considered _pii_, personal identifiable information, although it's public and it does not contain any private data at face value.

### Microledger identifier

The unique identifier is derived from the genesis block of the Microledger. In essence this is the cryptographic hash declared for `Genesis Block`.

## Characteristics

### End verifiability

Blocks are chained cryptographically. Each block encapsulates its own `digital fingerprint` and furthermore each next block includes the `digital fingerprint` of the previous block.

### Composability

Microledgers are composable, which means that any newly bootstrapped genesis block can be bound to another Microledger instance.


![composability](assets/composability.png)


### Ownership Transferability

A current custodian (or a set of custodians for multisig) may transfer the ownership of Microledger to one or more next custodians.

During the course of its lifetime a Microledger does not have an owner at all times. Ownership, under the form of Custodians, is optional and defined per block, in the `Controlling Identifiers` section. So by design Ownership is block scoped and control authority is limited to a given block. 
Knowing that a set of Custodians may anchor several consequtive blocks, it may be tempting to see them as owners of a Microledger, especially if these blocks are the only blocks in the chain. From the Microledger perspective, however, it is seen as a temporary state, because current set of Custodians may always be transferred to a new set of custodians.

![ownership-transferability](assets/ownership-transferability.png)

As Microledger fosters DAG characteristics, in particular Custodians control authority over given blocks is irrevocable, however they may anchor new blocks apart from the main chain. 

The diagram below illustrates this feature:

![ownership-transferability2](assets/ownership-transferability2.png)

### Serialization and Encoding 

Microledger uses a self-describing mechanism for encoding and serialization of blocks. 
Serialization and encoding is all about how to package and transmit the blocks over variouse protocols and data formats. A plugable and self-describing mechanism enables algorithmic agility. {what do you want to say here?}

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
