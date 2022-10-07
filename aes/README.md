# AES

Symmetric encryption

Gen a 256 bit key & 128bit IV

```
$ openssl rand -hex 32 > aes.key
$ openssl rand -hex 16 > aes.iv
```

Prepare file to encrypt

```
$ echo "super secret message" > plaintext.txt
```

Encrypt file

```
$ openssl enc -aes-256-cbc -in plaintext.txt -e -K $(cat aes.key) -iv $(cat aes.iv) -out ciphertext.enc
```

Inspecting ciphertext:

```
$ xxd ciphertext.enc
00000000: 7a37 2327 73b4 f5f8 03a9 5fac bd05 a4eb  z7#'s....._.....
00000010: 32f2 474d e1db c03e 4a48 b46e faa2 94a6  2.GM...>JH.n....
```

Decrypting file

```
$ openssl enc -aes-256-cbc -in ciphertext.enc -d -K $(cat aes.key) -iv $(cat aes.iv) -out decrypted.txt
```

Inspecting the result of decryption

```
$ xxd decrypted.txt
00000000: 7375 7065 7220 7365 6372 6574 206d 6573  super secret mes
00000010: 7361 6765 0a                             sage.
```