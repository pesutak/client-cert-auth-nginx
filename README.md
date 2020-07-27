# CLIENT-SIDE CERTIFICATE AUTHENTICATION WITH NGINX

## Create a CA Key
```
openssl genrsa -des3 -out ca.key 4096
```

## Create a CA Certificate
```
openssl req -new -x509 -days 365 -key ca.key -out ca.crt
```

## Create Client Key
```
openssl genrsa -des3 -out user.key 4096
```

## Create CSR
```
openssl req -new -key user.key -out user.csr
```

## Signing a CSR
```
openssl x509 -req -days 365 -in user.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out user.crt
```

## Creating a PKCS #12 (PFX)
```
openssl pkcs12 -export -out user.pfx -inkey user.key -in user.crt -certfile ca.crt
```
Install user.pfx on client machine!

## Generate Let's Encrypt Certs
```
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo apt-get update

sudo apt-get install certbot

sudo certbot certonly --standalone
```

## Run App
```
docker-compose up
```
