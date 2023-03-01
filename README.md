Generate the keys for the Certificate Signing Request

```bash
openssl genrsa -des3 -out server.key 4096
```

Create the insecure key, the one without a passphrase, and shuffle the key names

```bash
openssl rsa -in server.key -out server.key.insecure
mv server.key server.key.secure
mv server.key.insecure server.key
```

Create the CSR

```bash
openssl req -new -key server.key -out server.csr
```

Creating a Self-Signed Certificate

```bash
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```

Installing the Certificate

```bash
sudo cp server.crt /etc/ssl/certs
sudo cp server.key /etc/ssl/private
```

Create the directories to hold the CA certificate and related files

```bash
sudo mkdir /etc/ssl/CA
sudo mkdir /etc/ssl/newcerts
```

The CA needs a few additional files to operate, one to keep track of the last
serial number used by the CA, each certificate must have a unique serial number,
and another file to record which certificates have been issued

```bash
sudo sh -c "echo '01' > /etc/ssl/CA/serial"
sudo touch /etc/ssl/CA/index.txt
```
