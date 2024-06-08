# Ks Cryp
Introducing KsCryp, a new npm package that simplifies the process of using cryptographic algorithms in your Node.js applications. With KsCryp, you can easily perform RSA encryption, generate and verify JWTs, hash data using popular algorithms such as MD5, SHA1, SHA256, and SHA512, as well as perform JSON encoding and decoding and basic encoding.

One of the key features of KsCryp is that it provides a consistent and simple API for all of these operations. You can use the same methods to encode, decode, and verify data using any of the supported algorithms, which helps to simplify your code and reduce the potential for errors.

KsCryp also provides easy-to-use helper functions for common use cases, such as generating RSA key pairs and encrypting/decrypting data using those keys. Additionally, it includes a flexible configuration system that allows you to customize various aspects of the library to meet your specific needs.

Overall, KsCryp is a powerful yet easy-to-use cryptographic library that helps simplify the process of using popular cryptographic algorithms in your Node.js applications. Whether you're building a web application, a mobile app, or any other type of software that requires secure data handling, KsCryp can help you get up and running quickly and easily.

This library belong to the Ksike ecosystem:
- [KsMf](https://www.npmjs.com/package/ksmf) - Microframework (WEB, REST API, CLI, Proxy, etc)
- [Ksdp](https://www.npmjs.com/package/ksdp) - Design Patterns Library (GoF, GRASP, IoC, DI, etc)
- [KsCryp](https://www.npmjs.com/package/@taktikorg/totam-temporibus) - Cryptographic Library (RSA, JWT, x509, HEX, Base64, Hash, etc) 
- [KsHook](https://www.npmjs.com/package/kshook) - Event Driven Library
- [KsEval](https://www.npmjs.com/package/kseval) - Expression Evaluator Library 
- [KsWC](https://www.npmjs.com/package/kswc) - Web API deployment Library

## Install and Use
``` npm install @taktikorg/totam-temporibus ```
```js 
const KsCryp = require("@taktikorg/totam-temporibus");
```
## HASH 
[Hash encoding](doc/hash.md)  A hash function is a mathematical algorithm that takes in input data of arbitrary size and outputs a fixed-length string of characters, known as the hash value or digest. The goal of a hash function is to produce a unique output for each unique input, so that any change in the input data results in a different hash value. Hash functions are commonly used in cryptography to ensure the integrity and authenticity of data by generating unique digital signatures, and in data structures like hash tables to index and retrieve data.

## HEX
[HEX encoding](doc/hex.md) is a method for representing binary data in a human-readable format. In this encoding scheme, each byte of binary data is represented by two hexadecimal digits, which are numbers and letters ranging from 0-9 and A-F. For example, the byte 10101110 would be represented as the two hexadecimal digits "AE". HEX encoding is often used in computer systems for a variety of purposes, such as displaying error messages or encoding data in URLs. 

## Base64
[Base64 encoding](doc/base64.md) is a method for representing binary data in ASCII text format. In this encoding scheme, every three bytes of binary data are represented as four characters from a set of 64 characters, which includes letters, numbers, and special characters. The resulting text is larger than the original binary data, but can be transmitted or stored as plain text without modification. Base64 encoding is commonly used in email systems, as well as in web applications for encoding data such as images or audio files that need to be transmitted over HTTP or other text-based protocols. Overall, Base64 encoding provides a means of converting binary data into a form that can be transmitted or stored as plain text, making it easier to work with in various applications.

## Basic
[Basic encoding](doc/basic.md), also known as basic authentication, is a simple method for authenticating users over HTTP. It involves sending a username and password in plaintext, encoded using base64 encoding, in the HTTP header of each request. The format of the encoded credentials is "username:password", which is then base64 encoded and added to the Authorization header of the HTTP request. While basic encoding is simple to implement, it is not secure as the credentials are transmitted in plaintext and can easily be intercepted by malicious parties. As such, it is often used in combination with other security measures like HTTPS.

## JWT
[JWT (JSON Web Tokens)](doc/jwt.md) is a method for securely transmitting information between parties as a JSON object. JWT is composed of three parts: the header, the payload, and the signature. The header contains information about the type of token and the cryptographic algorithm used to secure it. The payload contains the actual information being transmitted, such as a user ID or permissions. The signature is created by encoding the header and payload with a secret key, which ensures the integrity of the token and prevents tampering. JWTs are commonly used for authentication and authorization purposes, as they allow users to securely transmit information between different systems without the need for an actual session or cookie.

## RSA 
[RSA (Rivest–Shamir–Adleman)](doc/rsa.md) is a public-key cryptographic algorithm that is widely used for secure data transmission. The RSA algorithm is based on the fact that it is very difficult to factor large prime numbers. The algorithm involves generating a public-private key pair, where the public key can be distributed to anyone who wants to send an encrypted message, and the private key is kept secret and used to decrypt the message.

### Generate RSA key pair with OpenSSL
```
openssl req -x509 -new -newkey rsa:2048 -nodes \
  -subj '/C=US/ST=California/L=San Francisco/O=JankyCo/CN=Test My' \
  -keyout myCert.key \
  -out myCert.pem \
  -days 7300
```
```js
const publicKey = fs.readFileSync(path.join(__dirname, 'myCert.pem'));
const privateKey = fs.readFileSync(path.join(__dirname, 'myCert.key'));
```

### Generate RSA key pair with KsCryp 
```js
const cfg = {
    phrase : "1234567890",
    cipher : "aes-256-cbc",
    length: 2048,
    public: {
        type: 'spki',
        format: "pem",
    },
    private: {
        type: 'pkcs8',
        format: "pem",
    }
};
const opt = await KsCryp.generate("rsa", cfg);
const publicKey = opt.publicKey;
const privateKey = opt.privateKey;
```

### RSA Encode 
```js
const dat = "123.-.456";
const enc = KsCryp.encode(dat, "rsa", { privateKey, publicKey });
```

### RSA Decode 
```js
const dec = KsCryp.decode(enc, "rsa", { privateKey, publicKey });
```

The security of RSA relies on the fact that it is difficult to factor large prime numbers. However, as computing power has increased, it has become easier to break RSA encryption using brute force methods. To counter this, larger key sizes are now recommended for RSA encryption.

## x509
[X.509 encoding:](doc/x509.md) In cryptography, X.509 is an [ITU](https://en.wikipedia.org/wiki/International_Telecommunication_Union) standard defining the format of public key certificates. 

### Generate x509 certificate
```js
const opts = {
    altNameIPs: ['127.0.0.1', '55.77.55.77'],
    altNameURIs: ['http://localhost', 'https://test.com'],
    validityDays: 300,
    length: 2048,
    data: {
        commonName: 'my.test.com',
        stateOrProvinceName: 'Barcelona',
        countryName: 'ES',
        localityName: 'Barcelona',
        organizationName: 'Aircraft',
        organizationNameShort: 'TesTas'
    }
};
const cert = await KsCryp.generate("x509", opts);

const fs = require("fs");
fs.writeFileSync("x509-cert.pem", cert.publicKey);
fs.writeFileSync("x509-key.pem", cert.privateKey);
```

X.509 certificates bind an identity to a public key using a digital signature. In the X.509 system, there are two types of certificates. The first is a CA certificate. The second is an end-entity certificate. A CA certificate can issue other certificates. The top level, self-signed CA certificate is sometimes called the Root CA certificate. Other CA certificates are called intermediate CA or subordinate CA certificates. An end-entity certificate identifies the user, like a person, organization or business. An end-entity certificate cannot issue other certificates. An end-entity certificate is sometimes called a leaf certificate since no other certificates can be issued below it.

## Json
[JSON (JavaScript Object Notation)](doc/json.md) is a lightweight data interchange format that uses a human-readable text structure to represent and transmit structured data. It consists of key-value pairs organized in objects and supports arrays, numbers, strings, booleans, and null values. JSON is widely used for data exchange between applications due to its simplicity, versatility, and ease of parsing across various programming languages.
