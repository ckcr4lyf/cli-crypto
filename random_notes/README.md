Some fun quirks discovered playing around with certs and the like.

## Full chain certs

A TLS cert for a website is usually signed by an intermediary CA, which is then signed by a root CA. 

Each cert can be a PEM file, and if you concatenate them all together, you've got a chain.

<details>

<summary> Using OpenSSL to connect and view certs</summary>

```
$ openssl s_client -connect tracker.mywaifu.best:443 -showcerts
CONNECTED(00000003)
depth=2 C = US, O = Internet Security Research Group, CN = ISRG Root X1
verify return:1
depth=1 C = US, O = Let's Encrypt, CN = R3
verify return:1
depth=0 CN = tracker.mywaifu.best
verify return:1
---
Certificate chain
 0 s:CN = tracker.mywaifu.best
   i:C = US, O = Let's Encrypt, CN = R3
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Apr 29 13:05:23 2023 GMT; NotAfter: Jul 28 13:05:22 2023 GMT
-----BEGIN CERTIFICATE-----
MIIFLTCCBBWgAwIBAgISBLQMkpV+psppoe9qWfe19/yXMA0GCSqGSIb3DQEBCwUA
MDIxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MQswCQYDVQQD
EwJSMzAeFw0yMzA0MjkxMzA1MjNaFw0yMzA3MjgxMzA1MjJaMB8xHTAbBgNVBAMT
FHRyYWNrZXIubXl3YWlmdS5iZXN0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIB
CgKCAQEAmqCmv89bSQWIt9hCdgwUALgmj78KRSMFDIKCbRihVzgc5Y0QqepHlFNv
GP6SrgiR6UhiJsgkybYLoEWoreCJ8OOAlVhVTbk2679Cx4Qo+RPeTzxOF9oHYjTW
ZCCMlfp/55IkGtSAkH+6lYb/+TAdRXTnpqiPHjESi+QacTdfz8pNb4gitQ1U0Blp
ztVxPx9WftIwkr+A4KAkAdtExyHxVMZhTdTb4vPsUw53dcHZn8M1zAoM3LUF5y8Q
e2lvORQmhKFcaMzhXRUBpbSV6uTkooxGSqCk/jveEta9nZsi/IUslt1FrsNuCG3W
HR3jOtJEDg0DoGMRMtB2RXeK/mldSQIDAQABo4ICTjCCAkowDgYDVR0PAQH/BAQD
AgWgMB0GA1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAMBgNVHRMBAf8EAjAA
MB0GA1UdDgQWBBQRSmMJr9KeH/BWMpjMWx+1MTtRDjAfBgNVHSMEGDAWgBQULrMX
t1hWy65QCUDmH6+dixTCxjBVBggrBgEFBQcBAQRJMEcwIQYIKwYBBQUHMAGGFWh0
dHA6Ly9yMy5vLmxlbmNyLm9yZzAiBggrBgEFBQcwAoYWaHR0cDovL3IzLmkubGVu
Y3Iub3JnLzAfBgNVHREEGDAWghR0cmFja2VyLm15d2FpZnUuYmVzdDBMBgNVHSAE
RTBDMAgGBmeBDAECATA3BgsrBgEEAYLfEwEBATAoMCYGCCsGAQUFBwIBFhpodHRw
Oi8vY3BzLmxldHNlbmNyeXB0Lm9yZzCCAQMGCisGAQQB1nkCBAIEgfQEgfEA7wB1
AHoyjFTYty22IOo44FIe6YQWcDIThU070ivBOlejUutSAAABh81VKZcAAAQDAEYw
RAIgUPXD7nXACatXd8j2asWKlJLX0urG3hE5zNVKwctFdgoCIHrLit3HMh9TQ3Rf
0KrHP8cYTfLenvUDN89wGTp34pkYAHYAtz77JN+cTbp18jnFulj0bF38Qs96nzXE
nh0JgSXttJkAAAGHzVUpsAAABAMARzBFAiBn90l6iTjefMJe02INy+68Cvv2xe0P
PJF36t85FpA7IQIhAN8NL88+UTTAHgm+C6bg9Lrpur72crfkzFa6EVMP6LSgMA0G
CSqGSIb3DQEBCwUAA4IBAQB3fjio5ECF5mVSeeyITW7yObPg16lXkYDQ8tORY36R
54HtY5SGjlz+tSOqcZ9I6/nEsGNyc08aSM0u8jgToizHDQ92qhO4X16sWb3K34AX
JwhCiiRB5Kl7afJj1SS3bkeq4EJL6acGGjpNtybN9BmgPMiqRjpMpcFWLmMd2W9o
0hPh+R6G1SgbTsqYyWZlo0ZJ48Xd9dWJkXo7FMWaDKzG3cfXlTkOSMrcbjuwqrAo
601AalkM0NMNhneJ7bV24ys3KLQ1a4mAIIMqGtzgZHMbvY/PVUOQBdk1q4Q6urhy
GvCyLGE+3o8FP/Df1Nbws7Js8GwKInBx4Gdn28QLiKaY
-----END CERTIFICATE-----
 1 s:C = US, O = Let's Encrypt, CN = R3
   i:C = US, O = Internet Security Research Group, CN = ISRG Root X1
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Sep  4 00:00:00 2020 GMT; NotAfter: Sep 15 16:00:00 2025 GMT
-----BEGIN CERTIFICATE-----
MIIFFjCCAv6gAwIBAgIRAJErCErPDBinU/bWLiWnX1owDQYJKoZIhvcNAQELBQAw
TzELMAkGA1UEBhMCVVMxKTAnBgNVBAoTIEludGVybmV0IFNlY3VyaXR5IFJlc2Vh
cmNoIEdyb3VwMRUwEwYDVQQDEwxJU1JHIFJvb3QgWDEwHhcNMjAwOTA0MDAwMDAw
WhcNMjUwOTE1MTYwMDAwWjAyMQswCQYDVQQGEwJVUzEWMBQGA1UEChMNTGV0J3Mg
RW5jcnlwdDELMAkGA1UEAxMCUjMwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEK
AoIBAQC7AhUozPaglNMPEuyNVZLD+ILxmaZ6QoinXSaqtSu5xUyxr45r+XXIo9cP
R5QUVTVXjJ6oojkZ9YI8QqlObvU7wy7bjcCwXPNZOOftz2nwWgsbvsCUJCWH+jdx
sxPnHKzhm+/b5DtFUkWWqcFTzjTIUu61ru2P3mBw4qVUq7ZtDpelQDRrK9O8Zutm
NHz6a4uPVymZ+DAXXbpyb/uBxa3Shlg9F8fnCbvxK/eG3MHacV3URuPMrSXBiLxg
Z3Vms/EY96Jc5lP/Ooi2R6X/ExjqmAl3P51T+c8B5fWmcBcUr2Ok/5mzk53cU6cG
/kiFHaFpriV1uxPMUgP17VGhi9sVAgMBAAGjggEIMIIBBDAOBgNVHQ8BAf8EBAMC
AYYwHQYDVR0lBBYwFAYIKwYBBQUHAwIGCCsGAQUFBwMBMBIGA1UdEwEB/wQIMAYB
Af8CAQAwHQYDVR0OBBYEFBQusxe3WFbLrlAJQOYfr52LFMLGMB8GA1UdIwQYMBaA
FHm0WeZ7tuXkAXOACIjIGlj26ZtuMDIGCCsGAQUFBwEBBCYwJDAiBggrBgEFBQcw
AoYWaHR0cDovL3gxLmkubGVuY3Iub3JnLzAnBgNVHR8EIDAeMBygGqAYhhZodHRw
Oi8veDEuYy5sZW5jci5vcmcvMCIGA1UdIAQbMBkwCAYGZ4EMAQIBMA0GCysGAQQB
gt8TAQEBMA0GCSqGSIb3DQEBCwUAA4ICAQCFyk5HPqP3hUSFvNVneLKYY611TR6W
PTNlclQtgaDqw+34IL9fzLdwALduO/ZelN7kIJ+m74uyA+eitRY8kc607TkC53wl
ikfmZW4/RvTZ8M6UK+5UzhK8jCdLuMGYL6KvzXGRSgi3yLgjewQtCPkIVz6D2QQz
CkcheAmCJ8MqyJu5zlzyZMjAvnnAT45tRAxekrsu94sQ4egdRCnbWSDtY7kh+BIm
lJNXoB1lBMEKIq4QDUOXoRgffuDghje1WrG9ML+Hbisq/yFOGwXD9RiX8F6sw6W4
avAuvDszue5L3sz85K+EC4Y/wFVDNvZo4TYXao6Z0f+lQKc0t8DQYzk1OXVu8rp2
yJMC6alLbBfODALZvYH7n7do1AZls4I9d1P4jnkDrQoxB3UqQ9hVl3LEKQ73xF1O
yK5GhDDX8oVfGKF5u+decIsH4YaTw7mP3GFxJSqv3+0lUFJoi5Lc5da149p90Ids
hCExroL1+7mryIkXPeFM5TgO9r0rvZaBFOvV2z0gp35Z0+L4WPlbuEjN/lxPFin+
HlUjr8gRsI3qfJOQFy/9rKIJR0Y/8Omwt/8oTWgy1mdeHmmjk7j1nYsvC9JSQ6Zv
MldlTTKB3zhThV1+XWYp6rjd5JW1zbVWEkLNxE7GJThEUG3szgBVGP7pSWTUTsqX
nLRbwHOoq7hHwg==
-----END CERTIFICATE-----
 2 s:C = US, O = Internet Security Research Group, CN = ISRG Root X1
   i:O = Digital Signature Trust Co., CN = DST Root CA X3
   a:PKEY: rsaEncryption, 4096 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jan 20 19:14:03 2021 GMT; NotAfter: Sep 30 18:14:03 2024 GMT
-----BEGIN CERTIFICATE-----
MIIFYDCCBEigAwIBAgIQQAF3ITfU6UK47naqPGQKtzANBgkqhkiG9w0BAQsFADA/
MSQwIgYDVQQKExtEaWdpdGFsIFNpZ25hdHVyZSBUcnVzdCBDby4xFzAVBgNVBAMT
DkRTVCBSb290IENBIFgzMB4XDTIxMDEyMDE5MTQwM1oXDTI0MDkzMDE4MTQwM1ow
TzELMAkGA1UEBhMCVVMxKTAnBgNVBAoTIEludGVybmV0IFNlY3VyaXR5IFJlc2Vh
cmNoIEdyb3VwMRUwEwYDVQQDEwxJU1JHIFJvb3QgWDEwggIiMA0GCSqGSIb3DQEB
AQUAA4ICDwAwggIKAoICAQCt6CRz9BQ385ueK1coHIe+3LffOJCMbjzmV6B493XC
ov71am72AE8o295ohmxEk7axY/0UEmu/H9LqMZshftEzPLpI9d1537O4/xLxIZpL
wYqGcWlKZmZsj348cL+tKSIG8+TA5oCu4kuPt5l+lAOf00eXfJlII1PoOK5PCm+D
LtFJV4yAdLbaL9A4jXsDcCEbdfIwPPqPrt3aY6vrFk/CjhFLfs8L6P+1dy70sntK
4EwSJQxwjQMpoOFTJOwT2e4ZvxCzSow/iaNhUd6shweU9GNx7C7ib1uYgeGJXDR5
bHbvO5BieebbpJovJsXQEOEO3tkQjhb7t/eo98flAgeYjzYIlefiN5YNNnWe+w5y
sR2bvAP5SQXYgd0FtCrWQemsAXaVCg/Y39W9Eh81LygXbNKYwagJZHduRze6zqxZ
Xmidf3LWicUGQSk+WT7dJvUkyRGnWqNMQB9GoZm1pzpRboY7nn1ypxIFeFntPlF4
FQsDj43QLwWyPntKHEtzBRL8xurgUBN8Q5N0s8p0544fAQjQMNRbcTa0B7rBMDBc
SLeCO5imfWCKoqMpgsy6vYMEG6KDA0Gh1gXxG8K28Kh8hjtGqEgqiNx2mna/H2ql
PRmP6zjzZN7IKw0KKP/32+IVQtQi0Cdd4Xn+GOdwiK1O5tmLOsbdJ1Fu/7xk9TND
TwIDAQABo4IBRjCCAUIwDwYDVR0TAQH/BAUwAwEB/zAOBgNVHQ8BAf8EBAMCAQYw
SwYIKwYBBQUHAQEEPzA9MDsGCCsGAQUFBzAChi9odHRwOi8vYXBwcy5pZGVudHJ1
c3QuY29tL3Jvb3RzL2RzdHJvb3RjYXgzLnA3YzAfBgNVHSMEGDAWgBTEp7Gkeyxx
+tvhS5B1/8QVYIWJEDBUBgNVHSAETTBLMAgGBmeBDAECATA/BgsrBgEEAYLfEwEB
ATAwMC4GCCsGAQUFBwIBFiJodHRwOi8vY3BzLnJvb3QteDEubGV0c2VuY3J5cHQu
b3JnMDwGA1UdHwQ1MDMwMaAvoC2GK2h0dHA6Ly9jcmwuaWRlbnRydXN0LmNvbS9E
U1RST09UQ0FYM0NSTC5jcmwwHQYDVR0OBBYEFHm0WeZ7tuXkAXOACIjIGlj26Ztu
MA0GCSqGSIb3DQEBCwUAA4IBAQAKcwBslm7/DlLQrt2M51oGrS+o44+/yQoDFVDC
5WxCu2+b9LRPwkSICHXM6webFGJueN7sJ7o5XPWioW5WlHAQU7G75K/QosMrAdSW
9MUgNTP52GE24HGNtLi1qoJFlcDyqSMo59ahy2cI2qBDLKobkx/J3vWraV0T9VuG
WCLKTVXkcGdtwlfFRjlBz4pYg1htmf5X6DYO8A4jqv2Il9DjXA6USbW1FzXSLr9O
he8Y4IWS6wY7bCkjCWDcRQJMEhg76fsO3txE+FiYruq9RUWhiF1myv4Q6W+CyBFC
Dfvp7OOGAN6dEOM4+qR9sdjoSYKEBpsr6GtPAQw4dy753ec5
-----END CERTIFICATE-----
---
Server certificate
subject=CN = tracker.mywaifu.best
issuer=C = US, O = Let's Encrypt, CN = R3
---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 4585 bytes and written 406 bytes
Verification: OK
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 2048 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
---
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 8F57D5A610307B2EB5F66A038F4CFFACFB3DC9F0F0F8462BB41CC8FC7E347633
    Session-ID-ctx:
    Resumption PSK: 9BBC01FA7242FBD87617E0ED34A3864500608ACDF34F9CF00932CB114155D9ADC2429E4489B6D5E7BDD8E0424C5E2C8B
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 86400 (seconds)
    TLS session ticket:
    0000 - 86 af ac 50 33 90 13 9c-5a 48 20 41 31 65 1d f1   ...P3...ZH A1e..
    0010 - 4b f3 b4 21 d2 28 0f ec-ce 21 9b ca 45 4c f4 6b   K..!.(...!..EL.k

    Start Time: 1683775902
    Timeout   : 7200 (sec)
    Verify return code: 0 (ok)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 546A75128836238AA7A529AD1961FAA42493DC633BB8E1811451CD5D88D2691A
    Session-ID-ctx:
    Resumption PSK: 8B0BED37B60929F37A5523FEAC79E171B7D07E2FB97C7ED27CF1010074C97DD0A1F423859D4D1CA00CE5AFB708A6D0B7
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 86400 (seconds)
    TLS session ticket:
    0000 - 21 3f f3 d1 f0 be 44 78-34 f6 a1 f8 54 77 d9 11   !?....Dx4...Tw..
    0010 - bb 84 31 57 2b d1 f1 50-8b cd 43 dd db 3d 0f ef   ..1W+..P..C..=..

    Start Time: 1683775902
    Timeout   : 7200 (sec)
    Verify return code: 0 (ok)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
```

