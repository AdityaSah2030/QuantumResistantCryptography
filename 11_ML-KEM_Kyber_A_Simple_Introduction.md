# 11 · ML-KEM (Kyber): A Simple Introduction

## Lecture Overview

**Topic:** Post-Quantum Cryptography and ML-KEM  
**Lecture title:** *Breaking the Unbreakable Lock*  
**Lecture focus:** Understanding why classical public-key cryptography is threatened by quantum computing and how ML-KEM provides a post-quantum key-establishment mechanism.

The lecture is intentionally visual and intuitive. Instead of beginning with the full mathematical specification of ML-KEM, it builds the idea through:

```text
RSA / ECC
   ↓
Quantum threat
   ↓
Shor's algorithm
   ↓
Need for Post-Quantum Cryptography
   ↓
NIST standardisation
   ↓
CRYSTALS-Kyber
   ↓
ML-KEM
   ↓
Lattice intuition
   ↓
Key Generation
   ↓
Encapsulation
   ↓
Decapsulation
   ↓
Shared secret
```

The central idea is:

> **ML-KEM replaces the vulnerable public-key key-exchange step with a lattice-based mechanism designed to remain secure against known quantum attacks.**

---

# 1. The Big Picture

Today, much of Internet security depends on public-key cryptography.

The lecture highlights technologies such as:

- banking applications,
- UPI,
- WhatsApp,
- Wi-Fi,
- HTTPS.

The security of many current systems depends on mathematical problems that are difficult for classical computers.

For example:

```text
RSA
  ↓
Integer factorisation

ECC
  ↓
Elliptic-curve discrete logarithm
```

The important assumption is:

> These problems are computationally difficult enough that an attacker cannot solve them within a useful amount of time.

---

# 2. Why Quantum Computing Changes the Situation

A sufficiently powerful quantum computer could run:

> **Shor's algorithm**

Shor's algorithm provides an efficient quantum approach to mathematical problems underlying important public-key systems.

The lecture explains the threat as:

```text
Classical computer
       ↓
Factoring huge numbers
       ↓
Extremely difficult

Quantum computer
       ↓
Shor's algorithm
       ↓
Same mathematical problem
       ↓
Potentially solvable efficiently
```

This does not mean that a quantum computer can instantly break everything.

The important point is:

> **A sufficiently powerful quantum computer could make currently hard public-key problems practical to solve.**

---

# 3. The "Harvest Now, Decrypt Later" Threat

One of the most important reasons migration needs to begin before large-scale quantum computers exist is:

> **Harvest now, decrypt later.**

An attacker can:

```text
Today
  ↓
Record encrypted traffic
  ↓
Store it
  ↓
Wait for a sufficiently powerful quantum computer
  ↓
Break the vulnerable public-key protection
  ↓
Decrypt previously captured information
```

Therefore, information that needs to remain confidential for many years can already be at risk.

The lecture emphasises that no quantum computer capable of performing this large-scale attack has been built yet, but migration itself can take many years. fileciteturn13file0L37-L56

---

# 4. Toy Example: Breaking N = 91

Before explaining ML-KEM, the lecture gives an intuitive example of Shor's algorithm.

Consider:

```text
N = 91
```

The hidden factorisation is:

```text
91 = 7 × 13
```

The goal is to recover `7` and `13` from the public value `91`.

This example is intentionally tiny. Any ordinary laptop can factor 91 instantly.

The point is not the size.

The point is to understand the **mechanism** of Shor's algorithm.

---

# 5. Step 1: Pick a Number

Choose:

```text
a = 3
```

The selected value must be smaller than `91` and share no common factor with it.

In mathematical terms:

```text
gcd(a, N) = 1
```

For the example:

```text
gcd(3, 91) = 1
```

---

# 6. Step 2: Find the Period

Calculate powers of `3` modulo `91`:

```text
3¹ mod 91 = 3
3² mod 91 = 9
3³ mod 91 = 27
3⁴ mod 91 = 81
3⁵ mod 91 = 61
3⁶ mod 91 = 1
```

The sequence then repeats.

Therefore:

```text
r = 6
```

where `r` is the period.

The lecture identifies finding this period as:

> **The one genuinely quantum step.**

---

# 7. Step 3: Do Simple Mathematics

Since:

```text
r = 6
```

we have:

```text
r / 2 = 3
```

Now compute:

```text
gcd(3³ - 1, 91)
```

which becomes:

```text
gcd(27 - 1, 91)
= gcd(26, 91)
= 13
```

Similarly:

```text
gcd(3³ + 1, 91)
```

gives:

```text
gcd(28, 91)
= 7
```

Therefore:

```text
91 = 7 × 13
```

The hidden factors have been recovered.

---

# 8. Why the Toy Example Matters

The lecture explicitly warns that `91` is only a toy example.

The important concept is:

```text
Finding the period
       ↓
Quantum advantage
       ↓
Recover factors
```

For cryptographic numbers with hundreds of digits, the same conceptual mechanism is what creates the threat to factoring-based public-key cryptography. fileciteturn13file0L8-L35

---

