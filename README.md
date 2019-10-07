# Certificate management


This about how to generate self signed certificates with a private CA.

## Create openssl config files

Create .CA.cnf for generating the CA key and certificate files and for signing certificate requests.

Create .host.cnf for generating a server certificate request. The .host.cnf can contain SubjectAlternateName entries (aliases for the server or wildcards for a domain).


## Generate the root CA key and certificate

These steps have to be done only once.
```
touch index.txt index.txt.attr
echo "01" > serial.txt

openssl req -x509 -config .CA.cnf -newkey rsa:4096 -sha512 -nodes -out MYCOMPANY-cacert.pem -outform PEM
```

## Generate the server certificate request and sign the CSR with the root CA
```
openssl req -config .host.cnf -newkey rsa:4096 -sha512 -nodes -out MYCOMPANY-hostcert.csr -outform PEM
openssl ca -config .CA.cnf -policy signing_policy -extensions signing_req -out MYCOMPANY-hostcert.pem \ 
  -infiles MYCOMPANY-hostcert.csr
```

The CSR and the certificate can be verified with the following command:
```
openssl req -text -noout -verify -in
```


## Next steps

To get rid of certificate error warnings in browsers, import MYCOMPANY-cacert.pem to every client that will connect to hosts using self signed certificates.


