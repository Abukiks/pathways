### SSL and TLS Basics

To secure SSH or web servers, we use certificates to ensure encrypted communication and validate the identity of the server. Certificates help establish trust between two parties during a transaction. For example, when a user accesses a web server, a TLS certificate ensures that the communication is encrypted and verifies the server's authenticity.

### Symmetric Encryption

When a user sends data to a server in plain text, it is vulnerable to hacking. Symmetric encryption solves this by using a single key to encrypt the data. However, this method is still vulnerable because the server also needs the key to decrypt the data, and if the key is intercepted, the data can be decrypted by an attacker.

### Asymmetric Encryption - SSH

Asymmetric encryption uses a pair of keys: a private key and a public key (lock). This method is more secure because the public key is used to encrypt data, and only the corresponding private key can decrypt it.

To generate a private and public key:
```bash
ssh-keygen -t rsa
```
You can secure your server by adding the public key to it. The public key can be viewed with:
```bash
cat ~/.ssh/authorized_keys
```
This key can be shared with others, but only the associated private key can access the server.

### Create a Public/Private Key Pair

To configure a passwordless SSH connection between the jump host and app01 for user thor:
```bash
ssh-copy-id -i ~/.ssh/mykey.pub thor@app01
```
To access the server:
```bash
ssh -i id_rsa user1@server1
```
You can copy your public key to multiple servers to access them using the same private key. To allow others access, generate another key pair for them and add their public key to the server's `authorized_keys` file.

### Generating Key Pairs with OpenSSL

To generate a public and private key pair:
```bash
openssl genrsa -out my-bank.key 1024
```
```bash
openssl rsa -in my-bank.key -pubout > mybank.pem
```

### Certificates

To ensure the server's authenticity, use a certificate signed by a Certificate Authority (CA). Self-signed certificates can be used, but for browser trust, use a certificate from a recognized CA.

Generate a CSR (Certificate Signing Request):
```bash
openssl req -new -key my-bank.key -out my-bank.csr -subj "/C=US/ST=CA/O=MyOrg, Inc./CN=my-bank.com"
```
This CSR is then validated and signed by a CA.

### Creating a CSR and Self-Signed Certificate

1. Create a CSR on app01:
```bash
cd /etc/httpd/csr
sudo openssl req -new -newkey rsa:2048 -nodes -keyout app01.key -out app01.csr
```
2. Verify the CSR:
```bash
openssl req -noout -text -in app01.csr
```
3. Create a self-signed certificate on app01:
```bash
cd /etc/httpd/certs
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout app01.key -out app01.crt
```

### Configuring SSL on Apache Web Server

1. Update the SSL configuration to use your certificate and key in `/etc/httpd/conf.d/ssl.conf`:
```bash
SSLCertificateFile /etc/httpd/certs/app01.crt
SSLCertificateKeyFile /etc/httpd/certs/app01.key
```
2. Restart Apache to apply the changes:
```bash
sudo service httpd restart
```
3. Verify the certificate:
```bash
echo | openssl s_client -showcerts -servername app01.com -connect app01:443 2>/dev/null | openssl x509 -inform pem
```

### Internal CA for Organizations

For internal use, create a private CA server to generate and sign certificates, and distribute the public key to all internal employees' browsers. This ensures secure communication within the organization.

### Naming Conventions

**Certificate (Public Key):**
```
server.crt
server.pem
client.crt
client.pem
```
**Private Key:**
```
server.key
server-key.pem
client.key
client-key.pem
```