# Chapter 03

# Issue a web server certificate

## Geneting a key for the web server certificate

This is explained in chapter00, so you can revisit that for instructions. We can call it e.g. ca.key for the CA certificate.

## Creating the web server certificate CSR

When creating a CSR for a web certificate, we need to take into account some properties that need to be fulfilled in order for web browsers to accept the certificate as valid.

These are basically (as of August 21, 2023):

* TLS server certificates and issuing CAs using RSA keys must use key sizes greater than or equal to 2048 bits.
* TLS server certificates and issuing CAs must use a hash algorithm from the SHA-2 family in the signature algorithm.
* TLS server certificates must present the DNS name of the server in the Subject Alternative Name extension of the certificate.
* TLS server certificates must contain an ExtendedKeyUsage (EKU) extension containing the id-kp-serverAuth OID.
* TLS server certificates issued on or after September 1, 2020 00:00 GMT/UTC must not have a validity period greater than 398 days.

So we need to create a private key corresponding to the requirements, we need to use a hash function corresponding to the requierments and we specify SAN (Subject Alternate Name) in the CSR.

For these things we need to use a config file. Take a look at the config file here in chapter 03.

For creating the CSR, we'll use the following command:

`openssl req -new -key tls_key.pem -sha256 -out tls_certificate.csr -config openssl_tls.conf -extensions v3_req_server`

And the output will look like:

```
Enter pass phrase for tls_key.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (e.g. server FQDN or YOUR name) [localhost]:localhost
Organization Name (eg, company) [Omegapoint AB]:Omegapoint AB
Organizational Unit Name (eg, section) [R&D]:R&D
Email Address [my.name@omegapoint.se]:my.name@omegapoint.se
Locality Name (eg, city) [Stockholm]:Stockholm
State or Province Name (full name) [Stockholm]:Stockholm
Country Name (2 letter code) [SE]:SE
```

### And if we want t use batch mode (mode advanced)

