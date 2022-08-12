# Elliptic Curve

Asymmetric Encryption

## Brief Overview

This doc will focus on x25519 , which is based on Curve25519

Elliptic Curve Cryptography can be used to derive a shared secret between two parties, with an adversary in the middle able to see all messages, but unable to obtain the secret key.

It consists of both parties having a private and a public key. However, unlike RSA, the public key isn't used to encrypt the payload directly, but as a part of the key exchange to arrive at the shared secret. We will use good old Alice & Bob as the parties on either end, with Eve in the middle. The goal is for Alice to send Bob a secret message, whose contents should not be revealed to Eve.

Specifically, this is Elliptic Curve Diffie Hellman

## Keys Involved

There are 4 initial keys:

* Alice's Public Key
* Alice's Private Key
* Bob's Public Key
* Bob's Private Key

Which will be used to derive a 256-bit AES key.

## Key generation

On the CLI, we can generate the required keys with openssl. Lets generate Alice's private key, and extract her public key from it:

```
$ openssl genpkey -algorithm x25519 -out aliceX.pem
$ openssl pkey -pubout -in aliceX.pem -out aliceX.pub
```

Similarly for Bob:

```
$ openssl genpkey -algorithm x25519 -out bobX.pem
$ openssl pkey -pubout -in bobX.pem -out bobX.pub
```

At this point, Alice & Bob have not communicated with each other, the state looks like:

```
+----------------+          +-----------------+           +----------------+
|    ALICE       |          |      EVE        |           |     BOB        |
| * aPub         |          |                 |           |  * bPub        |
| * aPri         |          |                 |           |  * bPri        |
|                |          |                 |           |                |
|                |          |                 |           |                |
+----------------+          +-----------------+           +----------------+
```

## Key exchange

Alice and Bob need each other's public keys. For this example, we will assume Eve can observe but not manipulate communication between Alice & Bob. (In the real world we need to employ other techniques to ensure the data is not tampered with, such as message signing).

Let us assume Bob announces his public key via some reputable channel. The state now looks like:

```
+----------------+          +-----------------+           +----------------+
|    ALICE       |          |      EVE        |           |     BOB        |
| * aPub         |          |   * bPub        |           |  * bPub        |
| * aPri         |          |                 |           |  * bPri        |
| * bPub         |          |                 |           |                |
|                |          |                 |           |                |
+------^---------+          +--------^--------+           +-------+--------+
       |                             |                            |
       |                             |                            |
       |                             |                            |
       +-----------------------------+----------------------------+
```

## Key Derivation (Alice)

Alice can now perform math on the elliptic curve , using her private key and Bob's public key, to arrive at the secret:

```
$ openssl pkeyutl -derive -out alicePriBobPub.key -inkey aliceX.pem -peerkey bobX.pub
$ xxd alicePriBobPub.key
00000000: 2ab8 f321 6406 6064 ede0 787d 0f0b 7c68  *..!d.`d..x}..|h
00000010: 41b5 891d 20c6 2384 0d10 e46c 8550 a01b  A... .#....l.P..
```

As you can see, the derived value is 32 bytes.

She can now use this key with a symmetric algorithm like [AES](../aes/README.md) to encrypt her message, producing symetrically encrypted ciphertext.

She can now send this ciphertext, along with her public key, to Bob over an open channel, which Eve can read. The state now looks like:

```
+----------------+         +------------------+           +----------------+
|    ALICE       |         |       EVE        |           |     BOB        |
| * aPub         |         |    * bPub        |           |  * bPub        |
| * aPri         |         |    * aPub        |           |  * bPri        |
| * bPub         |         |    * ciphertext  |           |  * aPub        |
| * aesKey       |         |                  |           |  * ciphertext  |
| * ciphertext   |         |                  |           |                |
|                |         |                  |           |                |
+------+---------+         +---------^--------+           +-------^--------+
       |                             |                            |
       +-----------------------------+----------------------------+
             (some public channel which Bob & Eve can read)
```

## Key Derivation (Bob)

Bob can now perform math on the elliptic curve, using his private key and Alice's public key, to arrive at the secret:

```
$ openssl pkeyutl -derive -out bobPriAlicePub.key -inkey bobX.pem -peerkey aliceX.pub
$ xxd bobPriAlicePub.key
00000000: 2ab8 f321 6406 6064 ede0 787d 0f0b 7c68  *..!d.`d..x}..|h
00000010: 41b5 891d 20c6 2384 0d10 e46c 8550 a01b  A... .#....l.P..
```

Which is the same secret as Alice! Bob can now use this to decrypt the ciphertext and read the message

The final state:

```
+----------------+         +------------------+           +----------------+
|    ALICE       |         |       EVE        |           |     BOB        |
| * aPub         |         |    * bPub        |           |  * bPub        |
| * aPri         |         |    * aPub        |           |  * bPri        |
| * bPub         |         |    * ciphertext  |           |  * aPub        |
| * aesKey       |         |                  |           |  * ciphertext  |
| * ciphertext   |         |                  |           |  * aesKey      |
|                |         |                  |           |                |
+----------------+         +------------------+           +----------------+
```

## What about Eve?

Even though Eve has both Alice & Bob's public keys, the math behind elliptic curves ensures the shared secret cannot be dervied with just those values. So even if Eve has the ciphertext, she can't generate the key required to decrypt it.

## Notes

If Alice used AES encryption, she would need to trasmit the IV to bob as well. However the IV isn't secret, so it can be transmitted along with the ciphertext, visible to Eve

## References

* https://x25519.xargs.org/
* https://datatracker.ietf.org/doc/html/rfc7748
* https://crypto.stackexchange.com/a/75548
* https://crypto.stackexchange.com/a/71563
