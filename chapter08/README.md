
# Creating RSA and Elliptic Curve keys

In [Chapter 00](../chapter00/README.md) there is a brief overview of the commands to create RSA and Elliptic Curve keys. There we will look more closely at how to create these kinds of keys.

## Creating RSA keys

As shown in [Chapter 00](../chapter00/README.md) RSA keys can be created with the `genrsa` command:

`openssl genrsa [<parameters>]`

```shell
Usage: genrsa [options] numbits

General options:
 -help               Display this summary
 -engine val         Use engine, possibly a hardware device

Input options:
 -3                  (deprecated) Use 3 for the E value
 -F4                 Use the Fermat number F4 (0x10001) for the E value
 -f4                 Use the Fermat number F4 (0x10001) for the E value

Output options:
 -out outfile        Output the key to specified file
 -passout val        Output file pass phrase source
 -primes +int        Specify number of primes
 -verbose            Verbose output
 -traditional        Use traditional format for private keys
 -*                  Encrypt the output with any supported cipher

Random state options:
 -rand val           Load the given file(s) into the random number generator
 -writerand outfile  Write random data to the specified file

Provider options:
 -provider-path val  Provider load path (must be before 'provider' argument if required)
 -provider val       Provider to load (can be specified multiple times)
 -propquery val      Property query used when fetching algorithms

Parameters:
 numbits             Size of key in bits
```

For the number of bits it is now recommended to use at least 3072 bits. Severeal organisations will not trust 2k keys in a couple of years.



## Crearing Elliptic Curve keys

For generating an elliptic curve key, we'll need to create elliptic curve parameters. The command for managing elliptic curve parameters is the following command:

`openssl ecparam [<parameters>]`

```shell
Usage: ecparam [options]

General options:
 -help               Display this summary
 -list_curves        Prints a list of all curve 'short names'
 -engine val         Use engine, possibly a hardware device
 -genkey             Generate ec key
 -in infile          Input file  - default stdin
 -inform PEM|DER     Input format - default PEM (DER or PEM)
 -out outfile        Output file - default stdout
 -outform PEM|DER    Output format - default PEM

Output options:
 -text               Print the ec parameters in text form
 -noout              Do not print the ec parameter
 -param_enc val      Specifies the way the ec parameters are encoded

Parameter options:
 -check              Validate the ec parameters
 -check_named        Check that named EC curve parameters have not been modified
 -no_seed            If 'explicit' parameters are chosen do not use the seed
 -name val           Use the ec parameters with specified 'short name'
 -conv_form val      Specifies the point conversion form

Random state options:
 -rand val           Load the given file(s) into the random number generator
 -writerand outfile  Write random data to the specified file

Provider options:
 -provider-path val  Provider load path (must be before 'provider' argument if required)
 -provider val       Provider to load (can be specified multiple times)
 -propquery val      Property query used when fetching algorithms
```

To be able to generate an elliptic curve key, we need to choose a curve. Curves that OpenSSL supports can be listed with the following command

`openssl ecparam -list_curves`

And the output may for instance be:

```shell
  secp112r1 : SECG/WTLS curve over a 112 bit prime field
  secp112r2 : SECG curve over a 112 bit prime field
  secp128r1 : SECG curve over a 128 bit prime field
  secp128r2 : SECG curve over a 128 bit prime field
  secp160k1 : SECG curve over a 160 bit prime field
  secp160r1 : SECG curve over a 160 bit prime field
  secp160r2 : SECG/WTLS curve over a 160 bit prime field
  secp192k1 : SECG curve over a 192 bit prime field
  secp224k1 : SECG curve over a 224 bit prime field
  secp224r1 : NIST/SECG curve over a 224 bit prime field
  secp256k1 : SECG curve over a 256 bit prime field
  secp384r1 : NIST/SECG curve over a 384 bit prime field
  secp521r1 : NIST/SECG curve over a 521 bit prime field
  prime192v1: NIST/X9.62/SECG curve over a 192 bit prime field
  prime192v2: X9.62 curve over a 192 bit prime field
  prime192v3: X9.62 curve over a 192 bit prime field
  prime239v1: X9.62 curve over a 239 bit prime field
  prime239v2: X9.62 curve over a 239 bit prime field
  prime239v3: X9.62 curve over a 239 bit prime field
  prime256v1: X9.62/SECG curve over a 256 bit prime field
  sect113r1 : SECG curve over a 113 bit binary field
  sect113r2 : SECG curve over a 113 bit binary field
  sect131r1 : SECG/WTLS curve over a 131 bit binary field
  sect131r2 : SECG curve over a 131 bit binary field
  sect163k1 : NIST/SECG/WTLS curve over a 163 bit binary field
  sect163r1 : SECG curve over a 163 bit binary field
  sect163r2 : NIST/SECG curve over a 163 bit binary field
  sect193r1 : SECG curve over a 193 bit binary field
  sect193r2 : SECG curve over a 193 bit binary field
  sect233k1 : NIST/SECG/WTLS curve over a 233 bit binary field
  sect233r1 : NIST/SECG/WTLS curve over a 233 bit binary field
  sect239k1 : SECG curve over a 239 bit binary field
  sect283k1 : NIST/SECG curve over a 283 bit binary field
  sect283r1 : NIST/SECG curve over a 283 bit binary field
  sect409k1 : NIST/SECG curve over a 409 bit binary field
  sect409r1 : NIST/SECG curve over a 409 bit binary field
  sect571k1 : NIST/SECG curve over a 571 bit binary field
  sect571r1 : NIST/SECG curve over a 571 bit binary field
  c2pnb163v1: X9.62 curve over a 163 bit binary field
  c2pnb163v2: X9.62 curve over a 163 bit binary field
  c2pnb163v3: X9.62 curve over a 163 bit binary field
  c2pnb176v1: X9.62 curve over a 176 bit binary field
  c2tnb191v1: X9.62 curve over a 191 bit binary field
  c2tnb191v2: X9.62 curve over a 191 bit binary field
  c2tnb191v3: X9.62 curve over a 191 bit binary field
  c2pnb208w1: X9.62 curve over a 208 bit binary field
  c2tnb239v1: X9.62 curve over a 239 bit binary field
  c2tnb239v2: X9.62 curve over a 239 bit binary field
  c2tnb239v3: X9.62 curve over a 239 bit binary field
  c2pnb272w1: X9.62 curve over a 272 bit binary field
  c2pnb304w1: X9.62 curve over a 304 bit binary field
  c2tnb359v1: X9.62 curve over a 359 bit binary field
  c2pnb368w1: X9.62 curve over a 368 bit binary field
  c2tnb431r1: X9.62 curve over a 431 bit binary field
  wap-wsg-idm-ecid-wtls1: WTLS curve over a 113 bit binary field
  wap-wsg-idm-ecid-wtls3: NIST/SECG/WTLS curve over a 163 bit binary field
  wap-wsg-idm-ecid-wtls4: SECG curve over a 113 bit binary field
  wap-wsg-idm-ecid-wtls5: X9.62 curve over a 163 bit binary field
  wap-wsg-idm-ecid-wtls6: SECG/WTLS curve over a 112 bit prime field
  wap-wsg-idm-ecid-wtls7: SECG/WTLS curve over a 160 bit prime field
  wap-wsg-idm-ecid-wtls8: WTLS curve over a 112 bit prime field
  wap-wsg-idm-ecid-wtls9: WTLS curve over a 160 bit prime field
  wap-wsg-idm-ecid-wtls10: NIST/SECG/WTLS curve over a 233 bit binary field
  wap-wsg-idm-ecid-wtls11: NIST/SECG/WTLS curve over a 233 bit binary field
  wap-wsg-idm-ecid-wtls12: WTLS curve over a 224 bit prime field
  Oakley-EC2N-3:
	IPSec/IKE/Oakley curve #3 over a 155 bit binary field.
	Not suitable for ECDSA.
	Questionable extension field!
  Oakley-EC2N-4:
	IPSec/IKE/Oakley curve #4 over a 185 bit binary field.
	Not suitable for ECDSA.
	Questionable extension field!
  brainpoolP160r1: RFC 5639 curve over a 160 bit prime field
  brainpoolP160t1: RFC 5639 curve over a 160 bit prime field
  brainpoolP192r1: RFC 5639 curve over a 192 bit prime field
  brainpoolP192t1: RFC 5639 curve over a 192 bit prime field
  brainpoolP224r1: RFC 5639 curve over a 224 bit prime field
  brainpoolP224t1: RFC 5639 curve over a 224 bit prime field
  brainpoolP256r1: RFC 5639 curve over a 256 bit prime field
  brainpoolP256t1: RFC 5639 curve over a 256 bit prime field
  brainpoolP320r1: RFC 5639 curve over a 320 bit prime field
  brainpoolP320t1: RFC 5639 curve over a 320 bit prime field
  brainpoolP384r1: RFC 5639 curve over a 384 bit prime field
  brainpoolP384t1: RFC 5639 curve over a 384 bit prime field
  brainpoolP512r1: RFC 5639 curve over a 512 bit prime field
  brainpoolP512t1: RFC 5639 curve over a 512 bit prime field
  SM2       : SM2 curve over a 256 bit prime field
```

For creating a parameter file for, e.g. for curve secp256k1, we'll use the following command:

`openssl ecparam -name secp256k1 -out our_curve.pem`

This will store the parameters for curve secp256k1 in a file calles our_curve.pem.

To generate a key for that curve we can use the `genpkey` command:

`openssl genpkey [<paramters>]`

