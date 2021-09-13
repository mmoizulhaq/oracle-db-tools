# Oracle Database Tools - DevOPs - Terraform - Vanity URLs for ADB without customer managed ORDS

This project is a terraform script to help register a Vanity URL on an ADB instance in OCI without the need for creating and maintaining an ORDS instance on compute. Just provide your certs, the ADB OCID and off you go!

It creates the following:
- VCN and Public Subnet
- Security Lists for access over 443 and 8080
- A Load Balancer
- A compute instance (full or micro)
- Installs ORDS, SQLcl and stages the APEX images
- Uses LetsEncrypt to get the certs for your customer domain
- Starts up ORDS connected to an ADB instance on 443 with the certs installed

The Variables.tf file can be used to tell terraform what ADB instance you are going to use and what your custom domain is named.

## The Variables (in the variables.tf file)

### Environment

**region**: What region you are in. Example would be us-ashburn-1

**tenancy_ocid**: The OCID of your tenancy

**vcn_ocid**: The OCID of the existing VCN you want to use

**compartment_ocid**: The OCID of an existing compartment where you want to place these resources

**adb_ocid**: The OCID of the existing ADB-S you want to use

**backend_port**: Port used for the Load Balancer to talk to the ADB-S. Is set to 443.

### Certificate Variables

**certificate_certificate_name**: A display name for the cert

**certificate_ca_certificate**: The text for the certificate, usually enclosed by -----BEGIN CERTIFICATE-----

**certificate_private_key**: The text for the certificate private key, usually enclosed by -----END RSA PRIVATE KEY-----
    
**certificate_public_certificate**: The text for the public certificate, usually enclosed by -----BEGIN CERTIFICATE-----

The text for the certificates/key are multi-line and need to be enclosed with EOT as in the example here:

```
variable "certificate_public_certificate" {

    default = <<-EOT
-----BEGIN CERTIFICATE-----
MIIGSDCCBTCgAwIBAgISA+V2MiuwZLh0jNru+kjD8LzMMA0GCSqGSIb3DQEBCwUA
MDIxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MQswCQYDVQQD
EwJSMzAeFw0yMTA4MjUxNjU2NDBaFw0yMTExMjMxNjU2MzlaMB8xHTAbBgNVBAMT
FGRpbm9zYXVyZm9vdGJhbGwuY29tMIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIIC
CgKCAgEAzBfcuiZATNwQgaoM5F88jOR+lJ4oDPTNPc+eXy62Pqb5aJFiHtM4I+RX
ZFbRw5RCOle7+tMWK/pgHJGeQF7qXB4r0r24ByEQV+SRtn110xpbaG1RLBnmHkNu
/Mqdp1KRIcH+DOuaR56oybAehQEOnsfkyBXAqikLdAqWNfP1ONjxVdzrSi3XkrYL
Ct2wXiFoz/mmTjUtBFMYfkTxPBpJMisMhjS+j9iofEXNok94m592YH1/IAPiQ0yP
.....
MDIxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MQswCQYDVQQD
EwJSMzAeFw0yMTA4MjUxNjU2NDBaFw0yMTExMjMxNjU2MzlaMB8xHTAbBgNVBAMT
FGRpbm9zYXVyZm9vdGJhbGwuY29tMIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIIC
CgKCAgEAzBfcuiZATNwQgaoM5F88jOR+lJ4oDPTNPc+eXy62Pqb5aJFiHtM4I+RX
-----END CERTIFICATE-----
EOT

}
```