# 10 · Understanding Modern Post-Quantum Algorithms

## Lecture Overview

**Speaker:** Dr. Debasis Giri  
**Department:** Information Technology, Maulana Abul Kalam Azad University of Technology, West Bengal, India  
**Date:** 24 September 2026

This lecture moves from the foundations of cryptography to modern post-quantum cryptography.

The overall journey is:

```text
Secure communication
        ↓
Classical cryptography
        ↓
Modern symmetric cryptography
        ↓
Public-key cryptography
        ↓
Authenticated encryption and signatures
        ↓
Quantum algorithms
        ↓
Shor's algorithm
        ↓
Post-quantum cryptography
        ↓
Lattice mathematics
        ↓
LWE / SVP / CVP
        ↓
Lattice-based schemes
        ↓
Quantum-safe authentication
        ↓
IoT contactless payment case study
```

The lecture's central message is:

> **Modern cryptography depends on computational hardness. Quantum algorithms change which hardness assumptions remain reliable, so new cryptographic foundations are required.**

---

# 1. Introduction to Cryptography

## 1.1 The basic communication problem

Consider two users:

```text
Alice                         Bob
  │                            │
  └──────── Public Channel ────┘
                 ↑
             Adversary
```

The communication channel is unsecured.

An adversary can interact with the channel in different ways.

### Passive adversary

A passive adversary can only:

> **Read information from the unsecured channel.**

The adversary observes communication but does not modify it.

### Active adversary

An active adversary can:

- transmit information,
- alter information,
- delete information.

Therefore:

```text
Passive attacker → observes
Active attacker  → observes + interferes
```

This distinction is fundamental when designing secure protocols.

---

# 2. Cryptosystem Model

The basic cryptosystem has three major stages:

```text
Plaintext
   │
   ↓
Encryption + Key
   │
   ↓
Ciphertext
   │
   │  insecure channel
   ↓
Decryption + Key
   │
   ↓
Plaintext
```

In the lecture's notation:

```text
c = E_e(m)

m = D_d(c)
```

where:

- `m` = plaintext
- `c` = ciphertext
- `e` = encryption key
- `d` = decryption key

The attacker sees the ciphertext travelling through the public channel but should not be able to recover the plaintext without the required secret information.

---

# 3. Classical Cryptography

The lecture divides classical cryptographic techniques into two broad categories:

```text
Classical Cryptography
        │
        ├── Substitution
        │
        └── Transposition
```

---

# 4. Substitution Technique

## Definition

A substitution technique replaces plaintext symbols with other:

- letters,
- numbers,
- symbols,
- or bit patterns.

The relationship between plaintext and ciphertext is defined by a substitution rule.

For example:

```text
Plaintext:
A B C D E

Ciphertext:
D E F G H
```

The characters themselves are replaced.

---

# 5. Substitution Cipher

A substitution cipher:

- replaces each plaintext symbol with another symbol,
- keeps the substitution rule fixed throughout the message,
- is one of the earliest and simplest encryption techniques.

Examples:

1. Caesar Cipher
2. Monoalphabetic Cipher

---

# 6. Caesar Cipher

The Caesar cipher uses a fixed shift.

## Key generation

The key is:

```text
k ∈ {0, 1, 2, ..., 25}
```

## Encryption

Each plaintext letter is shifted `k` positions forward.

## Decryption

Each ciphertext letter is shifted `k` positions backward.

---

## Mathematical rule

Using:

```text
A = 0
B = 1
...
Z = 25
```

encryption is:

```text
C = (P + k) mod 26
```

where:

- `P` = plaintext letter position
- `C` = ciphertext letter position
- `k` = shift

Decryption is:

```text
P = (C - k) mod 26
```

---

## Example

Given:

```text
Key: k = 1
Message: attack
```

Encryption:

```text
attack
  ↓
buubdl
```

Therefore:

```text
Enc(1, "attack") = "buubdl"
```

Decryption:

```text
Dec(1, "buubdl") = "attack"
```

---

## Another example

Plaintext:

```text
HELLO
```

Shift:

```text
k = 3
```

Then:

```text
H → K
E → H
L → O
L → O
O → R
```

Ciphertext:

```text
KHOOR
```

---

# 7. Monoalphabetic Cipher

The Caesar cipher restricts us to a fixed shift.

A monoalphabetic cipher is more general.

Instead of shifting the alphabet, an arbitrary permutation of the alphabet is selected.

Example:

```text
Plaintext alphabet:
ABCDEFGHIJKLMNOPQRSTUVWXYZ

Cipher alphabet:
QWERTYUIOPASDFGHJKLZXCVBNM
```

Each plaintext letter maps to exactly one ciphertext letter.

The mapping remains fixed for the entire message.

---

## Key-space size

The number of possible alphabet permutations is:

```text
26!
```

This is much larger than the Caesar cipher's 26 possible shifts.

However, a large key space alone does not make the scheme secure.

Monoalphabetic ciphers remain vulnerable to:

> **Frequency analysis**

---

# 8. Frequency Analysis

Natural languages do not use every letter equally often.

For English, the lecture gives:

### Common letters

| Letter | Approx. frequency |
|---|---:|
| E | 12.7% |
| T | 9.1% |
| A | 8.2% |
| O | 7.5% |
| I | 7.0% |
| N | 6.7% |

A common mnemonic is:

```text
ETAOIN SHRDLU
```

### Moderate-frequency letters

| Letter | Frequency |
|---|---:|
| S | 6.3% |
| H | 6.1% |
| R | 6.0% |
| D | 4.3% |
| L | 4.0% |

Other letters such as C, U, M, W, F, G, Y, P and B range approximately from 2.8% down to 1.5%.

### Rare letters

| Letter | Frequency |
|---|---:|
| V | 0.98% |
| K | 0.77% |
| J | 0.15% |
| X | 0.15% |
| Q | 0.095% |
| Z | 0.074% |

---

## Why frequency analysis works

Suppose a ciphertext contains one symbol that appears far more frequently than others.

An attacker may hypothesise:

```text
Most common ciphertext symbol
          ↓
Possibly E
```

The attacker then looks at:

- repeated patterns,
- word lengths,
- common letter combinations,
- language statistics.

Thus, even though `26!` possible mappings exist, the structure of natural language leaks information.

---

# 9. Transposition Technique

Substitution changes the symbols.

Transposition does something fundamentally different.

> **The symbols remain unchanged. Their positions are rearranged.**

Example:

```text
PLAINTEXT

P L A I N T E X T

      ↓ rearrange

CIPHERTEXT

...
```

No letter itself is replaced.

Only its location changes.

---

# 10. Transposition Cipher

Key ideas:

- no substitution of characters,
- only positions change,
- security depends on the permutation,
- the same permutation key is used for encryption and decryption.

Examples:

- Rail Fence Cipher
- Columnar Transposition Cipher

---

# 11. Rail Fence Cipher

Rail Fence is a classical transposition cipher.

## Encryption procedure

1. Choose the number of rails.
2. Write the plaintext diagonally in a zig-zag pattern.
3. Read the rows from top to bottom.

---

## Example

Plaintext:

```text
ATTACK
```

Number of rails:

```text
3
```

The characters are arranged approximately as:

```text
A       C
  T   A   K
    T
```

Reading row by row gives:

```text
ACTAKT
```

Therefore:

```text
Plaintext  = ATTACK
Rails      = 3
Ciphertext = ACTAKT
```

---

# 12. Rail Fence Decryption

The receiver needs the same number of rails.

Conceptually:

```text
Ciphertext
    ↓
Place symbols row-by-row
    ↓
Reconstruct zig-zag pattern
    ↓
Read diagonally
    ↓
Plaintext
```

