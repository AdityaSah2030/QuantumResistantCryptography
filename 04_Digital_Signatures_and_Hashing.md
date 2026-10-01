# Lecture 04: Digital Signatures & Hashing

> **Source:** *Digital Signatures & Hashing: How the Web Verifies Trust*
> by Dr. Arpita Dutta, Assistant Professor, CSE(AI & ML), Techno Main
> Salt Lake.

## 1. The Big Picture

This lecture explains how cryptography helps answer four practical
questions:

1.  **Has the data changed?**
2.  **Did it really come from the claimed sender?**
3.  **Can the sender later deny sending it?**
4.  **How does a browser know that a website is the website it claims to
    be?**

The story develops like this:

``` text
Hashing -> Integrity -> MAC/HMAC -> Authentication
        -> Digital Signatures -> Certificates -> HTTPS/TLS
```

------------------------------------------------------------------------

# Part 1: Hashing

## 2. What Is a Hash?

A **hash function** takes input of any size and produces a fixed-length
output called a **hash**, **digest**, or **checksum**. The lecture
compares it to a fingerprint for data.

``` text
Input: "Hello"
        |
        v
     SHA-256
        |
        v
256-bit hash = 64 hexadecimal characters
```

The same input always gives the same output. A hash is intended to be
one-way, meaning that recovering the original input from the hash is
computationally infeasible.

### Hashing is not encryption

``` text
Encryption:
Plaintext -> Encryption -> Ciphertext -> Decryption -> Plaintext

Hashing:
Input -> Hash Function -> Digest
```

Encryption is designed to be reversible for an authorized party. Hashing
is designed as a one-way transformation.

## 3. Fingerprint Analogy

A fingerprint identifies a person without containing their complete
identity or story. Similarly, a hash acts like a fingerprint for a file
or message.

Important properties:

-   Same file -\> same hash.
-   A tiny change -\> a very different hash.
-   The original file cannot practically be reconstructed from the hash.
-   Two different files should be extremely unlikely to have the same
    cryptographic hash.

------------------------------------------------------------------------

# 4. Properties of a Good Hash Function

The lecture gives five properties.

### 4.1 Deterministic

The same input must always produce the same output.

### 4.2 Fast to Compute

Hashing should be efficient even for large inputs.

### 4.3 One-Way / Pre-image Resistant

Given only the hash, finding an input that produces it should be
computationally infeasible.

### 4.4 Avalanche Effect

A very small input change should cause a large, unpredictable change in
the output.

Example from the lecture:

``` text
"HelloWorld"  -> 872e4e50...
"HelloWorld!" -> 7f83b165...
```

Only one character was added, but the hashes are completely different.

### 4.5 Collision Resistant

A **collision** occurs when two different inputs produce the same hash.

``` text
Input A -> Hash X
Input B -> Hash X
```

A good cryptographic hash makes intentionally finding such a pair
extremely difficult.

------------------------------------------------------------------------

# 5. SHA-256 at a High Level

The lecture shows the following simplified pipeline:

``` text
Message
   |
   v
Padding
   |
   v
512-bit blocks
   |
   v
Compression rounds
   |
   v
Chaining
   |
   v
256-bit digest
```

The compression stage uses operations such as AND, OR, XOR, bit shifts,
and fixed constants. SHA-256 processes each block through 64 rounds. You
do not need to perform these calculations by hand. The important idea is
that repeated mixing makes reversing the process computationally
infeasible.

------------------------------------------------------------------------

# 6. Common Hash Algorithms

  Algorithm            Size Lecture's description
  ----------- ------------- --------------------------------
  MD5               128-bit Broken, collisions found
  SHA-1             160-bit Deprecated, broken in practice
  SHA-256           256-bit Recommended in the lecture
  SHA-3         224-512 bit Modern alternative

The lecture mentions the 2017 **SHAttered** attack against SHA-1 and
emphasizes retiring MD5 and SHA-1 for security applications.

