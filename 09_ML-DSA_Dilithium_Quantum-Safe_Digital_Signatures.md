# 09 · ML-DSA (Dilithium): Quantum-Safe Digital Signatures

## Introduction

Digital signatures are one of the foundations of digital trust. They answer three practical questions:

1. **Who sent this?**
2. **Was the content changed after it was signed?**
3. **Can the signer later deny having signed it?**

Traditional digital signatures such as RSA and elliptic-curve signatures are built on mathematical problems that are hard for ordinary computers. The arrival of sufficiently powerful quantum computers changes that assumption.

This lecture follows the journey:

> **Handwritten signatures → digital signatures → their real-world failure modes → quantum threat → post-quantum signatures → ML-DSA (Dilithium)**

The central idea is simple:

> **ML-DSA is a post-quantum digital signature algorithm designed to provide digital authenticity and integrity using lattice-based mathematics that is not known to be efficiently breakable by quantum computers.**

---

# 1. What Is a Digital Signature?

## 1.1 The bank-message problem

Imagine receiving a message:

> “Your account will be CLOSED tomorrow. Transfer your balance to this new account to keep it active.”

Before trusting it, we naturally ask:

- Who really sent it?
- Was the message changed while travelling?
- How can a computer determine whether it is genuine?

In the physical world, we use things such as:

- letterheads
- stamps
- handwritten signatures
- official documents
- identity checks

The digital world cannot depend on visual appearance. Anyone can copy text or images perfectly.

A digital signature provides a **machine-checkable way to establish trust**.

---

# 2. What a Handwritten Signature Promises

A handwritten signature traditionally provides three ideas:

### 1. Identity

> “It was really me.”

### 2. Intent

> “I agree to exactly this.”

### 3. Commitment

> “I cannot later deny that I signed it.”

However, handwritten signatures have serious limitations.

- They can be forged.
- The same signature can appear on many documents.
- A scanned signature can be copied.
- A handwritten signature does not mathematically bind itself to the exact contents of a document.

For example:

> A signature on a ₹100 cheque can look exactly the same as the signature on a ₹1,00,000 cheque.

The signature itself does not encode the amount.

---

# 3. Why a Scanned Signature Is Not a Digital Signature

A scanned signature is simply an image.

Suppose:

```text
[Image of handwritten signature]
```

is inserted into a PDF.

Someone can:

1. copy the image,
2. paste it into another document,
3. send the modified document.

The image itself gives no cryptographic proof that:

- the actual owner created the document,
- the contents were not modified,
- the signature belongs to this exact document.

A true digital signature is fundamentally different.

> **A digital signature is a mathematical value generated using a private key and tied to the exact document being signed.**

---

# 4. Three Core Properties of Digital Signatures

A digital signature provides three major security properties.

## 4.1 Authenticity

The signature provides evidence that the document came from the holder of the corresponding private key.

```text
“Did this really come from Priya?”
                ↓
           Signature
                ↓
             Yes / No
```

---

## 4.2 Integrity

The signature detects modification.

If even one character changes, the verification should fail.

Example:

```text
Original:
Pay Ravi Rs 100

Modified:
Pay Ravi Rs 900
```

The two messages produce completely different cryptographic fingerprints.

---

## 4.3 Non-repudiation

Non-repudiation means the signer cannot easily deny having signed a document when the signing key was properly controlled.

It is especially important for:

- contracts
- government documents
- company filings
- tenders
- financial transactions

---

## 4.4 What a digital signature does NOT provide

A digital signature does **not** hide the message.

```text
Digital signature → authenticity + integrity + trust

Encryption → confidentiality / secrecy
```

A signed document may still be readable by anyone who receives it.

### Remember

> **Signatures are about trust, not secrecy.**

---

# 5. The Two-Key Idea

Digital signatures use asymmetric cryptography.

Each signer has two mathematically related keys:

```text
             KEY PAIR
                │
       ┌────────┴────────┐
       ↓                 ↓
 PRIVATE KEY         PUBLIC KEY
       │                 │
     SECRET            SHARED
       │                 │
   Create/sign       Check/verify
```

## 5.1 Private key

The private key is secret.

It is used to:

> **CREATE digital signatures**

Think of it as a king's signet ring.

Only the owner should have access to it.

---

## 5.2 Public key

The public key can be distributed to everyone.

It is used to:

> **CHECK / VERIFY digital signatures**

Think of it as a publicly available picture of the king's seal.

Everyone can check a signature, but knowing the public key should not allow them to create a valid signature.

---

## 5.3 The central asymmetry

```text
Signing:
Private key → Signature

Verification:
Public key + Signature + Document → Valid / Invalid
```

This asymmetry is what makes digital signatures usable between strangers over the Internet.

---

# 6. Hash Functions: The Document Fingerprint

Documents can be enormous.

Instead of directly working with an entire document, a digital signature scheme uses a cryptographic hash.

A hash function converts arbitrary-length input into a fixed-length output.

```text
SMS
Contract
500-page report
      │
      ↓
  HASH FUNCTION
      │
      ↓
Short fixed-size fingerprint
```

For example:

```text
8de03023ba83280d...0642e5
```

The lecture emphasizes three important properties:

### Same input → same hash

```text
Document A
    ↓
Hash X

Document A
    ↓
Hash X
```

### Tiny change → drastically different hash

```text
Pay Ravi Rs 100
        ↓
Hash A

Pay Ravi Rs 900
        ↓
Hash B
```

### Collision resistance

It should be computationally infeasible to find two different documents that produce the same hash.

---

# 7. How Digital Signing and Verification Work

Consider Priya and Ravi.

## 7.1 Signing

