# RSA

The commands used here use OpenSSL 3.1.1

```sh
$ openssl version
OpenSSL 3.1.1 30 May 2023 (Library: OpenSSL 3.1.1 30 May 2023)
```

## Overview

// TODO: Some kinda explanantion of public key crypto?

## Characters

We'll go with good old Alice & Bob again, both of them using 2048-bit RSA keys.

Generating Alice's keys:

```
$ openssl genpkey -algorithm RSA -out alice_private.pem -pkeyopt rsa_keygen_bits:2048
$ openssl rsa -in alice_private.pem -pubout -out alice_public.pem
```

Generating Bob's keys:

```
$ openssl genpkey -algorithm RSA -out bob_private.pem -pkeyopt rsa_keygen_bits:2048
$ openssl rsa -in bob_private.pem -pubout -out bob_public.pem
```