------------------------------------------------------------------------

# 7. Hashing Passwords

Websites should not normally store a user's actual password in
plaintext.

### Bad approach

``` text
Database
alice -> Summer2024!
```

If the database is stolen, the real password is immediately exposed.

### Hash-based approach

``` text
User enters password
        |
        v
   Hash password
        |
        v
Store only the hash
```

During login, the server hashes the entered password again and compares
it with the stored value.

``` text
Entered password
      |
      v
     Hash
      |
      +------ compare ------ Stored hash
                         |
                    match -> login
```

The important idea is that the server can check the password without
storing the original password itself.

## 7.1 Salting

Hashing alone is not enough for password storage. If two users choose
the same password and no salt is used, their hashes are identical.

A **salt** is a random value added to a password before hashing.

``` text
password + random salt
          |
          v
         hash
```

For example:

``` text
Alice: password123 + x7&qP2 -> hash A
Bob:   password123 + k9!mZ4 -> hash B
```

The passwords are the same, but the stored hashes are different. The
lecture explains that this defeats the usefulness of pre-computed
**rainbow tables** for those salted values.

------------------------------------------------------------------------

# 8. Hashes for File Integrity

Software publishers can publish a file's checksum alongside a download.

``` text
Publisher:
File -> SHA-256 -> Published hash

User:
Downloaded file -> SHA-256 -> Calculated hash

Compare the two hashes
```

If they match, the downloaded bytes correspond to the published
fingerprint. If they differ, the file is different from the one
represented by the published hash.

This is useful for detecting corruption or modification.

------------------------------------------------------------------------

# 9. Why a Plain Hash Cannot Authenticate a Sender

Suppose Alice sends:

``` text
"Pay Bob $100" + hash(message)
```

Eve intercepts the message and changes it to:

``` text
"Pay Eve $10,000"
```

Eve can also calculate the hash of the modified message because hash
functions are public.

So Bob receives:

``` text
"Pay Eve $10,000" + hash("Pay Eve $10,000")
```

The hash matches, but Eve created both the modified message and its
hash.

Therefore:

> **A plain hash proves consistency, not identity.**

This is the point where the lecture moves from integrity to
authentication.

------------------------------------------------------------------------

# Part 2: Authentication and Integrity

## 10. Three Different Guarantees

  -----------------------------------------------------------------------
  Guarantee               Question                Typical tool
  ----------------------- ----------------------- -----------------------
  Integrity               Has this been changed?  Hashing

  Authentication          Is it really from who   MACs / Digital
                          it claims?              Signatures

  Non-repudiation         Can the sender deny     Digital Signatures
                          sending it later?       
  -----------------------------------------------------------------------

Remember:

``` text
Integrity       -> Has it changed?
Authentication  -> Who sent it?
Non-repudiation -> Can the sender deny sending it?
```

------------------------------------------------------------------------

# 11. MAC: Message Authentication Code

A **MAC** adds a secret key to the authentication process.

Alice and Bob share a secret key. Eve does not know it.

Conceptually, the lecture represents this as:

``` text
MAC = Hash(message + secret key)
```

The important idea is that the authentication value depends on a secret
that the attacker does not possess.

### Verification

``` text
Sender:
message + secret key -> MAC

Send:
message + MAC

Receiver:
received message + secret key -> new MAC

Compare:
new MAC == received MAC
```

If the values do not match, the receiver rejects the message.

------------------------------------------------------------------------

# 12. HMAC

**HMAC** means **Hash-based Message Authentication Code**. The lecture
presents it as the industry-standard way to construct a MAC from a hash
function such as SHA-256.

Example:

``` text
Message: "Transfer $50"
Secret key: ********
             |
             v
        HMAC-SHA256
             |
             v
            MAC
```

The sender sends the message and MAC. The receiver recomputes the HMAC
using the shared key and compares the result.

If the message changes, the HMAC changes. If the key changes, the HMAC
also changes dramatically.