```shell
Usage: genpkey [options]

General options:
 -help               Display this summary
 -engine val         Use engine, possibly a hardware device
 -paramfile infile   Parameters file
 -algorithm val      The public key algorithm
 -quiet              Do not output status while generating keys
 -pkeyopt val        Set the public key algorithm option as opt:value
 -config infile      Load a configuration file (this may load modules)

Output options:
 -out outfile        Output file
 -outform PEM|DER    output format (DER or PEM)
 -pass val           Output file pass phrase source
 -genparam           Generate parameters, not key
 -text               Print the in text
 -*                  Cipher to use to encrypt the key

Provider options:
 -provider-path val  Provider load path (must be before 'provider' argument if required)
 -provider val       Provider to load (can be specified multiple times)
 -propquery val      Property query used when fetching algorithms
Order of options may be important!  See the documentation.
``

For generating an elliptic curve key with are parameters (curve secp256k1), we can use the following command:

`openssl genpkey -paramfile our_curve.pem -out ec_key.pem -aes256`

We can use the `ec` command to manage elliptic curve keys:

`openssl ec [<parameters>]`

```shell
Usage: ec [options]

General options:
 -help               Display this summary
 -engine val         Use engine, possibly a hardware device

Input options:
 -in val             Input file
 -inform format      Input format (DER/PEM/P12/ENGINE)
 -pubin              Expect a public key in input file
 -passin val         Input file pass phrase source
 -check              check key consistency
 -*                  Any supported cipher
 -param_enc val      Specifies the way the ec parameters are encoded
 -conv_form val      Specifies the point conversion form

Output options:
 -out outfile        Output file
 -outform PEM|DER    Output format - DER or PEM
 -noout              Don't print key out
 -text               Print the key
 -param_out          Print the elliptic curve parameters
 -pubout             Output public key, not private
 -no_public          exclude public key from private key
 -passout val        Output file pass phrase source

Provider options:
 -provider-path val  Provider load path (must be before 'provider' argument if required)
 -provider val       Provider to load (can be specified multiple times)
 -propquery val      Property query used when fetching algorithms
```

If we want to store the public key for a private key in a separate file, we can use the following command:

`openssl % openssl ec -in ec_key.pem -pubout -out ec_key.pub.pem`

This file can be used to create a CSR, see [Chapter 03](../chapter03/README.md)

A certificate created with these elliptic curve parameters will have a refernce (OID) to the curve in the certificate, for instance:

```shell
        Subject Public Key Info:
            Public Key Algorithm: id-ecPublicKey
                Public-Key: (256 bit)
                pub:
                    04:8e:ed:0d:1d:cc:1b:4e:30:6c:e0:4e:62:46:b0:
                    5c:19:19:1e:04:e9:d3:a3:cd:1d:0a:10:98:5a:a9:
                    51:80:40:36:be:5d:c5:f7:85:d2:8d:95:89:34:70:
                    13:d5:e4:52:63:40:ec:47:7a:f6:02:27:bc:3b:95:
                    50:a6:77:74:f0
                ASN1 OID: secp256k1
```

We could also use explicit elliptic curve parameters instead of referencing a named curve. We can for instance use the same named curve and create a parameters file with explicit parameters:

`openssl ecparam -name secp256k1 -param_enc explicit -out our_explicit_curve.pem`

If we then genereate an elliptic curve key with the above parameters file and create a certificate for that key, we will instead see the explicit paramters in the public key section of the certificate, for instance:

```shell
        Subject Public Key Info:
            Public Key Algorithm: id-ecPublicKey
                Public-Key: (256 bit)
                pub:
                    04:1e:e7:8a:b3:88:40:2b:f3:b0:4b:2e:3d:7f:24:
                    4e:e5:3c:36:56:b7:44:05:d0:82:cb:40:e2:4b:a6:
                    30:b6:58:99:27:df:34:c6:4e:26:53:db:ca:5c:63:
                    68:89:57:77:5d:7f:17:03:0e:a1:d8:31:e0:06:fa:
                    05:e6:af:37:16
                Field Type: prime-field
                Prime:
                    00:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
                    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:fe:ff:
                    ff:fc:2f
                A:    0
                B:    7 (0x7)
                Generator (uncompressed):
                    04:79:be:66:7e:f9:dc:bb:ac:55:a0:62:95:ce:87:
                    0b:07:02:9b:fc:db:2d:ce:28:d9:59:f2:81:5b:16:
                    f8:17:98:48:3a:da:77:26:a3:c4:65:5d:a4:fb:fc:
                    0e:11:08:a8:fd:17:b4:48:a6:85:54:19:9c:47:d0:
                    8f:fb:10:d4:b8
                Order:
                    00:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
                    ff:fe:ba:ae:dc:e6:af:48:a0:3b:bf:d2:5e:8c:d0:
                    36:41:41
                Cofactor:  1 (0x1)
```
