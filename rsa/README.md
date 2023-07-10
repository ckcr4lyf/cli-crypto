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


## Encryption

We'll prepare some data first. For this example, we will use 32 bytes of random data, which could represent a 256bit AES key for instance.

```
$ head -c 32 /dev/urandom > plaintext.bin
$ sha256sum plaintext.bin 
a7481ba2300a76b58f659d2078d3468c5c570e2060d2d41f38ace0da008b431a  plaintext.bin
$ xxd plaintext.bin 
00000000: 8822 9fb2 f625 2968 6605 cbda fafb 4b91  ."...%)hf.....K.
00000010: 7098 b639 a884 ae43 35f1 6927 b42a 5dfc  p..9...C5.i'.*].
```

Now, we pretend to be alice, and encrypt this file to bob. For this example, we will use explicit RSA padding - PKCS#1.

```
$ openssl pkeyutl -encrypt -in plaintext.bin -pubin -inkey bob_public.pem -pkeyopt rsa_padding_mode:pkcs1 -out encrypted_to_bob.bin
$ sha256sum encrypted_to_bob.bin 
3c7959ce3145a78c8c4a445a1243ae004c593e51e08d93b1e413d455c0c1ad97  encrypted_to_bob.bin
$ xxd encrypted_to_bob.bin 
00000000: 150b 9785 b9bc 516e 528d b516 4d8f 9117  ......QnR...M...
00000010: e375 625f 0436 3d5c b0e3 0f02 0405 c12e  .ub_.6=\........
00000020: f7e6 c051 c44b 1af5 d7bd 3d02 6469 19fd  ...Q.K....=.di..
00000030: eba4 12fc 40b5 0214 9124 60df f9bd afec  ....@....$`.....
00000040: 882a c2f4 7a84 56cb 2989 cbc7 f23b eef0  .*..z.V.)....;..
00000050: cd7a 5ac5 7c7f 0e36 d6e7 4247 dd7b 4e21  .zZ.|..6..BG.{N!
00000060: 547d b5ce d34c 8046 38b9 eae4 14fd 8039  T}...L.F8......9
00000070: 5d7f c270 ddd4 7d4f f86c e467 c95a ae7f  ]..p..}O.l.g.Z..
00000080: f230 95f6 bd62 d1f6 297e 25ad 33c2 5502  .0...b..)~%.3.U.
00000090: af63 8c9a 6298 21c2 382f c88e 4064 238c  .c..b.!.8/..@d#.
000000a0: 3744 ed92 35b2 e299 b4ed 1705 1101 3261  7D..5.........2a
000000b0: 8389 941f fb88 b84e d6fa 1852 e164 fe7c  .......N...R.d.|
000000c0: 3a76 9b20 c100 397e 3a1f 6cff a341 73c5  :v. ..9~:.l..As.
000000d0: a583 30f4 c788 a125 c905 8bca 056e fb35  ..0....%.....n.5
000000e0: fe78 3349 0310 c95d 7948 632c 3622 3de3  .x3I...]yHc,6"=.
000000f0: 2cd8 8ac1 abc4 f5c5 66fc 6120 eebe c47b  ,.......f.a ...{
```

## Decryption

We will now decrypt it as if we were bob

```
$ openssl pkeyutl -decrypt -in encrypted_to_bob.bin -inkey bob_private.pem -pkeyopt rsa_padding_mode:pkcs1 -out decrypted_by_bob.txt
$ sha256sum decrypted_by_bob.txt 
a7481ba2300a76b58f659d2078d3468c5c570e2060d2d41f38ace0da008b431a  decrypted_by_bob.txt
$ xxd decrypted_by_bob.txt 
00000000: 8822 9fb2 f625 2968 6605 cbda fafb 4b91  ."...%)hf.....K.
00000010: 7098 b639 a884 ae43 35f1 6927 b42a 5dfc  p..9...C5.i'.*].
```

This is the same as the original plaintext!