Priya has a document.

```text
Priya's document
       ↓
Compute hash
       ↓
Sign using PRIVATE KEY
       ↓
Digital signature
```

Priya sends:

```text
Document + Signature
```

---

## 7.2 Verification

Ravi receives the document and signature.

```text
Received document
       ↓
Compute hash
       ↓
Use Priya's PUBLIC KEY
       ↓
Check signature
       ↓
VALID ✓ / REJECTED ✕
```

Ravi never needs Priya's private key.

---

## 7.3 Why the signature is tied to the document

Suppose Priya signs:

```text
Pay Ravi Rs 100
```

An attacker changes it to:

```text
Pay Ravi Rs 900
```

The document's hash changes.

Therefore the old signature no longer matches.

```text
Original document
       ↓
Original hash
       ↓
Original signature

Modified document
       ↓
Different hash
       ↓
Signature verification fails
```

This is one of the major advantages over a scanned signature.

---

# 8. Who Vouches for the Public Key?

A new problem appears:

> How does Ravi know that the public key actually belongs to Priya?

The answer is a **trusted third party**.

## 8.1 Certificate Authorities

A **Certificate Authority (CA)** verifies identity and issues digital certificates.

Conceptually:

```text
Certifying Authority
        │
        ↓
“This public key belongs to Priya”
        │
        ↓
Priya's certificate
        │
        ↓
Others can trust Priya's public key
```

This resembles a passport authority:

```text
Government checks identity
        ↓
Passport issued
        ↓
Others trust the passport
```

Similarly:

```text
CA checks identity
        ↓
Certificate issued
        ↓
Others trust the public key
```

In India, the lecture notes that Certifying Authorities are licensed under the **Controller of Certifying Authorities (CCA)**.

---

# 9. Certificate Chains and Browser Trust

Modern browsers contain trusted root certificates.

Conceptually:

```text
Root Authority
      ↓
Certifying Authority
      ↓
Website / Person Certificate
      ↓
Public Key
```

The browser checks this chain.

The familiar website padlock represents a successful validation of the relevant certificate chain.

### Important idea

> **Digital trust is not only mathematics. It also depends on institutions and trust infrastructure that connect public keys to real identities.**

---

# 10. Where Digital Signatures Are Used

Digital signatures are used throughout modern digital infrastructure.

Examples from the lecture include:

- Website authentication
- App and operating-system updates
- UPI and net banking
- Secure messaging
- Income-tax filings
- Company filings
- Aadhaar eSign
- DigiLocker documents
- e-passports
- e-tendering
- Digital contracts

They are effectively invisible infrastructure supporting digital trust.

---

# 11. Digital Signatures and Law

Digital signatures also have legal significance.

## India

The lecture notes that India's **Information Technology Act, 2000** gave digital signatures legal recognition, with later developments broadening recognition to electronic signatures, including Aadhaar-based eSign.

## Other examples

- European Union → eIDAS
- United States → ESIGN Act

The basic concept is that appropriately created digital signatures can have legal significance similar to handwritten signatures.

---

# 12. What Happens Without Digital Signatures?

Imagine removing digital signatures from the Internet.

Potential consequences include:

### Fake software updates

Attackers could distribute malicious code disguised as legitimate updates.

### Fake websites

Users would have less reliable ways to determine whether a site is genuinely operated by the claimed organisation.

### Tampered contracts

Amounts and dates could be altered after signing.

### Forged records

Digital certificates, land records, degrees, and other documents would become easier to manipulate.

Therefore:

> **Digital signatures are a foundation of digital trust, not merely an optional security feature.**

---

# 13. Where Digital Signatures Go Wrong

Digital signatures can fail even when their underlying mathematics is strong.

The lecture groups practical failures into several categories:

1. Stolen keys
2. Bad randomness
3. Broken chains of trust
4. Human mistakes
5. Eventually, the threat of broken mathematics

The first four largely go around the mathematics.

The quantum threat attacks the mathematical assumptions themselves.

---

# 14. Pitfall 1: Stolen Private Keys

## Stuxnet, 2010

The lecture uses Stuxnet as an example.

Malware associated with Stuxnet used genuine digital signatures created with private keys stolen from legitimate hardware companies.

The important lesson is:

```text
Valid signature
      ≠
Software is necessarily safe
```

If an attacker obtains the genuine private key, they may create signatures that verify correctly.

### Lesson

> **Private-key protection is critical.**

Keys may therefore be stored using:

- hardware security modules (HSMs)
- tamper-resistant devices
- PIN-protected DSC tokens

---

# 15. Pitfall 2: Bad Randomness

## PlayStation 3, 2010

Some digital signature schemes require a fresh random value for each signature.

The lecture describes the PlayStation 3 incident where the same value was reused.

Conceptually:

```text
Signature 1 → random value R
Signature 2 → same random value R
                     ↓
               information leaks
                     ↓
              private key recovered
```

Researchers were able to derive Sony's private signing key from the repeated value.

### Lesson

> **Cryptography depends not only on mathematical algorithms but also on correct engineering and randomness.**

A small implementation mistake can compromise an otherwise secure mathematical scheme.

---

# 16. Pitfall 3: Broken Chains of Trust

## DigiNotar, 2011

DigiNotar was a Dutch Certificate Authority that was compromised.

Attackers issued fraudulent certificates, including certificates associated with Google.

Browsers subsequently removed DigiNotar from their trusted lists.

### Lesson

A certificate system is a chain of trust.

If a trusted authority is compromised:

```text
Trusted root
    ↓
Compromised CA
    ↓
Fake certificate
    ↓
False identity
```

