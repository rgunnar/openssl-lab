# Chapter 00

# Create a self-signed certificate

## Generating a key

In order to have a useful meaning of the certificate, an assymentric enryption key pair is needed. This can be e.g. an RSA key pair, an Elliptic Curve key pair.

Creating a key pair is quite straight forward using openssl with the following command:

`openssl genpkey [<parameters>]`

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
```
or

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

The `genpkey` can be used to create all kinds of key pair, e.g. elliptic curve, rsa, dsa, and the genrsa and gendsa command are specific to drearing a DSA or RSA key pair. There are also commands for converting, checking, etc. for the specific key.

`openssl rsa [<parameters>]` handles RSA keys

`openssl dsa [<parameters>]` handles DSA keys

`openssl ec [<parameters>]` handles elliptic curve keys

### Generating a key

Let's create a 4096 bit RSA key and store it in a file, encrypted with a pass phrase.

`openssl genrsa -out rsa_key.pem -aes256 4096`

The `-aes256` encrypts the private key using the provided pass phrase.

If we want to have the public key in a file as well, we can store it using openssl:

`openssl rsa -in rsa_key.pem -pubout -out rsa_key.pub.pem`

This stores the public key in file `rsa_key.pub.pem`.

## Creating a self-signed certificate

We will not create a self-signed certificate using the key pair we created.

We will do this the "normal" way, in starting with creating a CSR (Certificate Signing Reqeust), and then signing the certificate with its own private key. This special case can be done with a single command in openssl, but we will start with creating a CSR, and then sign it.

### Creating a CSR

This is done with the `openssl req` command:
```shell
Usage: req [options]

General options:
 -help                 Display this summary
 -engine val           Use engine, possibly a hardware device
 -keygen_engine val    Specify engine to be used for key generation operations
 -in infile            X.509 request input file (default stdin)
 -inform PEM|DER       Input format - DER or PEM
 -verify               Verify self-signature on the request