</details>

If you notice, above we have 3 certs:

1. For CN=tracker.mywaifu.best (signed by Let's Encrypt , CN=R3)
2. For Let's Encrypt (signed by Internet Security Research Group , CN=ISRG Root X1)
3. For Internet Security Research Group (signed by Digital Signature Trust Co., CN=DST Root CA X3)

But we don't have the final root cert from OpenSSL. This is because the root cert is _in your system_.

If we manually concatenate the certs to `waifu.pem` , we can inspect the certs with OpenSSL.

<details>

<summary>Inspecting the `.pem` chain with OpenSSL</summary>

```
$ openssl storeutl -noout -text -certs waifu.pem
0: Certificate
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            04:b4:0c:92:95:7e:a6:ca:69:a1:ef:6a:59:f7:b5:f7:fc:97
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=US, O=Let's Encrypt, CN=R3
        Validity
            Not Before: Apr 29 13:05:23 2023 GMT
            Not After : Jul 28 13:05:22 2023 GMT
        Subject: CN=tracker.mywaifu.best
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:9a:a0:a6:bf:cf:5b:49:05:88:b7:d8:42:76:0c:
                    14:00:b8:26:8f:bf:0a:45:23:05:0c:82:82:6d:18:
                    a1:57:38:1c:e5:8d:10:a9:ea:47:94:53:6f:18:fe:
                    92:ae:08:91:e9:48:62:26:c8:24:c9:b6:0b:a0:45:
                    a8:ad:e0:89:f0:e3:80:95:58:55:4d:b9:36:eb:bf:
                    42:c7:84:28:f9:13:de:4f:3c:4e:17:da:07:62:34:
                    d6:64:20:8c:95:fa:7f:e7:92:24:1a:d4:80:90:7f:
                    ba:95:86:ff:f9:30:1d:45:74:e7:a6:a8:8f:1e:31:
                    12:8b:e4:1a:71:37:5f:cf:ca:4d:6f:88:22:b5:0d:
                    54:d0:19:69:ce:d5:71:3f:1f:56:7e:d2:30:92:bf:
                    80:e0:a0:24:01:db:44:c7:21:f1:54:c6:61:4d:d4:
                    db:e2:f3:ec:53:0e:77:75:c1:d9:9f:c3:35:cc:0a:
                    0c:dc:b5:05:e7:2f:10:7b:69:6f:39:14:26:84:a1:
                    5c:68:cc:e1:5d:15:01:a5:b4:95:ea:e4:e4:a2:8c:
                    46:4a:a0:a4:fe:3b:de:12:d6:bd:9d:9b:22:fc:85:
                    2c:96:dd:45:ae:c3:6e:08:6d:d6:1d:1d:e3:3a:d2:
                    44:0e:0d:03:a0:63:11:32:d0:76:45:77:8a:fe:69:
                    5d:49
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Key Usage: critical
                Digital Signature, Key Encipherment
            X509v3 Extended Key Usage:
                TLS Web Server Authentication, TLS Web Client Authentication
            X509v3 Basic Constraints: critical
                CA:FALSE
            X509v3 Subject Key Identifier:
                11:4A:63:09:AF:D2:9E:1F:F0:56:32:98:CC:5B:1F:B5:31:3B:51:0E
            X509v3 Authority Key Identifier:
                14:2E:B3:17:B7:58:56:CB:AE:50:09:40:E6:1F:AF:9D:8B:14:C2:C6
            Authority Information Access:
                OCSP - URI:http://r3.o.lencr.org
                CA Issuers - URI:http://r3.i.lencr.org/
            X509v3 Subject Alternative Name:
                DNS:tracker.mywaifu.best
            X509v3 Certificate Policies:
                Policy: 2.23.140.1.2.1
                Policy: 1.3.6.1.4.1.44947.1.1.1
                  CPS: http://cps.letsencrypt.org
            CT Precertificate SCTs:
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : 7A:32:8C:54:D8:B7:2D:B6:20:EA:38:E0:52:1E:E9:84:
                                16:70:32:13:85:4D:3B:D2:2B:C1:3A:57:A3:52:EB:52
                    Timestamp : Apr 29 14:05:23.223 2023 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:44:02:20:50:F5:C3:EE:75:C0:09:AB:57:77:C8:F6:
                                6A:C5:8A:94:92:D7:D2:EA:C6:DE:11:39:CC:D5:4A:C1:
                                CB:45:76:0A:02:20:7A:CB:8A:DD:C7:32:1F:53:43:74:
                                5F:D0:AA:C7:3F:C7:18:4D:F2:DE:9E:F5:03:37:CF:70:
                                19:3A:77:E2:99:18
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : B7:3E:FB:24:DF:9C:4D:BA:75:F2:39:C5:BA:58:F4:6C:
                                5D:FC:42:CF:7A:9F:35:C4:9E:1D:09:81:25:ED:B4:99
                    Timestamp : Apr 29 14:05:23.248 2023 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:45:02:20:67:F7:49:7A:89:38:DE:7C:C2:5E:D3:62:
                                0D:CB:EE:BC:0A:FB:F6:C5:ED:0F:3C:91:77:EA:DF:39:
                                16:90:3B:21:02:21:00:DF:0D:2F:CF:3E:51:34:C0:1E:
                                09:BE:0B:A6:E0:F4:BA:E9:BA:BE:F6:72:B7:E4:CC:56:
                                BA:11:53:0F:E8:B4:A0
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        77:7e:38:a8:e4:40:85:e6:65:52:79:ec:88:4d:6e:f2:39:b3:
        e0:d7:a9:57:91:80:d0:f2:d3:91:63:7e:91:e7:81:ed:63:94:
        86:8e:5c:fe:b5:23:aa:71:9f:48:eb:f9:c4:b0:63:72:73:4f:
        1a:48:cd:2e:f2:38:13:a2:2c:c7:0d:0f:76:aa:13:b8:5f:5e:
        ac:59:bd:ca:df:80:17:27:08:42:8a:24:41:e4:a9:7b:69:f2:
        63:d5:24:b7:6e:47:aa:e0:42:4b:e9:a7:06:1a:3a:4d:b7:26:
        cd:f4:19:a0:3c:c8:aa:46:3a:4c:a5:c1:56:2e:63:1d:d9:6f:
        68:d2:13:e1:f9:1e:86:d5:28:1b:4e:ca:98:c9:66:65:a3:46:
        49:e3:c5:dd:f5:d5:89:91:7a:3b:14:c5:9a:0c:ac:c6:dd:c7:
        d7:95:39:0e:48:ca:dc:6e:3b:b0:aa:b0:28:eb:4d:40:6a:59:
        0c:d0:d3:0d:86:77:89:ed:b5:76:e3:2b:37:28:b4:35:6b:89:
        80:20:83:2a:1a:dc:e0:64:73:1b:bd:8f:cf:55:43:90:05:d9:
        35:ab:84:3a:ba:b8:72:1a:f0:b2:2c:61:3e:de:8f:05:3f:f0:
        df:d4:d6:f0:b3:b2:6c:f0:6c:0a:22:70:71:e0:67:67:db:c4:
        0b:88:a6:98
1: Certificate
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            91:2b:08:4a:cf:0c:18:a7:53:f6:d6:2e:25:a7:5f:5a
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=US, O=Internet Security Research Group, CN=ISRG Root X1
        Validity
            Not Before: Sep  4 00:00:00 2020 GMT
            Not After : Sep 15 16:00:00 2025 GMT
        Subject: C=US, O=Let's Encrypt, CN=R3
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:bb:02:15:28:cc:f6:a0:94:d3:0f:12:ec:8d:55:
                    92:c3:f8:82:f1:99:a6:7a:42:88:a7:5d:26:aa:b5:
                    2b:b9:c5:4c:b1:af:8e:6b:f9:75:c8:a3:d7:0f:47:
                    94:14:55:35:57:8c:9e:a8:a2:39:19:f5:82:3c:42:
                    a9:4e:6e:f5:3b:c3:2e:db:8d:c0:b0:5c:f3:59:38:
                    e7:ed:cf:69:f0:5a:0b:1b:be:c0:94:24:25:87:fa:
                    37:71:b3:13:e7:1c:ac:e1:9b:ef:db:e4:3b:45:52:
                    45:96:a9:c1:53:ce:34:c8:52:ee:b5:ae:ed:8f:de:
                    60:70:e2:a5:54:ab:b6:6d:0e:97:a5:40:34:6b:2b:
                    d3:bc:66:eb:66:34:7c:fa:6b:8b:8f:57:29:99:f8:
                    30:17:5d:ba:72:6f:fb:81:c5:ad:d2:86:58:3d:17:
                    c7:e7:09:bb:f1:2b:f7:86:dc:c1:da:71:5d:d4:46:
                    e3:cc:ad:25:c1:88:bc:60:67:75:66:b3:f1:18:f7:
                    a2:5c:e6:53:ff:3a:88:b6:47:a5:ff:13:18:ea:98:
                    09:77:3f:9d:53:f9:cf:01:e5:f5:a6:70:17:14:af:
                    63:a4:ff:99:b3:93:9d:dc:53:a7:06:fe:48:85:1d:
                    a1:69:ae:25:75:bb:13:cc:52:03:f5:ed:51:a1:8b:
                    db:15
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Key Usage: critical
                Digital Signature, Certificate Sign, CRL Sign
            X509v3 Extended Key Usage:
                TLS Web Client Authentication, TLS Web Server Authentication
            X509v3 Basic Constraints: critical
                CA:TRUE, pathlen:0
            X509v3 Subject Key Identifier:
                14:2E:B3:17:B7:58:56:CB:AE:50:09:40:E6:1F:AF:9D:8B:14:C2:C6
            X509v3 Authority Key Identifier:
                79:B4:59:E6:7B:B6:E5:E4:01:73:80:08:88:C8:1A:58:F6:E9:9B:6E
            Authority Information Access:
                CA Issuers - URI:http://x1.i.lencr.org/
            X509v3 CRL Distribution Points:
                Full Name:
                  URI:http://x1.c.lencr.org/
            X509v3 Certificate Policies:
                Policy: 2.23.140.1.2.1
                Policy: 1.3.6.1.4.1.44947.1.1.1
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        85:ca:4e:47:3e:a3:f7:85:44:85:bc:d5:67:78:b2:98:63:ad:
        75:4d:1e:96:3d:33:65:72:54:2d:81:a0:ea:c3:ed:f8:20:bf:
        5f:cc:b7:70:00:b7:6e:3b:f6:5e:94:de:e4:20:9f:a6:ef:8b:
        b2:03:e7:a2:b5:16:3c:91:ce:b4:ed:39:02:e7:7c:25:8a:47:
        e6:65:6e:3f:46:f4:d9:f0:ce:94:2b:ee:54:ce:12:bc:8c:27:
        4b:b8:c1:98:2f:a2:af:cd:71:91:4a:08:b7:c8:b8:23:7b:04:
        2d:08:f9:08:57:3e:83:d9:04:33:0a:47:21:78:09:82:27:c3:
        2a:c8:9b:b9:ce:5c:f2:64:c8:c0:be:79:c0:4f:8e:6d:44:0c:
        5e:92:bb:2e:f7:8b:10:e1:e8:1d:44:29:db:59:20:ed:63:b9:
        21:f8:12:26:94:93:57:a0:1d:65:04:c1:0a:22:ae:10:0d:43:
        97:a1:18:1f:7e:e0:e0:86:37:b5:5a:b1:bd:30:bf:87:6e:2b:
        2a:ff:21:4e:1b:05:c3:f5:18:97:f0:5e:ac:c3:a5:b8:6a:f0:
        2e:bc:3b:33:b9:ee:4b:de:cc:fc:e4:af:84:0b:86:3f:c0:55:
        43:36:f6:68:e1:36:17:6a:8e:99:d1:ff:a5:40:a7:34:b7:c0:
        d0:63:39:35:39:75:6e:f2:ba:76:c8:93:02:e9:a9:4b:6c:17:
        ce:0c:02:d9:bd:81:fb:9f:b7:68:d4:06:65:b3:82:3d:77:53:
        f8:8e:79:03:ad:0a:31:07:75:2a:43:d8:55:97:72:c4:29:0e:
        f7:c4:5d:4e:c8:ae:46:84:30:d7:f2:85:5f:18:a1:79:bb:e7:
        5e:70:8b:07:e1:86:93:c3:b9:8f:dc:61:71:25:2a:af:df:ed:
        25:50:52:68:8b:92:dc:e5:d6:b5:e3:da:7d:d0:87:6c:84:21:
        31:ae:82:f5:fb:b9:ab:c8:89:17:3d:e1:4c:e5:38:0e:f6:bd:
        2b:bd:96:81:14:eb:d5:db:3d:20:a7:7e:59:d3:e2:f8:58:f9:
        5b:b8:48:cd:fe:5c:4f:16:29:fe:1e:55:23:af:c8:11:b0:8d:
        ea:7c:93:90:17:2f:fd:ac:a2:09:47:46:3f:f0:e9:b0:b7:ff:
        28:4d:68:32:d6:67:5e:1e:69:a3:93:b8:f5:9d:8b:2f:0b:d2:
        52:43:a6:6f:32:57:65:4d:32:81:df:38:53:85:5d:7e:5d:66:
        29:ea:b8:dd:e4:95:b5:cd:b5:56:12:42:cd:c4:4e:c6:25:38:
        44:50:6d:ec:ce:00:55:18:fe:e9:49:64:d4:4e:ca:97:9c:b4:
        5b:c0:73:a8:ab:b8:47:c2
2: Certificate
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            40:01:77:21:37:d4:e9:42:b8:ee:76:aa:3c:64:0a:b7
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: O=Digital Signature Trust Co., CN=DST Root CA X3
        Validity
            Not Before: Jan 20 19:14:03 2021 GMT
            Not After : Sep 30 18:14:03 2024 GMT
        Subject: C=US, O=Internet Security Research Group, CN=ISRG Root X1
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (4096 bit)
                Modulus:
                    00:ad:e8:24:73:f4:14:37:f3:9b:9e:2b:57:28:1c:
                    87:be:dc:b7:df:38:90:8c:6e:3c:e6:57:a0:78:f7:
                    75:c2:a2:fe:f5:6a:6e:f6:00:4f:28:db:de:68:86:
                    6c:44:93:b6:b1:63:fd:14:12:6b:bf:1f:d2:ea:31:
                    9b:21:7e:d1:33:3c:ba:48:f5:dd:79:df:b3:b8:ff:
                    12:f1:21:9a:4b:c1:8a:86:71:69:4a:66:66:6c:8f:
                    7e:3c:70:bf:ad:29:22:06:f3:e4:c0:e6:80:ae:e2:
                    4b:8f:b7:99:7e:94:03:9f:d3:47:97:7c:99:48:23:
                    53:e8:38:ae:4f:0a:6f:83:2e:d1:49:57:8c:80:74:
                    b6:da:2f:d0:38:8d:7b:03:70:21:1b:75:f2:30:3c:
                    fa:8f:ae:dd:da:63:ab:eb:16:4f:c2:8e:11:4b:7e:
                    cf:0b:e8:ff:b5:77:2e:f4:b2:7b:4a:e0:4c:12:25:
                    0c:70:8d:03:29:a0:e1:53:24:ec:13:d9:ee:19:bf:
                    10:b3:4a:8c:3f:89:a3:61:51:de:ac:87:07:94:f4:
                    63:71:ec:2e:e2:6f:5b:98:81:e1:89:5c:34:79:6c:
                    76:ef:3b:90:62:79:e6:db:a4:9a:2f:26:c5:d0:10:
                    e1:0e:de:d9:10:8e:16:fb:b7:f7:a8:f7:c7:e5:02:
                    07:98:8f:36:08:95:e7:e2:37:96:0d:36:75:9e:fb:
                    0e:72:b1:1d:9b:bc:03:f9:49:05:d8:81:dd:05:b4:
                    2a:d6:41:e9:ac:01:76:95:0a:0f:d8:df:d5:bd:12:
                    1f:35:2f:28:17:6c:d2:98:c1:a8:09:64:77:6e:47:
                    37:ba:ce:ac:59:5e:68:9d:7f:72:d6:89:c5:06:41:
                    29:3e:59:3e:dd:26:f5:24:c9:11:a7:5a:a3:4c:40:
                    1f:46:a1:99:b5:a7:3a:51:6e:86:3b:9e:7d:72:a7:
                    12:05:78:59:ed:3e:51:78:15:0b:03:8f:8d:d0:2f:
                    05:b2:3e:7b:4a:1c:4b:73:05:12:fc:c6:ea:e0:50:
                    13:7c:43:93:74:b3:ca:74:e7:8e:1f:01:08:d0:30:
                    d4:5b:71:36:b4:07:ba:c1:30:30:5c:48:b7:82:3b:
                    98:a6:7d:60:8a:a2:a3:29:82:cc:ba:bd:83:04:1b:
                    a2:83:03:41:a1:d6:05:f1:1b:c2:b6:f0:a8:7c:86:
                    3b:46:a8:48:2a:88:dc:76:9a:76:bf:1f:6a:a5:3d:
                    19:8f:eb:38:f3:64:de:c8:2b:0d:0a:28:ff:f7:db:
                    e2:15:42:d4:22:d0:27:5d:e1:79:fe:18:e7:70:88:
                    ad:4e:e6:d9:8b:3a:c6:dd:27:51:6e:ff:bc:64:f5:
                    33:43:4f
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints: critical
                CA:TRUE
            X509v3 Key Usage: critical
                Certificate Sign, CRL Sign
            Authority Information Access:
                CA Issuers - URI:http://apps.identrust.com/roots/dstrootcax3.p7c
            X509v3 Authority Key Identifier:
                C4:A7:B1:A4:7B:2C:71:FA:DB:E1:4B:90:75:FF:C4:15:60:85:89:10
            X509v3 Certificate Policies:
                Policy: 2.23.140.1.2.1
                Policy: 1.3.6.1.4.1.44947.1.1.1
                  CPS: http://cps.root-x1.letsencrypt.org
            X509v3 CRL Distribution Points:
                Full Name:
                  URI:http://crl.identrust.com/DSTROOTCAX3CRL.crl
            X509v3 Subject Key Identifier:
                79:B4:59:E6:7B:B6:E5:E4:01:73:80:08:88:C8:1A:58:F6:E9:9B:6E
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        0a:73:00:6c:96:6e:ff:0e:52:d0:ae:dd:8c:e7:5a:06:ad:2f:
        a8:e3:8f:bf:c9:0a:03:15:50:c2:e5:6c:42:bb:6f:9b:f4:b4:
        4f:c2:44:88:08:75:cc:eb:07:9b:14:62:6e:78:de:ec:27:ba:
        39:5c:f5:a2:a1:6e:56:94:70:10:53:b1:bb:e4:af:d0:a2:c3:
        2b:01:d4:96:f4:c5:20:35:33:f9:d8:61:36:e0:71:8d:b4:b8:
        b5:aa:82:45:95:c0:f2:a9:23:28:e7:d6:a1:cb:67:08:da:a0:
        43:2c:aa:1b:93:1f:c9:de:f5:ab:69:5d:13:f5:5b:86:58:22:
        ca:4d:55:e4:70:67:6d:c2:57:c5:46:39:41:cf:8a:58:83:58:
        6d:99:fe:57:e8:36:0e:f0:0e:23:aa:fd:88:97:d0:e3:5c:0e:
        94:49:b5:b5:17:35:d2:2e:bf:4e:85:ef:18:e0:85:92:eb:06:
        3b:6c:29:23:09:60:dc:45:02:4c:12:18:3b:e9:fb:0e:de:dc:
        44:f8:58:98:ae:ea:bd:45:45:a1:88:5d:66:ca:fe:10:e9:6f:
        82:c8:11:42:0d:fb:e9:ec:e3:86:00:de:9d:10:e3:38:fa:a4:
        7d:b1:d8:e8:49:82:84:06:9b:2b:e8:6b:4f:01:0c:38:77:2e:
        f9:dd:e7:39
Total found: 3
```

</details>

Note: Windows _CANNOT_ read a concatenated `.pem` file including intermediary certs etc. We would need to convert it to a PKCS#7 message first:

```
$ openssl crl2pkcs7 -nocrl -certfile waifu.pem -out waifu.pem.p7b
```
