# Chapter 01

Create a CA (Certificate Authority)

The certificate authority is signs certificate requests. We will start by creating the root certificate for the CA.

## Generate the key for the root certificate

This is explained in chapter00, so you can revisit that for instructions.

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