The cryptographic mathematics can be functioning correctly while the surrounding trust infrastructure fails.

---

# 17. MD5 and the Flame Example

The lecture also discusses Flame and the weakness of MD5.

The important concept is a **hash collision**.

A collision occurs when:

```text
Document A → Hash X
Document B → Hash X
```

If an attacker can deliberately construct such collisions, systems relying on the hash can potentially be abused.

### Lesson

> **Cryptographic building blocks must be retired when they become weak.**

This is why algorithms and hash functions need lifecycle management.

---

# 18. Pitfall 4: The Human Factor

A digital signature proves that a particular private key was used.

It does not necessarily prove:

- who physically pressed the button,
- that the signer understood the document,
- that the content is truthful.

For example, if someone gives their DSC token and PIN to an agent, documents can be signed using that credential.

Also:

```text
Digitally signed lie
        ↓
Still a valid signature
```

### Important distinction

> **A signature proves origin and integrity, not truth.**

### Good practices

- Never casually share your DSC token and PIN.
- Read documents before signing.
- Take certificate warnings seriously.

---

# 19. The Fundamental Quantum Threat

Until now, the failures were caused by:

- stolen keys
- bad randomness
- compromised authorities
- human mistakes

The next problem is fundamentally different.

> **Quantum computers threaten the mathematical assumptions behind today's public-key cryptography.**

---

# 20. Why RSA Is Secure Today

RSA relies on the difficulty of factoring large integers.

Multiplication is easy:

```text
61 × 53 = 3233
```

But reversing the operation is harder:

```text
3233 = ? × ?
```

For very large numbers, factoring becomes computationally difficult for ordinary computers.

RSA relies on this asymmetry.

---

# 21. Elliptic-Curve Signatures

Elliptic-curve schemes such as ECDSA rely on a different mathematical problem:

> **The discrete logarithm problem in elliptic-curve groups.**

The underlying principle is similar:

```text
Easy in one direction
        ↓
Hard to reverse
        ↓
Security
```

Common traditional public-key systems threatened by Shor's algorithm include:

- RSA
- ECDSA
- ECDH
- Diffie-Hellman-type systems

---

# 22. What Is a Quantum Computer?

A classical computer uses bits:

```text
0 or 1
```

A quantum computer uses **qubits**.

A qubit can exist in a quantum superposition of states.

The lecture uses the analogy:

```text
Classical bit:
coin lying heads or tails

Qubit:
coin still spinning
```

Quantum computing also uses:

- Superposition
- Entanglement
- Interference

---

# 23. Superposition

A qubit can represent a quantum combination of the basis states:

```text
|0⟩
|1⟩
```

before measurement.

This does **not** mean a quantum computer simply performs every possible calculation and automatically gets all answers.

The useful advantage comes from carefully controlling quantum amplitudes and interference.

---

# 24. Entanglement

Qubits can become correlated through **entanglement**.

The state of multiple qubits can no longer always be described as independent states.

This allows quantum algorithms to manipulate large quantum states in ways that classical systems cannot directly reproduce efficiently.

---

# 25. Interference

Quantum algorithms use interference to manipulate probability amplitudes.

Conceptually:

```text
Wrong possibilities
       ↓
interfere destructively
       ↓
reduced

Correct possibilities
       ↓
interfere constructively
       ↓
amplified
```

The lecture describes this using waves in a pond.

---

# 26. Important: Quantum Computers Are Not Faster at Everything

Quantum computers are not simply faster CPUs.

They are expected to provide major advantages only for particular classes of problems.

The problem for cryptography is that:

> **The mathematical problems underlying RSA and elliptic-curve cryptography are among the problems for which quantum algorithms can provide dramatic advantages.**

---

# 27. Shor's Algorithm

In 1994, **Peter Shor** demonstrated a quantum algorithm capable of efficiently solving:

- integer factoring
- discrete logarithms

on a sufficiently powerful quantum computer.

Therefore:

```text
Public key
    ↓
Shor's algorithm
    ↓
Private key
    ↓
Forge signatures
```

This is fundamentally different from stealing a key.

The attacker could derive the private key mathematically from public information.

---

# 28. What Would Shor Mean for Digital Signatures?

If a cryptographically relevant quantum computer becomes available:

```text
RSA public key
      ↓
Shor
      ↓
RSA private key
      ↓
Forge signatures
```

Similarly:

```text
ECC public key
      ↓
Quantum discrete-log algorithm
      ↓
Private key
      ↓
Forge signatures
```

Potential consequences include forged:

- software updates
- bank messages
- certificates
- digital contracts
- authentication credentials

---

# 29. When Will This Happen?

The exact timeline is unknown.

The lecture presents a historical/illustrative timeline including:

- 1994 → Shor's algorithm
- 2019 → estimates involving millions of noisy qubits
- 2024 → NIST publishes initial post-quantum standards
- 2025 → newer estimates suggest fewer than one million noisy qubits may be sufficient under certain assumptions
- 2030 → NIST roadmap target for deprecation of RSA/ECC in relevant federal contexts
- 2035 → NIST roadmap target for disallowing them in relevant federal contexts

### Important

These are planning milestones and estimates, not a guaranteed date for the arrival of a cryptographically relevant quantum computer.

> **Nobody knows the exact date.**

---

# 30. Why Migration Must Start Before Quantum Computers Arrive

Replacing cryptography across large systems can take many years.

Consider systems that must remain trustworthy for decades:

- land records
- wills
- long-term contracts
- 20-year loans
- vehicle firmware
- satellites
- medical devices
- electricity meters

If quantum computers arrive while these systems still rely on vulnerable signatures, their signatures could eventually become forgeable.

