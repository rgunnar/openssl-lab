# Chapter 02

## OpenSSL config file

The config file can be used to configure all kinds of stuff, and is very powerful. We looked at using a rudimentary config file when we created the CA certificate.

## Variables

A useful thing in the config file is usage of variables. It is just as easy as writing `variable_name = value` on a line.

For instance:

`myDefaultEmail = my.name@omegapoint.se`

and the variable can then be used in the config file using `${variable_name}`, e.g.:

`emailAddress_default = ${myDefaultEmail}`

## Setting variable from outside the config file

Variables can be set dynamically when using the openssl config file. One easy way of doing that is with environment variable in the shell.
Environment variables are referenced using `${ENV:<variable_name>}`. For instance setting the e-mail address from an environment variable.

`EMAIL_ADDRESS=my.name@omegapoint.se openssl req .... -config my_openssl.conf ...`

And in the config file (using a variable as well):

```shell
email_address = ${ENV:EMAIL_ADDRESS}
...

emailAddress_default = ${email_address}
```

## Sections

The config file can have different sections usig swuare parenthesis. For instance, the config file can have different sections for x509v3 extensions, used for different types of certificates. These can then be referenced using the `-extensions <section name>` on the command line or in a script.

```
[ req_web_server_cert ]
...

[ req_ca_cert ]
...
```

Sections can be named and are referred to by name.
```
...
policy                          = policy_match

[ policy_match ]
...
```

## Input fields

For the input fields, there is a possibility to set default values. This is especially handy when running openssl in batch mode.

For instance the variable:

`commonName = Common Name (Certificate Authority name)`

you can also set the default value using:

`commonName_default = my.name@omegapoint.se`

or using a variable

`commonName_default = ${email_address}`

## CA section

In the CA section of the config file you can specify some things that are handy.

`serial` for specifying the file containing the serial counter for certificates issued.
`database` for specifying the database file containing issued certificates.
`new_certs_dir` for specifying where issued certificates should be placed.
`certificate` for specifying the path to the CA certificate.
`private_key` for specifying the path to the CA private key file.

For instance:

```
dir = .

[ CA_default ]
serial        = $dir/CA/my_serial.srl
database      = $dir/CA/certindex.txt
new_certs_dir = $dir/server_certificates
certificate   = $dir/CA/ca_certificate.pem
private_key   = $dir/CA/private/cakey.pem
default_days  = 825
default_md    = sha256
preserve      = no
email_in_dn   = yes
nameopt       = default_ca
certopt       = default_ca
policy        = policy_match
```