### Real-world uses mentioned in the lecture

-   AWS API requests can use HMAC-SHA256 with secret access keys.
-   Payment webhooks can contain HMAC signatures so a server can verify
    a notification.
-   Messaging systems use MAC-like constructions to detect message
    modification.

------------------------------------------------------------------------

# Part 3: Digital Signatures

## 13. Why Move Beyond MACs?

A MAC requires the sender and receiver to share a secret.

That is useful when two parties already have a shared secret, but it
becomes inconvenient when many independent people need to verify that a
particular person created a document.

Public-key cryptography solves this with a pair of keys.

------------------------------------------------------------------------

# 14. Public-Key Cryptography

A public/private key pair contains:

### Private Key

-   Kept secret.
-   Held by the owner.
-   Used to create a digital signature.

### Public Key

-   Can be shared freely.
-   Used by others to verify signatures made with the matching private
    key.
-   Is mathematically related to the private key.

A simple analogy from the lecture is a wax seal:

``` text
Private key -> the seal-making tool only you possess
Public key  -> information others use to recognize the seal
```

------------------------------------------------------------------------

# 15. Tiny RSA-Style Example

The lecture uses deliberately tiny numbers to make the pattern visible.
These numbers are **not secure real-world RSA parameters**.

``` text
Private key = 7
Public key  = 3
Modulus     = 33
Message "HI" = 15
```

Signing:

``` text
15^7 mod 33 = 27
```

So the signature is 27.

Verification:

``` text
27^3 mod 33 = 15
```

The result matches the original message representation.

The example illustrates the core idea:

``` text
Private key -> create signature
Public key  -> verify signature
```

Real cryptographic systems use much larger parameters and algorithms.

------------------------------------------------------------------------

# 16. What Is a Digital Signature?

A **digital signature** is a block of cryptographic data associated with
a message or file and created using the signer's private key.

Anyone with the corresponding public key can verify that:

1.  The signature corresponds to the claimed signer.
2.  The content has not changed since it was signed.

The lecture describes digital signatures as combining:

``` text
Hashing
   +
Public-key cryptography
   |
   v
Digital Signature
```

This provides integrity and authentication, with the lecture also
associating digital signatures with non-repudiation.

------------------------------------------------------------------------

# 17. Digital Signature vs Electronic Signature

These terms are not identical.

An **electronic signature** is a broad concept. Examples include:

-   typed name
-   scanned signature
-   checkbox indicating agreement

A **digital signature** is a specific cryptographic technique based on
public-key cryptography.

``` text
Electronic signature -> broad digital indication of consent
Digital signature    -> cryptographic signing mechanism
```

The lecture notes legal recognition of digital/electronic signature
frameworks in places including India, the EU, and the US.

------------------------------------------------------------------------

# 18. Signing a Document

Suppose Alice wants to sign `Contract_v3.pdf`.

The lecture's process is:

### Step 1: Start with the document

``` text
Contract_v3.pdf
```

### Step 2: Hash it

``` text
hash(document) -> digest
```

### Step 3: Sign the digest

The lecture's simplified explanation describes using Alice's private key
with the digest to produce the signature.

``` text
Digest + Alice's private key -> Signature
```

### Step 4: Send the document and signature

``` text
Document + Signature
```

### Why sign the hash?

A large file may contain millions of bytes. Instead of applying the
public-key signing operation directly to the whole file, the system
signs its small fixed-size digest.

``` text
Large document
      |
      v
    Hash
      |
      v
Small digest
      |
      v
   Signature
```

------------------------------------------------------------------------

# 19. Verifying a Digital Signature

Bob receives:

``` text
Document + Signature
```

### Step 1: Hash the received document

``` text
Received document -> Hash -> Digest B
```

### Step 2: Verify the signature with Alice's public key

``` text
Signature + Alice's public key -> Digest A
```

### Step 3: Compare

``` text
Digest A == Digest B
```

If they match, verification succeeds.

