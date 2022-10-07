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
00000000: 2a4e beb2 9fac bc5b f814 6743 b4bd 0ae0  *N.....[..gC....
00000010: bed0 2025 95f3 aa67 c28e 8398 6417 575e  .. %...g....d.W^
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