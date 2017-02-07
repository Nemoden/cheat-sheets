# OpenSSL

[Source](https://www.sslshopper.com/article-most-common-openssl-commands.html)

## General commands

#### Generate a new private key and Certificate Signing Request (csr)

    openssl req -out CSR.csr -new -newkey rsa:2048 -nodes -keyout privateKey.key

#### Generate a self-signed certificate

    openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out certificate.crt

#### Generate a certificate signing request (CSR) for an existing private key

    openssl req -out CSR.csr -key privateKey.key -new

#### Generate a certificate signing request based on an existing certificate

    openssl x509 -x509toreq -in certificate.crt -out CSR.csr -signkey privateKey.key

#### Remove a passphrase from a private key

    openssl rsa -in privateKey.pem -out newPrivateKey.pem


## Various checks

#### Check a Certificate Signing Request (CSR)

    openssl req -text -noout -verify -in CSR.csr

#### Check a private key

    openssl rsa -in privateKey.key -check

#### Check a certificate

    openssl x509 -in certificate.crt -text -noout

#### Check a PKCS#12 file (.pfx or .p12)

    openssl pkcs12 -info -in keyStore.p12

#### Check certificate validity through date example

`$ openssl x509 -in someKey.pem -text -noout | grep "Not After" | awk -F: '{$1="";print $0}'`

    Feb 10 08 21 49 2017 GMT


## Debugging using OpenSSL

If you are receiving an error that the private doesn't match the certificate or that a certificate that you installed to a site is not trusted, try one of these commands. If you are trying to verify that an SSL certificate is installed correctly, be sure to check out the [SSL Checker](https://www.sslshopper.com/ssl-checker.html).

#### Check an MD5 hash of the public key to ensure that it matches with what is in a CSR or private key

    openssl x509 -noout -modulus -in certificate.crt | openssl md5
    openssl rsa -noout -modulus -in privateKey.key | openssl md5
    openssl req -noout -modulus -in CSR.csr | openssl md5

#### Check an SSL connection. All the certificates (including Intermediates) should be displayed

    openssl s_client -connect www.paypal.com:443


## Converting Using OpenSSL

#### Convert a DER file (.crt .cer .der) to PEM

    openssl x509 -inform der -in certificate.cer -out certificate.pem

#### Convert a PEM file to DER

    openssl x509 -outform der -in certificate.pem -out certificate.der

#### Convert a PKCS#12 file (.pfx .p12) containing a private key and certificates to PEM

    openssl pkcs12 -in keyStore.pfx -out keyStore.pem -nodes

You can add -nocerts to only output the private key or add -nokeys to only output the certificates

#### Convert a PEM certificate file and a private key to PKCS#12 (.pfx .p12)

    openssl pkcs12 -export -out certificate.pfx -inkey privateKey.key -in certificate.crt -certfile CACert.crt

# General info on certificates and types

[Source](http://serverfault.com/questions/9708/what-is-a-pem-file-and-how-does-it-differ-from-other-openssl-generated-key-file)

## Basic formats

### Certificate Signing Request (.csr)

Some applications can generate these for submission to certificate-authorities. The actual format is **PKCS10** which is defined in [RFC 2986](https://tools.ietf.org/html/rfc2986). It includes some/all of the key details of the requested certificate such as subject, organization, state, whatnot, as well as the public key of the certificate to get signed. These get signed by the CA and a certificate is returned. The returned certificate is the public *certificate* (which includes the public key but not the private key), which itself can be in a couple of formats.

#### Generate .csr file along with private key (.key file)

    openssl req -out someCsrFile.csr -new -newkey rsa:2048 -nodes -keyout someKeyFile.key

### Container which **may** contain public key, private key and root certficates (.pem)

Defined in RFC's [1421](https://tools.ietf.org/html/rfc1421) through [1424](https://tools.ietf.org/html/rfc1421), this is a **container format _that may include_** just the public certificate (such as with Apache installs, and CA certificate files `/etc/ssl/certs`), or may include an entire certificate chain including public key, private key, and root certificates. Confusingly, it may also encode a CSR (e.g. as used [here](https://jamielinux.com/docs/openssl-certificate-authority/create-the-intermediate-pair.html)) as the PKCS10 format can be translated into PEM. The name is from [Privacy Enhanced Mail (PEM)](https://en.wikipedia.org/wiki/Privacy-enhanced_Electronic_Mail), a failed method for secure email but the container format it used lives on, and is a base64 translation of the x509 ASN.1 keys.

### Private key of a specific certificate (.key)

This is a PEM formatted file containing just the private-key of a specific certificate and is merely a conventional name and not a standardized one. In Apache installs, this frequently resides in `/etc/ssl/private`. The rights on these files are very important, and some programs will refuse to load these certificates if they are set wrong.

### Passworded and encrypted container format (.pkcs12 .pfx .p12)

Originally defined by RSA in the [Public-Key Cryptography Standards](http://www.rsa.com/rsalabs/node.asp?id=2124), the "12" variant was enhanced by Microsoft. This is a passworded container format that contains both public and private certificate pairs. Unlike .pem files, this container is fully encrypted. Openssl can turn this into a .pem file with both public and private keys:

    openssl pkcs12 -in file-to-convert.p12 -out converted-file.pem -nodes

## Some other formats

### .der

A way to encode ASN.1 syntax in binary, a .pem file is just a Base64 encoded .der file. OpenSSL can convert these to .pem. Windows sees these as Certificate files. By default, Windows will export certificates as .DER formatted files with a different extension. Like...

#### Convert .der to .pem

    openssl x509 -inform der -in to-convert.der -out converted.pem

### .cert .cer .crt

A .pem (or rarely .der) formatted file with a different extension, one that is recognized by Windows Explorer as a certificate, which .pem is not.

### .p7b

Defined in [RFC 2315](https://tools.ietf.org/html/rfc2315), this is a format used by windows for certificate interchange. Java understands these natively. Unlike .pem style certificates, this format has a defined way to include certification-path certificates.

## Summary

There are four different ways to present certificates and their components:

### PEM

Governed by RFCs, it's used preferentially by open-source software. It can have a variety of extensions (.pem, .key, .cer, .cert, more)

### PKCS7

An open standard used by Java and supported by Windows. Does not contain private key material.

### PKCS12

A private standard that provides enhanced security versus the plain-text PEM format. This can contain private key material. It's used preferentially by Windows systems, and can be freely converted to PEM format through use of openssl.

### DER

The parent format of PEM. It's useful to think of it as a binary version of the base64-encoded PEM file. Not routinely used by much outside of Windows.
