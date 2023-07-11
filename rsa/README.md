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

For our adversary, we will use Eve. Generating Eve's keys:

```
$ openssl genpkey -algorithm RSA -out eve_private.pem -pkeyopt rsa_keygen_bits:2048
$ openssl rsa -in eve_private.pem -pubout -out eve_public.pem
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


## Signing

Signing helps verify, for instance if Alice is sending a message to Bob, that it is really Alice who sent it.

We will create a new file of 1MiB to sign.

```
$ head -c 1M /dev/urandom > plaintext_1M.bin
$ sha256sum plaintext_1M.bin 
c0db01a5020d4696c732d490be67dff945d7d912fa5c8292a55eaffa77d1d560  plaintext_1M.bin
$ openssl pkeyutl -sign -inkey alice_private.pem -in plaintext_1M.bin -rawin -out plaintext.sig
$ xxd plaintext.sig 
00000000: 9fb0 de66 2bd0 9aee 0442 315d e7f9 4aec  ...f+....B1]..J.
00000010: 671a 2624 45c7 0bbf c974 dc35 a12b 6cb9  g.&$E....t.5.+l.
00000020: 0381 2664 aad6 934d 1b95 9028 6930 d9db  ..&d...M...(i0..
00000030: f546 0287 ce0e 522d c03f 154f a9d2 da2f  .F....R-.?.O.../
00000040: 253e 8072 149a 0886 6312 7ca4 e190 b663  %>.r....c.|....c
00000050: 6291 ee21 bef6 467d 85bd 08b3 4d37 3d6b  b..!..F}....M7=k
00000060: 0603 5284 e061 c55a 9f20 c1ef c69a 946b  ..R..a.Z. .....k
00000070: 0f3d cfab 5e13 2da7 0ca9 c71f a1b1 bd0a  .=..^.-.........
00000080: 594e 24fa ee44 5b03 5ab6 587c 7149 b4aa  YN$..D[.Z.X|qI..
00000090: f5f0 56ab de9f 0e7c cfdb 9d5c 00ce 5d0b  ..V....|...\..].
000000a0: 7e86 2f0e 316a 4486 5130 9999 3d3a efde  ~./.1jD.Q0..=:..
000000b0: 6b27 4cb2 bd5a 4531 26cc efe9 25a3 c4d3  k'L..ZE1&...%...
000000c0: b29f 949a 18c7 378b 66bb 1ea1 d96b 93f0  ......7.f....k..
000000d0: a7da 6c39 245a fb29 65f2 9396 70ba cd38  ..l9$Z.)e...p..8
000000e0: df55 2e51 4045 f66c 7c9b 2850 d566 1f78  .U.Q@E.l|.(P.f.x
000000f0: ab1d 8a7c df40 dd89 a83d ee2b e07d 18fc  ...|.@...=.+.}..
```

Actually, we sign a hash of the data, since RSA keys can only sign data less than the keylength. By default SHA256 is used.

## Verification

We now have a signature from Alice for the plaintext.

Bob can verify this signature using Alice's public key:

```
$ openssl pkeyutl -verify -inkey alice_public.pem -pubin -in plaintext_1M.bin -rawin -sigfile plaintext.sig 
Signature Verified Successfully
```

### Wrong plaintext

If we try and verify the wrong plaintext, we will get a failure

```
$ openssl pkeyutl -verify -inkey alice_public.pem -pubin -in plaintext.bin -rawin -sigfile plaintext.sig 
Signature Verification Failure
40279503327F0000:error:02000068:rsa routines:ossl_rsa_verify:bad signature:crypto/rsa/rsa_sign.c:430:
40279503327F0000:error:1C880004:Provider routines:rsa_verify:RSA lib:providers/implementations/signature/rsa_sig.c:788:
```

### Signed by someone else

Let's assume Eve wants to pretend to be Alice. Since she wouldn't have Alice's private key, she just generates one of her own (`eve_private.pem`) and uses that to sign the data:

```
$ openssl pkeyutl -sign -inkey eve_private.pem -in plaintext_1M.bin -rawin -out plaintext_eve.sig
```

Let's see what happens if Bob tries to verify this:

```
$ openssl pkeyutl -verify -inkey alice_public.pem -pubin -in plaintext_1M.bin -rawin -sigfile plaintext_eve.sig 
Signature Verification Failure
40A715C9097F0000:error:0200008A:rsa routines:RSA_padding_check_PKCS1_type_1:invalid padding:crypto/rsa/rsa_pk1.c:75:
40A715C9097F0000:error:02000072:rsa routines:rsa_ossl_public_decrypt:padding check failed:crypto/rsa/rsa_ossl.c:598:
40A715C9097F0000:error:1C880004:Provider routines:rsa_verify:RSA lib:providers/implementations/signature/rsa_sig.c:788:
```

Thus Bob knows that the message didn't really come from Alice!

## Recovering the message hash

RSA signatures don't sign the entire plaintext technically, but only it's hash. We can use the `-verifyrecover` option with a public key, to extract the hash from a signature, _even if we don't have the original plaintext_.

```
$ openssl pkeyutl -verifyrecover -inkey alice_public.pem -pubin -in plaintext.sig -asn1parse 
    0:d=0  hl=2 l=  49 cons: SEQUENCE          
    2:d=1  hl=2 l=  13 cons:  SEQUENCE          
    4:d=2  hl=2 l=   9 prim:   OBJECT            :sha256
   15:d=2  hl=2 l=   0 prim:   NULL              
   17:d=1  hl=2 l=  32 prim:  OCTET STRING      
      0000 - c0 db 01 a5 02 0d 46 96-c7 32 d4 90 be 67 df f9   ......F..2...g..
      0010 - 45 d7 d9 12 fa 5c 82 92-a5 5e af fa 77 d1 d5 60   E....\...^..w..`
```

The OCTET STRING section contains the hash of the original plaintext, which we can see matches what we started with!

```
$ sha256sum plaintext_1M.bin 
c0db01a5020d4696c732d490be67dff945d7d912fa5c8292a55eaffa77d1d560  plaintext_1M.bin
```