Therefore:

> **Cryptographic migration has to happen before the threat becomes practical.**

---

# 31. Mosca's Rule of Thumb

The lecture introduces a useful planning model.

Let:

- `x` = how long information must remain secure/trusted
- `y` = how long migration takes
- `z` = estimated time until a cryptographically relevant quantum computer

Then:

```text
If x + y > z
       ↓
You are already late
```

### Example

Suppose:

```text
System must remain trusted = 15 years
Migration takes             = 5 years
Possible quantum threat     = 10–15 years
```

Then:

```text
15 + 5 = 20 years
```

If the threat arrives within 10–15 years, the organisation has a migration problem today.

---

# 32. Quantum Digital Signatures vs Post-Quantum Signatures

These terms are often confused.

## Quantum digital signatures

These use quantum physics directly.

They may involve:

- photons
- special optical links
- specialised hardware

They remain experimental and are not the practical replacement for ordinary software signatures.

---

## Post-quantum digital signatures

Post-quantum cryptography, or PQC:

- runs on classical computers
- runs on today's phones and servers
- does not require quantum hardware
- uses mathematical problems believed to resist known quantum attacks

ML-DSA is a **post-quantum digital signature algorithm**.

### Key distinction

```text
Quantum signature
    ↓
Uses quantum hardware/physics

Post-quantum signature
    ↓
Runs on classical hardware
    ↓
Designed to survive quantum attacks
```

---

# 33. NIST's Post-Quantum Cryptography Competition

NIST began a public worldwide competition in 2016 to identify quantum-resistant cryptographic algorithms.

The process involved:

```text
2016
↓
NIST opens competition

2017
↓
Many candidates submitted

2017–2022
↓
Public cryptanalysis
↓
Researchers attack the candidates

2022
↓
Winners/finalists selected

2024
↓
First major standards published
```

The open process is important because candidate algorithms were publicly attacked.

Some candidates were broken during the competition.

This provided an extended period of public cryptanalysis before standardisation.

---

# 34. Major Post-Quantum Signature Families

The lecture highlights three major families.

| Algorithm | Family | Main characteristic | Role |
|---|---|---|---|
| **ML-DSA / FIPS 204** | Lattice-based | Fast and balanced | General-purpose |
| **FN-DSA / FIPS 206** | Lattice-based | Smaller signatures | Size-sensitive applications |
| **SLH-DSA / FIPS 205** | Hash-based | Conservative security basis | Backup / conservative choice |

Different mathematical families are valuable because they provide diversity.

If a weakness is discovered in one family, another family can provide an alternative.

---

# 35. Meet ML-DSA

## Full name

> **Module-Lattice-Based Digital Signature Algorithm**

ML-DSA was formerly known as:

> **CRYSTALS-Dilithium**

Therefore:

```text
ML-DSA ≈ Dilithium
```

The standard is:

> **NIST FIPS 204**

The lecture gives the publication date as:

> **13 August 2024**

---

# 36. Understanding the Name ML-DSA

Break the name into pieces:

```text
Module
   +
Lattice
   +
Digital Signature Algorithm
```

### Module

A structured mathematical construction that makes the lattice calculations practical and compact.

### Lattice

A structured grid of points in many dimensions.

### DSA

Digital Signature Algorithm.

---

# 37. What Is a Lattice?

A lattice can be visualised as a regular grid of points.

In 2D:

```text
•   •   •   •
  •   •   •
•   •   •   •
  •   •   •
```

The same concept can be extended into many dimensions.

ML-DSA works with extremely high-dimensional mathematical structures.

The lecture describes dimensions exceeding one thousand in its intuitive explanation.

---

# 38. The Lattice Hardness Intuition

Imagine a lattice with two descriptions.

### Good description

Short, nearly perpendicular vectors:

```text
↗
↑
```

The structure is relatively easy to understand.

### Bad description

Long, nearly parallel vectors:

```text
↗
↗
```

The same underlying lattice can become difficult to navigate from the poor description.

This intuition helps explain why:

```text
Private key → useful secret structure

Public key → difficult-to-use description
```

A verifier can use the public information, but an attacker should not be able to efficiently recover the private structure.

---

# 39. SVP and CVP

The lecture introduces two important lattice problems.

## Shortest Vector Problem (SVP)

Find the shortest non-zero vector in a lattice.

```text
Find the shortest useful arrow
```

---

## Closest Vector Problem (CVP)

Given a point outside the lattice:

```text
       ★  target

•   •   •   •
  •   •   •
•   •   •   •
```

find the lattice point closest to the target.

In low dimensions this may be manageable.

In high dimensions, these problems become extremely difficult.

---

# 40. Why Lattices Are Considered Quantum-Resistant

RSA and ECC have known quantum algorithms that exploit their mathematical structure.

Shor's algorithm efficiently attacks:

- factoring
- discrete logarithms

For lattice problems used by ML-DSA, there is currently no known quantum algorithm that provides a comparable efficient attack.

The lecture explains this using the idea of **periodic structure**.

```text
Factoring / discrete logs
        ↓
Useful periodic structure
        ↓
Quantum algorithms exploit it

Lattice problems with noise
        ↓
No known equivalent exploitable structure
        ↓
No known efficient quantum attack
```

### Important caveat

This is not a mathematical proof that ML-DSA can never be broken.

Cryptographic confidence comes from extensive analysis and the absence of known efficient attacks.

---

# 41. Learning With Errors

One of the central ideas behind lattice cryptography is **Learning With Errors (LWE)** and related constructions.

Start with easy equations:

```text
2a + 3b = 21
4a + b  = 17
```

These can be solved.

