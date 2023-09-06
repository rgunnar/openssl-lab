# Chapter 01

# Create a CA (Certificate Authority)

The certificate authority is signs certificate requests, creating ceertificates. The CA also manages the certificates, maintaining CRLs (Certificate Revocation Lists) etc.
We will start by creating the root certificate for the CA.

## Generate the key for the root certificate

This is explained in chapter00, so you can revisit that for instructions. We can call it e.g. ca.key for the CA certificate.

## Creating the Root Certificate CSR

There are certain properties the root certificate should have.

The is a x509, version 3, extension, which states that this is a Certificate Authority:
```
X509v3 extensions:
    X509v3 Basic Constraints: 
        CA:TRUE
```
There is also the key usage extension, which specifies what the private key is used for, e.g.:
```
X509v3 Key Usage: critical
    Digital Signature, Certificate Sign, CRL Sign
```
There is also other extensions, which can be nice to include in the certificate, such as:
```
X509v3 Authority Key Identifier: 
```
where you could specify the keyid, the certificate serial, the name of the authority etc.

For setting these values we need an openssl config file. Take a look at the config file in this chapter for setting the extensions mentioned above. We'll use that for createing a CSR for the CA ceetificate. We'll need to create the CSR and sign it in the same step in order to be able to include the authority key identifier in the certificate. For the we'll include the `-x509` paramter to the `openssl req` command, as well as the number of days we want the certificate to be valid. We can choose 10 years for the test CA certificate.

`openssl req -new -x509 -days 3650 -key cakey.pem -sha256 -out ca_certificate.pem -config openssl_ca.conf -extensions v3_req_ca`

The output may look like this:

```shell
Enter pass phrase for cakey.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (Certificate Authority name) []:Test CA
Email Address []:my.name@omegapoint.se
Organization Name (eg, company) []:Omegapoint AB
Organizational Unit Name (eg, section) []:R&D
Locality Name (eg, city) []:Stockholm
State or Province Name (full name) []:Stockholm
Country Name (2 letter code) []:SE
```

We can now look at the certificate using the command (which should be known by now):

`openssl x509 -in ca_certificate.pem -text -noout`