The characters themselves never change.

---

# 13. Columnar Transposition Cipher

A columnar transposition cipher:

1. writes plaintext row-wise into a matrix,
2. uses a key to define a column permutation,
3. reads columns according to that permutation.

Example from the lecture:

```text
Plaintext: MEET AT DAWN
Key:       4312
```

The resulting ciphertext is shown as:

```text
EDT AETN MAW
```

The key controls the ordering of the columns.

---

# 14. Characteristics of Classical Ciphers

The lecture identifies several characteristics:

- simple algorithms,
- small key space,
- vulnerable to cryptanalysis,
- limited security compared with modern ciphers.

Their historical importance is high, but they are not appropriate foundations for modern large-scale secure communication.

---

# 15. Limitations of Classical Cryptography

Classical techniques are:

### 1. Susceptible to frequency analysis

Patterns in natural language can reveal substitutions.

### 2. Vulnerable to brute force

Small key spaces can be exhaustively searched.

### 3. Unsuitable for large-scale modern encryption

Modern systems require:

- strong security,
- efficient computation,
- scalability,
- formal security analysis.

---

# 16. Classical vs Modern Cryptography

| Classical | Modern |
|---|---|
| Substitution | Complex mathematical constructions |
| Transposition | Symmetric and asymmetric algorithms |
| Small/simple key structures | Large cryptographic key spaces |
| Vulnerable to basic cryptanalysis | Designed against modern attacks |
| Limited scalability | Designed for large-scale systems |

The important transition is:

```text
Simple transformations
        ↓
Mathematical hardness
        ↓
Modern cryptography
```

---

# 17. Modern Encryption Techniques

The lecture identifies two major types:

```text
Modern Encryption
       │
       ├── Symmetric-key
       │
       └── Asymmetric-key
```

A **key** may be represented by:

- bits,
- a number,
- a word,
- a phrase,
- or another cryptographic value.

The security of a cryptosystem depends heavily on how keys are generated and used.

---

# 18. When Is a Cryptosystem Breakable?

The lecture defines an encryption scheme as **breakable** when a third party, without prior knowledge of the key pair `(e, d)`, can systematically recover plaintext from corresponding ciphertext within an appropriate time frame.

This introduces an important idea:

> **Security is not simply "can it theoretically be broken?" The practical question is whether it can be broken within a useful time and cost.**

---

# 19. Brute-Force Attack

A brute-force attack tries every possible key.

If the attacker knows the algorithm but not the key:

```text
Try key 1
Try key 2
Try key 3
...
Try every possible key
```

This is also called:

> **Exhaustive search of the key space**

---

# 20. Why Key Size Matters

The lecture gives the following illustrative values, assuming `10^6` decryptions per microsecond:

| Key size | Number of possible keys | Approx. exhaustive-search time |
|---:|---:|---:|
| 32 bits | `2^32 ≈ 4.3 × 10^9` | 2.15 ms |
| 56 bits | `2^56 ≈ 7.2 × 10^16` | 10 hours |
| 128 bits | `2^128 ≈ 3.4 × 10^38` | `5.4 × 10^18` years |
| 168 bits | `2^168 ≈ 3.7 × 10^50` | `5.9 × 10^30` years |

The core lesson:

> **Increasing the key space can make exhaustive search computationally infeasible.**

---

# 21. Unconditionally Secure vs Computationally Secure

## Unconditionally secure

An encryption scheme is unconditionally secure when the ciphertext does not contain enough information to uniquely determine the plaintext, regardless of how much computational time the attacker has.

The security is therefore not based on computational limitations.

The lecture describes the idea as:

```text
More computing power
        ↓
Still cannot recover plaintext
        ↓
Required information is absent
```

---

## Computationally secure

A computationally secure scheme satisfies two practical criteria:

1. The cost of breaking the cipher exceeds the value of the encrypted information.
2. The time needed to break it exceeds the useful lifetime of the information.

This definition is extremely important for understanding modern cryptography.

---

# 22. Symmetric-Key Encryption

In symmetric encryption:

```text
             Same secret key
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
    Encryption          Decryption
        ↓                   ↓
    Plaintext             Plaintext
        │
        ↓
    Ciphertext
```

The same secret key is used by communicating parties.

The lecture's model includes:

- message source,
- encryption algorithm,
- key source,
- insecure channel,
- decryption algorithm,
- destination,
- cryptanalyst.

The major practical problem is:

> **How do communicating parties securely obtain and share the secret key?**

This motivates public-key cryptography.

---

# 23. Kerberos Authentication Protocol

The lecture includes Kerberos as an authentication protocol.

Its architecture contains:

- client,
- Authentication Server (AS),
- Ticket Granting Server (TGS),
- application/service server.

The protocol uses timestamps, tickets and session keys.

A simplified flow is:

```text
Client
  │
  │ request ticket
  ↓
Authentication Server
  │
  │ Ticket Granting Ticket
  ↓
Client
  │
  │ request service ticket
  ↓
TGS
  │
  │ Service Ticket
  ↓
Client
  │
  │ authenticate to service
  ↓
Server
```

The lecture gives the detailed six-message formulation:

```text
1. C → AS
2. AS → C
3. C → TGS
4. TGS → C
5. C → V
6. V → C
```

The final response:

```text
EKC,V[TS5 + 1]
```

supports mutual authentication.

---

# 24. Public-Key Cryptography

Symmetric cryptography requires a shared secret.

Public-key cryptography uses a key pair:

```text
Public key
Private key
```

Conceptually:

```text
Public key
    ↓
Known to others

Private key
    ↓
Kept secret
```

This makes it possible to establish secure communication without first sharing a secret key over a secure physical channel.

---

# 25. RSA

RSA was introduced in 1978 by:

- Ronald Rivest
- Adi Shamir
- Leonard Adleman

The lecture describes RSA as a public-key cryptosystem based on elementary number theory.

The fundamental security connection is the difficulty of factoring a large integer:

```text
n = p × q
```

where `p` and `q` are large primes.

---

# 26. RSA Key Generation

RSA key generation follows these steps.

### Step 1: Select two primes

Choose:

```text
p, q
```

such that:

```text
p ≠ q
```

and both are large primes.

### Step 2: Calculate modulus

```text
n = p × q
```

### Step 3: Calculate Euler's totient

```text
φ(n) = (p - 1)(q - 1)
```

### Step 4: Select public exponent

Choose `e` such that:

```text
gcd(e, φ(n)) = 1
```

and:

```text
1 < e < φ(n)
```

### Step 5: Calculate private exponent

Find:

```text
d ≡ e⁻¹ mod φ(n)
```

or equivalently:

```text
ed ≡ 1 mod φ(n)
```

### Keys

Public key:

```text
KU = {e, n}
```

Private key:

```text
KR = {d, n}
```

---

# 27. RSA Encryption

For plaintext `M` where:

```text
M < n
```

the ciphertext is:

```text
C = M^e mod n
```

The public key is used for encryption.

---

# 28. RSA Decryption

The receiver computes:

```text
M = C^d mod n
```

using the private exponent `d`.

Thus:

```text
M
 ↓
M^e mod n
 ↓
C
 ↓
C^d mod n
 ↓
M
```

---

# 29. RSA Correctness

Since:

```text
ed ≡ 1 mod φ(n)
```

we can write:

```text
ed = 1 + kφ(n)
```

Then:

```text
C^d
= (M^e)^d
= M^(ed)
= M^(1 + kφ(n))
```

Under the relevant RSA number-theoretic conditions, this reduces modulo `n` to:

```text
M
```

Therefore encryption followed by decryption recovers the original message.

---

# 30. RSA Security

