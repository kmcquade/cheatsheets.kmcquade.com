# Certificates

### Get the certificate of a remote IP Address

```bash
ex +'g/BEGIN CERTIFICATE/,/END CERTIFICATE/p' <(echo | openssl s_client -showcerts -connect 127.0.0.1:8080) -scq
```

### Get the entire LDAP certificate chain from an LDAP Server:

```bash
ex +'g/BEGIN CERTIFICATE/,/END CERTIFICATE/p' <(echo | openssl s_client -showcerts -connect 10.1.1.187:636) -scq >> ldap_ca_chain.pem
```
