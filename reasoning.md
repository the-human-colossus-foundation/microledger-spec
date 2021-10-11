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

As we've learned above, data integrity in many cases is not enough, we need to verify the authenticity of the data. This is where Microledger introduces `controlling identifiers`, that is a mechanism of cryptographic algorithms based on public-private key infrastructure. With controlling identifiers we can cryptographically verify the authenticity of the given block, all the way back to the controlling key pair.\
The aim of controlling identifiers is to provide just enough information for anyone who has received microledger at any point in time, is be able to verify the authenticity of the referenced information in Microledger. As we've pointed out before, the state of the art for authenticity is the digital signature. The only trustworthty way to verify the authenticity of the given ML block is to use the self-signed public key of the signature to cryptografically proof its origin.

 `Microledger` allows the controlling mechanism to be built in various ways. It assumes that the community will drive implementations. The only requirement is to make sure that it would be based on strong digital signatures cryptography.

### Privacy concerns and regulations
Rules and regulations are in place and further emerging to address privacy concerns on public servers and ledgers, e.g. the GDPR in Europe. The mechanism we can put in place to assure compliance with regulations are: 
- hiding the identifier/public key of some entity
- prevent correlation of the controlling identifier with the outside world (salt / peper)
- anchored similar way as payload in a form of a seal.

Mind you, the controlling identifier is only related to the `authenticity` goal. If there's any other logic that needs to be applied in the context of the payload which is anchored, this can always be added in addition. However, to label data as authentic, `controlling identifiers` are required, for it's the build-in mechanism for the general authenticity of the data model and does not depend on specific use cases.\
Should the controlling identifier be missing, we can still fall back to the nano ledger and do some useful things with it. Sometimes it's even possible to achieve authenticity to the level of the payload. However, this needs specification per case, which makes it harder to offer general tooling across use cases.

### Governance for Veracity

As soon as we have `integrity` and `authenticity` in place, we can reach the final state: `veracity` of the linked data. Truthfull, coherent and consistent data is more valuable than ordinairy data for the decision-making processes or, straightforward, the consumption of the data. Knowing that one can trust content in some context, implicitly means it has more value in this context.\
Governance is responsible for setting up the rules, establish a schema, draw the lines between fields of meaningful application. It does not mean that it {What do you mean by 'it' here?} needs to be built around centralized systems but it does not forbid that either. It's up to the community of a given ecosystem to define how it would work. The assumption is that through authentic data we can build a reputation system that could help to establish a truly decentralized governance system that could be easily deployed in any jurisdiction and environment.

### Summary

Taking into consideration all described layers we can represent them in the context of a concrete roadmap or implementation path as show in the diagram below.


![image](https://user-images.githubusercontent.com/312837/136104142-6b354f31-8761-4322-9b25-050c0d62572c.png)