If we want to use the default values specified in the config file, we can use batch mode instead (same command with added `-batch`parameter:

`openssl req -new -batch -key tls_key.pem -sha256 -out tls_certificate.csr -config openssl_tls.conf -extensions v3_req_server`

## The CSR

We can inspect the CSR using the following command:

`openssl req -in tls_certificate.csr -text -noout`

Note that we in our CSR now have the following x509 version 3 extensions:

```
X509v3 Key Usage: critical
    Digital Signature, Key Encipherment
X509v3 Extended Key Usage:
    TLS Web Client Authentication, TLS Web Server Authentication
X509v3 Subject Alternative Name:
    DNS:localhost, DNS:127.0.0.1
```

And we have our two SAN names (we can provide more SAN's if we like).

## Signing the certificate using the CA

We can now sign the TLS certificate using our CA certificate and key. We will provide the CSR and specifi the name of the output file, along with the CA certificate and private key. We'll also create a serial number.

`openssl x509 -req -in tls_certificate.csr -CA CA/ca_certificate.pem -CAkey CA/cakey.pem -CAcreateserial -sha256 -out tls_certificate.pem -extfile openssl_tls.conf -extensions v3_req_server`


### Batch signing (more advanced)

We can use batch more here as well with the config file specifying behaviour and data.

First we need to create the "database" for storing info about the issued certificates. This can be done with:

`touch CA/certindex.txt`

The previous command created a serial file, so that is already done, otherwise we would need to create the serial file as well.

The command for signing the certificate in batch mode is (check openssl_tls.conf for directory structure for CA certificate and key etc):

`openssl ca -batch -config openssl_tls.conf -in tls_certificate.csr -out tls_certificate.pem -extfile openssl_tls.conf -extensions v3_req_server`

## The TLS certicate

We now have a TLS certificate, which can look like:

```shell
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            5c:cb:ba:64:f9:e1:a8:15:09:cd:aa:b5:8d:fd:38:f5:90:cd:dd:5f
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: CN = Test CA, emailAddress = my.name@omegapoint.se, O = Omegapoint AB, OU = R&D, L = Stockholm, ST = Stockholm, C = SE
        Validity
            Not Before: Sep  6 20:48:27 2023 GMT
            Not After : Oct  8 20:48:27 2024 GMT
        Subject: CN = localhost, emailAddress = my.name@omegapoint.se, O = Omegapoint AB, OU = R&D, L = Stockholm, ST = Stockholm, C = SE
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (4096 bit)
                Modulus:
                    00:91:76:64:94:d4:c8:a1:e1:74:80:23:4b:96:18:
                    34:19:57:35:4c:db:1e:39:33:8b:f0:77:ce:39:2c:
                    bd:3b:a6:e6:bf:fa:ef:fe:9e:e5:ad:77:ab:ba:6d:
                    49:47:39:b5:fa:c8:29:7b:e8:15:db:99:3e:cc:e3:
                    89:8b:e0:19:89:fb:6a:46:02:23:92:7a:dc:18:8a:
                    68:e0:ca:00:b4:b7:3b:b4:f9:40:bf:63:6d:ee:53:
                    78:ba:15:ea:30:6d:7b:fa:bf:d5:a3:ea:60:df:8e:
                    78:48:c1:7c:34:a5:95:8f:09:aa:e8:1d:0d:31:6e:
                    cc:7c:f2:fd:ed:3b:81:79:16:6a:7d:1d:3b:a4:52:
                    fb:9a:0f:90:05:85:66:97:ff:0e:51:5b:1a:cd:81:
                    de:41:55:50:9a:26:ba:ca:18:ec:10:da:b4:ec:7a:
                    d8:32:07:28:07:77:4d:47:cc:19:24:4f:6e:f9:c2:
                    27:24:26:5c:3f:c0:1f:ad:63:74:81:5e:81:6f:e1:
                    a7:be:d1:7f:31:f2:d5:1e:2b:62:39:5d:52:6c:d8:
                    2e:34:15:b7:6f:b7:dd:1f:34:c2:9e:d2:7f:9e:47:
                    38:bd:05:22:22:2e:10:ae:11:5f:9c:74:0f:b5:86:
                    43:80:f6:fa:17:87:85:3d:f5:56:96:dc:89:78:2a:
                    2e:b8:91:70:d2:d7:38:3f:31:a3:cd:f9:40:41:bb:
                    3b:f8:48:9e:53:20:af:3d:4b:c1:28:b4:a0:f1:04:
                    06:18:b3:82:7a:b6:4b:d5:3e:f3:e5:11:18:ca:0f:
                    9f:1e:80:be:92:be:37:0d:2e:e7:a1:40:3b:0f:3a:
                    2f:2f:1a:d9:d5:fd:39:af:1d:f6:f6:89:09:f0:8c:
                    0f:65:c3:f4:38:a1:aa:6e:d9:5b:ee:a7:88:ed:59:
                    39:66:7a:0a:d2:75:2a:f3:c4:e0:91:0a:f2:18:35:
                    ad:73:d3:d7:a1:de:ff:8b:c3:ac:9e:a3:2c:77:51:
                    88:b0:24:23:80:9c:ec:a1:76:86:aa:22:be:63:04:
                    4a:fe:2d:e1:7a:1b:f0:15:81:f7:c2:5d:a2:00:e7:
                    bd:3a:4b:a7:a1:ab:b1:fc:1c:d3:cd:2e:cf:b4:52:
                    32:aa:51:cc:52:30:21:3c:e4:e2:28:39:3b:ae:f7:
                    ed:39:5d:e1:31:f0:c5:9e:cd:16:8e:4b:0d:09:79:
                    75:7e:e7:ed:01:1a:e0:00:4d:6b:ec:42:20:f5:22:
                    30:87:5f:72:c7:3c:f6:72:81:87:a6:cb:15:e4:b2:
                    1e:ed:c2:21:69:d6:e5:46:5a:9a:f4:73:70:68:70:
                    95:95:4b:0a:e5:88:fc:58:3f:75:4c:f0:3f:45:72:
                    60:49:9f
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints:
                CA:FALSE
            X509v3 Subject Key Identifier:
                C1:15:C1:37:3D:1E:1A:F0:FA:A0:C0:24:66:4F:91:CA:F3:BB:7E:46
            X509v3 Key Usage: critical
                Digital Signature, Key Encipherment
            X509v3 Extended Key Usage:
                TLS Web Client Authentication, TLS Web Server Authentication
            X509v3 Subject Alternative Name:
                DNS:localhost, DNS:127.0.0.1
            X509v3 Authority Key Identifier:
                D5:FD:2C:B4:98:CF:B5:61:7F:58:21:A0:87:57:64:66:A7:C8:23:99
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        22:84:db:4a:ed:a2:19:d6:c4:5c:c1:e0:4b:cd:62:13:c1:fb:
        f4:d8:87:64:e1:95:06:32:1b:90:e2:7b:d8:a2:d6:2c:89:c7:
        7f:6c:d5:da:f8:26:21:d0:8c:04:84:82:66:f7:c1:9d:73:7d:
        ed:e7:a7:83:d2:d7:e8:c4:a0:4a:60:78:d0:cb:dd:a8:79:fd:
        f5:34:55:73:99:b9:e3:90:1b:12:fe:1f:c8:1b:9e:d3:69:12:
        30:3a:bb:84:76:d9:37:67:fe:0a:25:b8:57:75:61:f4:74:96:
        2e:11:3a:29:51:a7:21:fc:a4:0d:5e:41:ae:ea:17:8e:93:58:
        b0:c2:36:2e:11:79:2a:c4:21:df:58:9b:83:03:bc:20:fd:68:
        30:df:5a:01:09:ac:9f:46:96:ef:1b:98:5d:54:64:68:ed:07:
        3a:39:49:b0:4b:74:7a:0e:c4:4f:c3:e0:6e:03:22:6a:6f:60:
        43:e1:c3:f8:53:9d:83:e8:e2:83:1a:92:0b:25:b7:02:b5:48:
        31:16:14:29:3a:bf:70:c5:1e:c8:d6:c0:ad:d6:33:1f:47:ff:
        9b:ef:46:3c:f2:ca:3b:cb:d4:d9:08:1f:07:c4:79:c2:35:72:
        17:f4:e4:e6:dd:8f:28:77:f8:98:34:c5:c8:3a:ad:10:4a:33:
        e4:a0:02:9d:5f:1f:0f:60:71:20:f9:0f:fa:ea:32:cf:b4:16:
        05:64:93:3e:d7:59:21:76:22:d2:a8:ae:5d:3e:1d:cd:f2:5e:
        ad:99:f9:79:c5:6d:f8:71:22:4f:e9:fc:93:35:f4:9b:70:d0:
        6d:87:e0:e5:82:d0:50:20:dc:aa:5f:70:06:ec:26:47:2f:12:
        6c:03:36:a8:90:89:c4:bf:48:75:4d:77:23:5a:f7:8b:01:f7:
        09:75:48:8f:65:85:6a:af:c3:10:2e:4d:0c:8f:10:70:d4:3f:
        e0:29:76:c1:17:ce:1e:9a:47:79:6b:21:b8:de:a0:fe:02:b4:
        f2:68:1a:6e:76:3b:3f:92:40:ad:57:2c:e9:23:ec:fc:7c:6c:
        c1:48:3b:83:fa:75:7e:52:fa:a6:72:45:cf:32:0f:50:ee:8a:
        ee:4f:94:5b:ed:e6:e1:ff:83:3e:48:80:f7:cb:42:84:86:8d:
        e1:3b:18:52:88:29:82:e9:dd:91:98:95:fe:4e:2b:d5:77:19:
        e3:4d:ab:c9:86:aa:25:03:46:9b:1b:63:00:bd:95:e1:c3:ae:
        2c:2e:8d:91:37:de:6c:da:1f:9e:03:31:97:7f:ff:46:66:bc:
        0e:a9:11:1a:95:b7:cf:5e:a1:9c:78:77:05:a3:1d:e3:85:5b:
        01:d8:3e:93:e0:0e:a2:ca
```

### And if batch mode was used, we get the certificate database

And we have a certificate database (certindex.txt) which mey look like:

```shell
V	241008204827Z		5CCBBA64F9E1A81509CDAAB58DFD38F590CDDD5F	unknown	/CN=localhost/emailAddress=my.name@omegapoint.se/O=Omegapoint AB/OU=R&D/L=Stockholm/ST=Stockholm/C=SE
```

Where `V` stands for Valid. We also have a copy of the certificate in the server_certificates directory, with the filename being the serialnumber.