The lecture identifies several attack categories.

### Brute force

Try possible private keys.

### Mathematical attacks

Attack the underlying number-theoretic structure, particularly the Integer Factorization Problem.

### Timing attacks

Exploit information leaked through the running time of decryption operations.

---

# 31. The RSA Factoring Problem

Three conceptual approaches are identified:

### Approach 1

Factor:

```text
n = p × q
```

Then calculate:

```text
φ(n) = (p - 1)(q - 1)
```

and recover:

```text
d = e⁻¹ mod φ(n)
```

### Approach 2

Determine `φ(n)` directly without first finding `p` and `q`.

Then recover `d`.

### Approach 3

Determine `d` directly without first determining `φ(n)`.

The security of RSA depends on these attacks being computationally infeasible at cryptographic sizes.

---

# 32. RSA Worked Example

The lecture gives:

```text
Public key:
(e, n) = (223, 1643)
```

Ciphertext:

```text
1451 0103 1263 0560 0127 0897
```

Encoding:

```text
A = 01
B = 02
...
Z = 26
space = 00
```

The modulus factors as:

```text
1643 = 31 × 53
```

Therefore:

```text
p = 31
q = 53
```

and:

```text
φ(1643)
= (31 - 1)(53 - 1)
= 30 × 52
= 1560
```

The modular inverse of `223` modulo `1560` is:

```text
d = 7
```

Therefore the private key is:

```text
(d, n) = (7, 1643)
```

Each ciphertext block is decrypted using:

```text
M = C^7 mod 1643
```

The lecture obtains:

```text
M1 = 180
M2 = 516
M3 = 122
M4 = 500
M5 = 141
M6 = 523
```

Using the given encoding, the plaintext becomes:

```text
18 05 16 12 25 00 14 15 23
```

which gives:

```text
REPLY NOW
```

---

# 33. Modular Arithmetic

The lecture introduces:

```text
Zn = {0, 1, 2, ..., n - 1}
```

For `a, b ∈ Zn`:

### Addition

```text
(a + b) mod n
```

### Subtraction

```text
(a - b) mod n
```

### Multiplication

```text
ab mod n
```

### Modular inverse

An element `a` is invertible modulo `n` if there exists a `u` such that:

```text
ua mod n = 1
```

Modular arithmetic is a core mathematical tool behind RSA and many other cryptographic systems.

---

# 34. Intractable Problems

The lecture introduces two important hard problems.

## Discrete Logarithm Problem (DLP)

Given:

```text
g^a mod p
```

determine:

```text
a
```

when `p` is prime and `g` is a generator.

---

## Computational Diffie-Hellman Problem (CDHP)

Given:

```text
g^x
g^y
```

modulo `p`, compute:

```text
g^(xy)
```

without knowing the private exponents.

These mathematical problems historically support several public-key systems.

---

# 35. Authenticated Encryption

The lecture defines an authenticated encryption construction as a tuple:

```text
AE = (KeyGen, Sign-and-Encrypt, Decrypt-and-Verify)
```

The goal is to combine confidentiality and authenticity-related operations into a unified construction.

---

# 36. Authenticated Encryption: Key Generation

`KeyGen` receives a security parameter:

```text
1^k
```

and produces a key pair:

```text
(SDK, VEK)
```

where:

- `SDK` = signing and decryption key, kept secret
- `VEK` = verification and encryption key, made public

Both sender and recipient generate their respective key pairs.

For Alice:

```text
(SDK_A, VEK_A)
```

For Bob:

```text
(SDK_B, VEK_B)
```

---

# 37. Sign-and-Encrypt

The sender uses:

```text
SDK_A
VEK_B
m
```

to produce:

```text
s
```

Conceptually:

```text
Sender's secret key
        +
Recipient's public key
        +
Message
        ↓
Sign-and-Encrypt
        ↓
s
```

---

# 38. Decrypt-and-Verify

The recipient uses:

```text
VEK_A
SDK_B
s
```

to recover the message.

The operation should output the original message if the signature is valid.

Otherwise, it outputs an invalid-signature indication.

Conceptually:

```text
Signed + encrypted message
          ↓
Decrypt
          ↓
Verify
       /      \
    valid    invalid
      ↓         ↓
  message     reject
```

---

# 39. Cryptographic Hash Functions

A cryptographic hash function maps arbitrary-length input to a fixed-length output:

```text
h : X → Y
```

where:

```text
X = {0,1}*
```

and the output has a fixed number of bits.

A good cryptographic hash has several important properties.

---

## 39.1 Easiness

For a given message `m`:

```text
h(m)
```

should be easy to compute.

---

## 39.2 One-Wayness

Given:

```text
c = h(m)
```

it should be computationally infeasible to recover an input `m` that produces `c`.

---

## 39.3 Collision Resistance

It should be computationally infeasible to find two different messages:

```text
m ≠ m'
```

such that:

```text
h(m) = h(m')
```

---

## 39.4 Second Pre-image Resistance

Given `m`, it should be computationally infeasible to find another:

```text
m' ≠ m
```

such that:

```text
h(m') = h(m)
```

---

# 40. Attacks Against Signature Schemes

The lecture lists several attack goals.

## Total break

The attacker computes the secret trapdoor information or secret key.

---

## Universal forgery

The attacker finds an efficient algorithm functionally equivalent to the user's signing algorithm.

---

## Selective forgery

The attacker creates a valid signature for a particular message chosen by the attacker.

---

## Existential forgery

The attacker produces a valid signature on some message.

---

## Signature modification

The attacker modifies a valid signature in a way that the verification process fails to detect.

These categories help describe how badly a signature system can be compromised.

---

# 41. ElGamal Signature

The lecture presents the ElGamal signature construction.

## Key generation

Choose:

- large prime `p`,
- generator `g`,
- private key `x`.

Compute:

```text
y = g^x mod p
```

where:

```text
x = private key
y = public key
```

---

## Signature generation

Choose a random:

```text
k
```

with the required invertibility condition.

Compute:

```text
r = g^k mod p
```

and:

```text
s = (m - xr)k⁻¹ mod (p - 1)
```

The signature is:

```text
(r, s)
```

---

## Verification

The verifier checks:

```text
r^s y^r ≡ g^m mod p
```

The lecture also presents the hash-based form using `h(m)`.

---

# 42. Weakness of ElGamal Signature

The lecture discusses an attack described by He and Kiesler.

If three signatures use related random values:

```text
k3 = k1 + k2
```

with the corresponding relationship:

```text
r3 = r1 r2 mod p
```

then the private key can be algebraically recovered using the three signatures.

### Lesson

> **Randomness is an essential part of cryptographic security.**

A signature algorithm can have strong mathematical foundations but still become vulnerable if its randomness or implementation is poorly handled.

---

# 43. Authenticated Encryption Scheme in the Lecture

The lecture then gives a more detailed signcryption-style construction.

The system has three phases:

```text
1. Setup
2. Sign-and-encrypt
3. Decrypt-and-verify
```

---

# 44. Setup Phase

A trusted Certification Authority selects:

- large primes `p` and `q`,
- with `q | (p - 1)`,
- an element `α ∈ Zq*` of order `q` modulo `p`,
- a collision-resistant one-way hash function `h`.

For signer `UA`:

```text
Private key = xA
Public key  = yA = α^xA mod p
```

For recipient `UB`:

```text
Private key = xB
Public key  = yB = α^xB mod p
```

---

# 45. Sign-and-Encrypt Phase

For a message `m`, the signer:

### Step 1

Chooses random:

```text
k
```

### Step 2

Computes:

```text
t = h(m)
```

and:

```text
r = m yB^k mod p
```