Now introduce small errors:

```text
2a + 3b ≈ 22
4a + b  ≈ 16
5a + 2b ≈ 24
```

The equations are now noisy.

With many unknowns and many noisy equations, recovering the hidden values becomes computationally difficult.

Conceptually:

```text
Private key
   ↓
small secret values

Public key
   ↓
many noisy mathematical relationships
```

The public information does not straightforwardly reveal the secret.

---

# 42. Why Noise Matters

Without noise:

```text
Equations
    ↓
Solve algebraically
    ↓
Secret recovered
```

With carefully designed noise:

```text
Noisy equations
    ↓
Huge number of possible explanations
    ↓
Secret recovery becomes difficult
```

This noisy structure is one of the reasons lattice-based cryptography is considered suitable for the post-quantum era.

---

# 43. The "Module" in ML-DSA

A direct high-dimensional lattice representation could produce very large mathematical objects.

ML-DSA introduces structured modules to make the scheme practical.

The lecture uses a useful analogy:

> **Think of a huge wall built from repeating Lego bricks.**

Instead of storing the entire structure independently:

```text
Huge structure
    ↓
Repeated smaller components
    ↓
More compact representation
    ↓
Faster computation
```

The lecture describes each block as a structured bundle of 256 numbers and gives the following parameter structures:

```text
ML-DSA-44 → 4 × 4
ML-DSA-65 → 6 × 5
ML-DSA-87 → 8 × 7
```

These structured modules make the scheme practical.

---

# 44. Technical Note: Polynomial Arithmetic

For a more technical understanding, the lecture notes that the underlying "bricks" can be viewed as polynomials with 256 coefficients.

The arithmetic uses a modulus given in the lecture as:

```text
q = 8,380,417
```

Efficient polynomial multiplication can use the:

> **Number Theoretic Transform (NTT)**

The NTT is conceptually related to the Fast Fourier Transform used in signal and image processing, although the mathematical setting is different.

---

# 45. How ML-DSA Signs a Document

The lecture presents the signing process as four conceptual steps.

```text
1. Commit
      ↓
2. Challenge
      ↓
3. Respond
      ↓
4. Check or retry
```

This construction is associated with:

> **Fiat-Shamir with aborts**

---

# 46. Step 1: Commit

The signer chooses fresh random masking information.

Conceptually:

```text
Private secret
      +
Fresh random mask
      ↓
Commitment
```

Think of it as putting a guess into a sealed envelope.

The commitment is created before the challenge is known.

---

# 47. Step 2: Challenge

The challenge is derived from the document and commitment through hashing.

Conceptually:

```text
Document
    +
Commitment
    ↓
Hash
    ↓
Challenge
```

The hash makes the challenge deterministic from these inputs while preventing the signer from conveniently choosing a favourable challenge.

---

# 48. Step 3: Respond

The signer combines:

- the random mask
- the challenge
- the private secret

to construct a response.

Conceptually:

```text
Mask + Challenge + Secret
            ↓
         Response
```

The masking process prevents the signature from exposing the secret key.

---

# 49. Step 4: Check or Retry

This is one of the important ideas in ML-DSA.

Sometimes a generated response could leak information about the secret.

Instead of publishing such a response:

```text
Potential leakage
       ↓
Discard attempt
       ↓
Generate a new mask
       ↓
Try again
```

The final signature contains the appropriate challenge and response.

The lecture describes this as:

> **Fiat-Shamir with aborts**

The idea is similar to a magician reshuffling cards if an audience member might have seen the selected card.

---

# 50. How ML-DSA Verification Works

Verification does not require the private key.

The verifier has:

```text
Document
Signature
Public key
```

The verifier reconstructs the relevant commitment information from the public key and signature.

Then it checks conditions including:

### Check 1: Response values are acceptable

The response must satisfy the required bounds.

### Check 2: Hash challenge matches

The verifier recomputes the challenge from the document and reconstructed commitment.

Conceptually:

```text
Document
   +
Reconstructed commitment
   ↓
Hash
   ↓
Expected challenge
```

The expected challenge must match the challenge contained in the signature.

---

# 51. Verification Summary

```text
             Document
                │
                │
Signature ──────┼────── Public Key
                │
                ↓
        Rebuild commitment
                │
                ↓
      Check response bounds
                │
                ↓
       Recompute challenge
                │
                ↓
        Challenge matches?
          /            \
        YES             NO
         ↓               ↓
      VALID           INVALID
```

A successful forgery would require solving the underlying difficult mathematical problem.

---

# 52. ML-DSA Security Levels

ML-DSA comes in three parameter sets:

```text
ML-DSA-44
ML-DSA-65
ML-DSA-87
```

The lecture uses an intuitive comparison:

| Variant | Intuitive strength | Typical role in lecture |
|---|---|---|
| **ML-DSA-44** | Around AES-128 level | Everyday/high-volume applications |
| **ML-DSA-65** | Around AES-192 level | General-purpose choice |
| **ML-DSA-87** | Around AES-256 level | Highly sensitive / long-lived systems |

The numbers also correspond to the module dimensions:

```text
44 → 4 × 4
65 → 6 × 5
87 → 8 × 7
```

---

# 53. ML-DSA Key and Signature Sizes

One major trade-off is size.

The lecture compares:

| Scheme | Public Key | Signature |
|---|---:|---:|
| ECDSA P-256 | 64 bytes | 64 bytes |
| RSA-2048 | 256 bytes | 256 bytes |
| ML-DSA-44 | 1,312 bytes | 2,420 bytes |
| ML-DSA-65 | 1,952 bytes | 3,309 bytes |
| ML-DSA-87 | 2,592 bytes | 4,627 bytes |

