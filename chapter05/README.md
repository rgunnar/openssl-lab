# Chapter 05

# Add Certificate Authority as trusted in web browser

We have now installed our TLS certificate in the Apache2 web server running locally, and we can therefore use the web browser (Firefox in this example) to view the start page <https://localhost/>

## Security Warning

In Firefox there should be a `Warning: Potential Security Risk Ahead`, since the certificate provided by the web server is not trusted och issued by a trusted CA. Under the `Advanced...` button, there is a possibility to view the certificate. When viewed we should see that it is the certificate we have issued.

## Configure Firefox to trust our CA

We can now configure Firefox to trust our CA, which means that all issued certificates from our CA will be considered trusted, if and only if they also follow the rules which we discussed earlier, i.e. validity is no longer than 398 days, etc.

Edit Settings (e.g. by typing "about:preferences" in the address field)

1. Go to "Privacy and Security"
2. Scroll down to "Certificates"
3. Click "View Certificates"
4. Verify that the "Authorities" tab is active
5. Click on button "Import..."
6. Choose our ca_certificate.pem (or whatever it is named)
7. Check the checkbox for "Trust this CA to identify websites"
8. Click "OK"

## View the start page as trusted

You should now be able to view the start page <https://localhost>, and it should be trusted.