### Step 3

Computes:

```text
s = k(r + xA)^(-1) mod q
```

### Step 4

Sends:

```text
(r, s, t)
```

to the recipient.

---

# 46. Message Recovery

The recipient computes:

```text
m = r(yA · α^r)^(-s xB) mod p
```

The lecture proves correctness algebraically.

The recovered message can then be checked using:

```text
t = h(m)
```

Thus the hash provides message verification.

---

# 47. Security Argument

The lecture argues that recovering the original message from the signcrypted message is computationally infeasible.

The reasoning is based on the Diffie-Hellman assumption.

If an adversary could efficiently recover the message, the adversary could be transformed into an algorithm capable of solving a Diffie-Hellman instance.

That contradicts the assumed hardness of the Diffie-Hellman problem.

Therefore:

```text
Efficient message recovery
        ↓
Solve Diffie-Hellman
        ↓
Contradiction to hardness assumption
```

---

# 48. Quantum Computing Enters the Picture

The previous public-key constructions rely on mathematical problems believed to be hard for classical computers.

Quantum algorithms change this situation.

The lecture introduces:

> **Bounded-Error Quantum Polynomial Time (BQP)**

BQP is the complexity class associated with problems solvable by a quantum computer in polynomial time with bounded error.

---

# 49. Shor's Algorithm

Integer factorization is a key example.

Problem:

```text
Given N
        ↓
Find p and q
such that
N = p × q
```

For large cryptographic numbers, classical factorization is difficult.

Shor's algorithm provides a quantum approach.

---

# 50. Shor's Algorithm: Steps

The lecture gives the following process.

### Step 1

Choose a random number `r < N` such that:

```text
gcd(r, N) = 1
```

### Step 2

Use a quantum computer to determine the period `a` of:

```text
f(r,N)(x) = r^x mod N
```

This is the quantum part.

### Step 3

If `a` is odd:

```text
Restart
```

### Step 4

Since `a` is even:

```text
(r^(a/2) - 1)(r^(a/2) + 1)
= r^a - 1
= 0 mod N
```

### Step 5

If:

```text
r^(a/2) + 1 = 0 mod N
```

restart.

Otherwise continue.

### Step 6

Compute:

```text
p = gcd(r^(a/2) - 1, N)
```

### Step 7

The required factor is obtained.

---

# 51. Shor's Algorithm Complexity

The lecture compares:

### Classical factorization

```text
O(2^log N)
```

as presented in the lecture.

### Shor

```text
O((log N)^3)
```

The key conceptual difference is:

```text
Classical:
exponential-type growth

Quantum:
polynomial-time factorization
```

This is why RSA's mathematical foundation becomes threatened by a sufficiently powerful quantum computer.

---

# 52. Bounded Error

Shor's algorithm is probabilistic with bounded error.

It does not necessarily guarantee the correct result with absolute certainty in one execution.

However:

```text
Run repeatedly
      ↓
Reduce probability of error
      ↓
Obtain arbitrarily high confidence
```

This is characteristic of algorithms in BQP.

---

# 53. Post-Quantum Cryptography

At this point, the lecture moves to:

> **Post-Quantum Cryptography (PQC)**

The problem is no longer merely:

> "Can classical attackers break RSA?"

The new question is:

> "What mathematical foundations remain difficult even when the attacker has a quantum computer?"

The lecture focuses heavily on lattice-based cryptography.

---

# 54. Vector Spaces

Lattice mathematics begins with vector spaces.

Let:

- `V` be a non-empty set,
- `F` be a field,
- `+` be vector addition,
- scalar multiplication be defined.

Then `V` is a vector space over `F` when the required axioms hold.

---

## Addition axioms

### A1. Closure

```text
u + v ∈ V
```

### A2. Associativity

```text
(u + v) + w = u + (v + w)
```

### A3. Commutativity

```text
u + v = v + u
```

### A4. Additive identity

There exists `0` such that:

```text
0 + v = v
```

### A5. Additive inverse

For every `v`, there exists `-v` such that:

```text
v + (-v) = 0
```

Together, these make `(V, +)` an abelian group.

---

# 55. Scalar Multiplication Axioms

The lecture also gives:

### M1

```text
αu ∈ V
```

### M2

```text
α(u + v) = αu + αv
```

### M3

```text
(α + β)u = αu + βu
```

### M4

```text
(αβ)u = α(βu)
```

### M5

```text
1u = u
```

Examples given in the lecture include:

- real numbers as a vector space over real numbers,
- complex numbers as a vector space over complex numbers.

---

# 56. Linearly Dependent Vectors

Vectors:

```text
v1, v2, ..., vn
```

are linearly dependent if there exist scalars:

```text
α1, α2, ..., αn
```

not all zero, such that:

```text
α1v1 + α2v2 + ... + αnvn = 0
```

The important phrase is:

> **A non-trivial linear combination gives the zero vector.**

---

# 57. Linearly Independent Vectors

Vectors are linearly independent when:

```text
α1v1 + α2v2 + ... + αnvn = 0
```

implies:

```text
α1 = α2 = ... = αn = 0
```

Only the trivial combination produces zero.

---

# 58. Basis of a Vector Space

A subset `S` is a basis for `V` if:

1. `S` is linearly independent.
2. `S` spans `V`.

That means every vector in the space can be represented as a finite linear combination of basis vectors.

---

## Example

The standard basis of `R^3` is:

```text
B = {
(1,0,0),
(0,1,0),
(0,0,1)
}
```

Any vector:

```text
(a,b,c)
```

can be written as:

```text
(a,b,c)
= a(1,0,0)
+ b(0,1,0)
+ c(0,0,1)
```

---

# 59. What Is a Lattice?

This is one of the most important definitions in the lecture.

A lattice `L` in `R^n` is the set of all integer linear combinations of `n` linearly independent basis vectors.

Formally:

```text
L = {Bz | z ∈ Z^n}
```

where:

```text
B = [b1, b2, ..., bn]
```

and each `bi` is a basis vector.

---

# 60. Lattice Intuition

In two dimensions, imagine a regular collection of points:

```text
•     •     •     •

   •     •     •

•     •     •     •

   •     •     •
```

The lattice is generated from basis vectors.

An important property:

> **The same lattice can have many different bases.**

This becomes important in lattice-based cryptography because one representation can be easy to use as a secret while another representation can be publicly available but difficult to exploit.

---

# 61. Norm

A norm measures the size or length of a vector.

A norm satisfies:

### Positivity

```text
||v|| ≥ 0
```

and:

```text
||v|| = 0 ⇔ v = 0
```

### Homogeneity

```text
||αv|| = |α| ||v||
```

### Triangle inequality

```text
||u + v|| ≤ ||u|| + ||v||
```

A vector space equipped with a norm is called a normed vector space.

---

# 62. Common Norms

## Euclidean norm / 2-norm

```text
||x||₂ =
√(x₁² + x₂² + ... + xₙ²)
```

This is the familiar geometric distance.

---

## 1-norm / Manhattan norm

```text
||x||₁ =
|x₁| + |x₂| + ... + |xₙ|
```

---

## p-norm

```text
||x||p =
(Σ |xi|^p)^(1/p)
```

for:

```text
p ≥ 1
```

---

## Infinity norm

```text
||x||∞ = max |xi|
```

---

# 63. Hard Problems in Lattice Cryptography

The lecture introduces three central problems:

```text
SVP
CVP
LWE
```

These are important foundations of modern lattice-based cryptography.

---

# 64. Shortest Vector Problem

## SVP

Given a lattice `L`, find its shortest non-zero vector.

Formally:

```text
λ1(L) =
min ||v||
for v ∈ L \ {0}
```