# 9. Why RSA and ECC Need Replacement

The lecture describes the current situation as:

```text
Today's lock
     ↓
RSA + ECC
     ↓
Protect banking, UPI, messaging, Wi-Fi, HTTPS
```

Their security depends on hard mathematical problems.

The quantum threat is:

```text
Quantum computer
       ↓
Shor's algorithm
       ↓
Efficient solution of relevant problems
       ↓
RSA / ECC security assumptions fail
```

Therefore the cryptographic community needs algorithms based on different mathematical problems.

This is the motivation for:

> **Post-Quantum Cryptography (PQC)**

---

# 10. What Is Post-Quantum Cryptography?

Post-Quantum Cryptography refers to cryptographic algorithms designed to remain secure against attackers equipped with sufficiently powerful quantum computers.

An important point:

> **PQC does not require a quantum computer.**

It is designed to run on conventional computing systems.

The goal is to replace vulnerable classical public-key mechanisms before large-scale quantum attacks become practical.

---

# 11. NIST's Post-Quantum Cryptography Competition

The lecture describes NIST's standardisation process as a large cryptographic competition.

### 2016: Call for Proposals

NIST asked cryptographers to submit quantum-resistant algorithms.

### 2017: Submissions

The competition received:

```text
82 submissions
```

from teams in more than:

```text
25 countries
```

The candidates then underwent years of:

- public analysis,
- cryptanalysis,
- review,
- comparison.

### 2022: Selection

The lecture states that:

> **CRYSTALS-Kyber** was selected for encryption/key exchange.

### 2024: Standardisation

NIST published:

> **FIPS 203**

and the algorithm became:

> **ML-KEM**

The lecture therefore presents:

```text
CRYSTALS-Kyber
       ↓
NIST selection
       ↓
FIPS 203
       ↓
ML-KEM
```

The timeline is shown visually on page 4 of the lecture. fileciteturn13file0L59-L87

---

# 12. What Does ML-KEM Stand For?

ML-KEM stands for:

> **Module-Lattice-based Key-Encapsulation Mechanism**

Break the name into parts:

```text
ML
↓
Module-Lattice-based

KEM
↓
Key Encapsulation Mechanism
```

The lecture explains ML-KEM as the mechanism that replaces the **key-exchange step** of vulnerable public-key systems.

That step allows two parties who initially share no secret to establish a common secret over an open channel. fileciteturn13file0L81-L86

---

# 13. What Is a KEM?

KEM means:

> **Key Encapsulation Mechanism**

A KEM is used to establish a shared secret between parties.

Instead of thinking about it as directly encrypting a long message, think of it as:

```text
Generate a shared secret
        ↓
Use that secret with symmetric encryption
        ↓
Protect actual application data
```

A simplified system is:

```text
Public-key / KEM layer
        ↓
Shared secret
        ↓
Symmetric encryption
        ↓
Large amounts of application data
```

---

# 14. ML-KEM as a Lockbox

The lecture uses a very useful analogy.

Imagine a special lockbox.

The goal is:

```text
Sender and receiver
        ↓
Both end up with the same secret
        ↓
Eavesdropper sees the exchange
        ↓
Eavesdropper cannot recover the secret
```

The three major operations are:

```text
1. Key Generation
2. Encapsulation
3. Decapsulation
```

These are the foundation of understanding ML-KEM. fileciteturn13file0L89-L99

---

# 15. ML-KEM Key Generation

The first operation is:

> **Key Generation**

The receiver generates two related keys:

```text
Public Key
Private Key
```

The analogy from the lecture is:

```text
Public key
≈ UPI ID
```

It can be shared.

Whereas:

```text
Private key
≈ PIN
```

It must never be shared.

The simplified flow is:

```text
Key Generation
      ↓
Public Key + Private Key
      ↓
Public key → share freely
Private key → keep secret
```

---

# 16. ML-KEM Encapsulation

Suppose Alice wants to establish a shared secret with you.

She obtains your public key.

She then:

1. Generates a random secret.
2. Uses your public key to encapsulate that secret.
3. Produces a ciphertext-like object called a **ciphertext/capsule**.
4. Sends the capsule to you.

Conceptually:

```text
Your Public Key
      +
Random Secret
      ↓
Encapsulation
      ↓
Ciphertext / Capsule
      ↓
Send over public channel
```

Anyone may observe the capsule.

The important property is:

> Only the holder of the corresponding private key should be able to recover the same secret.

---

# 17. ML-KEM Decapsulation

The receiver uses the private key to process the capsule.

```text
Received Capsule
       +
Private Key
       ↓
Decapsulation
       ↓
Recovered Secret
```

The resulting secret should match the secret obtained by the sender.

Therefore:

```text
Sender
Shared Secret = K

Receiver
Shared Secret = K
```

An eavesdropper sees the public information but should not be able to recover `K`.

---

# 18. Complete ML-KEM Flow

The most important diagram to remember is:

```text
                    KEY GENERATION
                         │
                         ↓
                ┌──────────────────┐
                │ Public Key       │
                │ Private Key      │
                └──────────────────┘
                         │
                  Public key shared
                         │
                         ↓
Alice                                      Receiver
  │                                            │
  │ random secret                              │
  │                                            │
  │ Encapsulation using public key             │
  │                                            │
  │────── Capsule / Ciphertext ───────────────>│
  │                                            │
  │                                      Decapsulation
  │                                      using private key
  │                                            │
  │<──────── Same shared secret ──────────────>│
```

The capsule travels over the public channel.

The true shared secret does not.

---

# 19. The Secret Sauce: Lattices

ML-KEM is based on lattice mathematics.

The lecture introduces the intuition using a two-dimensional picture.

Imagine a huge grid of points:

```text
•   •   •   •   •
  •   •   •   •
•   •   •   •   •
  •   •   •   •
•   •   •   •   •
```

A real cryptographic lattice is not merely 2-dimensional.

The lecture describes the actual problem as involving:

> **Hundreds of dimensions.**

This high-dimensional structure is what makes the relevant computational problem difficult. fileciteturn13file0L101-L115

---

# 20. The Basic Lattice Intuition

Imagine that the secret is related to a point that is:

> **Close to, but not exactly on, the lattice.**

The receiver has special information that allows the correct point to be recovered.

An attacker sees the public structure but does not have the same easy route.

The simplified idea is:

```text
High-dimensional lattice
        ↓
Secret-related noisy point
        ↓
Private information
        ↓
Correct point recovered
```

---

# 21. Why Shor's Algorithm Does Not Directly Apply

This is one of the most important concepts in the lecture.

Shor's algorithm exploits particular mathematical structure associated with:

- factoring,
- discrete logarithms,
- related algebraic structures.

RSA and ECC have exactly the kind of structure Shor exploits.

Lattice problems used by ML-KEM do not have the same structure.

Therefore:

```text
Shor
 ↓
Factoring / discrete logs
 ↓
RSA / ECC
```

but:

```text
Shor
 ↓
No known equivalent shortcut
 ↓
Lattice problems used by ML-KEM
```

The lecture states that no quantum shortcut is currently known for the relevant lattice problem. fileciteturn13file0L110-L114

---

# 22. Basis A and Basis B

The lecture introduces a particularly useful way of understanding ML-KEM.

Imagine the same lattice described using two different bases:

```text
Basis A
↓
Private
↓
Easy map

Basis B
↓
Public
↓
Tangled map
```

These are not two different lattices.

They are:

> **Two descriptions of the same lattice.**

This distinction is extremely important.

---

# 23. The Lattice Is Not Secret

A common misconception is:

> "The lattice itself must be hidden."

The lecture explicitly says this is not the case.

The same lattice can have:

```text
Basis A → private, easy-to-use representation
Basis B → public, tangled representation
```

The security comes from possessing the easy private representation, not from hiding the lattice itself.

---

# 24. ML-KEM in Basis Language

The three operations become intuitive.

### Key Generation

```text
Generate Basis A
       ↓
Derive Basis B
       ↓
Basis A = private
Basis B = public
```

### Encapsulation

```text
Use public Basis B
       ↓
Choose a true lattice point
       ↓
Add a small amount of noise
       ↓
Send noisy point
```

### Decapsulation

```text
Receive noisy point
       ↓
Use private Basis A
       ↓
Round / recover correct lattice point
       ↓
Recover shared secret
```

The lecture presents exactly this three-step interpretation. fileciteturn13file0L117-L128

---

# 25. Toy Example: Rounding to the Nearest Point

The lecture uses:

```text
p = (5.4, 3.2)
```

as the public noisy point.

The true point is:

```text
(5, 3)
```

The objective is to recover that original point.

---

# 26. Basis A: Private Easy Map

The private basis is:

```text
(1,0)
(0,1)
```

This is an easy coordinate system.

Simply round each coordinate:

```text
5.4 → 5
3.2 → 3
```

Therefore:

```text
p = (5.4, 3.2)
        ↓
     (5, 3)
```

The correct point is recovered.

---

# 27. Why Basis A Makes the Problem Easy

With:

```text
(1,0)
(0,1)
```

the coordinates are independent.

A small rounding error remains small.

The lecture describes this as:

> **Independent steps ⇒ a small rounding slip stays small.**

Therefore, the holder of the private basis has an easy way to decode the noisy point.

---

# 28. Basis B: Public Tangled Map

Now consider the public basis:

```text
(1,0)
(17,1)
```

This represents the same lattice but in a skewed way.

The same point:

```text
p = (5.4, 3.2)
```

must now be interpreted through the tangled coordinate system.

---

# 29. Rounding with Basis B

First:

```text
up-steps:
3.2 → 3
```

Then the right-step calculation is:

```text
5.4 - 17(3.2)
```

which gives:

```text
5.4 - 54.4
= -49.0
```

Round to:

```text
-49
```

Now reconstruct:

```text
-49(1,0) + 3(17,1)
```

which gives:

```text
(-49,0) + (51,3)
= (2,3)
```

Therefore the public basis gives:

```text
(2,3)
```

instead of:

```text
(5,3)
```

The lecture uses this to show how a small coordinate error can be amplified by a skewed basis. fileciteturn13file0L131-L149

---

# 30. The Security Intuition

The example gives:

```text
Correct result:
(5,3)

Public-basis result:
(2,3)
```

The important idea is not the exact toy arithmetic.

It is:

```text
Private easy basis
       ↓
Correctly recover hidden point

Public tangled basis
       ↓
Small uncertainty
       ↓
Large coordinate error
       ↓
Wrong point
```

In real cryptography, the corresponding dimensions and mathematics are vastly more complex.

---

# 31. Same Public Point, Two Outcomes

The lecture gives a step-by-step comparison.

| Step | Actor | Uses | Point | Result |
|---|---|---|---|---|
| 0. Key Generation | Receiver | Basis A | - | Basis A private, Basis B public |
| 1. Encapsulation | Alice | Basis B | `(5.4, 3.2)` | Noisy point sent publicly |
| 2. Decapsulation | Receiver | Basis A | `(5.4, 3.2)` | `(5,3)` correct |
| 3. Eavesdrop | Attacker | Basis B | `(5.4, 3.2)` | `(2,3)` wrong |

The entire security intuition can therefore be summarised as:

> **The attacker sees the same public noisy point but lacks the private easy map needed to recover the intended point.** fileciteturn13file0L151-L160

---

# 32. What Actually Travels Over the Network?

Another important point:

> **The true point is never transmitted directly.**

Only the noisy point is sent.

In the toy example:

```text
True point:
(5,3)

Publicly transmitted:
(5.4,3.2)
```

The receiver independently reconstructs:

```text
(5,3)
```

using the private basis.

This is essential.

Otherwise, an eavesdropper could simply read the secret from the network.

---

# 33. Full ML-KEM Story

The lecture presents the entire story in this order.

## Step 0: Key Generation

The receiver:

```text
Generates Basis A
        ↓
Derives Basis B
        ↓
Keeps A private
        ↓
Publishes B
```

---

## Step 1: Encapsulation

Alice:

```text
Uses Basis B
      ↓
Chooses true point
      ↓
Adds noise
      ↓
Sends noisy point
```

---

## Step 2: Decapsulation

The receiver:

```text
Receives noisy point
        ↓
Uses Basis A
        ↓
Rounds / decodes
        ↓
Recovers original point
        ↓
Derives shared secret
```

The lecture emphasises that the noisy point is the only information that travels publicly. fileciteturn13file0L164-L187

---

# 34. Why Noise Is Important

The noise is not an implementation mistake.

It is part of the security mechanism.

The simplified idea is:

```text
True point
    +
Small noise
    ↓
Public noisy point
```

The legitimate receiver has enough information to remove or tolerate the noise.

The attacker does not have the private structure needed to do so reliably.

Therefore:

> **Noise helps create the computational hardness that protects the secret.**

---

# 35. ML-KEM Security Intuition

The security story can be reduced to:

```text
Public information
        ↓
Tangled lattice representation
        +
Noisy information
        ↓
Hard problem
        ↓
Attacker cannot efficiently recover secret
```

while:

```text
Private information
        ↓
Easy lattice representation
        +
Noisy information
        ↓
Efficient recovery
        ↓
Shared secret
```

---

# 36. The Three ML-KEM Security Components

For conceptual understanding, remember:

### 1. High-dimensional lattice structure

The real construction operates in a very large-dimensional mathematical space.

### 2. Public/private representations

The public representation is deliberately structured so that the private representation provides an efficient decoding route.

### 3. Noise

Noise makes the public information insufficient for straightforward recovery.

Together:

```text
Lattice
+
Tangled public representation
+
Noise
        ↓
Post-quantum security foundation
```

---

# 37. ML-KEM Parameter Sets

The lecture introduces three versions:

```text
ML-KEM-512
ML-KEM-768
ML-KEM-1024
```

The lecture gives approximate symmetric-security comparisons:

| ML-KEM variant | Approximate comparison in lecture | Practical framing |
|---|---|---|
| ML-KEM-512 | ≈ AES-128 | Rarely used in practice |
| ML-KEM-768 | ≈ AES-192 | Default deployment discussed in lecture |
| ML-KEM-1024 | ≈ AES-256 | Higher-security / sensitive systems |

The lecture specifically identifies ML-KEM-768 as the default it describes for deployments such as Chrome, Signal and Cloudflare. fileciteturn13file0L190-L207

---

# 38. ML-KEM-512

The lecture associates:

```text
ML-KEM-512
≈ AES-128
```

and describes it as:

> **Rarely used in practice**

The lower parameter set provides lower security compared with the larger variants.

---

# 39. ML-KEM-768

The lecture associates:

```text
ML-KEM-768
≈ AES-192
```

and describes it as:

> **The default everyone ships**

The lecture gives examples including:

- Chrome,
- Signal,
- Cloudflare.

---

# 40. ML-KEM-1024

The lecture associates:

```text
ML-KEM-1024
≈ AES-256
```

and describes it as intended for:

> **Top-secret / high-security systems**

The important concept is:

```text
Higher parameter
       ↓
Higher security level
       ↓
Usually greater communication/storage requirements
```

---

# 41. Kyber vs ML-KEM

This is an extremely important terminology question.

During the NIST competition:

```text
2017–2022
```

the algorithm was called:

> **CRYSTALS-Kyber**

After standardisation:

```text
FIPS 203
```

NIST's standard name became:

> **ML-KEM**

Therefore:

```text
CRYSTALS-Kyber
        =
ML-KEM
```

in the context of the standardised algorithm.

So if a paper, tutorial or library refers to:

```text
Kyber
```

it is referring to the algorithm family that became ML-KEM.

The lecture explicitly highlights that these are two names for the same algorithmic lineage. fileciteturn13file0L203-L206

---

# 42. ML-KEM Is Not a Digital Signature

This is one of the most important distinctions.

ML-KEM handles:

> **Key establishment / encapsulation**

It does not provide the complete digital-signature function needed to prove:

> "This website, server or entity is really who it claims to be."

The lecture identifies:

> **ML-DSA**

as the sibling standard responsible for digital signatures.

Therefore:

```text
ML-KEM
   ↓
Key establishment

ML-DSA
   ↓
Digital signatures
```

A complete post-quantum system may need both. fileciteturn13file0L215-L219

---

# 43. ML-KEM vs ML-DSA

| Feature | ML-KEM | ML-DSA |
|---|---|---|
| Main purpose | Key establishment | Digital signatures |
| Primitive | KEM | Signature |
| Main question answered | "Can we establish a shared secret?" | "Can we verify who signed this?" |
| Private key | Used for decapsulation | Used for signing |
| Public key | Used for encapsulation | Used for verification |

The two mechanisms solve different problems.

---

# 44. ML-KEM and Symmetric Encryption

ML-KEM is generally not intended to replace AES as the bulk-data encryption algorithm.

Instead:

```text
ML-KEM
   ↓
Establish shared secret
   ↓
Symmetric key
   ↓
AES / other symmetric encryption
   ↓
Encrypt large application data
```

This is a key architectural distinction.

Public-key mechanisms establish secrets.

Symmetric mechanisms efficiently encrypt large quantities of data.

---

# 45. A Simple HTTPS-Style Mental Model

Imagine visiting a secure website.

Conceptually:

```text
Browser                         Server
   │                              │
   │ ML-KEM public key           │
   │<────────────────────────────│
   │                              │
   │ Encapsulation                │
   │─────────────────────────────>│
   │                              │
   │       Shared secret          │
   │<────────────────────────────>│
   │                              │
   │ AES / symmetric encryption   │
   │<────────────────────────────>│
```

The exact protocol implementation can be more complicated and may use hybrid mechanisms, but this captures the role of the KEM.

---

# 46. ML-KEM's Practical Performance

The lecture challenges a common misconception:

> **"Post-quantum cryptography must be slow."**

According to the lecture, ML-KEM's actual CPU operations can be cheaper than RSA's.

The major practical cost highlighted is:

> **Bandwidth**

because post-quantum keys and ciphertexts are larger.

So the simplified trade-off is:

```text
Computation:
ML-KEM → fast

Communication:
ML-KEM → larger data sizes
```

The lecture specifically identifies larger keys and ciphertexts as the important wire-level cost. fileciteturn13file0L209-L219

---

# 47. ML-KEM Failure Probability

The lecture mentions an extremely small probability that the two parties can derive different secrets because of the noise-based construction.

The stated figure is approximately:

```text
1 in 2^138
```

This is astronomically small.

The lecture explains that if such a failure occurs:

```text
Shared secrets differ
       ↓
Connection fails
       ↓
Retry
```

It is described as:

> **A side effect of the noise that makes the scheme secure, not a bug.**

---

# 48. Why a Failure Can Be Acceptable

Cryptographic protocols sometimes use constructions where an extremely small probability of failure is preferable to making the scheme vulnerable.

Here:

```text
Noise
 ↓
Security
 ↓
Tiny failure probability
```

The probability is sufficiently small that practical systems can handle the rare failure through protocol-level retry or connection failure handling.

---

# 49. Public vs Private Information

A useful exam table:

| Information | Public / Private |
|---|---|
| ML-KEM public key | Public |
| ML-KEM private key | Private |
| Public noisy capsule/ciphertext | Public |
| Shared secret | Secret |
| Private basis / easy decoding information | Private |
| Public tangled representation | Public |

The central principle is:

> **The attacker may see the public information, but should not have an efficient way to recover the shared secret.**

---

# 50. What the Eavesdropper Sees

Suppose Alice encapsulates a secret for the receiver.

The attacker may see:

```text
Public key
+
Ciphertext / capsule
+
Network metadata
```

But the attacker does not have:

```text
Private key
```

Therefore:

```text
Public information
       ↓
No efficient secret recovery known
```

This is the security objective.

---

# 51. Why the Public Key Can Be Shared

This follows the standard public-key principle.

A public key is intentionally designed to be distributed.

