# NFData

NFData is a metadata proposal for NFT interoperability. Every US dollar is worth $1. You can use any dollar to pay for anything that costs $1. That property is called "fungibility". But most things are non-fungible, like cars, art, shoes houses, or equity. To represent those things, the blockchain community created Non-Fungible Tokens (NFTs). NFT standards exist to represent everything that isn't fungible currency.

And because it can represent everything, the metadata we use to describe those things must be very flexible. That has already led to splintering and NFT protocols which can't talk to each other. This document exists to push towards a future where NFT marketplaces can easily get all the data it needs to present a listing by looking at on-chain references to find the relevant data storage associated with the token.

Our aim here is to describe a minimal set of metadata that NFT authors should include if they want their NFTs to be listed on marketplaces.

NFData needs to be:

* **Discoverable**
* **Interoperable**
* **Extensible**
* **Composable**

There are a few more important goals:

* Support existing NFT specifications
* Start with something minimal, with optional extensions for marketplaces and apps that understand specialized NFTs
* Support new common NFT use-cases with standard representations (video, audio, 3d assets)
* Make it easy for marketplaces to discover and use associated metadata
* Don't prescribe hosting - use URIs that can refer to IPFS/Filecoin, Storj, etc
* Build on prior art instead of reinventing the wheel
* In casees where we need to diverge from prior art to bake in consistency, ensure mappings to commonly implemented standards to ease interoperability

## Prior Art

Many schemas that may be suitable for NFTs have already been defined and are available at [schema.org](https://schema.org/docs/schemas.html) including [images](https://schema.org/ImageObject), [video](https://schema.org/VideoObject), [audio](https://schema.org/AudioObject), and so on.

* [ERC-721 Metadata Standards and IPFS](https://medium.com/blockchain-manchester/erc-721-metadata-standards-and-ipfs-94b01fea2a89)


### ERC-721

ERC-721 tokens get associated with their metadata on-chain using a [tokenURI field in the token contract](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC721/ERC721.sol#L21). Further information such as description and image are parsed from the corresponding JSON record referenced by URI. That metadata is stored in a JSON document using a structure that looks like this:


```js
{
    "title": "Asset Metadata",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "description": "Identifies the asset to which this NFT represents",
        },
        "description": {
            "type": "string",
            "description": "Describes the asset to which this NFT represents",
        },
        "image": {
            "type": "string",
            "description": "A URI pointing to a resource with mime type image/* representing the asset to which this NFT represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive.",
        }
    }
}
```

That's a good start, but there's a lot more that we'll need in our applications and marketplaces. How do we handle audio, or video, and how do we verify authenticity of an NFT? How can users make claims of IP violations and get scams and spam removed from marketplace listings?

Metadata needs to be discoverable, interoperable, and composable.
