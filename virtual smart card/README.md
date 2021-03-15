# Virtual Smart Card

## create virtual card 
```
tpmvscmgr.exe create /name ExcaliburSmartCard /pin prompt /pinpolicy minlen 4 maxlen 8 /adminkey random /generate
```
## generate cert and export to pfx or p12
 
 i use [xca util](https://www.hohnstaedt.de/xca/)  
 or  
 ```
 openssl pkcs12 -export -out client.pfx -inkey client.key -in client.crt
```
only certs with key are exportable

## import cert into Smart Card
```
certutil -csp "Microsoft Base Smart Card Crypto Provider" -importpfx client.pfx
```

## TPM-backed cert
create tpm_csr.inf file
```
[NewRequest]
Subject = "CN=PixonTPM" ; can be anything, can also be changed when signing
Keylength = 2048 ; your TPM may support larger key lengths
Exportable = FALSE
UserProtected = TRUE
MachineKeySet = FALSE
ProviderName = "Microsoft Platform Crypto Provider"
ProviderType = 1
RequestType = PKCS10
KeyUsage = 0xB0 ; this is a https client certificate, can change when signing
FriendlyName = "My tpm-backed certificate"
```
generate CSR  
```
certreq -new -f .\tpm_csr.inf csr_pixontpm.csr
```
sign CSR wih CA  


  
## Notes
it is posible to use Personal ID (PCA cert) in ssh auth process via [putty-cac](http://risacher.org/putty-cac/)
