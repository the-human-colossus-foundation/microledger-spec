# Back story for Microledger 

Microledger tries to address specific needs, build a data model which can lead us towards `authentic data`.
By `authentic data` we mean provenance chain of any arbitrary information which can be verified
cryptographically at `any point of time`. This means that whoever would ever possess the data in a form of a microledger
he/she/it would be able to cryptographically verify the integrity and authenticity of the information.

## Intro

When we operate on any data within digital space we are always interested in tree characteristics:
- integrity
- authenticity
- veracity

`Integrity` addresses the topics related to the state of being whole and undivided, 
we can verify cryptographically what we got is exactly what we are expecting to get.
Integrity is achieved through the simple mechanism of one-way functions which generates a digest of given content.

`Authenticity` provides the quality of being authentic. To state the authenticity of arbitrary data
we need to understand where it comes from. We can achieve that by simply doing a cryptographic signature
which would tell us who create it in the first place. We could chain them if the data pass through different entities.
Authenticity can be achieved through digital signature over mentioned data. Everyone anytime knowing the public key of an entity who signed that payload can reason from where this information is coming from. 

`Veracity` is the most tricky one since it involves trust which is pure human emotion. Important thing is to realize
that veracity over data can be established only if we can assure about `integrity and `authenticity in the first place.
Without it, we do not have the basis to establish trust. Veracity can be achieved through governance which is built within
a given ecosystem. The rules and reputation which is operating within our governance giving us the basis for trust over any data
issued in that ecosystem.

Taking into consideration the above relationship we conclude that this dependency can be presented visually by authentic data pyramid:

![image](https://user-images.githubusercontent.com/312837/136097939-9a3e5f2e-33a8-46ad-9ecd-49003341beaf.png)

## Nano, micro, and beyond

To increase interoperability and assure about base security characteristics we wanted to build a data model which could sever
any type of use case within the scope of the data economy. If data are involved they need to be authentic. How we can generalize
on such a high level without losing the focus on that what needs to be implemented? 
Following the `authentic data pyramid`, we can easily address topics one by one without losing too much focus. 

### Nanoledger for Integrity
When we need integrity which needs a simple mechanism of linked data structures that are cryptographically linked. We can easily achieve this by using a content digest and connecting any arbitrary data in a form of a seal (digest of that content) into the data structure. Why seal not transactions or raw data? Two reasons:
- privacy
- controlled growth size

Even that it seems like `seal` is some kind of weird limitation from the implementation point of view is just a detail. It brings the same security aspects as if we would embed `raw data` in, it allows us to accumulate as much data we like similar to the concept of transactions in a blockchain. In addition, it gives us a way to have higher privacy since none of the data is exposed directly and allows us to have control over the size of such data structure since no matter how big data we want to include `seal` would have the same length depending on the used algorithm.

### Microledger for Authenticity

As we learned integrity in many cases is not enough, and we need to verify the authenticity. This is where Microledger introduces `controlling identifiers`, which is a mechanism of cryptographic algorithms based on public-private key infrastructure, where we can reason about the authenticity of the given block. The aim of controlling identifiers is just to provide enough information that anyone else who would receive microledger at any point in time would be able to verify the authenticity of that information. As we pointed out in the previous section of that document the state of the art for authenticity is the digital signature. That is the only reasonable way to do verification of the authenticity of the given block.

The controlling mechanism can be built in various ways which `microledger` assumes that the community will drive the implementation the only requirement is to make sure that it would be based on strong digital signatures cryptography. 

To address privacy concerns and for example not revealing the public key of someone directly into the block, or his identifier which both could be used for correlation the controlling identifier can be anchored similar way as payload in a form of a seal.

Important to mention is that controlling identifier goal is just about authenticity, if there are any other logic that needs to be applied in the context of the payload which is anchored, this always can be added in addition. But to talk about authentic data `controlling identifiers` are required mechanisms to rely on authenticity build into the data model and not depend on specific use cases. If the controlling identifier would be leftover we fall back to the nano ledger and we still can do some useful things, sometimes even achieving authenticity on the level of the payload but this would need to define specifically per case which makes it hard to establish some consensus across use cases.


### Governance for Veracity

Having in place integrity and authenticity we can reach towards the end state veracity of the data which then can be used for the decision-making process or simply consuming the data knowing that we can trust it. Governance is responsible for setting up the rules, establish schema, draw the line what and where can be used. It does not mean that it needs to be built around centralized systems but it does not forbid that either. Is up to the community of a given ecosystem to define how it would work. The assumption is that through authentic data we can build a reputation system that could help to establish a truly decentralized governance system that could be easily deployed in any jurisdiction and environment.

### Summary

Taking into consideration all described layers we can represent them in the context of concreate implementation as on below picture.


![image](https://user-images.githubusercontent.com/312837/136104142-6b354f31-8761-4322-9b25-050c0d62572c.png)