Its purpose is to allow another party to perform the public operation.

For ML-KEM:

```text
Public key
       ↓
Encapsulation
```

The private key performs:

```text
Private key
       ↓
Decapsulation
```

---

# 52. What Makes ML-KEM Different from RSA?

| RSA | ML-KEM |
|---|---|
| Classical public-key cryptosystem | Post-quantum KEM |
| Security connected to factoring | Security based on module-lattice assumptions |
| Threatened by Shor | Designed to resist known quantum attacks |
| Historically used for encryption/key establishment | Standardised as a KEM |
| Uses integer arithmetic | Uses structured lattice/module mathematics |

The key conceptual transition is:

```text
Factoring hardness
        ↓
RSA

Module-lattice hardness
        ↓
ML-KEM
```

---

# 53. What Makes ML-KEM Different from ECC?

ECC relies on:

```text
Elliptic-curve discrete logarithms
```

Shor's algorithm threatens this class of problems.

ML-KEM instead relies on:

```text
Module-lattice mathematical problems
```

for which no analogous efficient quantum attack is currently known.

---

# 54. The Migration Problem

The lecture's final slide places the transition on a practical timeline:

```text
2030 → 2035
```

It describes this as NIST's proposed migration window, with RSA/ECC being:

```text
Deprecated by 2030
Disallowed by 2035
```

The lecture uses this timeline to communicate the urgency of migration.

The important practical message is:

> **Cryptographic migration needs to begin before the quantum threat becomes operational because replacing cryptography across large systems takes years.** fileciteturn13file0L222-L232

---

# 55. Why Migration Takes So Long

Replacing a cryptographic algorithm is rarely just:

```text
Old algorithm
     ↓
New algorithm
```

A real system may contain cryptography in:

- servers,
- browsers,
- mobile applications,
- APIs,
- certificates,
- network protocols,
- databases,
- hardware,
- IoT devices,
- third-party dependencies.

Therefore:

```text
Cryptographic migration
        =
Inventory
+
Compatibility
+
Testing
+
Deployment
+
Monitoring
```

---

# 56. Hybrid Cryptography

The lecture's final slide mentions that future systems may need to support:

> **ML-KEM or a hybrid of it**

A hybrid design can conceptually combine:

```text
Classical algorithm
       +
Post-quantum algorithm
       ↓
Combined security strategy
```

This can help organisations migrate gradually while maintaining compatibility with existing infrastructure.

---

# 57. Libraries and Implementation

The lecture points out that developers do not necessarily need to implement the mathematics of ML-KEM themselves.

Existing libraries can provide implementations.

The lecture specifically mentions:

```text
liboqs
OpenSSL 3.x
```

The important engineering skill is therefore:

> **Knowing when and why to use post-quantum cryptography, rather than implementing cryptographic primitives from scratch.** fileciteturn13file0L228-L232

---

# 58. A Developer's Mental Model

For a software engineer, remember:

```text
Do NOT:
Implement ML-KEM from scratch
        ↓
unless doing specialised cryptographic research

DO:
Understand the primitive
        ↓
Use a vetted library
        ↓
Configure it correctly
        ↓
Understand compatibility
        ↓
Plan migration
```

Cryptography is extremely sensitive to implementation errors.

---

# 59. ML-KEM in a Real Application

Imagine a banking application.

Without PQC:

```text
Bank App
   ↓
RSA / ECC key establishment
   ↓
Shared session key
   ↓
AES
   ↓
Banking data
```

With PQC:

```text
Bank App
   ↓
ML-KEM / hybrid key establishment
   ↓
Shared session key
   ↓
AES or another symmetric cipher
   ↓
Banking data
```

The application data still benefits from efficient symmetric encryption.

The major change is the mechanism used to establish the shared key.

---

# 60. ML-KEM in HTTPS

A simplified modernisation path is:

```text
Old:
ECDH
 ↓
Session key
 ↓
AES

PQC:
ML-KEM / hybrid KEM
 ↓
Session key
 ↓
AES
```

The KEM is therefore part of the handshake/key-establishment layer.

---

# 61. ML-KEM and "Breaking the Unbreakable Lock"

The lecture title:

> **Breaking the Unbreakable Lock**

describes the transition from an old assumption to a new one.

The old assumption:

```text
Factoring / ECC problems
       ↓
Too hard
       ↓
Secure
```

The quantum challenge:

```text
Shor
 ↓
Efficient quantum solution
 ↓
Old lock becomes vulnerable
```

The new approach:

```text
Lattice problems
       ↓
No known equivalent quantum shortcut
       ↓
ML-KEM
       ↓
New cryptographic lock
```

---

# 62. Complete Conceptual Flow