In a low-dimensional lattice, this may be manageable.

In high dimensions, finding the exact shortest vector becomes computationally difficult.

---

# 65. Closest Vector Problem

## CVP

Given:

```text
x ∈ R^n
```

find the lattice vector `v` closest to `x`.

Formally:

```text
Find v ∈ L
such that
||x - v||
is minimal.
```

Conceptually:

```text
                 x
                 ★

•      •      •      •
    •      •      •
•      •      •      •

       ↑
closest lattice point
```

---

# 66. Learning With Errors

LWE is one of the most important concepts in the lecture.

Given:

```text
A ∈ Zq^(m×n)
s ∈ Zq^n
e = small error vector
```

the system provides:

```text
b = As + e mod q
```

The attacker knows:

```text
A
b
```

but wants to recover:

```text
s
```

---

# 67. Why the Error Matters

Without noise:

```text
b = As
```

The system resembles a normal linear-algebra problem.

With noise:

```text
b = As + e
```

where `e` is small but unknown.

Now the attacker must distinguish the true secret from many noisy possibilities.

The lecture states that even approximate recovery is computationally hard.

---

# 68. LWE Diagram

The structure can be remembered as:

```text
                 Secret
                   s
                   │
                   ↓
Public matrix A → As
                   │
             + small noise e
                   │
                   ↓
                 b = As + e
                   │
                   ↓
          Attacker receives A,b
                   │
                   ↓
          Recover secret s
                   │
                   ↓
              HARD PROBLEM
```

This is the core intuition behind Learning With Errors.

---

# 69. LWE and Lattice Hardness

The lecture connects the problems through reductions.

Conceptually:

```text
Worst-case lattice problems
        ↓
       SVP
        ↓
       LWE
        ↓
Lattice-based cryptographic schemes
```

The lecture states that LWE can be related to worst-case SVP hardness in certain lattices.

This gives lattice cryptography strong theoretical foundations.

---

# 70. Ring-LWE and Module-LWE

The lecture also introduces structured versions:

```text
LWE
 ↓
Ring-LWE / Module-LWE
```

These structured forms allow efficient computation.

They are used in modern lattice-based schemes.

The lecture explicitly connects:

- Kyber
- Dilithium
- NTRU

with LWE and Ring/Module-LWE-style constructions.

---

# 71. Why Lattices Matter for PQC

The lecture summarises the role of lattices as follows:

- lattices provide hard mathematical problems,
- SVP, CVP and LWE provide security foundations,
- reductions give theoretical guarantees,
- practical schemes can be efficient and scalable,
- the same mathematical family supports multiple cryptographic primitives.

Therefore:

```text
Lattice mathematics
        ↓
Hard computational problems
        ↓
Quantum-resistant constructions
        ↓
PQC
```

---

# 72. Security Advantages of Lattice-Based Cryptography

The lecture identifies:

### Quantum resistance

Lattice-based constructions are designed to resist known quantum attacks.

### Worst-case to average-case reductions

Security can be connected to hard worst-case lattice problems.

### Strong theoretical foundations

Security rests on well-studied mathematical assumptions.

### Multiple cryptographic uses

Lattice constructions can support:

- encryption,
- KEMs,
- digital signatures,
- homomorphic encryption.

---

# 73. Classical vs Quantum-Resistant Foundations

The lecture's conceptual comparison is:

```text
Classical public-key systems
        │
        ├── RSA
        ├── ECC
        └── DSA
                ↓
          Shor's algorithm
                ↓
             Vulnerable
```

versus:

```text
Post-quantum systems
        │
        ├── Lattice-based
        ├── Code-based
        └── Hash-based
                ↓
      Designed to resist
       known quantum attacks
```

---

# 74. Standard Lattice-Based Schemes

The lecture names:

### CRYSTALS-Kyber

A:

> **Key Encapsulation Mechanism (KEM)**

### CRYSTALS-Dilithium

A:

> **Digital Signature Scheme**

### NTRU and Falcon

Alternative lattice-based schemes.

These schemes became important in NIST's post-quantum cryptography standardisation effort.

---

# 75. Lattice-Based Cryptography: Big Picture

Remember:

```text
SVP
CVP
LWE
Ring/Module-LWE
       ↓
Lattice-based cryptography
       ↓
KEMs + signatures + other primitives
       ↓
Post-quantum security
```

This is the mathematical bridge from linear algebra to practical PQC.

---

# 76. Case Study: Quantum-Safe Contactless Smart Payments

The lecture presents a research case study:

> **Post-Quantum Secure Lattice-Based Authentication Scheme for IoT-Enabled Contactless Smart Payments**

Authors listed in the lecture:

- Abhishek Kumar Pandey
- Prithwi Bagchi
- Debnath Ghosh
- Ashok Kumar Das
- Ravi Kumar Kappagantu
- Srinivas V. Katakam
- Jitendra Chougala

Conference:

> 7th IEEE Computers, Communications and IT Applications Conference (ComComAp 2025)

Dates:

> 14–17 December 2025

Location:

> Madrid, Spain

---

# 77. Payment Architecture

The scenario involves:

```text
User
  ↓
Smart wearable
  ↓
NFC
  ↓
POS terminal
  ↓
Issuing bank
  ↓
Payment gateway
  ↓
Acquirer bank
  ↓
Merchant settlement
```

The wearable may be a:

- smartwatch,
- other IoT-enabled wearable.

---

# 78. NFC

NFC stands for:

> **Near Field Communication**

It is a short-range wireless technology allowing devices to exchange information when they are brought within a few centimetres of each other.

In this case:

```text
Wearable
   ↕
 NFC
   ↕
POS
```

The NFC link is used for the contactless transaction.

---

# 79. Quantum-Resistant Communication

The case study proposes a quantum-resistant security architecture.

The wearable establishes a secure session with the POS terminal.

The back-end processes are also described as using post-quantum SSL/TLS:

```text
Wearable
    ↓
POS
    ↓
Bank
    ↓
Gateway
    ↓
Acquirer
```

The objective is end-to-end protection against quantum attacks.

---

# 80. Threat Models

The system considers three adversarial models.

## 1. Dolev-Yao adversary

Can:

- eavesdrop,
- intercept sessions,
- observe exchanged messages.

---

## 2. Honest-but-Curious adversary

A legitimate entity may behave maliciously by observing sensitive information.

---

## 3. Canetti-Krawczyk adversary

Can:

- hijack active sessions,
- modify active exchanges,
- compromise session integrity.

These models represent both passive and active threats.

---

# 81. Threat Model Table

| Threat model | Main capability |
|---|---|
| Dolev-Yao | Eavesdropping and interception |
| Honest-but-Curious | Insider observation of sensitive data |
| Canetti-Krawczyk | Active-session hijacking and modification |

---

# 82. Lattice-Based Authentication Framework

The proposed authentication framework has two major phases:

```text
1. Registration
2. Login + Authentication
```

It also integrates:

> **Biometric verification through fuzzy extractors**

The objective is:

```text
Biometric
+
PIN
+
Lattice cryptography
+
Authentication
        ↓
Quantum-resistant access
```

---

# 83. Polynomial Rings in the Scheme

The case study uses polynomial rings.

Let:

```text
n = 2^α
```

for:

```text
α > 0
```

and let:

```text
Z[x]
```

and:

```text
Zq[x]
```

denote polynomial rings.

The lecture defines:

```text
R = Z[x] / <x^n + 1>
```

and:

```text
Rq = Zq[x] / <x^n + 1>
```

The polynomial:

```text
x^n + 1
```

is used as the relevant cyclotomic polynomial structure in the construction.

---

# 84. Characteristic and Modular Functions