Therefore, post-quantum signatures are substantially larger than traditional elliptic-curve signatures.

---

# 54. The Main ML-DSA Trade-Off

The lecture's central performance message is:

> **The main cost of ML-DSA is size rather than speed.**

ML-DSA provides:

- fast signing
- fast verification
- post-quantum security assumptions

But:

- public keys are larger
- signatures are larger
- storage requirements increase
- network bandwidth can increase

This matters especially for:

- tiny devices
- smart cards
- constrained networks
- systems producing millions of signatures

For ordinary web traffic, a few additional kilobytes may be manageable.

---

# 55. Strengths of ML-DSA

According to the lecture, important strengths include:

### 1. Post-quantum security

It is designed to resist known quantum attacks.

### 2. Fast signing and verification

Performance is practical for many modern systems.

### 3. Public cryptanalysis

The design survived a long public competition.

### 4. Official standard

ML-DSA is standardized as:

> **NIST FIPS 204**

### 5. Hedged randomness

The lecture highlights a "hedged" approach that combines fresh randomness with information derived from the key and message.

This reduces dependence on a perfect random-number generator.

---

# 56. What to Watch With ML-DSA

ML-DSA is not free of engineering challenges.

### Larger keys and signatures

Compared with ECDSA and RSA, the sizes are larger.

### Side-channel risks

Implementations must avoid leaking secrets through:

- timing
- power consumption
- other observable implementation behaviour

### Younger mathematical ecosystem

Lattice cryptography has not been studied for as long as classical factoring-based cryptography.

Therefore, continued research and cryptanalysis remain important.

---

# 57. Side-Channel Attacks

Even a mathematically secure algorithm can be implemented insecurely.

Imagine an attacker measuring:

```text
How long does signing take?
How much power does the device consume?
What physical patterns occur during computation?
```

If these measurements depend on secret values, the attacker may infer information about the private key.

Therefore:

> **Cryptographic security depends on both the algorithm and the implementation.**

Secure implementations aim to minimise secret-dependent observable behaviour.

---

# 58. Hybrid Signatures

During migration, organisations may not immediately abandon classical cryptography.

A transition strategy is to use both:

```text
Classical signature
        +
Post-quantum signature
        ↓
Hybrid signature
```

The lecture explains the intuition:

> An attacker would need to break both components to defeat the hybrid construction, assuming the hybrid is correctly designed and implemented.

Hybrid approaches can provide a transition path while organisations gain confidence in post-quantum algorithms and update infrastructure.

---

# 59. Why Different PQC Families Matter

Post-quantum cryptography does not rely on only one mathematical family.

Examples:

```text
Lattice-based
    ↓
ML-DSA
FN-DSA

Hash-based
    ↓
SLH-DSA
```

The purpose of having different families is mathematical diversity.

If an unexpected weakness appears in one family, an alternative family may remain available.

---

# 60. What Individuals Should Do

For individuals, the immediate changes are mostly operational.

### Keep devices updated

Quantum-safe algorithms can arrive through:

- operating-system updates
- browser updates
- application updates
- cryptographic library updates

### Protect signing credentials

Treat your:

- DSC token
- PIN
- private keys

as highly sensitive credentials.

### Read before signing

A valid signature does not prove that the content is truthful or beneficial.

### Take certificate warnings seriously

Certificate warnings can indicate problems in the trust chain.

---

# 61. What Organisations Should Do

Organisations have a much larger migration problem.

## Step 1: Inventory

Find every place where the organisation uses:

- digital signatures
- certificates
- public-key cryptography
- long-lived signing keys

---

## Step 2: Identify long-lived systems

Prioritise systems where signatures must remain trustworthy for many years.

Examples:

- root certificates
- government records
- firmware
- vehicles
- satellites
- medical devices
- industrial systems

---

## Step 3: Ask vendors about PQC

Organisations should understand:

- whether products support PQC
- whether ML-DSA is supported
- whether hybrid modes are available
- how upgrades will be delivered

---

## Step 4: Build crypto-agility

### Crypto-agility

Crypto-agility means designing systems so that cryptographic algorithms can be replaced without rebuilding the entire application.

Conceptually:

```text
Application
     │
Crypto interface
     │
 ┌───┴───────────────┐
 ↓                   ↓
Classical         PQC
algorithm        algorithm
```

The goal is to avoid hard-coding a single cryptographic algorithm everywhere.

---

# 62. The Migration Problem Is Bigger Than Replacing an Algorithm

Post-quantum migration is not simply:

```text
RSA → ML-DSA
```

Real systems contain:

- certificates
- private keys
- firmware
- libraries
- protocols
- hardware
- third-party services
- embedded devices
- old systems

Therefore migration requires:

```text
Inventory
   ↓
Classify
   ↓
Prioritise
   ↓
Test
   ↓
Hybridise / migrate
   ↓
Monitor
```

---

# 63. A Practical PQC Migration Lifecycle

## Discover

Find all cryptographic dependencies.

## Classify

Determine:

- algorithm
- key type
- purpose
- system owner
- data lifetime
- hardware constraints

## Prioritise

Move first on systems with:

- long-lived data
- long-lived signatures
- difficult replacement cycles
- high impact

## Test

Evaluate:

- performance
- signature size
- compatibility
- bandwidth
- storage
- hardware support

## Hybridise

Use classical + PQC signatures where appropriate during transition.

## Migrate and monitor

Deploy new cryptography and continue watching for:

- new attacks
- implementation vulnerabilities
- standards updates
- performance issues

---

# 64. Why Long-Lived Signatures Are Especially Important

