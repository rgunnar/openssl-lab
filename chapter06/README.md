# openssl-lab chapter 06

# s_client

The command s_client can be used to act like a client trying to connect to a host using SSL/TLS. It can be used as a client when you want to verify that your server works as intended or it can be used if you want to retrieve certificates from the server. It works for any kind of host using SSL/TLS, not just web servers.
It is possible to use client certificates to verify client authentication. 
The command is designed to be interactive, so after connection is done, you can send commands the the server if it accepts that.

## Use s_client to connect to your web server

`echo | openssl s_client -connect localhost:443 -showcerts`

The use of "echo |" is to get the result without having to wait for the server to timeout since the command is interactive.

The output will be something like this:

```shell
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = localhost, O = Omegapoint AB, OU = R&D, emailAddress = my.name@omegapoint.se, L = Stockholm, ST = Stockholm, C = SE
verify error:num=20:unable to get local issuer certificate
verify return:1
depth=0 CN = localhost, O = Omegapoint AB, OU = R&D, emailAddress = my.name@omegapoint.se, L = Stockholm, ST = Stockholm, C = SE
verify error:num=21:unable to verify the first certificate
verify return:1
depth=0 CN = localhost, O = Omegapoint AB, OU = R&D, emailAddress = my.name@omegapoint.se, L = Stockholm, ST = Stockholm, C = SE
verify return:1
---
Certificate chain
 0 s:CN = localhost, O = Omegapoint AB, OU = R&D, emailAddress = my.name@omegapoint.se, L = Stockholm, ST = Stockholm, C = SE
   i:CN = Test CA, emailAddress = my.name@omegapoint.se, O = Omegapoint AB, OU = R&D, L = Stockholm, ST = Stockholm, C = SE
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Sep  7 18:30:33 2023 GMT; NotAfter: Oct  7 18:30:33 2023 GMT
-----BEGIN CERTIFICATE-----
MIIFVzCCAz+gAwIBAgIUTzTK+0mh3bZ+dbdNGJ8H2rkBIUIwDQYJKoZIhvcNAQEL
BQAwgZMxEDAOBgNVBAMMB1Rlc3QgQ0ExJDAiBgkqhkiG9w0BCQEWFW15Lm5hbWVA
b21lZ2Fwb2ludC5zZTEWMBQGA1UECgwNT21lZ2Fwb2ludCBBQjEMMAoGA1UECwwD
UiZEMRIwEAYDVQQHDAlTdG9ja2hvbG0xEjAQBgNVBAgMCVN0b2NraG9sbTELMAkG
A1UEBhMCU0UwHhcNMjMwOTA3MTgzMDMzWhcNMjMxMDA3MTgzMDMzWjCBlTESMBAG
A1UEAwwJbG9jYWxob3N0MRYwFAYDVQQKDA1PbWVnYXBvaW50IEFCMQwwCgYDVQQL
DANSJkQxJDAiBgkqhkiG9w0BCQEWFW15Lm5hbWVAb21lZ2Fwb2ludC5zZTESMBAG
A1UEBwwJU3RvY2tob2xtMRIwEAYDVQQIDAlTdG9ja2hvbG0xCzAJBgNVBAYTAlNF
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAmSuPMDWK9N+uZKvYBBzz
ihTo8hcQ5vyZ6Lt7cStCTJg2G5ssa3Ne8Diu4mfjJgV8pkyTIwSXDYulMLV8jAEh
rDRFZAvYPJrbWjGai+03YeGG4xSZAW5a/CTfm54Yct7QClGIfVUgYYw5mQYQrDwm
Yhha0alSTogC9iBy1Bxr/Rvd4L8Si9It708kkqAE3dYGBz3/m3xBfoDfukoCcdqX
BS2ZUKiWI2/WsIZEg8zAYLiyoHVKMvJGTpfSOv2oIQ/fV6DLRaPRFAjR7fJx75S7
40G+gE7omCmIW6SSnO1eXACcT9GhO1ocPQeqWmkUW1XETI+o+J9uA4Rhw+K0Scw3
CwIDAQABo4GeMIGbMAkGA1UdEwQCMAAwHQYDVR0OBBYEFBiAY5UFlqQqljmXmzKt
X9g+4U0HMA4GA1UdDwEB/wQEAwIFoDAdBgNVHSUEFjAUBggrBgEFBQcDAgYIKwYB
BQUHAwEwHwYDVR0RBBgwFoIJbG9jYWxob3N0ggkxMjcuMC4wLjEwHwYDVR0jBBgw
FoAU1wryM2791LJqAZdx8CHhDfU2s54wDQYJKoZIhvcNAQELBQADggIBAFB4dKkQ
DT8PgJi6T4aIOfl2BPX6b/n2uTgI+4lV+kAciFTM7Au0nk7Z8mH/0irzZpPPZwDY
OV1acfjEQ0hD9Nhsg79Zll/WF5Wl8UbZ0uaxk3DfR49RcVi9vVsQqhqqa1CGFheF
0iA4DmmCKlsspGGRHzizHQydXDxSj7t/0x9QstO7HVTOl+CadN8hilQNAq6BYGtK
J05MBOKRYw6fG0DlqKlo4qlmR1qKSFgkSOSu+jOZAg8XHarycvxgieR9uFN9Iylp
+WSOb04NZ6l2hx73tPncgMW3PhXwW3ZWS1UK63DR9AVshLqh0VXAYRmuaSqalGXr
BinRHUj+fLRjslQ832Uq/ZYxY0KkEQVTPy4m/AAytZUNubHIJfuXWCWNIqjZcbhA
7asvB7rjvfzqleO93aeXRRSPMPfCJvT7UWMyrBJbtl4v58hL1DvAhAhCePKtf704
shuHzMnUzPHONKXyrMkfouWnFAqQctJOoB2tkbd/R2EInrH6jChVQhJoQCbDIYDc
Pa0yX1H6j4ksoqIFY0ihk1J5f5Qd7rT9qdSn7cCvReteq1ay2ERRPJPjxfuZ1MZ1
/fwu+jYSeMk1HHTXIMlX7OWVUwMS8onGdconiXm5f5jat7NJeOY453+LVJslQexg
R/tqu/X7lo2oOyIaY55UKtRIxlYc+PtK6G7g
-----END CERTIFICATE-----
---
Server certificate
subject=CN = localhost, O = Omegapoint AB, OU = R&D, emailAddress = my.name@omegapoint.se, L = Stockholm, ST = Stockholm, C = SE
issuer=CN = Test CA, emailAddress = my.name@omegapoint.se, O = Omegapoint AB, OU = R&D, L = Stockholm, ST = Stockholm, C = SE
---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 1927 bytes and written 377 bytes
Verification error: unable to verify the first certificate
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 2048 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 21 (unable to verify the first certificate)
---
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: F6A6D923812C01C01185D042CAB2C4323C0CBCC8B5FCAD960F3DD46EAB7B751C
    Session-ID-ctx: 
    Resumption PSK: 1F787FB2126DD9D39289C641B83B0E618D8505228B01F565D1AE5C437A721091AFC7E66BD5D1A936091845C7D5FA6EE2
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - d3 31 3b 9c 3f 15 ce e8-f4 32 6c 2b 69 5c 4f b1   .1;.?....2l+i\O.
    0010 - dd 28 2b 5f 98 a0 d9 73-56 2b 00 6a cc 13 3b 95   .(+_...sV+.j..;.

    Start Time: 1694160918
    Timeout   : 7200 (sec)
    Verify return code: 21 (unable to verify the first certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 9D71386E47DED1AA1F7108E03134D4AE7FC5B101DBAEF2E9681E70A0B63159CC
    Session-ID-ctx: 
    Resumption PSK: 8C4709DA97A291BD071104C5BE4DEB3FD51C7D325F702FB64427843C551A26DE2AD114796E1B8C3B8EE1EA119564A4D1
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - 49 c3 98 be 36 c8 7e ac-1b 91 53 56 5a 88 9a 5d   I...6.~...SVZ..]
    0010 - 9c 94 b4 9c 03 b3 af b7-50 94 b6 69 10 bd 7d c1   ........P..i..}.

    Start Time: 1694160918
    Timeout   : 7200 (sec)
    Verify return code: 21 (unable to verify the first certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
DONE
```

## Compare with a host using a trusted root certificate

If you look at the beginning of the result from the run above you will see "unable to get local issuer certificate" etc

```shell
Can't use SSL_get_servername
depth=0 CN = localhost, O = Omegapoint AB, OU = R&D, emailAddress = my.name@omegapoint.se, L = Stockholm, ST = Stockholm, C = SE
verify error:num=20:unable to get local issuer certificate
verify return:1
depth=0 CN = localhost, O = Omegapoint AB, OU = R&D, emailAddress = my.name@omegapoint.se, L = Stockholm, ST = Stockholm, C = SE
verify error:num=21:unable to verify the first certificate
verify return:1
depth=0 CN = localhost, O = Omegapoint AB, OU = R&D, emailAddress = my.name@omegapoint.se, L = Stockholm, ST = Stockholm, C = SE
verify return:1
```

Try the same command against a web server with official certificates, like for instance omegapoint.se
