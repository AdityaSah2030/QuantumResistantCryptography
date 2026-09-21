# Lecture 02: Understanding Cryptography

## 1. Introduction to Cryptography

The word **Cryptography** comes from:

- **Kryptos** → hidden
- **Graphein** → to write

### What is Cryptography?

Cryptography is the science of protecting information by transforming it into a form that cannot be understood by unauthorized people.

The goal is to ensure that information can be securely stored or transmitted and accessed only by the intended parties.

### Security Goals of Cryptography

#### Confidentiality
Only authorized parties should be able to read the information.

#### Integrity
Data should not be altered without the alteration being detected.

#### Authentication
Authentication confirms the identity of the sender or the party communicating with the system.

#### Non-repudiation
A sender should not be able to deny having sent a message or performed a particular transmission.

#### Availability
The required data or service should remain available to the intended person when needed.

---

# 2. Core Concepts: Plaintext and Ciphertext

Two basic terms used throughout cryptography are **plaintext** and **ciphertext**.

### Plaintext

Plaintext is the original, readable message before encryption.

### Ciphertext

Ciphertext is the scrambled or transformed form of the plaintext after encryption.

The basic process is:

```text
Plaintext
    |
    | Encryption + Key
    v
Ciphertext
    |
    | Decryption + Key
    v
Plaintext
```

---

## 3. Symmetric Encryption

In **symmetric encryption**, the same secret key is used for both encryption and decryption.

```text
                 Secret Key
                     |
                     v
Plaintext --Encryption--> Ciphertext --Decryption--> Plaintext
                     ^                         ^
                     |                         |
                Same Secret Key          Same Secret Key
```

The sender and receiver therefore need to have access to the same secret key.

### Worked Example: Caesar Cipher

A simple example of symmetric encryption is the **Caesar Cipher**.

Here, the key is a shift of **3**.

```text
HELLO
  |
  | Shift each letter by 3
  v
KHOOR
```

So:

- Plaintext = `HELLO`
- Key = shift by 3
- Ciphertext = `KHOOR`

Applying the reverse shift during decryption recovers the original plaintext.

### Important Idea

Modern cryptography follows the same basic logical structure:

> **Plaintext + Key + Transformation → Ciphertext**

However, modern cryptographic algorithms use much more complex mathematics than a simple Caesar shift.

---

# 4. Asymmetric Encryption

**Asymmetric encryption** uses two different but mathematically related keys:

- **Public Key**
- **Private Key**

A simplified representation is:

```text
Plaintext
    |
    | Encryption
    | Public Key
    v
Ciphertext
    |
    | Decryption
    | Private Key
    v
Plaintext
```

The public key can be shared openly, while the private key must be kept secret.

This solves an important problem in cryptography: two parties do not need to share the same secret key before communication.

---

# 5. Cryptographic Key Management

According to **Kerckhoffs's principle**, the security of a cryptographic system should not depend on keeping the algorithm secret.

The algorithm can be public.

> **A system's security rests on the protection of its keys.**

Therefore, key management is a critical part of cryptographic security.

Important areas of key management include:

1. **Secure key generation**
2. **Hardware storage**
3. **Safe distribution**
4. **Key lifecycle and rotation**

A strong encryption algorithm is not sufficient if its keys are poorly generated, stored, distributed, or managed.

---

# 6. Secure Key Generation and Entropy

A cryptographic key is only as secure as the randomness used to create it.

> **A cryptographic key is only as secure as the mathematical randomness (entropy) used to create it.**

Poor randomness can make otherwise strong cryptographic algorithms vulnerable.

### Sources and Methods Mentioned

#### True Randomness (TRNG)

A **True Random Number Generator (TRNG)** obtains randomness from physical or environmental sources.

The purpose is to provide unpredictable random values suitable for security-sensitive operations.

#### Cryptographically Secure Pseudo-Randomness (CSPRNG)

