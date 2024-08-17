Author: Stefano De Vuono

Date: 16 August 2024

Status: Draft

---
# RFC: Embedding Security Features in Digital Image Files

## Problem Statement

Artists and collectors alike place a premium on the authenticity of a piece, to protect both the artist's intellectual property and reputation as well as the collector's value and investment. Artists in digital media face greater stakes, as ensuring the uniqueness of digital artwork is particularly challenging. Copying a digital image with perfect fidelity is trivially easy, and editing or manipulating digital media poses little to no barrier. While this need is particularly pressing in the digital art community, it also extends to secondary applications in sectors like journalism, law, and digital archiving, where image authenticity is similarly critical. Current methods of verifying digital images either disrupt the user experience by altering the image format or are vulnerable to being bypassed.

## Proposed Solution

The proposed solution involves embedding a security feature within digital image files that is imperceptible to users but allows for the verification of the image’s authenticity. Analagous to printmaking, an artist's work  will function as a "final proof"[^1] from which they make a limited "print edition"[^2]. Within each print edition, each "numbered prints"[^3] will receive a digital signature in the form of  a **unique** identifier or token embedded into the image file. Each unique signature can verify the artwork as an authentic.

## Scope and Out of Scope

In Scope:

- Creation of that that combining content data and public-private key pairs.
- Embedding unique identifiers or hashes into digital image files.
- Proof of concept for image authentification

Out of Scope:

- Who should own private key: artist, an auction house, museum, or a combination?
- Whether or not authentication should be centralized or decentralized. Focus is on authenticity, not proof of ownership/title.
- Security questions of what should happen if the artist's private key gets stolen.

 ## Technical Approach

The solution involves three primary components:

- Tokens: Similar to a JWT, the token will contain three parts: a public key generated from the final proof, metadata about the print number, and a signature. Content hashing will generate the public key, tying it to a particular artwork. The metadata will reflect the unique print number within the edition (ie: print 7/10 or 2/5) and other relevant information. Finally, the content hash and metadata will be hashed by a private to make the print authentic.

    - Data Embedding: A unique identifier or hash is generated for each digital image and embedded into the image file using an imperceptible method (e.g., steganography[^5]). This process does not alter the image's visible content.

    - Verification: While anyone will be able to extract and read the content hash and print number metadata. Only the authenticator[^4], who holds the private key, can certify that it is genuine.
    
This process can be integrated into existing image-processing or storage workflows, making it seamless for artists.

## **Risks and Mitigations**

- **Risk**: Someone who gets their hands on both the private key and the artist's original could start "printing" their own copies.

    **Mitigation**: The current art market is worth hundreds of billions and still subject to theft, so solving theft is not feasible. Someone could steal the file a collector has. This becomes a proof of ownership problem. A hardened DB or blockchain could provide a record of title that mitigates opportunities for theft.

- **Risk**: Possession of the private key involves a trade-off of creative control. For example, if a recognized authenticator, like Sotheby's controls the key, they might decide to make additional prints against the artists will. Conversely, an artist holding the private key, could decide to unilaterally issue new editions depressing the price of the existing pieces.

    **Mitigation**: Perhaps the authentication token will need two private keys: one from the artist and one from an authenticator.

- **Risk**: Ethics of having a centralized authenticator. Even if neither the artist nor the authenticator can unilaterally issue new editions or prints, ethically the creative output belongs to the artist and they should not be beholden to a single authenticator.

    **Mitigation**: Open up the standard, allowing an artist to switch to a new authenticator. Subsequent editions will have different authentication tokens.

## Alternatives Considered

- Watermarking: Watermarks would ruin the visual appeal of art. Moreover, they could be removed, altered, and reapplied.

- Metadata Embedding: Embedding identifiers in image metadata is insecure, as metadata can easily be altered.


## Implementation

- Authentication Token: Develop token that can combine content hashing of the artist's final proof, metadata about the print number and a private key signature generated of the content and metadata.

 - Data Embedding: Develop or integrate existing steganography techniques to embed unique identifiers into digital images. In particular, make sure that the steganograph is robust enough to handle token length

- Verification Tools: Develop tools or APIs for users to verify the content hash and metadata, as well as authentication by private key holders.

## Success Metrics

- **Security**: Similar to a JWT, the token should not be valid if edited. Test 10_000 images with altered tokens. No images should be authenticated.

- **Flexibility**: The system should accept be able to sign more than 75% of digital images with resolution of at least 4k.

- **Accuracy**: The system should accurately identify authentic images with a 99% success rate.

### Notes

Performance is not a major concern. Art takes a long time to make. According to [Artsy](https://www.artsy.net/article/artsy-editorial-what-to-do-if-you-think-you-ve-found-a-masterpiece-in-the-attic), current time to authenticate a painting can be a long time, as paperwork and shipping are often required. In some cases (if the artist has a cagalogue, its committee may only meet twice a year) can be a minimum of 6 months.

### Possible Token Representation
#### Example Input:
```json
{
  "content": "[Hashed Content of Adrian Piper's Funk Lessons]",
  "metadata" {
    "title": "Funk Lessons",
    "print edition": 1,
    "print number": "3/5"
  },
  "private_key": "[Some private key]"
}
```

#### Example Output:
```jwt
eyJjb250ZW50IjoiQWRyaWFuIFBpcGVyJ3MgRnVuayBMZXNzb25zIiwiYWxnIjoiSFMyNTYifQ.eyJ0aXRsZSI6IkZ1bmsgTGVzc29ucyIsInByaW50IGVkaXRpb24iOjEsInByaW50IG51bWJlciI6IjMvNSJ9._cVj6EVj2vJ6vSV6JLeZFOSKHHN3A0VS84h0qR9aSEs
```


#### Example steganograph:

![steganograph of cat](./steganograph.jpg)


### Key Terminology:

[^1] Final Proof: An original print or pre-print from which other prints are made.

[^2] Print Edition: a limited series of copies of an artwork made by an artist.

[^3] Numbered Print: shows the identity of the unique piece and how many pieces are in the edition. A numbered print of "3/10" would be the third print of an edition of 10 prints.

[^4]Authenticator: Agent that can verify that a piece is an authentic numbered print.

[^5]Steganography: The process of hiding information in an existing object such that it is not perceptible to the human eye. In this context it would mean a token hidden in a numbered print, but a staganograph can also exist in video, audio, or other media.