# Elliptic Curve

Asymmetric Encryption

## Brief Overview

This doc will focus on x25519 , which is based on Curve25519

Elliptic Curve Cryptography can be used to derive a shared secret between two parties, with an adversary in the middle able to see all messages, but unable to obtain the secret key.

It consists of both parties having a private and a public key. However, unlike RSA, the public key isn't used to encrypt the payload directly, but as a part of the key exchange to arrive at the shared secret. We will use good old Alice & Bob as the parties on either end, with eve in the middle.

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