Encryption has a well-known "harvest now, decrypt later" problem.

Digital signatures have a related long-term issue:

> **Trust now, forge later.**

If a document must remain trustworthy for decades, an attacker who obtains a quantum computer later could potentially forge signatures based on vulnerable classical public keys.

This matters for:

- land records
- wills
- long-term contracts
- firmware
- certificates
- medical devices
- vehicles
- satellites

Therefore, post-quantum migration should consider the entire lifetime of a system, not merely today's threat level.

---

# 65. Classical vs Post-Quantum Signatures

| Feature | Classical signatures | Post-quantum signatures |
|---|---|---|
| Examples | RSA, ECDSA | ML-DSA, SLH-DSA, FN-DSA |
| Runs on classical computers | Yes | Yes |
| Vulnerable to known Shor attack | RSA/ECC: Yes | Designed to resist known quantum attacks |
| Key/signature size | Usually smaller | Usually larger |
| Main mathematical basis | Factoring / discrete logs | Lattices / hashes |
| Current role | Existing infrastructure | Migration target |
| Quantum hardware required | No | No |

---

# 66. ML-DSA vs RSA/ECDSA

| Property | RSA | ECDSA | ML-DSA |
|---|---|---|---|
| Security basis | Factoring | Elliptic-curve discrete log | Lattice problems |
| Shor threat | Yes | Yes | No known equivalent efficient attack |
| Signature size | Larger than ECC | Very small | Larger |
| Quantum-safe | No | No | Designed for PQ era |
| Standard discussed | Existing standards | Existing standards | FIPS 204 |
| Main migration issue | Quantum vulnerability | Quantum vulnerability | Larger data sizes and implementation care |

---

# 67. Important Terminology

## Digital Signature

A cryptographic mechanism used to establish authenticity, integrity, and non-repudiation.

## Private Key

Secret key used to create signatures.

## Public Key

Public key used to verify signatures.

## Hash

Fixed-length cryptographic fingerprint of data.

## Certificate

A signed binding between an identity and a public key.

## Certificate Authority

Trusted organisation that issues certificates.

## Quantum Computer

A computer using quantum phenomena such as superposition, entanglement, and interference.

## Shor's Algorithm

Quantum algorithm that threatens factoring and discrete-log-based cryptography.

## Post-Quantum Cryptography

Cryptography designed to remain secure against attacks from sufficiently powerful quantum computers.

## Lattice

A structured set of points in a mathematical space.

## ML-DSA

Module-Lattice-Based Digital Signature Algorithm.

## Dilithium

The former name of ML-DSA and the name under which it entered the NIST competition.

## FIPS 204

NIST standard defining ML-DSA.

## Fiat-Shamir with Aborts

A construction used in ML-DSA where potentially leaking signing attempts are discarded and regenerated.

## Crypto-Agility

The ability to change cryptographic algorithms without redesigning the whole system.

---

# 68. Common Misconceptions

## Misconception 1: A scanned signature is a digital signature

**False.**

A scanned signature is just an image.

---

## Misconception 2: A digital signature hides the document

**False.**

Digital signatures provide trust properties. Encryption provides confidentiality.

---

## Misconception 3: A valid signature means the content is true

**False.**

A valid signature indicates that the corresponding private key signed the content and that the signed content has not been altered.

It does not prove that the statement itself is true.

---

## Misconception 4: Quantum computers are simply faster computers

**False.**

They use different computational principles and provide advantages only for particular problems.

---

## Misconception 5: Post-quantum cryptography requires quantum hardware

**False.**

PQC algorithms such as ML-DSA run on ordinary classical computers.

---

## Misconception 6: ML-DSA is completely proven unbreakable

**False.**

Its security is based on mathematical problems for which no efficient classical or quantum attack is currently known.

Cryptographic security is based on evidence and analysis, not an absolute proof of impossibility.

---

## Misconception 7: The only problem with ML-DSA is speed

The lecture emphasises that ML-DSA is fast. Its major practical trade-off is larger keys and signatures.

---

# 69. The Entire ML-DSA Story in One Flow

```text
Traditional digital signatures
        │
        ↓
RSA / ECDSA
        │
        ↓
Depend on hard classical problems
        │
        ↓
Quantum computers
        │
        ↓
Shor's algorithm
        │
        ↓
Factoring / discrete logs become tractable
        │
        ↓
Traditional signatures become forgeable
        │
        ↓
Need post-quantum signatures
        │
        ↓
NIST competition
        │
        ↓
Dilithium selected / standardised
        │
        ↓
ML-DSA / FIPS 204
        │
        ↓
Lattice-based security
        │
        ↓
No known efficient quantum attack
        │
        ↓
Quantum-resistant digital signatures
```

---

# 70. Exam-Oriented Questions and Answers

## Q1. What is a digital signature?

A digital signature is a cryptographic mechanism that allows a verifier to establish the authenticity and integrity of a digitally signed message or document. It also supports non-repudiation.

---

## Q2. What are the three major properties of a digital signature?

1. **Authenticity**: establishes the claimed origin.
2. **Integrity**: detects modification.
3. **Non-repudiation**: supports evidence that the signer signed the content.

---

## Q3. What is the role of the private key?

The private key is kept secret and is used to generate digital signatures.

---

## Q4. What is the role of the public key?

The public key is distributed to others and is used to verify signatures.

---

## Q5. Why is hashing used in digital signatures?

Hashing converts a large document into a fixed-size fingerprint. The signature can then be associated with this fingerprint, making signing efficient and ensuring that document modifications are detected.

---

## Q6. What is the difference between a scanned signature and a digital signature?