Certificate options:
 -new                  New request
 -config infile        Request template file
 -section val          Config section to use (default "req")
 -utf8                 Input characters are UTF8 (default ASCII)
 -nameopt val          Certificate subject/issuer name printing options
 -reqopt val           Various request text options
 -text                 Text form of request
 -x509                 Output an X.509 certificate structure instead of a cert request
 -CA infile            Issuer cert to use for signing a cert, implies -x509
 -CAkey val            Issuer private key to use with -CA; default is -CA arg
                       (Required by some CA's)
 -subj val             Set or modify subject of request or cert
 -subject              Print the subject of the output request or cert
 -multivalue-rdn       Deprecated; multi-valued RDNs support is always on.
 -days +int            Number of days cert is valid for
 -set_serial val       Serial number to use
 -copy_extensions val  copy extensions from request when using -x509
 -addext val           Additional cert extension key=value pair (may be given more than once)
 -extensions val       Cert extension section (override value in config file)
 -reqexts val          Request extension section (override value in config file)
 -precert              Add a poison extension to the generated cert (implies -new)

Keys and Signing options:
 -key val              Key for signing, and to include unless -in given
 -keyform format       Key file format (ENGINE, other values ignored)
 -pubkey               Output public key
 -keyout outfile       File to write private key to
 -passin val           Private key and certificate password source
 -passout val          Output file pass phrase source
 -newkey val           Generate new key with [<alg>:]<nbits> or <alg>[:<file>] or param:<file>
 -pkeyopt val          Public key options as opt:value
 -sigopt val           Signature parameter in n:v form
 -vfyopt val           Verification parameter in n:v form
 -*                    Any supported digest

Output options:
 -out outfile          Output file
 -outform PEM|DER      Output format - DER or PEM
 -batch                Do not ask anything during request generation
 -verbose              Verbose output
 -noenc                Don't encrypt private keys
 -nodes                Don't encrypt private keys; deprecated
 -noout                Do not output REQ
 -newhdr               Output "NEW" in the header lines
 -modulus              RSA modulus

Random state options:
 -rand val             Load the given file(s) into the random number generator
 -writerand outfile    Write random data to the specified file

Provider options:
 -provider-path val    Provider load path (must be before 'provider' argument if required)
 -provider val         Provider to load (can be specified multiple times)
 -propquery val        Property query used when fetching algorithms
```

The command for creating a CSR can also generate a new key, and the use the newly generated key for the CSR, but we will use the key we already created.

`openssl req -new -key rsa_key.pub.pem -out certificate.csr`

The command will prompt for information to use in the certificate, which may look like this:
```
Enter pass phrase for rsa_key.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:SE
State or Province Name (full name) [Some-State]:Stockholm
Locality Name (eg, city) []:Stockholm
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Omegapoint AB
Organizational Unit Name (eg, section) []:R&D
Common Name (e.g. server FQDN or YOUR name) []:Test certificate
Email Address []:my.name@omegapoint.se

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

If we want to check the content of the CSR, we can do that with the same command `openssl req`:

`openssl req -in certificate.csr -text -noout`

And the output may look like:

```
Certificate Request:
    Data:
        Version: 1 (0x0)
        Subject: C = SE, ST = Stockholm, L = Stockholm, O = Omegapoint AB, OU = R&D, CN = Test certificate, emailAddress = my.name@omegapoint.se
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (4096 bit)
                Modulus:
                    00:c4:0d:76:da:42:f3:ca:d6:c3:34:b9:69:45:8f:
                    d2:22:d0:ed:b7:71:8c:d5:07:54:24:7f:2f:2e:ae:
                    97:cd:1f:cb:45:2c:0a:af:cf:2e:66:f4:f5:95:14:
                    4c:4d:9b:72:1d:a7:55:a8:10:10:2c:2c:52:8f:8d:
                    9e:b5:cc:9b:48:b8:ef:14:0b:8b:f7:af:72:16:da:
                    f9:c9:0e:4a:92:1f:a8:93:4e:47:cd:f9:0e:45:d9:
                    da:3b:a4:9d:8e:47:31:ef:cb:e6:d2:c5:75:1c:47:
                    5e:a8:d6:fb:a8:f5:f3:b9:23:d2:7e:f3:ac:04:b6:
                    0b:2a:ee:fe:99:9e:9a:14:68:81:f5:e3:1f:e2:53:
                    28:91:b0:9c:25:16:50:7d:2b:c2:14:1c:f6:71:62:
                    fd:74:d6:da:f3:cf:0c:5b:13:f0:e5:71:9f:6a:4d:
                    bb:10:23:a9:54:34:9c:36:aa:37:a2:c0:fa:cc:27:
                    0b:ef:52:94:d5:38:cc:ca:5f:06:f5:ca:aa:b4:3c:
                    d0:f4:d9:95:d1:6f:85:84:bc:1b:02:5d:c9:24:0a:
                    c3:02:59:6a:36:9a:29:0a:ea:60:d8:e2:be:d4:9a:
                    0c:df:1a:69:c0:3c:f4:f0:6c:77:19:0e:2d:bd:4b:
                    cc:22:da:78:f4:84:74:31:31:08:2c:71:2c:0c:80:
                    c3:77:45:6c:b6:f7:96:38:8a:e6:e0:b4:68:86:58:
                    14:9c:18:34:67:3c:97:c6:c9:96:a4:c7:c3:41:d9:
                    e1:8f:9b:1d:7d:b8:93:42:b4:69:9b:23:0d:68:6e:
                    e9:23:2d:7e:c1:4e:cb:6d:33:d0:b9:49:3c:ee:74:
                    16:8b:54:d4:87:5f:59:a3:08:80:a7:e7:d5:d0:df:
                    3f:c3:f2:c5:d7:7b:90:5a:f0:65:4e:1c:ba:f6:f5:
                    13:15:33:e7:2f:89:9a:e3:e9:34:57:56:d5:99:b7:
                    d6:10:56:47:45:bf:be:b4:5c:08:e0:60:35:e6:9a:
                    69:79:e6:22:f9:ff:0b:0a:16:b5:ae:d7:71:51:d8:
                    46:f2:06:ed:ef:a2:84:64:57:59:76:d7:bd:a9:e4:
                    23:0d:14:74:65:59:e7:61:45:cb:80:6e:92:a6:90:
                    09:e2:ba:76:a1:c1:92:cb:20:1d:c4:12:f6:ab:43:
                    22:44:2c:95:89:a5:13:45:70:e6:67:fc:32:2e:39:
                    18:8b:c3:6f:d0:13:37:d6:d8:95:71:fb:27:94:45:
                    9c:d1:e5:3b:c2:ce:94:00:85:13:cd:3b:3c:58:45:
                    05:fd:47:b2:cf:3d:6a:fe:60:8e:89:16:30:fb:4a:
                    40:08:c3:66:76:d8:58:08:22:38:eb:fe:7e:f8:42:
                    ac:c3:89
                Exponent: 65537 (0x10001)
        Attributes:
            (none)
            Requested Extensions:
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        ba:93:17:dd:2e:ce:9a:82:8c:41:22:67:03:00:b8:a3:ab:18:
        cc:0a:41:ae:55:07:31:4f:da:6a:11:e1:45:5d:58:f9:c2:fc:
        f1:47:d2:ed:cb:5b:1d:96:c3:bf:0f:d6:bc:72:35:3e:a5:cb:
        1e:47:b4:03:cd:d4:c2:03:32:5b:21:59:39:af:fc:e8:a4:ad:
        f6:13:8a:96:b6:ac:62:46:41:b4:7d:c3:14:bf:fe:2a:67:27:
        f6:04:73:13:0c:8b:c4:68:4e:d6:ca:0a:4a:3b:df:5c:6f:b5:
        32:89:a2:55:d9:ed:3a:51:7b:73:27:a8:69:b3:b0:45:49:97:
        9a:de:9d:42:b3:5c:a3:fc:c7:2a:36:f5:98:9c:bf:af:2d:33:
        12:49:26:09:8d:34:c2:47:08:c5:5b:10:4a:09:ec:59:10:b3:
        79:71:91:62:f7:f9:4b:e0:c8:93:f9:f8:a4:9f:9b:9d:b3:c1:
        f7:c2:76:f2:db:a7:05:83:9c:cd:1a:d5:bc:c9:d5:02:83:af:
        ce:4a:c9:e2:2f:82:84:96:8d:22:16:f3:4d:2d:3a:37:b1:9d:
        77:88:42:01:0b:c0:79:52:5c:df:6b:26:c1:54:44:ca:19:fe:
        92:8c:9b:14:17:50:8d:ba:60:c5:3c:a6:10:9d:af:ff:7e:c8:
        14:dc:56:ff:76:52:98:53:d0:35:f1:9d:7c:e9:06:5c:aa:1f:
        9b:9a:85:fd:4d:44:9a:44:ea:cb:3b:68:85:84:7b:2a:ca:29:
        41:f4:8b:01:b1:65:c5:ea:11:3e:f0:d1:dd:70:0b:47:c9:34:
        29:8a:8f:fe:7c:25:ef:ea:a1:46:8e:11:4f:78:19:dd:32:b4:
        da:5f:01:e0:80:7e:73:05:bf:56:74:64:52:45:01:a7:37:18:
        2a:68:53:e2:42:b1:68:61:6b:2c:ab:90:97:24:4b:50:39:a2:
        0f:ee:49:61:cd:2b:e2:e9:79:4f:04:e9:f0:b6:68:4f:b8:2d:
        06:ee:78:c0:ac:59:92:c8:6c:1c:1e:10:58:22:4b:a4:fd:f2:
        e1:3b:ee:52:bc:4e:f1:49:15:15:88:f3:a0:ab:74:41:fa:72:
        b2:99:d8:99:0a:83:7b:80:0b:0a:78:55:73:a2:e5:a3:6c:cc:
        51:9f:99:09:c4:53:8e:04:0c:2d:3e:e6:d0:65:5c:11:6e:13:
        71:72:f5:d2:ba:be:ce:e4:97:99:db:6a:d6:48:43:bc:dd:10:
        88:d6:e3:52:8f:6b:df:8a:06:19:46:0d:4c:b5:82:bc:1d:07:
        bb:1b:a3:70:da:39:0a:b1:ff:21:db:32:c5:78:7c:93:af:bb:
        27:b1:a2:c7:5a:3e:54:c9
```

### Signing the certificate request with our private key

Lets sign the certificate request with out private key and create the certificate
This can be done with the following command:

`openssl x509 [<parameters>]`

The command for signing the certificate request is:

`openssl x509 -signkey rsa_key.pem -in certificate.csr -req -days 365 -out certificate.pem`

This will sign the certificate request and create a signed certificate, with a validity of one year, and the output may look like:
```
Enter pass phrase for rsa_key.pem:
Certificate request self-signature ok
subject=C = SE, ST = Stockholm, L = Stockholm, O = Omegapoint AB, OU = R&D, CN = Test certificate, emailAddress = my.name@omegapoint.se
```

We can now check the content of the certificate using the `openssl x509´ command:

`openssl x509 -in certificate.pem -text -noout`

And the output may look like:
```
Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number:
            22:4a:1d:f9:21:05:16:30:78:85:8d:b7:51:41:fa:50:ec:3b:5a:ee
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = SE, ST = Stockholm, L = Stockholm, O = Omegapoint AB, OU = R&D, CN = Test certificate, emailAddress = my.name@omegapoint.se
        Validity
            Not Before: Sep  5 20:08:07 2023 GMT
            Not After : Sep  4 20:08:07 2024 GMT
        Subject: C = SE, ST = Stockholm, L = Stockholm, O = Omegapoint AB, OU = R&D, CN = Test certificate, emailAddress = my.name@omegapoint.se
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (4096 bit)
                Modulus:
                    00:c4:0d:76:da:42:f3:ca:d6:c3:34:b9:69:45:8f:
                    d2:22:d0:ed:b7:71:8c:d5:07:54:24:7f:2f:2e:ae:
                    97:cd:1f:cb:45:2c:0a:af:cf:2e:66:f4:f5:95:14:
                    4c:4d:9b:72:1d:a7:55:a8:10:10:2c:2c:52:8f:8d:
                    9e:b5:cc:9b:48:b8:ef:14:0b:8b:f7:af:72:16:da:
                    f9:c9:0e:4a:92:1f:a8:93:4e:47:cd:f9:0e:45:d9:
                    da:3b:a4:9d:8e:47:31:ef:cb:e6:d2:c5:75:1c:47:
                    5e:a8:d6:fb:a8:f5:f3:b9:23:d2:7e:f3:ac:04:b6:
                    0b:2a:ee:fe:99:9e:9a:14:68:81:f5:e3:1f:e2:53:
                    28:91:b0:9c:25:16:50:7d:2b:c2:14:1c:f6:71:62:
                    fd:74:d6:da:f3:cf:0c:5b:13:f0:e5:71:9f:6a:4d:
                    bb:10:23:a9:54:34:9c:36:aa:37:a2:c0:fa:cc:27:
                    0b:ef:52:94:d5:38:cc:ca:5f:06:f5:ca:aa:b4:3c:
                    d0:f4:d9:95:d1:6f:85:84:bc:1b:02:5d:c9:24:0a:
                    c3:02:59:6a:36:9a:29:0a:ea:60:d8:e2:be:d4:9a:
                    0c:df:1a:69:c0:3c:f4:f0:6c:77:19:0e:2d:bd:4b:
                    cc:22:da:78:f4:84:74:31:31:08:2c:71:2c:0c:80:
                    c3:77:45:6c:b6:f7:96:38:8a:e6:e0:b4:68:86:58:
                    14:9c:18:34:67:3c:97:c6:c9:96:a4:c7:c3:41:d9:
                    e1:8f:9b:1d:7d:b8:93:42:b4:69:9b:23:0d:68:6e:
                    e9:23:2d:7e:c1:4e:cb:6d:33:d0:b9:49:3c:ee:74:
                    16:8b:54:d4:87:5f:59:a3:08:80:a7:e7:d5:d0:df:
                    3f:c3:f2:c5:d7:7b:90:5a:f0:65:4e:1c:ba:f6:f5:
                    13:15:33:e7:2f:89:9a:e3:e9:34:57:56:d5:99:b7:
                    d6:10:56:47:45:bf:be:b4:5c:08:e0:60:35:e6:9a:
                    69:79:e6:22:f9:ff:0b:0a:16:b5:ae:d7:71:51:d8:
                    46:f2:06:ed:ef:a2:84:64:57:59:76:d7:bd:a9:e4:
                    23:0d:14:74:65:59:e7:61:45:cb:80:6e:92:a6:90:
                    09:e2:ba:76:a1:c1:92:cb:20:1d:c4:12:f6:ab:43:
                    22:44:2c:95:89:a5:13:45:70:e6:67:fc:32:2e:39:
                    18:8b:c3:6f:d0:13:37:d6:d8:95:71:fb:27:94:45:
                    9c:d1:e5:3b:c2:ce:94:00:85:13:cd:3b:3c:58:45:
                    05:fd:47:b2:cf:3d:6a:fe:60:8e:89:16:30:fb:4a:
                    40:08:c3:66:76:d8:58:08:22:38:eb:fe:7e:f8:42:
                    ac:c3:89
                Exponent: 65537 (0x10001)
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        9d:f2:84:fc:34:fa:3e:7e:23:db:c4:2e:99:40:10:b7:28:88:
        8d:8e:1d:95:a4:bb:dc:5a:6f:de:99:04:9c:a1:af:3b:0a:26:
        5d:52:f5:18:39:5b:a2:c5:19:4f:44:4c:08:64:32:cc:2f:0d:
        e8:7a:5f:af:6a:2c:3a:a0:c3:22:7c:1a:c7:c5:5f:ed:00:a4:
        e3:a9:82:e9:05:b0:73:93:b9:8a:5d:05:55:d4:b7:f7:04:e0:
        47:39:20:1e:5b:22:bb:8c:b6:6c:87:a9:0a:24:2c:49:31:22:
        9a:2a:64:b2:7e:9c:81:e4:8c:22:e2:03:f7:ed:7a:26:28:e8:
        1e:f6:48:e9:14:19:80:32:a0:50:32:e3:5a:ef:f6:bf:a7:3c:
        7a:ec:a0:b4:d4:e7:2c:59:7f:72:38:d9:8f:10:61:44:43:a2:
        d9:b6:08:1b:33:cf:c3:07:c5:02:d8:c4:92:ae:53:21:e3:bf:
        de:8f:8c:bb:32:48:12:61:86:a0:4a:71:76:d4:c8:11:f8:14:
        c7:a0:1e:03:00:6f:6a:a9:cf:43:db:9e:be:ad:27:4c:c9:58:
        02:21:ce:39:c3:d2:1e:c6:45:18:d9:f5:63:3e:57:01:76:ed:
        ae:21:c1:e0:a6:72:50:40:71:7d:6c:af:e6:17:4f:9f:d5:6a:
        b7:e5:78:e6:0f:28:c6:ac:eb:ca:9d:6f:62:dd:12:36:09:e2:
        5b:2a:eb:1a:df:59:97:04:d4:b9:b6:b5:4c:b7:8b:16:6c:8b:
        99:7a:b1:ed:7e:57:39:f5:4b:5f:6b:c4:50:3e:7d:af:dc:c7:
        d4:45:8e:97:f7:a7:62:83:b9:1f:71:74:70:a9:ba:03:5c:5f:
        a5:1f:04:74:fa:ec:6f:85:e5:f3:f7:bf:8e:f2:97:23:90:6a:
        e5:99:86:b9:17:77:0c:7b:43:ab:e9:85:3d:6b:07:69:d8:98:
        21:a6:05:dc:fc:70:6c:9e:ed:89:8e:9c:7e:06:c1:02:11:49:
        c2:c2:9f:7f:84:0c:32:35:1a:8e:1f:72:63:68:fc:0b:bf:11:
        91:18:6a:ec:d9:c9:8d:68:b6:2b:f1:13:d1:1e:4f:da:61:ef:
        51:42:c0:19:41:59:3f:ac:4f:84:12:26:4f:c3:aa:4f:15:50:
        d2:04:74:31:fe:e3:4a:5d:40:57:35:a8:43:fb:ec:e9:e4:21:
        b3:55:41:c2:f5:6e:82:0b:6e:9d:48:e8:f9:bb:31:43:98:3b:
        38:0d:13:5b:4e:1c:e6:13:07:79:98:81:36:2b:2d:fa:fc:ac:
        c1:92:fe:4a:a6:33:91:56:1b:b8:2b:f5:c8:69:5f:ec:dd:c3:
        8d:e9:95:30:a6:89:4d:46
```

Now we have created our first certificate, which is signed with its own private key.