```text
                    TODAY
                      │
             RSA / ECC protect data
                      │
                      ↓
             Quantum computers
                      │
                      ↓
                Shor's algorithm
                      │
                      ↓
          Factoring / discrete logs
                      │
                      ↓
             RSA / ECC threatened
                      │
                      ↓
             Post-Quantum Crypto
                      │
                      ↓
               NIST competition
                      │
                      ↓
              CRYSTALS-Kyber
                      │
                      ↓
                 FIPS 203
                      │
                      ↓
                  ML-KEM
                      │
                      ↓
                Lattice basis
                      │
             ┌────────┴────────┐
             ↓                 ↓
        Private Basis A    Public Basis B
             │                 │
             ↓                 ↓
         Easy recovery      Hard recovery
             │                 │
             └────────┬────────┘
                      ↓
             Noisy public point
                      ↓
                Decapsulation
                      ↓
               Shared secret
                      ↓
             Symmetric encryption
                      ↓
              Secure application
```

---

# 63. Most Important Definitions

## Post-Quantum Cryptography

Cryptographic techniques designed to remain secure against sufficiently powerful quantum computers.

## KEM

A Key Encapsulation Mechanism establishes a shared secret between parties using public-key cryptographic operations.

## ML-KEM

A Module-Lattice-based Key-Encapsulation Mechanism standardised by NIST as FIPS 203.

## CRYSTALS-Kyber

The algorithm name used during the NIST competition that became the basis for ML-KEM.

## Encapsulation

The process by which a sender uses the recipient's public key to create a capsule containing a shared secret.

## Decapsulation

The process by which the recipient uses the private key to recover the shared secret.

## Lattice

A mathematical structure consisting of regularly generated points based on integer combinations of basis vectors.

## Basis

A set of vectors used to describe a lattice.

## Private basis

In the lecture's intuition, the easy representation that allows efficient recovery of the intended lattice point.

## Public basis

The tangled representation that can be shared publicly.

## Noise

A small perturbation added to the true point in the lecture's simplified lattice intuition.

## Shor's algorithm

A quantum algorithm capable of efficiently solving important mathematical problems such as integer factorisation and discrete logarithms on a sufficiently powerful quantum computer.

## Harvest Now, Decrypt Later

The strategy of collecting encrypted data today with the intention of decrypting it after future cryptographic breakthroughs.

---

# 64. Important Comparisons

## RSA vs ML-KEM

| RSA | ML-KEM |
|---|---|
| Classical public-key cryptography | Post-quantum KEM |
| Factoring-related security | Module-lattice security |
| Threatened by Shor | Designed to resist known quantum attacks |
| Public/private key system | Public/private KEM system |
| Not a modern PQC standard | NIST FIPS 203 |

---

## Kyber vs ML-KEM

| CRYSTALS-Kyber | ML-KEM |
|---|---|
| Competition/algorithm name | Standardised name |
| Used during NIST selection | FIPS 203 |
| 2017–2022 context | 2024 standard context |
| Same algorithmic lineage | Standardised form |

---

## ML-KEM vs ML-DSA

| ML-KEM | ML-DSA |
|---|---|
| Key encapsulation | Digital signature |
| Establishes shared secret | Signs/verifies messages |
| Replaces key-exchange role | Replaces vulnerable signature role |
| FIPS 203 | FIPS 204 |

---

# 65. High-Value Exam Questions

## Q1. What is ML-KEM?

ML-KEM stands for Module-Lattice-based Key-Encapsulation Mechanism. It is a post-quantum KEM standardised by NIST as FIPS 203 and derived from CRYSTALS-Kyber.

---

## Q2. What is the purpose of ML-KEM?

Its purpose is to establish a shared secret between parties over a public channel using a lattice-based post-quantum cryptographic mechanism.

---

## Q3. What are the three main ML-KEM operations?

```text
1. Key Generation
2. Encapsulation
3. Decapsulation
```

---

## Q4. What happens during key generation?

A key pair is generated:

```text
Public key
Private key
```

The public key is distributed while the private key is kept secret.

---

## Q5. What happens during encapsulation?

A sender uses the receiver's public key to encapsulate a random secret and generates a capsule/ciphertext that is sent over the public channel.

---

## Q6. What happens during decapsulation?

The receiver uses the private key to process the capsule and recover the same shared secret.

---

## Q7. Why is ML-KEM considered post-quantum?

It is based on module-lattice mathematical assumptions for which no known efficient quantum algorithm analogous to Shor's algorithm currently exists.

---

## Q8. Why doesn't Shor's algorithm directly break ML-KEM?

Shor's algorithm exploits mathematical structure associated with factoring and discrete logarithms. The lattice problems underlying ML-KEM do not have the same known structure.

---

## Q9. What is the role of noise in the lattice intuition?

Noise moves the transmitted point slightly away from the exact lattice point. The private representation allows the legitimate receiver to recover the intended point, while the public tangled representation makes reliable recovery computationally difficult.

---

## Q10. Is the lattice itself secret?

No.

The lecture explicitly explains that the lattice can be publicly represented. The private information is the easy representation or basis that enables efficient recovery.

---

## Q11. What is the difference between Basis A and Basis B?

```text
Basis A → private, easy map
Basis B → public, tangled map
```

Both describe the same lattice.

---

## Q12. What is "Harvest Now, Decrypt Later"?

It is the practice of recording encrypted information today and attempting to decrypt it in the future when a sufficiently powerful quantum computer becomes available.

---