A **CSPRNG** generates pseudorandom values using algorithms designed to make the output computationally unpredictable.

It is widely used when cryptographic keys and other security-sensitive random values need to be generated.

#### Key Derivation Function (KDF)

A **Key Derivation Function (KDF)** derives cryptographic keys from an existing secret, such as a password or another keying material.

This allows a suitable cryptographic key to be generated from an input that may not itself be directly suitable as a key.

---

# 7. Hardware-Level Key Protection

Keys stored directly in ordinary software memory can be vulnerable to compromise.

For high-security environments, physical or hardware-based isolation can provide stronger protection for sensitive key material.

Examples mentioned:

- **Hardware Security Modules (HSMs)**
- **Trusted Execution**
- **Key Encryption Keys (KEKs)**

### Hardware Security Modules (HSMs)

HSMs are specialized hardware designed to securely store and process cryptographic keys.

They help keep sensitive key material isolated from ordinary application memory and operations.

### Trusted Execution

Trusted execution provides an isolated environment in which sensitive operations can be performed with stronger protection against unauthorized access.

### Key Encryption Keys (KEKs)

A Key Encryption Key is used to encrypt or protect other cryptographic keys.

This creates an additional layer of protection for stored key material.

---

# 8. Secure Key Exchange Mechanisms

In cryptographic communication, two parties may need to establish shared secret information over a network that could be hostile or monitored.

The objective is:

> **Establish shared secrets over hostile, monitored networks without exposing the key material.**

Mechanisms mentioned include:

### Diffie-Hellman Exchange

**Diffie-Hellman** allows two parties to establish a shared secret over an insecure communication channel without directly transmitting the secret itself.

### Elliptic Curve Cryptography (ECC)

Elliptic Curve Cryptography provides public-key cryptographic techniques based on the mathematics of elliptic curves.

ECC can be used to achieve cryptographic security with relatively small key sizes.

### PKI Certificates

**Public Key Infrastructure (PKI)** uses digital certificates to associate public keys with identities.

Certificates help parties establish trust in the public key they receive.

---

# 9. Security Attacks

Security attacks can broadly be classified into **passive attacks** and **active attacks**.

| Passive Attacks | Active Attacks |
|---|---|
| The attacker observes or copies data without altering the system or data. | The attacker alters the data stream, modifies resources, or creates false data. |
| Release of message contents | Masquerade |
| Traffic analysis | Replay and modification |
|  | Denial of Service (DoS) |

---

## 9.1 Passive Attacks

In a passive attack, the attacker mainly attempts to **observe information** without changing the communication or system.

### Release of Message Contents

The attacker obtains access to the actual contents of a communication.

### Traffic Analysis

Even when the contents of communication are protected, an attacker may analyze communication patterns such as the amount, timing, or frequency of traffic.

The goal is to obtain useful information without directly modifying the communication.

---

## 9.2 Active Attacks

In an active attack, the attacker interacts with or modifies the system or communication.

### Masquerade

An attacker pretends to be a legitimate user or entity.

### Replay and Modification

An attacker captures previously transmitted data and later reuses it, or modifies the data before it reaches the intended recipient.

### Denial of Service (DoS)

An attacker attempts to make a system or service unavailable to legitimate users.

---

# 10. Cryptographic Methods

The lecture introduced two major categories of encryption:

1. **Symmetric Encryption**
2. **Asymmetric Encryption**

---

## 10.1 Symmetric Encryption

The same secret key is used for encryption and decryption.

```text
Plaintext
    |
    | Encryption
    | Private/Secret Key
    v
Ciphertext
    |
    | Decryption
    | Private/Secret Key
    v
Plaintext
```

The major requirement is that the secret key must remain protected and must be available to the legitimate communicating parties.

---

## 10.2 Asymmetric Encryption

Asymmetric encryption uses a pair of keys:

- Public key
- Private key

The public key can be shared, while the private key must remain secret.

