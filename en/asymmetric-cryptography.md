# Asymmetric Cryptography

**Asymmetric cryptography** has a pair of keys. Two keys are generated at once using a mathematical technique so that the two keys are not inferable from one to the other, and when one of the keys is used to encrypt data, only the other key can be used to decrypt.

The owner of the key pair generates a pair of keys. Then, one key is made public (**public key**), and the other is kept private (**private key**). Now the following actions are possible:

- *Sending a message securely to a designated recipient.* Transforming plaintext with the public key ensures that the data can only be read by the designated recipient, the owner of the private key.
- *Proving authenticity.* Transforming a piece of data with the private key proves the authenticity of the data because only the owner of the key can do so. Thus, this action is not called encrypting but **signing** as it does not make anything secret. In many contexts, signing implies endorsement of the content.

## Ideal workflow for sending a message

- Everyone knows everyone’s public key.
- Alice has the plaintext.
- Alice encrypts the plaintext with Bob’s public key.
- Alice sends the ciphertext to Bob.
- Bob decrypts the data with his private key.
- Bob has the plaintext.

Note that Alice does not encrypt with her own private key; that would be signing a publication.

However, in the real world it is not practical to know everyone’s public key in advance. A trusted channel is required to pre-distribute the public key, or the key has to be shared at the start of the connection. In the second case, the connection bearer can always perform a **man-in-the-middle** (**MITM**) attack. The next section shows how such attack works.

## Workflow for sending a message, but with in-time public key sharing and MITM attack

- Alice has the plaintext.
- Alice requests Bob for his public key and this request is intercepted by Charles.
- Charles requests Bob for his public key. Bob thinks Charles were Alice. Bob responds with his public key.
- Charles responds Alice with his own public key. Alice thinks Charles were Bob.
- Alice encrypts the plaintext with Charles’ public key.
- Alice sends the ciphertext to Bob and it is intercepted by Charles.
- Charles decrypts the ciphertext with his private key.
- Charles has the plaintext.
- Charles optionally modifies the plaintext.
- Charles encrypts the plaintext with Bob’s public key.
- Bob decrypts the ciphertext with his private key.
- Bob has the (possibly modified) plaintext.

Note that in order to perform an MITM attack, the attacker must be able to intercept and modify all messages between Alice and Bob. Merely viewing public keys does not compromise asymmetric cryptography.

The world currently has a widely used (and *pretty* solid) way to protect the authenticity of public keys shared in-time over an insecure channel, and we will talk about that in the Public Key Infrastructure chapter. If this technique is used, Alice should have noticed that the public key is not authentic.

The two scenarios above also apply to signing and signature verification. For brevity, only the ideal one is detailed.

## Ideal workflow for signing and signature verification

- Alice has a piece of data to publish.
- Alice signs (transforms) a cryptographic hash of the data with her private key. This is the **signature**.
- Alice sends the original data along with the signature.
- Bob receives the data along with the signature.
- Bob calculates the hash of the data.
- Bob transforms the signature with Alice’s public key, and gets the authentic hash of the data.
- If the hashes match, the signature is verified and the data is authentic. Else, the data or the signature has changed.