If even one character or byte changes, the document's hash changes and
the comparison fails.

Full flow:

``` text
ALICE

Document
   |
   v
Hash -> Digest
   |
   v
Sign with Alice's private key
   |
   v
Signature
   |
   +------ Document + Signature ------>

BOB

Document -> Hash -> Digest B

Signature + Alice's public key -> Digest A

Digest A == Digest B
        |
        v
     Verified
```

------------------------------------------------------------------------

# 20. Real-World Uses of Digital Signatures

### Software and App Updates

Developers can sign applications and updates. Platforms or operating
systems can verify the signature using the developer's public key. If
the software is modified after signing, verification can fail.

### Signed PDFs and Documents

Digitally signed contracts, government forms, certificates, and mark
sheets can contain embedded signature information that can be checked by
compatible software.

### Git Commits

Developers can GPG-sign commits so teams can verify the cryptographic
identity associated with the signed change.

------------------------------------------------------------------------

# Part 4: How Websites Verify Information

## 21. What HTTPS Means

When you visit an HTTPS website, the browser uses **TLS** to protect the
connection.

The lecture highlights three properties:

### Encryption

Data in transit is encrypted.

### Integrity

Data should not be silently modified while traveling between the browser
and server.

### Authentication

The browser verifies that the connection corresponds to the domain
represented by the certificate.

### The padlock does not mean the site is automatically trustworthy

A phishing site can also have HTTPS. The padlock is evidence about the
security and certificate-backed identity of the connection, not a
guarantee that the site's content or intentions are safe.

------------------------------------------------------------------------

# 22. Digital Certificates

A **digital certificate** is a digitally signed file that binds a public
key to a domain name.

Conceptually:

``` text
Domain name + Public key
          |
          v
      Certificate
          |
          v
   Signed by a CA
```

The signer is a trusted **Certificate Authority (CA)**.

------------------------------------------------------------------------

# 23. Chain of Trust

The lecture describes a hierarchy:

``` text
Root CA
   |
   | signs
   v
Intermediate CA
   |
   | signs
   v
Website Certificate
   |
   v
bank.example.com
```

### Root CA

A Root CA is deeply trusted and is typically included in the trust store
of an operating system or browser.

### Intermediate CA

An Intermediate CA is signed by a Root CA and can issue day-to-day
certificates.

### Website Certificate

The website certificate contains the site's public key and identity
information and is signed by an Intermediate CA.

The browser can trust the website certificate by tracing the signatures
back to a trusted Root CA.

------------------------------------------------------------------------

# 24. Simplified TLS Handshake

The lecture presents the handshake as five conceptual steps.

### 1. Browser says hello

The browser requests a secure connection and communicates supported
cryptographic options.

### 2. Server sends its certificate

The certificate contains the server's public key and CA-backed identity
information.

### 3. Browser verifies the certificate

The browser checks the signature chain and confirms that the domain
matches.

### 4. Both sides establish a shared session key

The verified public-key infrastructure is used as part of establishing
the session securely.

### 5. Encrypted communication begins

The established session key protects subsequent communication.

Simplified:

``` text
Browser -> Hello -> Server
Server -> Certificate -> Browser
Browser -> Verify certificate
Both sides -> Establish session key
Both sides -> Encrypted communication
```

The real TLS protocol contains more details than this teaching model,
but this captures the conceptual sequence presented in the lecture.

------------------------------------------------------------------------

# 25. Certificate Transparency

The lecture introduces **Certificate Transparency (CT)**.

Publicly trusted certificates are logged in public
certificate-transparency systems. This creates a searchable record of
certificates issued for domains.

The lecture mentions `crt.sh` and Google's Certificate Transparency
lookup as examples.

Security researchers can inspect these logs to find suspicious or
incorrectly issued certificates.

------------------------------------------------------------------------

# 26. Inspecting a Real Certificate

A browser can show certificate information for an HTTPS website.

The lecture suggests checking:

-   **Issued to**
-   **Issued by**
-   **Valid from**
-   **Valid to**

The lecture also introduces TLS testing tools such as Qualys SSL Labs,
which can show certificate paths, supported protocols, cipher suites,
and configuration issues.

------------------------------------------------------------------------

# 27. Hashing and Breach Checking

The lecture connects hashing with password-breach checking through
**Have I Been Pwned** and its Pwned Passwords service.

The lecture describes a privacy-preserving technique based on
**k-anonymity**:

``` text
Password
   |
   v
Hash locally
   |
   v
Send only part of the hash
```

According to the lecture, the service does not receive the full password
and only receives the first five characters of the hash.

For the practical exercise, the lecture recommends testing only an old,
retired password, never a currently used password.

------------------------------------------------------------------------

# 28. Hash vs MAC vs Digital Signature vs Certificate

  -----------------------------------------------------------------------
  Mechanism         Integrity         Authentication    Main requirement
  ----------------- ----------------- ----------------- -----------------
  Hash              Yes               No                Public hash
                                                        algorithm

  MAC / HMAC        Yes               Yes               Shared secret key

  Digital Signature Yes               Yes               Private/public
                                                        key pair

  Certificate       Used as part of   Yes, through CA   Trusted CA
                    the trust system  verification      
  -----------------------------------------------------------------------

Think of them as layers solving different problems:

``` text
Hash
  -> Is the data unchanged?

MAC / HMAC
  -> Is the data unchanged and does it come from someone who knows the shared secret?

Digital Signature
  -> Is the data associated with the holder of a particular private key?

Certificate
  -> Can we trust that this public key belongs to this domain/identity?
```

------------------------------------------------------------------------

# 29. The Complete Story

The easiest way to remember the entire lecture is as a chain of problems
and solutions.

## Problem 1: How do we detect changes?

Use hashing.

``` text
Data -> SHA-256 -> Digest
```

A changed input produces a different digest.

## Problem 2: A hash does not prove who created the data

Anyone can calculate a normal hash.

Use a MAC when two parties share a secret.

``` text
Message + Secret -> MAC
```

## Problem 3: We need public verification without sharing a secret

Use digital signatures.

``` text
Private key -> Sign
Public key  -> Verify
```

## Problem 4: How do we know a public key belongs to a website?

Use a digital certificate signed by a trusted Certificate Authority.

``` text
Domain + Public Key -> Certificate -> CA trust
```

## Problem 5: How does the browser use all of this?

HTTPS/TLS establishes a secure authenticated connection.

``` text
Browser
   |
   v
TLS handshake
   |
   v
Certificate verification
   |
   v
Secure session key
   |
   v
Encrypted communication
```

------------------------------------------------------------------------

# 30. Important Comparisons

## Hash vs Encryption

  -----------------------------------------------------------------------
  Hashing                             Encryption
  ----------------------------------- -----------------------------------
  One-way cryptographic               Designed to be reversible with the
  transformation                      correct key

  Produces a fixed-size digest        Produces ciphertext

  Used for integrity and related      Primarily used for confidentiality
  applications                        

  No decryption step                  Decryption is part of the process
  -----------------------------------------------------------------------

## Hash vs MAC

  -----------------------------------------------------------------------
  Hash                                MAC
  ----------------------------------- -----------------------------------
  No secret required                  Uses a secret key

  Anyone can calculate it             Only parties with the secret can
                                      create a valid MAC

  Does not prove sender identity      Provides authentication in the
                                      shared-secret model
  -----------------------------------------------------------------------

## MAC vs Digital Signature

  -----------------------------------------------------------------------
  MAC                                 Digital Signature
  ----------------------------------- -----------------------------------
  Shared secret                       Private/public key pair

  Sender and receiver share the       Signer keeps private key secret
  secret                              

  Useful between parties with an      Useful when many parties need to
  established shared key              verify a signer
  -----------------------------------------------------------------------

## Electronic vs Digital Signature

