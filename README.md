# blockchainid-specifications

## Spec C

There are three components to the identity descriptor file:

+ profile
+ auth
+ encryption

The profile section provides instructions for how a resolver can construct a user's profile.

The auth section outlines the conditions under which the user can be said to be authenticated.

The encryption section provides instructions on how to encrypt information such that the user will be able to decrypt it.

### Profile

The profile is constructed from a list of signed certificates that make individual statements about the user's profile.

Each of these certificates needs to be signed by a key that we know belongs to the user.

First, we need to discover where the files are. Then, we need to verify their authenticity.

To discover the files, we start with the `baseURI` field and build a URI for each item in the `publicItems` list and grab the data found at each of those combined URIs.

To verify the authenticity of a given item, we derive a child public key from the master public key found in the `publicKey` field, using the item's `chainPath` value as a chain path seed (where every 2 bytes in the chain path seed represents a child derivation step). We then use this public key and compare it against the key that signed the profile data item. If it matches, the data is accepted.

### Auth

Each item in this section outlines a public key identity that is given the ability to authenticate on behalf of the user, as well as the conditions under which that identity can authenticate.

The `authLevel` field represents the level of access that the identity is given.

The `publicKey` field is a list of public keys that are ALL required to sign the challenge given by the relying party (the website being signed into).

A site-specific public key is derived from the public keys provided, combined with the chain path provided by the user during authentication. This way, a user can provide a different public key to each site and can wait to reveal linkage between the user identity and the site specific public key (or never reveal the linkage at all).

### Encryption

Provides the public keys for which data can be encrypted that the user will later be able to decrypt.
