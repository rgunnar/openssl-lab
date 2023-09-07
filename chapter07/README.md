# Chapter 07

# Splitting and combining certificates and private key, and other useful stuff

## Certificate formats

There are a couple of different formats. Certificate can for instance be stored in DER format (Distinguished Encoding Rules, a binary format for data structures described by ASN.1) or in PEM format (originally “Privacy Enhanced Mail”). PEM is base64 encoded DER with a header and footer in text form.

PEM certificates usually have the file extension `.pem`, but sometimes `.crt`, `.cer` or other extensions are used as well. PEM files can contain several certificates, or both the PEM encoded certificate and PEM encoded private key.

DER certificates usually have the file extension `.der` or `.crt`, but sometimes `.cer` or other extensions are used as well.

## Converting a certificate from PEM to DER or vice versa

PEM format is default for openssl, so these two commands are eqvivalent:

`openssl x509 -in tls_certificate.pem -text -noout`

and

`openssl x509 -in tls_certificate.pem -inform PEM -text -noout`

### Converting from PEM to DER

`openssl x509 -in tls_certificate.pem -out tls_certificate.der -outform DER`

### Convertinf form DER to PEM

`openssl x509 -in tls_certificate.der -inform DER -out tls_certificate.pem`

## Viewing a certificate in readable form

As we have seen in the previous chapters, the easiest way of vieweing a certificate (in readable form) is:

`openssl x509 -in tls_certificate.pem -text -noout`

or

`openssl x509 -in tls_certificate.der -inform DER -text -noout`

## Checking the consistency of a private key

The consistency of a private key can be checked with:

`openssl rsa -in tls_key.pem -check -noout`

## Checking if a private key belongs to the public key in a certificate

Sometimes it is not known if a file with a private key is the private key corresponding to the public key in a certificate file. This can be checked easily.

Export public key from both certificate and private key (in pem format in this example), and compare them:

```shell
openssl x509 -in tls_certificate.pem -noout -pubkey > pub_key_from_cert.pem
openssl rsa -in tls_key.pem -pubout > pub_key_from_priv_key.pem
diff -y pub_key_from_priv_key.pem pub_key_from_cert.pem
```

## PKCS#12

PKCS#12 is a container which is often used to bundle a certificate with its private key, or bundle a whole certificate chains (or several chains).

The file extension for PKCS#12 files are usually `.p12` or `.pfx`.

### Combining a certificate with its private key

Sometimes it is nice to be able to able to combine the certificate with is private and have the private key pass phrase protected in a single file. This can be done with PKCS#12 format.

`openssl pkcs12 -export -in tls_certificate.pem -inkey tls_key.pem -out tls_certificate.p12`

We can also include the CA certificate to create a full certificate chain in the PKCS#12 file.

`openssl pkcs12 -export -in tls_certificate.pem -inkey tls_key.pem -certfile CA/ca_certificate.pem -out tls_certificate_full.p12`

### Listing the contents from a PKCS#12 file

We can list the contents of the PKCS#12 file. Lisring the CA certificates in the file can be done with:

`openssl pkcs12 -in tls_certificate_full.p12 -cacerts -nokeys`

Listing only the client certificates can be done with:

`openssl pkcs12 -in tls_certificate_full.p12 -clcerts -nokeys`

Listing everything (including keys), including info about the PKCS#12 structure can be done with:

`openssl pkcs12 -in tls_certificate_full.p12 -info`

### Exrtacting the contents of a PKCS#12 file

This is basically the same as for listing. If we want the store the client certificates in a PEM file.

`openssl pkcs12 -in tls_certificate_full.p12 -clcerts -nokeys -out tls_certificate.pem`

If we want the CA certificate:

`openssl pkcs12 -in tls_certificate_full.p12 -cacerts -nokeys -out tls_certificate.pem`

If we want the private key:

`openssl pkcs12 -in tls_certificate_full.p12 -nocerts -out tls_key.pem`

### PKCS#12 and JKS

JKS (Java Keystore Files) can import PKCS#12 files directly, so for java applications using JKS files, one can usually create a PKCS#12 file and import it using keytool (but that's left as an exercise for the reader).