``` text
Electronic signature -> broad concept of digital consent
Digital signature    -> specific cryptographic technique
```

------------------------------------------------------------------------

# 31. Practical Exercises From the Lecture

The lecture includes seven demonstrations:

1.  **Hash a message:** use CyberChef and observe the SHA-256 avalanche
    effect.
2.  **Verify a file checksum:** hash a file and compare its checksum.
3.  **Compute an HMAC:** change the secret key and observe the output
    change.
4.  **Generate and verify a digital signature:** sign a message and then
    modify it to make verification fail.
5.  **Search Certificate Transparency logs:** inspect certificates
    issued for a domain.
6.  **Inspect TLS:** examine a website's certificate chain and TLS
    configuration.
7.  **Check for password breaches:** use a retired password with the
    privacy-preserving breach checker.

------------------------------------------------------------------------

# 32. Quick Revision

### Hashing

> A hash is a fixed-size fingerprint of data.

``` text
Same input -> Same hash
Small input change -> Very different hash
Hash -> Original input is computationally infeasible to recover
```

### MAC / HMAC

> A MAC adds authentication using a shared secret.

``` text
Message + Secret -> MAC
```

### Digital Signature

> A digital signature uses a private key to sign and a public key to
> verify.

``` text
Private Key -> Sign
Public Key  -> Verify
```

### Certificate

> A certificate connects a public key to a domain through a trusted
> Certificate Authority.

``` text
Domain + Public Key -> Certificate -> CA trust
```

### HTTPS / TLS

> HTTPS uses TLS to protect communication with encryption, integrity
> protection, and certificate-backed authentication.

------------------------------------------------------------------------

# 33. Final Mental Model

``` text
                         CRYPTOGRAPHIC TRUST
                                |
             +------------------+------------------+
             |                  |                  |
             v                  v                  v
          HASHING              MAC          DIGITAL SIGNATURE
             |                  |                  |
             v                  v                  v
         Integrity       Shared-secret       Public-key
                         Authentication      Authentication
                                                   |
                                                   v
                                             CERTIFICATE
                                                   |
                                                   v
                                             Trusted Domain
                                                   |
                                                   v
                                               HTTPS / TLS
                                                   |
                                                   v
                                          Secure Web Connection
```

## Key Takeaways

-   A **hash** is a fixed-length fingerprint of data.
-   Good cryptographic hashes are deterministic, fast, one-way,
    avalanche-sensitive, and collision-resistant.
-   **SHA-256** and **SHA-3** are the modern algorithms emphasized in
    the lecture.
-   **MD5** and **SHA-1** should not be used for modern security
    applications.
-   Hashes are useful for password storage, file-integrity checking, and
    other integrity-related tasks.
-   **Salts** make identical passwords produce different stored hashes.
-   A plain hash does not prove who created a message.
-   **MAC/HMAC** adds authentication using a shared secret.
-   **Digital signatures** use private/public key pairs for signing and
    verification.
-   A signature is normally applied to a hash of the document rather
    than the entire large file.
-   A changed document produces a different digest, causing signature
    verification to fail.
-   A **digital signature** is a specific cryptographic technique, while
    an electronic signature is a broader concept.
-   A **digital certificate** binds a public key to a domain and is
    signed by a trusted CA.
-   Certificate chains commonly involve Root CAs, Intermediate CAs, and
    website certificates.
-   HTTPS/TLS uses certificates and secure key establishment to protect
    web communication.
-   A padlock does not mean a website itself is trustworthy; it
    describes security properties of the connection.
-   Certificate Transparency provides public logs of trusted
    certificates.
-   Hashing can also support privacy-preserving password-breach
    checking.

------------------------------------------------------------------------

## One-Line Summary

> **Hashing detects changes, MACs authenticate with shared secrets,
> digital signatures authenticate with public/private keys, certificates
> establish trust in public keys, and HTTPS/TLS brings these mechanisms
> together to secure communication on the web.**
