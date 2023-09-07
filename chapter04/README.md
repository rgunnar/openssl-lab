# Chapter 04

# Setup web server to use certificate

Now it is time to setup the web server for using the certificate we have created. We'll use Apache in this example. If you're running the provided Virtual Machine, Apache is already setup to serve web content over TLS. This can be tries using a web browser on the Virtual Machine and going to <https://localhost/>. You will see a security risk warning, since the certificate installed is not trusted.

## Remove the pass phrase from the private key

We will need to remove the pass phrase from the private key file, in order for the apache web server to read the file upon startup. There are of course many different solutions on how to handle private keys, but we will use the most rudimentary setup in having an unprotected private key, and placing it in a directory where only root and apache can read it.
We'll copy the private key for our TLS certificate to an unprotected (no pass phrase) file using openssl:

`openssl rsa -in tls_key.pem -out tls_key_unprotected.pem`

## Place the certificate and private key where it is appropriate

Place the certificate somewhere on the disk where it is appropriate, for instance in the `/etc/ssl/certs` and `/etc/ssl/private/` directories. We will also change owner and permissions on the private key file.

```shell
sudo cp tls_certifcate.pem /etc/ssl/certs/
sudo mv tls_key_unprotected.pem /etc/ssl/private/
sudo chown root:ssl-cert /etc/ssl/private/tls_key_unprotected.pem
sudo chmod 640 /etc/ssl/private/tls_key_unprotected.pem
```

## Setup Apache to use our certificate

Edit the file `/etc/apache2/sites-available/default-ssl.conf`
There are two lines specifying `SSLCertificateFile` and `SSLCertificateKeyFile`. These need to be changed to use our certificate and private key.

```shell
SSLCertificateFile    /etc/ssl/certs/tls_certificate.pem
SSLCertificateKeyFile /etc/ssl/private/tls_key_unprotected.pem
```

## Restart web server for settings to take effect

We need to restart the Apache web server in order for changed settings to take effect:

`sudo systemctl restart apache2`

Then we can check that the restart was successful:
`sudo systemctl status apache2`

And it should say `active (running)`