The scheme defines a characteristic function:

```text
Cha(β)
```

which determines whether a value lies in the specified subset of `Zq`.

It also defines:

```text
Mod2(u, v)
```

which maps the relevant values into `{0,1}`.

These functions are used later in the authentication and session-key calculations.

For exam purposes, the main point is:

> **The protocol uses structured polynomial arithmetic and helper functions as part of its lattice-based authentication construction.**

---

# 85. Fuzzy Extractor

A fuzzy extractor allows a noisy biometric measurement to reproduce the same cryptographic secret.

It has two core algorithms:

```text
Gen()
Rep()
```

---

## Generation

Input:

```text
Biometric Bio
```

Output:

```text
σ = secret key
τ = public helper data
```

The helper data is not itself the secret.

---

## Reproduction

Input:

```text
Noisy biometric Bio*
Helper data τ
```

Output:

```text
σ
```

if the biometric is sufficiently close to the original.

The lecture measures closeness using:

> **Hamming distance**

---

# 86. Why Fuzzy Extractors Matter

Biometric readings are never perfectly identical.

For example:

```text
Original fingerprint
       ↓
Stored biometric representation

New scan
       ↓
Slightly different measurements
```

A fuzzy extractor allows the system to tolerate limited differences while reproducing the same secret.

Conceptually:

```text
Biometric
    ↓
Fuzzy extractor
    ↓
Stable cryptographic secret
```

---

# 87. System Notation

Important notation from the case study includes:

| Notation | Meaning |
|---|---|
| `Ui` | ith user |
| `WDi` | ith user's wearable device |
| `CA` | Certification Authority |
| `POS` | Point of Sale terminal |
| `IDX` | Identity of entity X |
| `PINUi` | User PIN |
| `BIOUi` | User biometric |
| `prX` | Lattice-based private key |
| `PubX` | Lattice-based public key |
| `n, q` | Security parameters |
| `Zq` | Finite field/set modulo q |
| `H()` | One-way cryptographic hash |
| `TSX` | Timestamp |
| `ΔT` | Maximum allowed transmission delay |
| `skwp` | Wearable-POS session key |

---

# 88. System Initialization

The Certification Authority operates offline during initialization.

It:

1. selects the security parameter `n`,
2. defines the polynomial ring,
3. chooses a discrete Gaussian distribution,
4. selects lattice-based key material,
5. chooses a quantum-resistant hash function,
6. publishes public parameters.

The lecture gives SHA-256 as an example of the hash function used.

The CA keeps its private key secret.

---

# 89. Registration Phase

The CA registers:

- wearable devices,
- issuing bank,
- payment gateway,
- acquirer bank.

Each participant receives:

```text
Lattice-based key pair
+
Secure credentials
+
Certificate binding identity to public key
```

This creates the trust infrastructure required for later authentication.

---

# 90. Wearable Enrollment: Step 1

The user registers the wearable with the CA.

The user chooses:

- identity `IDUi`,
- 6-digit PIN,
- random nonce,
- biometric.

The fuzzy extractor generates:

```text
(bkUi, phUi) = Gen(BIOUi)
```

where:

- `bkUi` is the biometric-derived key material,
- `phUi` is helper information.

The user sends a protected registration value to the CA.

---

# 91. Wearable Enrollment: Step 2

The CA generates:

```text
PIDUi
```

a pseudo-identity.

It also calculates additional protected values involving:

- CA secret material,
- timestamps,
- PIN-derived values,
- the random nonce.

The CA securely sends required registration information back to the user.

---

# 92. Wearable Enrollment: Step 3

The wearable computes a masked pseudo-identity:

```text
PID*Ui
```

It generates a lattice-based private/public key pair:

```text
prUi
PubUi
```

The private components are further masked using hash-derived values.

This means sensitive credentials are not simply stored in plaintext.

---

# 93. Wearable Finalization

The wearable computes additional protected values, including:

```text
X*Ui
YUi
```

It stores:

```text
PID*Ui
pr*Ui,1
pr*Ui,2
X*Ui
pp
```

The public key:

```text
PubUi
```

is published for authentication.

---

# 94. POS Registration

The bank configures the POS terminal with:

1. Merchant and terminal information
2. Bank/acquirer details
3. Cryptographic credentials
4. Transaction parameters
5. Software/configuration parameters

The POS receives its own lattice-based key pair.

---

# 95. POS Key Generation

The bank generates:

```text
prPOS
PubPOS
```

The private key is stored securely.

The public key is published for authentication.

---

# 96. Registration of Other Entities

The CA registers:

- issuing bank,
- payment gateway,
- acquirer bank.

Lattice-based certificates are generated using CA private material.

The purpose is:

> **Securely bind public keys to the identities of participating entities.**

---

# 97. Login and Authentication Phase

A registered user can now perform a contactless payment.

The high-level flow is:

```text
User
 ↓
Wearable
 ↓ NFC
POS
 ↓
Authentication
 ↓
Session key establishment
 ↓
Encrypted payment
```

---

# 98. Step 1: User Authentication at Wearable

The user provides:

```text
IDUi
PINUi
BIO'Ui
```

The wearable runs the fuzzy extractor reproduction function:

```text
bkUi = Rep(BIO'Ui, phUi)
```

It reconstructs the required protected identity and private-key components.

It computes:

```text
Y'Ui
```

and checks:

```text
Y'Ui = YUi
```

If equal:

```text
User authenticated
```

Otherwise:

```text
Payment process terminates
```

---

# 99. Step 2: Message Generation

The wearable generates fresh random lattice values:

```text
(prWD,1, prWD,2)
```

and computes:

```text
RPubWD
```

It then constructs:

```text
Bi
ψwp
Δwp
ηwp
Veri
```

Finally, it sends:

```text
MSG1 =
<PIDUi, TS1, Bi, RPubWD, Veri>
```

to the POS terminal through NFC.

---

# 100. Step 3: POS Verification and Key Generation

The POS receives `MSG1`.

First, it checks timestamp freshness:

```text
|TS1' - TS1| < ΔT
```

Then it computes its own versions of the authentication values.

If:

```text
Ver'i = Veri
```

the wearable's message is authenticated.

The POS then generates fresh randomness and computes:

```text
RPubPOS
```

It derives a session key:

```text
skpw
```

and a verification value:

```text
skvpw
```

Then it sends:

```text
MSG2 =
<RPubPOS, TS2, skvpw>
```

back to the wearable.

---

# 101. Step 4: Session-Key Verification

The wearable validates the freshness of `MSG2`.

It independently computes:

```text
rψwp
rΔwp
rηwp
skwp
skvwp
```

Then checks:

```text
skvwp = skvpw
```

If they match:

```text
skwp = skpw
```

Therefore both sides possess the same session key.

This is the critical transition:

```text
Authenticated wearable
        +
Authenticated POS
        ↓
Shared session key
```

---

# 102. Message Exchange

The authentication messages are:

```text
Wearable                         POS
   │                              │
   │ MSG1                         │
   │─────────────────────────────>│
   │                              │
   │ MSG2                         │
   │<─────────────────────────────│
```

Both are transmitted through the NFC channel.

The session key is then used for secure payment communication.

---

# 103. Payment Information Transmission

After successful authentication, the wearable sends:

- transaction amount,
- currency code,
- transaction type,
- transaction date/time,
- NFC token for issuer bank.

The information is encrypted using the shared session key:

```text
Enc_skwp[Payment Information]
```

---

# 104. POS Processing

The POS decrypts:

```text
Dec_skpw[Payment Information]
```

It verifies the payment information against its database.

Then it connects to the issuing bank using the proposed post-quantum SSL/TLS infrastructure.