A scanned signature is an image that can be copied and reused. A digital signature is a cryptographic value mathematically bound to a private key and the exact document contents.

---

## Q7. What is a Certificate Authority?

A Certificate Authority is a trusted entity that verifies identities and issues certificates binding identities to public keys.

---

## Q8. Why are RSA and ECDSA threatened by quantum computing?

RSA relies on integer factoring, while ECDSA relies on elliptic-curve discrete logarithms. Shor's algorithm can efficiently solve these classes of problems on a sufficiently powerful quantum computer.

---

## Q9. What is Shor's algorithm?

Shor's algorithm is a quantum algorithm that can efficiently solve integer factoring and discrete logarithm problems, threatening RSA and elliptic-curve cryptography.

---

## Q10. What is post-quantum cryptography?

Post-quantum cryptography is cryptography designed to remain secure against attacks by sufficiently powerful quantum computers while running on classical computing hardware.

---

## Q11. What is ML-DSA?

ML-DSA stands for **Module-Lattice-Based Digital Signature Algorithm**. It is a lattice-based post-quantum digital signature algorithm standardised by NIST as **FIPS 204**. It was formerly known as CRYSTALS-Dilithium.

---

## Q12. Why is ML-DSA considered quantum-resistant?

ML-DSA relies on difficult lattice-based problems involving noisy mathematical structures for which no efficient classical or quantum attack comparable to Shor's algorithm is currently known.

---

## Q13. What is Learning With Errors?

Learning With Errors is a cryptographic hardness concept where an attacker receives many mathematical relationships containing small errors and must recover hidden secret information.

The noise makes solving the system computationally difficult.

---

## Q14. What is a lattice?

A lattice is a structured set of regularly arranged points in a mathematical space. In lattice cryptography, high-dimensional lattice problems provide the security foundation.

---

## Q15. What are SVP and CVP?

- **SVP**: Shortest Vector Problem, finding a shortest non-zero lattice vector.
- **CVP**: Closest Vector Problem, finding the lattice point closest to a target point.

Both become difficult in high-dimensional lattices.

---

## Q16. What is the role of the module in ML-DSA?

The module provides a structured way to construct large lattice systems from smaller repeating mathematical components, reducing key sizes and improving computational efficiency.

---

## Q17. What is Fiat-Shamir with aborts?

It is a signing construction where the signer commits, derives a challenge using hashing, produces a response, and discards attempts that could leak information about the secret.

---

## Q18. What are the ML-DSA parameter sets?

The lecture presents:

- ML-DSA-44
- ML-DSA-65
- ML-DSA-87

They represent increasing security levels and corresponding module dimensions.

---

## Q19. What is the main practical disadvantage of ML-DSA?

ML-DSA keys and signatures are considerably larger than traditional schemes such as ECDSA.

---

## Q20. What is crypto-agility?

Crypto-agility is the ability to replace or update cryptographic algorithms without redesigning the entire system.

---

# 71. Short Revision Table

| Topic | Remember |
|---|---|
| Digital signature | Authenticity + integrity + non-repudiation |
| Encryption | Confidentiality |
| Private key | Signs |
| Public key | Verifies |
| Hash | Digital fingerprint |
| CA | Vouches for public-key identity |
| RSA | Factoring |
| ECDSA | Elliptic-curve discrete logarithm |
| Quantum threat | Shor's algorithm |
| PQC | Quantum-resistant classical cryptography |
| ML-DSA | Module-Lattice-Based Digital Signature Algorithm |
| Dilithium | Former name |
| Standard | NIST FIPS 204 |
| Security basis | Lattice problems |
| LWE | Learning With Errors |
| SVP | Shortest Vector Problem |
| CVP | Closest Vector Problem |
| Signing construction | Fiat-Shamir with aborts |
| Variants | 44, 65, 87 |
| Main trade-off | Larger keys/signatures |
| Migration strategy | Inventory → test → hybridise → migrate |
| Architecture goal | Crypto-agility |

---

# 72. Final Five Things to Remember

### 1. Digital signatures establish trust

They provide authenticity and integrity and support non-repudiation.

### 2. Two keys make it possible

```text
Private key → Sign
Public key  → Verify
```

### 3. Real-world failures are often engineering failures

Keys can be stolen, randomness can fail, trust authorities can be compromised, and people can misuse signing credentials.

### 4. Quantum computing changes the mathematical threat

Shor's algorithm threatens the factoring and discrete-log assumptions behind RSA and elliptic-curve signatures.

### 5. ML-DSA is a major post-quantum replacement

ML-DSA, formerly CRYSTALS-Dilithium and standardised as FIPS 204, uses lattice-based mathematics and is designed for a world where powerful quantum computers exist.

---

# Final Mental Model

If you remember only one story, remember this:

```text
HANDWRITTEN SIGNATURE
        ↓
“I signed this.”

DIGITAL SIGNATURE
        ↓
“I signed this exact digital content,
and you can verify it without knowing my secret.”

RSA / ECDSA
        ↓
Secure because certain mathematical problems
are hard for classical computers.

QUANTUM COMPUTER
        ↓
Shor's algorithm
        ↓
Those mathematical problems become vulnerable.

POST-QUANTUM CRYPTOGRAPHY
        ↓
Use different mathematical foundations.

ML-DSA / DILITHIUM
        ↓
Module-lattice-based signatures
        ↓
FIPS 204
        ↓
Designed for quantum-resistant digital trust.
```

> **The important lesson is not merely that Dilithium is a new algorithm. The important lesson is that cryptographic systems have lifetimes, and the algorithms protecting those systems must be replaced before the mathematical assumptions underneath them stop being safe.**
