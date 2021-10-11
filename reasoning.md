# Backstory for Microledger 

With Microledger the Human Colossos Foundation (HCF) started building a data model which can lead to `authentic data`.
By `authentic data` we mean provenance chain of any arbitrary information which can be verified
cryptographically, anywhere at `any point of time`. This means that whoever would ever possess the data in a form of a microledger
he/she/it would be able to cryptographically verify the integrity and authenticity of the information.

This fulfills the specific need on the internet to not only be able to tell _who said what_ (authenticity), but also to be sure that what was said is true, consistent, and coherent (veracity).

## Intro

When we operate on any data in the digital space, we want this data to have at least these main features:
- integrity
- authenticity
- veracity

`Integrity` addresses the topics related to the state of being whole and undivided, we can verify cryptographically that what we've got is exactly what we've expected to get.\
Integrity is achieved through the simple mechanism of one-way functions which generates a digest of given content.

`Authenticity` provides the quality of being authentic. To state the authenticity of arbitrary data
we need to understand where it comes from. The sender can achieve that by simply cryptographically sign the data.
This would enable the receiver to an certain extent 'tell' who create the signature in the first place. This is done by verifying the the signature with a supplied public key. We could chain signatures if the data at hand (payload) pass through different entities.
Authenticity can be achieved through digital signature of the above mentioned data. Everyone with the provided public key of an entity who signed the payload can verify and trust the authenticity of the sender of the information at the time of signing. 

`Veracity` is the most tricky feature. Truthfullness involves trust and probability, the former is human emotion, the latter involves statistictics and assessment. Important thing is to realize
that veracity over data can be established only if we can assure  `integrity` and `authenticity` in the first place.
Without these two characteristics, there's no basis to establish trust. Veracity can be achieved through governance. Governance structures are built within a given ecosystem. The rules and reputation present in a governance framework enable trust over any data(provencance), issued in that ecosystem.

All considered, relationships and mutual dependencies can be presented visually by authentic data pyramid:

![image](https://user-images.githubusercontent.com/312837/136196364-e7994ec9-6ecc-4f0c-a777-b28f463bdf8a.png)


## Nano, micro, and beyond

To increase interoperability and promote widespread use we'd like to build a data model, that could serves
any type of use case within the scope of the data economy, while uniformly guaranteeing base security. 
That means: the data involved need to be authentic and consistent. 
This is easier said than done. How we can generalize on such a high level without losing the focus on that what needs to be implemented? 
Following the `authentic data pyramid`, we can easily address topics one by one without losing too much focus. 

### Nanoledger for Integrity
For integrity we use a simple mechanism of cryptographically-linked data structures. We can achieve this by using a content digest and connect any arbitrary data in a form of a seal (digest of that content) into the data structure. Arguments for seals above transactions or raw data:
- privacy
- controlled growth of volumes

Even though a `seal` seems a rather limited construct from the  implementation point of view, a seal brings the same security aspects as one would get by embedding `raw data` in the structure. Furthermore, introducing seals allows us to accumulate as much data(provenance) as we like, similar to the concept of transactions in a blockchain. In addition, it gives us a way to have higher privacy since none of the data is exposed directly.  Lastly, seals allow us to have control over the size of the data structure since regardless the volume of the data, using `seals` would result in the same amount of bytes*.
'* dependent of the used algorithm

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





