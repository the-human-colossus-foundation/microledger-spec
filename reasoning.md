# Back store for Microledger 

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