The output may look like:
```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            06:37:22:6c:a7:01:ba:bd:ca:14:9a:c5:27:0f:2b:95:d6:42:37:69
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: CN = Test CA, emailAddress = my.name@omegapoint.se, O = Omegapoint AB, OU = R&D, L = Stockholm, ST = Stockholm, C = SE
        Validity
            Not Before: Sep  6 18:46:41 2023 GMT
            Not After : Sep  3 18:46:41 2033 GMT
        Subject: CN = Test CA, emailAddress = my.name@omegapoint.se, O = Omegapoint AB, OU = R&D, L = Stockholm, ST = Stockholm, C = SE
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (4096 bit)
                Modulus:
                    00:e8:b9:b7:a5:17:3d:cb:ef:3e:47:6f:53:fd:b5:
                    15:47:df:ed:ee:d5:cc:dd:61:0c:eb:6d:9c:7a:b4:
                    5a:76:8f:fb:27:1e:0f:7e:1e:95:44:2b:64:47:9a:
                    32:a3:4e:29:df:f0:cb:85:aa:71:40:f0:de:3d:2a:
                    e9:be:96:35:62:6f:54:4f:ae:8a:1b:e4:e3:82:6e:
                    90:93:99:00:9b:3a:11:05:14:6f:ad:ee:35:3e:31:
                    a5:33:27:35:f7:dd:c2:a1:31:82:36:00:92:9a:17:
                    77:c2:d3:e4:4f:a0:32:af:9a:f3:3e:e6:88:7d:b9:
                    21:71:b1:9a:86:b1:e0:44:ea:f9:fd:7d:a6:2a:15:
                    ea:d5:d8:92:3a:0e:8a:ca:65:ad:4a:d9:5c:51:da:
                    10:d1:8d:69:36:10:ec:10:cd:53:18:96:79:dd:82:
                    69:fd:eb:e7:87:b2:cc:28:f9:28:cc:7e:3a:f3:be:
                    1a:ea:5e:c5:82:3f:d7:e3:26:b1:f6:7a:e1:26:ef:
                    12:d0:70:e2:f4:70:a0:57:01:1f:56:52:f1:ef:a0:
                    05:3f:3a:be:c4:41:27:75:78:ea:2c:83:c7:f5:8b:
                    a1:38:c7:97:bc:c9:96:40:f8:6f:6d:02:7d:f9:a2:
                    7e:56:ca:e6:3e:6e:c7:88:81:d7:d9:d4:84:54:0c:
                    37:07:33:35:a4:44:b6:eb:9d:e2:50:a2:ec:5b:0a:
                    41:14:ce:ad:2f:fe:5a:ad:07:77:bb:bb:dc:73:7b:
                    40:d7:f8:62:b4:b1:07:9a:5a:e1:6c:83:a9:a2:56:
                    a4:88:5c:7d:55:02:cf:27:4e:a0:d6:b9:a6:4f:9c:
                    e5:35:33:86:de:69:11:1c:8a:03:cf:db:76:6e:53:
                    98:94:46:3b:da:aa:c6:13:03:2f:57:65:70:f7:0b:
                    79:76:3f:38:ce:66:a5:9d:1a:6f:de:76:dd:f8:de:
                    5a:2d:67:86:f3:ac:2b:9f:ca:0e:96:5f:b3:72:01:
                    5a:42:e2:51:1f:32:c1:e9:a2:3a:9e:07:d0:f3:44:
                    73:56:88:7d:d0:15:fb:b6:58:fd:fd:2b:65:62:48:
                    de:2f:a6:db:ae:a0:89:e4:6b:2f:67:0e:3a:91:af:
                    38:fc:c6:3d:75:00:63:80:7c:97:a4:d6:0f:09:cf:
                    56:79:4b:37:f4:83:5f:a0:b6:c8:a8:20:bb:a1:0e:
                    eb:a6:17:ff:52:0a:63:f0:6c:44:db:a3:b6:61:b2:
                    69:f1:5b:87:52:9e:02:26:f6:6b:b4:2e:2f:35:1a:
                    a0:4f:70:67:9d:6d:ce:bb:dc:ec:13:ea:d9:e5:e1:
                    67:61:8d:32:49:5d:1f:be:15:ae:37:54:fe:14:05:
                    0c:d0:67
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints: critical
                CA:TRUE
            X509v3 Subject Key Identifier: 
                D5:FD:2C:B4:98:CF:B5:61:7F:58:21:A0:87:57:64:66:A7:C8:23:99
            X509v3 Authority Key Identifier: 
                D5:FD:2C:B4:98:CF:B5:61:7F:58:21:A0:87:57:64:66:A7:C8:23:99
            X509v3 Key Usage: critical
                Digital Signature, Certificate Sign, CRL Sign
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        a7:73:20:7f:41:1e:cb:2c:e9:e3:04:ad:ef:3e:e5:fb:ee:b9:
        96:fa:55:c4:50:32:8f:55:80:b3:f1:19:a2:4e:69:e4:f0:92:
        89:33:1b:a6:6e:29:2c:e4:88:56:64:f0:5c:08:91:dc:53:d2:
        00:f9:73:63:3c:53:87:09:5b:86:3a:d8:2d:ce:a0:80:a2:b0:
        e6:e4:ee:b3:f5:e7:a2:c3:21:55:0b:ca:65:56:5c:67:71:93:
        ae:85:81:ec:e5:e4:40:0d:b4:a9:5f:e9:c2:55:5b:e4:57:59:
        c0:82:f9:5d:15:ca:64:d1:72:c8:99:d0:fd:a0:86:c9:9f:ac:
        13:61:5b:b5:18:01:0d:20:2a:19:c2:12:d4:9d:73:8e:b7:19:
        4a:a9:e2:b5:f6:04:b5:ae:db:24:f7:60:cb:e2:28:c6:92:1a:
        80:48:bc:85:b6:f8:6e:2f:d9:0c:3b:b4:51:93:34:41:0e:07:
        24:b3:bc:19:0e:b1:67:17:9e:40:05:8c:6e:60:be:04:78:f1:
        71:a7:82:60:97:5a:8d:d5:5b:8b:42:e6:34:36:ae:4c:ee:69:
        c2:33:53:eb:18:5f:36:1a:77:65:6d:48:56:ad:61:94:eb:9f:
        9e:b3:ad:39:bf:f7:bc:55:32:4f:93:7a:db:ce:57:58:5e:27:
        b3:24:3f:44:03:ea:b4:df:d1:76:a4:be:db:f6:75:b6:b1:ea:
        a6:3f:61:36:57:39:3e:1c:eb:22:e6:a1:55:ed:52:ef:52:12:
        d1:69:5b:df:ae:2f:93:4f:57:6d:88:d6:5b:62:51:2b:1c:67:
        1e:6e:fc:1a:6a:07:9f:b3:b5:a0:aa:ad:2c:08:1a:4e:f5:a0:
        09:01:71:8b:c2:5a:89:50:45:7c:60:93:c3:f5:d9:8f:2b:16:
        c8:20:fe:75:3c:b7:e4:58:36:5c:80:1a:d8:9d:9d:2d:df:34:
        83:91:64:46:22:25:e2:e4:21:86:98:54:20:02:8d:6e:47:d0:
        96:40:d3:08:50:57:8b:7a:65:5c:68:d8:f9:c8:d6:28:31:10:
        7f:cc:41:9c:2d:bd:52:e6:60:ab:3e:6c:45:94:38:c4:e5:fc:
        a9:43:30:ef:09:fe:46:b2:57:79:c8:2f:f7:5a:05:7e:4c:1e:
        14:1a:00:99:9f:08:3a:3a:82:9b:b4:6c:46:82:7f:1c:fb:6f:
        ec:da:41:61:19:61:84:93:74:e7:42:8b:46:3d:4b:85:2e:fa:
        e5:e8:a5:03:90:3b:23:d3:b9:73:68:df:0b:1f:2e:3a:e4:5d:
        d8:f8:48:5a:c0:61:51:32:36:dd:c5:fc:6d:20:9f:7c:7f:fe:
        11:49:21:e2:8d:f3:fc:2b
```
