# RFC: Embedding Security Features in Digital Image Files

Author: Stefano De Vuono

Date: 16 August 2024

Status: Draft

---

1. Problem Statement

     Artists and collectors alike place a premium on the authenticity of a piece, to protect both the artist's intellectual property and reputation as well as the collector's value and investment. Artists in digital medium is digital images face greater stakes as uniqueness of digital media is particularly challenging Copying a digital image with perfect fidelity is trivially easy and editing or manipulating digital media poses little to no barrier. While this need is particularly pressing in the digital art community, it also extends to secondary applications in sectors like journalism, law, and digital archiving, where image authenticity is similarly critical. Current methods of verifying digital images either disrupt the user experience by altering the image format or are vulnerable to being bypassed.

2. Proposed Solution

    The proposed solution involves embedding a security feature within digital image files that is imperceptible to users but allows for the verification of the image’s authenticity. Analagous to printmaking, an artist's work  will function as a "final proof"[^1] from which they make a limited "print edition"[^2]. Within each print edition, each "numbered prints"[^3] will receive a digital signature in the form of  a **unique** identifier or token embedded into the image file. Each unique signature can verify the artwork as an authentic.

3. Scope and Out of Scope

    In Scope:

    - Creation of that that combining content data and public-private key pairs.
    - Embedding unique identifiers or hashes into digital image files.
    - Proof of concept for image authentification

    Out of Scope:

    - Who should own private key: artist, an auction house, museum, or a combination?
    - Whether or not authentication should be centralized or decentralized. Focus is on authenticity, not proof of ownership/title.
    - Security questions of what should happen if the artist's private key gets stolen.

 4. Technical Approach

    The solution involves three primary components:

    - Tokens: Analagous to a JWT the token will contain three parts: a public key generated from the final proof, metadata about the print number, and a signature. Content hashing will generate the public key, tying it to a particular artwork. The metadata will reflect the unique print number within the edition (ie: print 7/10 or 2/5) and other relevant information. Finally, a the content hash and metadata will be hashed by a private to make the print authentic.

     - Data Embedding: A unique identifier or hash is generated for each digital image and embedded into the image file using an imperceptible method (e.g., steganography[^5]). This process does not alter the image's visible content.

     - Verification: While anyone will be able to extract and read the content hash and print number metadata. Only the authenticator[^4], who holds the private key, can certify that it is genuine.
     
    This process can be integrated into existing image-processing or storage workflows, making it seamless for artists.

5. **Risks and Mitigations**

- **Risk**: Someone who gets their hand on both the private key and the artist original could start "printing" their own copies.

    **Mitigation**: Title of an artwork could be stored in some sort of database or blockchain so that if I own print 3/10 and someone makes another 3/10, their copy will appear legitmate, but they won't be able to prove they own it. Additionally, the artist (or posthumously, their foundation) could use that same data store to store information about the artwork, like the number of editions. That way, if--using the above example, I have print 3/10 from the _first_ edition and the only other authentic edition is a second edition of 5 prints, trying to print a third edition will be an obvious forgery.

- **Risk**: Control of the private key involves a trade-off of creative control. If a recognized authenticator, like Sotheby's controls the key, they might decide to make additional prints against the artists will. Conversely, if an artist holds the private key, tkey may decide to unilaterally issue new editions depressing the price of the existing pieces.

    **Mitigation**: Perhaps the authentication token will need two private keys: one from the artist and one from an authenticator.

- **Risk**: Ethics of having a centralized authenticator. Even if neither the artist nor the authenticator can unilaterally issue new editions or prints, ethically the creative output belongs to the artist and they should not be beholden to a single authenticator.

    **Mitigation**: Open up the standard, allowing an artist to switch to a new authenticator. Subsequent editions will have different authentication tokens.

6. Alternatives Considered

- Watermarking: Traditional watermarking was considered but rejected due to its impact on the visual quality of images and ease of removal by skilled individuals.

- Metadata Embedding: Embedding identifiers in image metadata was considered but found to be insecure, as metadata can easily be stripped or altered without affecting the image content.

- Certain images may not be able to hold enough embedded data (eg: large enough steganographs)


7. Implementation

- Authentication Token: Develop token that can combine content hashing of the artist's final proof, metadata about the print number and a private key signature generated of the content and metadata.

 - Data Embedding: Develop or integrate existing steganography techniques to embed unique identifiers into digital images. In particular, make sure that the steganograph is robust enough to handle token length

- Verification Tools: Develop tools or APIs for users to verify the content hash and metadata, as well as authentication by private key holders.

8. Success Metrics

- Flexibility: The system should accept be able to sign more than 75% of digital images with resolution of at least 4k.

- Accuracy: The system should accurately identify authentic images with a 99% success rate.

- Adoption by at least two major auction houses. Having more than one auction house will help development of an open standard.

- Adoption by at least 10 major artist. Art is at the heart of the project. Buy-in from major artists is sine qua non.

- Adoption by at least 5 major contemporary art museums around the world. Museums have a vested stake in making sure art is both availalbe to the public and authentic. Interest from different museums will incentivize artists to join.


### Notes

Performance is not major concern. Art takes a long time to make. According to [Artsy](https://www.artsy.net/article/artsy-editorial-what-to-do-if-you-think-you-ve-found-a-masterpiece-in-the-attic), current time to authenticate a painting can be a long time, as paperwork and shipping are often required. In some cases (if the artist has a cagalogue, its committee may only meet twice a year) can be a minimum of 6 months.



### Key Terminology:

[^1] Final Proof: An original print or pre-print from which other prints are made.

[^2] Print Edition: a limited series of copies of an artwowrk made by an artist.

[^3] Numbered Print: shows the identity of the unique piece and how many pieces are in the edition. A numbered print of "3/10" would be the third print of an edition of 10 prints.

[^4]Authenticator: Agent that can verify that a piece is an authentic numbered print.

[^5]Steganography: The process of hiding information in an existing object such that it is not perceptible to the human eye. In this context it would mean a token hidden in a numbered print, but a staganograph can also exist in video, audio, or other media.