```text
Plaintext
    |
    | Encryption
    | Public Key
    v
Ciphertext
    |
    | Decryption
    | Private Key
    v
Plaintext
```

This provides a fundamentally different approach from symmetric encryption because the encryption and decryption keys are different.

---

# 11. Cryptographic Fix: Full Disk Encryption

### Data at Rest Security

Data stored on a device is called **data at rest**.

Full Disk Encryption (FDE) is a cryptographic method that encrypts the data stored on a disk.

When the computer is switched off, the data stored on the disk remains encrypted rather than being available as readable information.

### Why It Matters

If someone gains unauthorized physical access to a storage device, encryption helps prevent them from simply reading the stored data.

> **Full Disk Encryption protects data at rest by encrypting the contents of the disk.**

---

# 12. Cryptographic Fix: Salting and Strong Hashing

Hashing is commonly used when sensitive values such as passwords need to be stored without storing the original plaintext password.

### Modern Hashing

A cryptographic hash function transforms input data into a derived fixed-size value.

```text
Input
  |
  v
Hash Function
  |
  v
Hash Value
```

A secure hash is designed so that recovering the original input from the hash is computationally difficult.

### Cryptographic Salting

A **salt** is a random value added to a password before hashing.

Conceptually:

```text
Password + Random Salt
          |
          v
     Hash Function
          |
          v
      Password Hash
```

Salting ensures that identical passwords do not automatically produce identical stored hashes when different salts are used.

---

# 13. Key Lifecycle and Crypto-Shredding

A cryptographic key should not be treated as something that can remain valid forever.

> **A key with an infinite lifespan poses an infinite risk.**

Key management therefore requires strict temporal controls.

### Cryptoperiods

A **cryptoperiod** is the period during which a cryptographic key is authorized for use.

After the appropriate period, the key should be rotated, replaced, revoked, or otherwise retired according to the system's security requirements.

### Rapid Key Revocation

Keys may need to be revoked quickly when they are:

- Compromised
- No longer trusted
- No longer required
- Outside their permitted cryptoperiod

### Crypto-Shredding

Crypto-shredding refers to destroying the cryptographic key that protects encrypted data.

If the encrypted data remains but the required decryption key is securely destroyed, the protected information becomes effectively inaccessible.

---

# 14. Real-World Example: Online Banking

Online banking demonstrates how cryptographic protection is used during real-world communication.

### Step 1: Establishing a Secure Connection

When you log in to a banking website, your browser and the bank's server establish a secure, encrypted connection before sensitive data is exchanged.

### Step 2: Entering Credentials

You enter information such as your password and account details.

The communication should be protected rather than transmitted as readable plaintext across the network.

### Step 3: Encryption

The browser uses cryptographic mechanisms to protect the data exchanged with the bank's server.

The goal is to prevent unauthorized parties monitoring the network from reading or modifying the communication.

---

# 15. Real-World Example: Messaging Apps

Messaging applications such as **WhatsApp** use **end-to-end encryption**.

The basic idea is:

> Only the intended communicating endpoints should be able to read the message.

The message is protected while travelling between the sender and intended recipient, preventing intermediaries from simply reading the message contents.

---

# 16. Key Takeaways

### Cryptography provides multiple security goals

- Confidentiality
- Integrity
- Authentication
- Non-repudiation
- Availability

### Encryption has two major forms

- **Symmetric:** Same secret key for encryption and decryption
- **Asymmetric:** Public key and private key are used for different operations

### Key management is critical

The cryptographic algorithm does not need to be secret. The **keys** must be properly protected.

Key security depends on:

- Strong randomness and entropy
- Secure storage
- Safe distribution
- Proper lifecycle management
- Rotation and revocation

### Cryptography is also about the surrounding system

Security is not achieved only by choosing a strong algorithm. The complete system must also securely handle keys, devices, communication channels, storage, and the lifecycle of cryptographic material.
