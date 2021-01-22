# NFData

NFData is a metadata proposal for NFT interoperability. Every US dollar is worth $1. You can use any dollar to pay for anything that costs $1. That property is called "fungibility". But most things are non-fungible, like cars, art, shoes houses, or equity. To represent those things, the blockchain community created Non-Fungible Tokens (NFTs). NFT standards exist to represent everything that isn't fungible value.

And because it can represent everything, the metadata we use to describe those things must be very flexible. That has already led to splintering and NFT protocols which can't talk to each other. This document exists to push towards a future where NFT marketplaces can easily get all the data they need to present a listing by looking at on-chain references to find the relevant data storage associated with the token.

Our aim here is to describe a minimal set of metadata that NFT authors should include if they want their NFTs to be listed on marketplaces. All metadata is optional, but if you include it, your marketplace listings might be better, and you may be able to take advantage of additional NFT features.

NFData needs to be:

* **Discoverable**
* **Interoperable**
* **Extensible**
* **Composable**

There are a few other important goals:

* Support existing NFT specifications
* Start with something minimal, with optional extensions for marketplaces and apps that understand specialized NFTs
* Support new common NFT use-cases with standard representations (video, audio, 3d assets)
* Make it easy for marketplaces to discover and use associated metadata
* Don't prescribe hosting - use URIs that can refer to IPFS/Filecoin, Storj, etc
* Build on prior art instead of reinventing the wheel
* In casees where we need to diverge from prior art to bake in consistency, ensure mappings to commonly implemented standards to ease interoperability


## Prior Art

* Many schemas that may be suitable for NFTs have already been defined and are available at [schema.org](https://schema.org/docs/schemas.html) including [images](https://schema.org/ImageObject), [video](https://schema.org/VideoObject), [audio](https://schema.org/AudioObject), and so on.
* [OpenSea: Adding Metadata](https://docs.opensea.io/docs/2-adding-metadata)
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


## The Metadata

### Metadata Presence

Metadata is OPTIONAL.

### Metadata Format

Use schema.org and JSON-LD to define the metadata structure and contents.

Examples on how to format data using schema.org and JSON-LD are available here:

* https://developers.google.com/search/docs/data-types/book#structured-data-type-definitions
* List of all the available schemas: https://schema.org/docs/schemas.html

### Metadata Storage

Both on-chain an off off-chain storage methods are accepted.

#### For Metadata That is Stored Off-Chain:

* A reference to the metadata file MUST be stored on-chain.
* The metadata file MAY be named using the [multihash](https://github.com/multiformats/multihash) of the file contents.
* Decentralized Storage: Metadata and associated files MAY be stored on IPFS, addressable by the multihash.
* 🔢 `{metadata: "https:\/\/s3.amazonaws.com\/your-bucket\/your-folder\/{file-hash}.json" ...`
* 💡 Metadata translations can be managed using the "workTranslation": attribute inside the metadata file.

#### For metadata that is stored on-chain

No other requirements.

#### Content Hash

Many property rights use-cases require the ability to provably associate specific, unique content with an NFT, on-chain. That association can then be used as evidence for intellectual property claims, contested listings, and on-chain governance which can provide a first layer of defense against IP infringement in order to protect asset rights, fight fraud, and fight spam - all on-chain, indpendent of geographical borders.

The proliferation of fake tokens on existing blockchain marketplaces such as OpenSea and Uniswap is evidence that we should bake the necessary metadata tools to fight abuse into future NFT specifications and protocols.

Each IP-backed token should supply a hash of the represented IP baked into the on-chain token metadata. If included, the hash must be an immutable root record. Of course, IP tends to change over time, with corrections, updates, and added features. Each future revision SHOULD be represented as an on-chain claim with a reference to the root token. Revisions SHOULD be added to an append-only log that keeps a complete history of IP versions, all addressable on-chain. Each revision MUST be signed by the revision issuer.

Each content hash should be a URI that dereferences to the represented data, for verification, similar to how IPFS addressing works (in fact, we can start by supporting IPFS out of the gate).

For tokens representing IP (art, video, audio, written word), it should be trivial to detect duplicate hashes and flag them as potentially fraudulent copies. Clients could do that and alert the user that the data they're trying to tokenize has already been tokenized.

Having this data on-chain means that we can handle many IP rights violations using on-chain governance, enforce and defend smart contracts with royalty splits, and so on.

Instead of engaging in the heavyweight physical world court system, we can report a fraudulent NFT, present our own on-chain NFT evidence, and let on-chain governance take over.

### Examples

...