---

# 105. Issuer Bank Verification

The issuer bank verifies:

- account details,
- account balance,
- payment amount,
- currency,
- token authenticity.

The bank then communicates with the payment gateway through secure post-quantum SSL/TLS.

---

# 106. Payment Gateway and Acquirer Bank

The payment gateway securely forwards the payment information to the acquirer bank.

The communication is protected using post-quantum SSL/TLS.

The acquirer bank records and processes the transaction.

---

# 107. Final Settlement

The acquirer bank updates the merchant account and sends confirmation to:

- payment gateway,
- issuer bank,
- merchant.

The journey is therefore:

```text
Wearable
   ↓
POS
   ↓
Issuer Bank
   ↓
Payment Gateway
   ↓
Acquirer Bank
   ↓
Merchant
```

with quantum-resistant protection incorporated into the proposed architecture.

---

# 108. Security Properties of the Case Study

The lecture states that the proposed scheme is designed to resist several classical attacks.

### Physical wearable capture

The attacker obtains physical access to the wearable.

### Replay attacks

An attacker attempts to reuse previously captured messages.

### Man-in-the-middle attacks

An attacker attempts to interfere with communication between legitimate parties.

### Impersonation attacks

An attacker attempts to pretend to be a legitimate user or device.

### Privileged-insider attacks

A trusted insider attempts to misuse legitimate access.

### Ephemeral Secret Key Leakage

The scheme is analysed under the Canetti-Krawczyk adversary model for leakage of temporary secrets.

---

# 109. Post-Quantum Security Argument

The lecture states that the scheme is designed to resist attacks exploiting lattice basis reduction because its security depends on the computational difficulty of:

```text
Ring-LWE
```

in high-dimensional environments.

The intended security chain is:

```text
High-dimensional lattice
        ↓
Ring-LWE hardness
        ↓
Lattice-based authentication
        ↓
Resistance to known quantum attacks
```

---

# 110. Why This Case Study Matters

This example demonstrates that PQC is not only about replacing RSA with another algorithm.

A complete quantum-safe system may require:

```text
PQC mathematics
+
Identity management
+
Biometrics
+
Key generation
+
Certificates
+
Session-key establishment
+
Secure communication
+
Payment infrastructure
```

The cryptographic algorithm is only one component of the overall security architecture.

---

# 111. The Full Lecture Story

The lecture can be understood as one continuous progression:

```text
Classical cryptography
        ↓
Substitution
        ↓
Transposition
        ↓
Modern cryptography
        ↓
Symmetric encryption
        ↓
Public-key cryptography
        ↓
RSA / ElGamal / Diffie-Hellman assumptions
        ↓
Mathematical hardness
        ↓
Quantum computing
        ↓
Shor's algorithm
        ↓
Factoring / discrete logarithm threat
        ↓
Need for PQC
        ↓
Vector spaces
        ↓
Lattices
        ↓
SVP / CVP
        ↓
LWE
        ↓
Ring/Module-LWE
        ↓
Lattice-based cryptography
        ↓
Kyber / Dilithium / NTRU / Falcon
        ↓
Quantum-safe authentication
        ↓
IoT contactless payment
```

---

# 112. Classical vs Post-Quantum Cryptography

| Feature | Classical public-key crypto | Post-quantum crypto |
|---|---|---|
| Examples | RSA, ECC, DSA | Kyber, Dilithium, lattice/code/hash based schemes |
| Security foundation | Factoring / discrete logarithms and related assumptions | Lattices, codes, hashes and other quantum-resistant assumptions |
| Quantum threat | Shor threatens RSA/ECC-type foundations | Designed against known quantum attacks |
| Hardware | Classical | Classical |
| Mathematical basis | Traditional number theory / algebra | Newer hard problems |
| Main transition challenge | Existing deployment | Migration, interoperability and implementation |

---

# 113. Important Definitions

## Cryptography

The study and practice of protecting information using mathematical techniques.

## Plaintext

Original readable message.

## Ciphertext

Encrypted representation of the message.

## Encryption

Transformation of plaintext into ciphertext.

## Decryption

Recovery of plaintext from ciphertext.

## Symmetric cryptography

Uses a shared secret key.

## Asymmetric cryptography

Uses a public/private key pair.

## Brute-force attack

Exhaustive search through possible keys.

## Computational security

Security based on the practical infeasibility of breaking the scheme within useful cost/time limits.

## DLP

Discrete Logarithm Problem.

## CDHP

Computational Diffie-Hellman Problem.

## Authenticated encryption

A construction combining encryption and verification/authentication operations.

## Hash function

A function mapping arbitrary-length input to fixed-length output.

## Lattice

The set of integer combinations of linearly independent basis vectors.

## SVP

Shortest Vector Problem.

## CVP

Closest Vector Problem.

## LWE

Learning With Errors.

## Ring-LWE

A structured variant of LWE using polynomial rings.

## Post-Quantum Cryptography

Cryptography designed to remain secure against sufficiently powerful quantum computers.

---

# 114. Most Important Mathematical Relationships

### Caesar cipher

```text
C = (P + k) mod 26
```

### RSA

```text
n = pq

φ(n) = (p-1)(q-1)

ed ≡ 1 mod φ(n)

C = M^e mod n

M = C^d mod n
```

### Lattice

```text
L = {Bz | z ∈ Z^n}
```

### LWE

```text
b = As + e mod q
```

### SVP

```text
Find the shortest non-zero v ∈ L
```

### CVP

```text
Find v ∈ L minimizing ||x-v||
```

---

# 115. High-Value Exam Comparisons

## Substitution vs Transposition

| Substitution | Transposition |
|---|---|
| Replaces symbols | Rearranges symbols |
| Character identity changes | Character identity remains |
| Example: Caesar | Example: Rail Fence |
| Example: Monoalphabetic | Example: Columnar |

---

## Symmetric vs Asymmetric

| Symmetric | Asymmetric |
|---|---|
| Shared secret key | Public/private key pair |
| Same secret for encryption/decryption | Different but related keys |
| Fast | Generally more computationally expensive |
| Key distribution is challenging | Easier public-key distribution |
| Used for bulk data encryption | Used for key establishment, signatures and related functions |

---

## SVP vs CVP vs LWE

| Problem | Task |
|---|---|
| SVP | Find shortest non-zero lattice vector |
| CVP | Find lattice vector closest to target |
| LWE | Recover secret from noisy linear equations |

---

# 116. Common Misconceptions

## Misconception 1: A larger key automatically means perfect security

Not necessarily.

Security also depends on:

- mathematical structure,
- implementation,
- randomness,
- protocol design,
- attack model.

---

## Misconception 2: Classical ciphers are insecure only because their keys are small

Key size is one issue.

They are also vulnerable to structural attacks such as frequency analysis.

---

## Misconception 3: Public-key cryptography means the private key is unnecessary

False.

The public key can be distributed, but the private key remains essential for the relevant private operation.

---

## Misconception 4: Quantum computers break every cryptographic algorithm

The lecture's message is more specific.

Quantum algorithms threaten particular mathematical assumptions, especially those behind important classical public-key systems.

---

## Misconception 5: PQC requires a quantum computer

False.

PQC is intended to run on ordinary classical computing hardware.

---

## Misconception 6: Lattice cryptography is based on one single problem

The lecture presents a family of problems:

```text
SVP
CVP
LWE
Ring/Module-LWE
```

Different constructions use structured variants.

---

## Misconception 7: A mathematically secure algorithm automatically gives a secure system

False.

The payment case study demonstrates that secure systems also require:

- identity management,
- secure key storage,
- authentication,
- timestamps,
- certificates,
- protocol design,
- secure implementation.