## Q13. What are the ML-KEM parameter sets?

```text
ML-KEM-512
ML-KEM-768
ML-KEM-1024
```

---

## Q14. Which ML-KEM parameter set does the lecture describe as the default?

The lecture describes:

```text
ML-KEM-768
```

as the default deployment level it discusses.

---

## Q15. Is ML-KEM a digital signature algorithm?

No.

ML-KEM is a KEM.

The lecture identifies ML-DSA as the sibling standard for digital signatures.

---

## Q16. Is post-quantum cryptography slower?

Not necessarily.

The lecture specifically notes that ML-KEM operations can be cheaper on the CPU than RSA. A major practical cost is the larger size of keys and ciphertexts, which increases bandwidth requirements.

---

## Q17. What is the relationship between Kyber and ML-KEM?

CRYSTALS-Kyber was the name used during the NIST competition. After standardisation as FIPS 203, the standardised algorithm is called ML-KEM.

---

# 66. One-Page Revision Sheet

```text
ML-KEM
│
├── Problem
│   ├── RSA
│   ├── ECC
│   ├── Shor
│   └── Quantum threat
│
├── Motivation
│   ├── Harvest now, decrypt later
│   └── Need PQC
│
├── NIST
│   ├── 2016 proposal
│   ├── 2017 submissions
│   ├── 2022 Kyber selected
│   └── 2024 FIPS 203
│
├── ML-KEM
│   ├── Key Generation
│   ├── Encapsulation
│   └── Decapsulation
│
├── Mathematics
│   ├── Lattices
│   ├── Basis A
│   ├── Basis B
│   └── Noise
│
├── Parameter Sets
│   ├── ML-KEM-512
│   ├── ML-KEM-768
│   └── ML-KEM-1024
│
└── Ecosystem
    ├── ML-KEM → key establishment
    ├── ML-DSA → signatures
    ├── liboqs
    └── OpenSSL 3.x
```

---

# 67. Final Mental Model

Think of ML-KEM as a mathematically sophisticated lockbox.

First, the receiver creates:

```text
Private key
+
Public key
```

The public key can be distributed to anyone.

Alice uses that public key to create:

```text
Random secret
+
Encapsulation
        ↓
Capsule
```

The capsule travels openly.

The attacker can observe it.

The receiver uses:

```text
Private key
+
Capsule
        ↓
Decapsulation
        ↓
Same secret
```

The reason the attacker cannot simply do the same thing is the underlying lattice-based mathematical hardness.

The lecture's toy example makes this intuitive:

```text
Private Basis A
      ↓
Easy rounding
      ↓
Correct point (5,3)

Public Basis B
      ↓
Tangled rounding
      ↓
Wrong point (2,3)
```

The real ML-KEM construction is far more sophisticated, but this captures the fundamental intuition presented in the lecture.

---

# 68. The Entire Lecture in 12 Lines

```text
1. RSA and ECC protect much of today's Internet.
2. Their security depends on mathematical problems that are hard for classical computers.
3. A sufficiently powerful quantum computer can use Shor's algorithm against those problems.
4. Attackers can potentially harvest encrypted data today and decrypt it later.
5. Therefore, cryptography must migrate to post-quantum algorithms.
6. NIST selected CRYSTALS-Kyber during its PQC standardisation process.
7. Kyber became ML-KEM under FIPS 203.
8. ML-KEM is a Key Encapsulation Mechanism.
9. It uses module-lattice-based mathematics rather than factoring or elliptic-curve discrete logarithms.
10. Key generation creates a public and private representation.
11. Encapsulation sends a noisy capsule, and decapsulation uses the private information to recover the same secret.
12. ML-KEM establishes the shared key; other mechanisms such as ML-DSA handle digital signatures.
```

---

# Final Takeaways

1. **ML-KEM is a post-quantum Key Encapsulation Mechanism.**
2. **CRYSTALS-Kyber is the algorithm lineage that became ML-KEM.**
3. **ML-KEM is standardised as NIST FIPS 203.**
4. **Its primary job is shared-secret establishment, not digital signatures.**
5. **The three core operations are Key Generation, Encapsulation and Decapsulation.**
6. **Its security foundation is module-lattice mathematics.**
7. **The lecture explains the lattice using private Basis A and public Basis B.**
8. **Noise makes the public problem difficult while allowing the private representation to recover the intended point.**
9. **Shor's algorithm threatens RSA/ECC because it exploits their mathematical structure, while no equivalent efficient quantum shortcut is known for the relevant ML-KEM lattice problem.**
10. **ML-KEM-512, ML-KEM-768 and ML-KEM-1024 provide different security levels.**
11. **The lecture presents ML-KEM-768 as the practical default in the examples discussed.**
12. **ML-KEM can be used with symmetric encryption after establishing a shared secret.**
13. **Post-quantum migration is driven partly by the Harvest Now, Decrypt Later threat.**
14. **The lecture highlights larger key/ciphertext sizes as a major practical cost, rather than CPU computation.**
15. **A complete post-quantum system needs both key-establishment and authentication/signature mechanisms.**
