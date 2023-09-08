# openssl-lab chapter 06

# s_client

The command s_client can be used to act like a client trying to connect to a host using SSL/TLS. It can be used as a client when you want to verify that your server works as intended or it can be used if you want to retrieve certificates from the server. It works for any kind of host using SSL/TLS, not just web servers.
It is possible to use client certificates to verify client authentication. 
The command is designed to be interactive, so after connection is done, you can send commands the the server if it accepts that.

## Use s_client to connect to your web server

echo | openssl s_client -connect localhost:443 -showcerts