---

# 117. What to Memorize for an Exam

If time is limited, prioritise these topics:

### Classical cryptography

```text
Substitution
Transposition
Caesar
Monoalphabetic
Rail Fence
Columnar
Frequency analysis
```

### Modern cryptography

```text
Symmetric encryption
Public-key cryptography
Brute-force attack
Computational security
RSA
DLP
CDHP
```

### Authentication and signatures

```text
Authenticated encryption
Hash properties
ElGamal signature
Signature attacks
```

### Quantum threat

```text
BQP
Shor's algorithm
Factorization
Polynomial-time quantum factoring
```

### PQC

```text
Vector spaces
Basis
Lattice
Norms
SVP
CVP
LWE
Ring-LWE / Module-LWE
Lattice-based cryptography
```

### Application

```text
IoT payment architecture
NFC
Threat models
Fuzzy extractor
Registration
Authentication
Session-key generation
Post-quantum SSL/TLS
```

---

# 118. Exam-Oriented Questions and Answers

## Q1. What is a substitution technique?

A substitution technique replaces plaintext symbols with other letters, numbers, symbols or bit patterns according to a substitution rule.

---

## Q2. What is a transposition technique?

A transposition technique keeps the plaintext symbols unchanged but rearranges their positions according to a fixed permutation.

---

## Q3. What is the Caesar cipher?

The Caesar cipher is a substitution cipher in which every plaintext letter is shifted by a fixed key `k` positions.

```text
C = (P + k) mod 26
```

---

## Q4. Why is a monoalphabetic cipher vulnerable to frequency analysis?

Because the same substitution is used throughout the message, the frequency pattern of the underlying language can remain visible in the ciphertext.

---

## Q5. What is a brute-force attack?

A brute-force attack exhaustively tests possible keys until the correct key is found.

---

## Q6. What is computational security?

A scheme is computationally secure when the cost of breaking it exceeds the value of the protected information and the time needed to break it exceeds the useful lifetime of that information.

---

## Q7. What is RSA based on?

RSA's security is closely connected to the difficulty of integer factorization.

---

## Q8. What is the RSA public key?

```text
KU = {e, n}
```

---

## Q9. What is the RSA private key?

```text
KR = {d, n}
```

---

## Q10. What is the RSA encryption equation?

```text
C = M^e mod n
```

---

## Q11. What is the RSA decryption equation?

```text
M = C^d mod n
```

---

## Q12. What is the Discrete Logarithm Problem?

Given:

```text
g^a mod p
```

recover the exponent `a`.

---

## Q13. What is LWE?

Learning With Errors gives an attacker noisy linear equations:

```text
b = As + e mod q
```

and asks the attacker to recover the secret vector `s`.

---

## Q14. What is a lattice?

A lattice is the set of all integer linear combinations of a collection of linearly independent basis vectors.

```text
L = {Bz | z ∈ Z^n}
```

---

## Q15. What is SVP?

SVP asks for the shortest non-zero vector in a lattice.

---

## Q16. What is CVP?

CVP asks for the lattice vector closest to a given target point.

---

## Q17. Why is LWE useful for cryptography?

The added small noise makes recovery of the secret from the public noisy equations computationally difficult.

---

## Q18. What is Shor's algorithm?

Shor's algorithm is a quantum algorithm that can solve integer factorization and discrete logarithm problems efficiently on a sufficiently powerful quantum computer.

---

## Q19. Why does Shor threaten RSA?

RSA depends on the difficulty of recovering information related to the factorization of a large modulus. Shor provides an efficient quantum method for factoring.

---

## Q20. What is post-quantum cryptography?

PQC is cryptography designed to resist attacks from sufficiently powerful quantum computers while running on classical computing hardware.

---

## Q21. What is a fuzzy extractor?

A fuzzy extractor derives stable secret information from biometric data while allowing limited differences between biometric measurements.

---

## Q22. What are the three adversary models in the payment case study?

1. Dolev-Yao
2. Honest-but-Curious
3. Canetti-Krawczyk

---

## Q23. What is the purpose of the session key in the payment system?

The session key provides a shared secret between the wearable and POS terminal for protecting subsequent payment communication.

---

# 119. One-Page Revision Sheet

```text
CRYPTOGRAPHY
│
├── Classical
│   ├── Substitution
│   │   ├── Caesar
│   │   └── Monoalphabetic
│   │
│   └── Transposition
│       ├── Rail Fence
│       └── Columnar
│
├── Modern
│   ├── Symmetric
│   └── Asymmetric
│       └── RSA / ElGamal
│
├── Security
│   ├── Brute force
│   ├── DLP
│   ├── CDHP
│   └── Hashing
│
├── Quantum Threat
│   └── Shor
│       └── Factoring / discrete logs
│
└── PQC
    │
    └── Lattice Cryptography
        ├── SVP
        ├── CVP
        ├── LWE
        ├── Ring/Module-LWE
        │
        ├── Kyber
        ├── Dilithium
        ├── NTRU
        └── Falcon
```

---

# 120. Final Mental Model

The entire lecture can be remembered through one story.

Imagine Alice and Bob communicating over a public channel.

At first:

```text
Classical ciphers
```

hide the message by changing or rearranging symbols.

As attackers become stronger, cryptography moves toward:

```text
Mathematical hardness
```

Modern systems such as RSA rely on mathematical problems such as:

```text
Factoring
Discrete logarithms
```

Then quantum computing arrives.

```text
Quantum computer
       ↓
Shor's algorithm
       ↓
Factoring / discrete logarithms
       ↓
Classical public-key assumptions threatened
```

So cryptographers need a different mathematical foundation.

That leads to:

```text
Vector spaces
       ↓
Basis
       ↓
Lattices
       ↓
SVP / CVP
       ↓
LWE
       ↓
Ring/Module-LWE
       ↓
Lattice-based cryptography
       ↓
Post-quantum cryptography
```

Finally, the lecture shows how those ideas can become a complete system:

```text
Lattice mathematics
       +
Biometrics
       +
Fuzzy extractor
       +
Certificate Authority
       +
NFC
       +
Session-key establishment
       +
Secure payment protocols
       +
Post-quantum TLS
       ↓
Quantum-resistant IoT payment architecture
```

> **The central lesson is that post-quantum cryptography is not simply a new encryption algorithm. It is a shift in the mathematical foundations used to build secure systems for a future in which today's public-key assumptions may no longer hold.**

---

# Final 10 Things to Remember

1. **Substitution changes symbols; transposition changes positions.**
2. **Classical ciphers are vulnerable to cryptanalysis and brute force.**
3. **Symmetric cryptography uses shared secret keys.**
4. **Public-key cryptography uses mathematically related public and private keys.**
5. **RSA depends on mathematical hardness closely related to integer factorization.**
6. **DLP and CDHP are important classical public-key hardness assumptions.**
7. **Shor's algorithm threatens factoring and discrete-log-based cryptography.**
8. **PQC must use mathematical problems without known efficient quantum solutions.**
9. **Lattices, SVP, CVP and LWE form an important foundation for lattice-based PQC.**
10. **A real quantum-safe system requires more than an algorithm: it requires secure protocols, authentication, key management, implementation and system architecture.**

---

## Source

These notes are based on the uploaded lecture:

**Understanding Modern Post-Quantum Algorithms**  
Dr. Debasis Giri, MAKAUT  
24 September 2026

The lecture spans 115 pages and covers cryptography fundamentals, classical and public-key cryptography, authenticated encryption, Shor's algorithm, lattice mathematics, LWE, lattice-based cryptography, and a quantum-resistant IoT contactless payment case study. fileciteturn10file7L1-L10
