# Importing VA Internally Signed Certificates into AWS ACM

## Purpose

Provide steps to a Devops Engineer to update/renew `cms.va.gov` TLS certificates generated by VA Venafi. The certificate will be stored in AWS ACM for use in AWS Services and Resources. 

## Importing Into AWS ACM

### Generate a TLS Certificate with VA Venafi
Instructions can be found [here](https://vfs.atlassian.net/wiki/spaces/OT/pages/1787953328/Venafi+Create+and+download+TLS+certificates) to renew the CMS TLS Certificate. Scroll down to the last section `How to import the certificate, private key, and chain into ACM` for tips on importing the certificate.

#### Unencrypt Venafi TLS certificate

At this stage the *.key is still passphrase protected and you cannot import the private key directly because ACM does not allow password/passphrase protected keys. You won’t get an error that you can’t upload with a passphrase, and it will look correct. So run this command to strip the password: `openssl rsa -in [original.key] -out [new.key]` this will ask you for the password.


### Prerequisites
This is not an exhaustive list of prerequisites but the most relevant to our purposes. Follow the AWS documentation found in the `References` section for more detail.
* To import a certificate signed by a certificate authority (CA), you must also include the certificate chain.
* The certificate must be valid at the time of import. You cannot import a certificate before its validity period begins or after it expires. The NotBefore certificate field contains the validity start date, and the NotAfter field contains the end date.
* The private key must be unencrypted. You cannot import a private key that is protected by a password or passphrase.
* The certificate, private key, and certificate chain must be PEM–encoded.
* DSVA AWS GovCloud Account IAM Credentials.

### Format

#### PEM-encoded certificate
```
-----BEGIN CERTIFICATE-----
Base64–encoded certificate
-----END CERTIFICATE-----
```
#### PEM–encoded certificate chain
```
-----BEGIN CERTIFICATE-----
Base64–encoded certificate
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
Base64–encoded certificate
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
Base64–encoded certificate
-----END CERTIFICATE-----
```
#### PEM–encoded private keys
```
-----BEGIN RSA PRIVATE KEY-----
Base64–encoded private key
-----END RSA PRIVATE KEY-----
```

### Reimport the certificate

1. Open the ACM console at https://console.aws.amazon.com/acm/home

1. In the certificate list find `cms.va.gov` in the `Domain name` column.

1. Open the details pane of the certificate and choose the Reimport certificate button. If you selected the certificate by checking the box beside its name, choose Reimport certificate on the Actions menu.

1. For Certificate body, paste the PEM-encoded end-entity certificate.

1. For Certificate private key, paste the unencrypted PEM-encoded private key associated with the certificate's public key.

1. For Certificate chain, paste the PEM-encoded certificate chain. The certificate chain includes one or more certificates for all intermediate issuing certification authorities, and the root certificate. If the certificate to be imported is self-assigned, no certificate chain is necessary.

1. Choose Review and import.

1. Review the information about your certificate. If there are no errors, choose Reimport.

## References
https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html

https://docs.aws.amazon.com/acm/latest/userguide/import-reimport.html

https://vfs.atlassian.net/wiki/spaces/OT/pages/1787953328/Venafi+Create+and+download+TLS+certificates