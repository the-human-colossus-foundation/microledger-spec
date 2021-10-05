# Microledger use cases 


## Digital identifier 

Key event log introduce by [KERI](https://keri.one) brings huge improvement over any digital identifiers in a form of decentralized key management solutions. Microledger is generalize data model of such key provenance log. With adjusting slightly the implementation of keri we can come up fully compatible mechanism. The rules are relatively simple in KERI:
- linear chain
- clear definition who is able to add next block / event
- clear definition what is allowed ROT, IXP and ICR events


## GIT

Introducing microledger concept to the versioning system like git, brings digest agility and crypto agility from perspective what types of crypto can or should be used. Not only that brings possibility to introduce authenticity which brings us to the decentralized mechanism of managing git repository, licensing, policy or even managing IPR.

## Authentic Data Chain Containers

[ACDC](https://github.com/trustoverip/TSS0033-technology-stack-acdc/blob/main/docs/index.md) was huge inspiration for microledger since it is the closest implementation of what we wanted to achieve. Having a generalized structure of cryptographically linked data brought us to create based rules how data, identifiers, semantic and rules can be set together to manage any arbitrary data in secure and authentic way. Implementation of ACDC over microledger data model would open up for different implementations
