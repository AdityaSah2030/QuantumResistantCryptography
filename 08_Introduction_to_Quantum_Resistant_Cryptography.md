# Lecture 08: Introduction to Quantum-Resistant Cryptography

> **Bootcamp:** Build Secure Futures: Bootcamp on Quantum-Resistant Cryptography  
> **Lecture:** Introduction to Quantum-Resistant Cryptography  
> **Speaker:** Abhishek Sen, Global Lead, Application Security Consultation & Architecture, PwC  
> **Date:** 23 September 2026  
> **Core story:** Quantum Computing → Threat to Current Cryptography → PQC → New Mathematical Foundations → ML-KEM → ML-DSA → Industry Adoption → Enterprise Migration

---

# 1. The Big Picture

This lecture answers a very practical question:

> **If quantum computers can eventually break some of today's cryptography, what do we replace it with, and how does the real-world migration happen?**

The lecture is intentionally practical rather than deeply mathematical.

The story is:

```text
Today's Internet
      ↓
Uses RSA, ECC, ECDH, ECDSA and other cryptography
      ↓
Quantum computers change the security assumptions
      ↓
Shor threatens important public-key systems
      ↓
Grover weakens some symmetric cryptography
      ↓
We need new cryptographic foundations
      ↓
Post-Quantum Cryptography (PQC)
      ↓
ML-KEM + ML-DSA + other families
      ↓
Hybrid deployment
      ↓
Enterprise migration + crypto-agility
```

The important message is:

> **PQC is not a distant science-fiction technology. It is an engineering and migration problem that the industry is already working on.**

---

# 2. First: What Is Quantum Computing?

A quantum computer is not simply a faster version of a classical computer.

It computes using the principles of quantum mechanics.

## Classical computer

A classical computer uses **bits**.

A bit is:

```text
0 OR 1
```

The physical implementation can use things such as transistor and voltage states.

Classical computers are general-purpose machines used for:

- browsers
- spreadsheets
- applications
- games
- servers
- everyday computing

---

## Quantum computer

A quantum computer uses **qubits**.

A qubit can exist in a quantum combination of states.

The lecture introduces three important ideas:

```text
Superposition
Entanglement
Interference
```

These allow quantum algorithms to solve certain problems very differently from classical algorithms.

---

# 3. Classical vs Quantum Computers

| Feature | Classical Computer | Quantum Computer |
|---|---|---|
| Basic unit | Bit | Qubit |
| State | 0 or 1 | Quantum combination of 0 and 1 |
| Processing | Classical computation | Quantum operations |
| Physical examples | Transistors, voltage states | Photons, trapped ions, superconducting circuits |
| Error behavior | Relatively stable | Highly sensitive to noise |
| Error correction | Conventional techniques | Requires substantial quantum error correction |
| Good at | General-purpose computing | Specific hard problems, simulation, optimization |

### Important

A quantum computer is **not** expected to replace your laptop.

It is useful for particular classes of problems where quantum algorithms provide an advantage.

---

# 4. Qubits and Superposition

A classical bit is easy to imagine as a coin lying flat:

```text
Heads → 1
Tails → 0
```

It has a definite state.

The lecture uses a **spinning coin** as an analogy for a qubit.

A spinning coin represents a mixture of possible outcomes until it is measured.

Conceptually:

```text
Classical bit

0 OR 1


Quantum bit

0 AND 1
in a quantum superposition
```

For `n` qubits, the quantum state can involve:

```text
2ⁿ basis states
```

For example:

```text
50 qubits
      ↓
2⁵⁰ possible basis states
      ↓
more than 1 quadrillion
```

### Important clarification

This does **not** mean a quantum computer can simply read all `2ⁿ` answers after computation.

The useful result comes from designing quantum operations so that **interference** increases the probability of the desired answer.

---

# 5. Entanglement

The second major concept is **entanglement**.

Entanglement creates correlations between quantum systems that do not have a classical equivalent.

The lecture uses the analogy of two magically linked coins.

Conceptually:

```text
Qubit A  ↔  Qubit B
```

When one is measured, the outcome of the other is correlated with it according to their entangled state.

### Why does it matter for computing?

Entanglement allows quantum algorithms to manipulate multiple qubits together in ways that classical systems cannot reproduce directly.

It is one of the resources used by quantum algorithms to obtain computational advantages.

---

# 6. Interference

Superposition alone is not enough.

The crucial idea is **interference**.

Quantum algorithms can be designed so that:

```text
Wrong possibilities
       ↓
Cancel each other

Useful possibilities
       ↓
Reinforce each other
```

Then measurement is more likely to produce the useful result.

So the three concepts fit together:

```text
Superposition
      +
Entanglement
      +
Interference
      ↓
Quantum computational advantage
```

---

# 7. Why Does This Matter to Cybersecurity?

Here is the connection:

```text
Quantum computers
      ↓
Efficient algorithms for certain mathematical problems
      ↓
Some mathematical assumptions used by cryptography
become unsafe
```

Two especially important algorithms are:

```text
Shor's Algorithm
Grover's Algorithm
```

They have different effects.

---

# 8. Shor's Algorithm: The Major Public-Key Threat

Shor's algorithm is the most important quantum algorithm from the perspective of today's public-key cryptography.

It provides an efficient quantum approach to:

- integer factoring
- discrete logarithm related problems

This threatens systems such as:

```text
RSA
ECC
ECDSA
ECDH
Diffie-Hellman
```

---

# 9. Why Does Shor Threaten RSA?

RSA relies on an important asymmetry.

Multiplying two large primes is easy:

```text
p × q = n
```

But recovering the original primes from a sufficiently large `n` is classically difficult.

For example:

```text
p = large prime
q = large prime

n = p × q
```

Everyone can know:

```text
n
```

but finding:

```text
p and q
```

is supposed to be computationally difficult.

That difficulty is part of the foundation of RSA.

---

# 10. What Shor Changes

Shor's algorithm gives a quantum computer an efficient route to factoring.

Conceptually:

```text
RSA security
     ↓
Difficulty of factoring
     ↓
Shor
     ↓
Efficient quantum factoring
     ↓
RSA security assumption fails
```

The lecture describes this as a quantum shortcut.

The important distinction is:

> Shor does not mean that RSA can be broken instantly today. It means that a sufficiently capable quantum computer would remove the classical computational-hardness assumption that RSA depends on.

---

# 11. ECC Is Also at Risk

Elliptic-curve cryptography is not a quantum-safe replacement for RSA.

Systems based on elliptic-curve discrete logarithms are also threatened by Shor.

The lecture specifically identifies:

```text
ECC
ECDSA
ECDH
```

as vulnerable to Shor.

The lecture also notes that ECC can be broken with fewer qubits than RSA, despite ECC often being considered a more modern classical public-key technology.

---

# 12. What About Diffie-Hellman?

Classic Diffie-Hellman key exchange also relies on a discrete-logarithm-type hardness assumption.

Therefore:

```text
Diffie-Hellman
      ↓
Shor
      ↓
Quantum vulnerability
```

This matters because key exchange is a fundamental part of secure communication protocols such as TLS.

---

# 13. Shor vs Grover

Do not confuse their effects.

```text
SHOR
↓
Threatens important public-key cryptography
↓
RSA / ECC / ECDH / ECDSA / DH


GROVER
↓
Speeds up brute-force search
↓
Weakens symmetric cryptography
```

This distinction is one of the most important things to remember from the lecture.

---

# 14. Grover's Algorithm

Grover's algorithm provides a quadratic speed-up for unstructured search.

Suppose there are:

```text
N
```

possible keys.

A classical brute-force search requires roughly:

```text
O(N)
```

attempts.

Grover reduces this to approximately:

```text
O(√N)
```

quantum oracle queries.

Therefore:

```text
Classical
N

Quantum with Grover
√N
```

---

# 15. What Does This Mean for Key Strength?

A useful simplified interpretation is:

> Grover approximately halves the effective security level of a symmetric key against an idealized quantum brute-force attack.

The lecture gives:

```text
AES-128
≈ 64-bit effective strength

AES-256
≈ 128-bit effective strength
```

Therefore the practical recommendation in the lecture is:

```text
Move toward AES-256
```

where appropriate.

---

# 16. Symmetric Cryptography Is Not "Broken" Like RSA

This distinction matters.

### RSA/ECC

Shor attacks the underlying mathematical problem.

So the security foundation is fundamentally compromised by a sufficiently capable quantum computer.

### AES

Grover speeds up search.

It does not provide the same kind of complete collapse of the primitive.

Therefore:

```text
Public-key crypto
→ major replacement problem

Symmetric crypto
→ increase security margin / key size
```

---

# 17. What About SHA-256 and SHA-3?

The lecture presents SHA-256 / SHA-3 as adequate at their current output sizes in the context discussed.

Grover provides a quadratic search-related speed-up, but this does not mean existing hash functions simply become useless.

The larger lesson is:

> Quantum resistance depends on the type of cryptographic primitive and the mathematical assumptions underneath it.

---

# 18. Quick Threat Table

| Cryptography | Used for | Quantum algorithm | Lecture's status |
|---|---|---|---|
| RSA | Key exchange, signatures, certificates | Shor | Fully broken by sufficiently capable quantum computing |
| ECC / ECDSA / ECDH | TLS, mobile, blockchain, IoT | Shor | Fully broken by sufficiently capable quantum computing |
| Diffie-Hellman | Key exchange | Shor | Vulnerable |
| AES-128 | Symmetric encryption | Grover | Weakened |
| AES-256 | Symmetric encryption | Grover | Still considered strong |
| SHA-256 / SHA-3 | Hashing, integrity, signatures | Grover-related effects | Adequate at current output sizes in the lecture |

---

# 19. The Most Important Practical Threat: Harvest Now, Decrypt Later

This is one of the central ideas of the lecture.

Imagine an attacker intercepts encrypted traffic today.

They cannot decrypt it now.

Instead, they:

```text
1. Capture encrypted data
        ↓
2. Store it
        ↓
3. Wait for sufficiently capable quantum computers
        ↓
4. Use quantum algorithms
        ↓
5. Decrypt the old data
```

This is known as:

> **Harvest Now, Decrypt Later**

---

# 20. Why Is This a Problem Today?

Suppose information must remain confidential for:

```text
10 years
20 years
30 years
```

Examples include:

- medical records
- government information
- national-security information
- intellectual property
- long-term financial information

An attacker does not necessarily need to break the encryption today.

They can collect the ciphertext today and wait.

Therefore:

```text
Q-Day is in the future
        ≠
Quantum risk starts in the future
```

For long-lived confidential data, the risk can already exist today.

---

# 21. Two "Quantums" That You Must Not Confuse

The lecture makes an important distinction between:

```text
Quantum Cryptography
```

and:

```text
Post-Quantum Cryptography
```

They are not the same thing.

---

# 22. Quantum Cryptography / QKD

Quantum Key Distribution, or QKD, uses quantum physics itself to distribute keys.

Its security is based on physical properties of quantum systems.

It requires specialized infrastructure such as:

- quantum hardware
- specialized fiber
- satellite links in some approaches

It is mainly suited to dedicated, high-assurance communication links.

---

# 23. Post-Quantum Cryptography / PQC

PQC takes a completely different approach.

Instead of using quantum hardware:

```text
PQC
↓
New mathematical problems
↓
Designed to resist classical and quantum attacks
↓
Runs on ordinary computers
```

That means PQC can be integrated into:

- browsers
- phones
- servers
- TLS
- VPNs
- applications
- cloud infrastructure

---

# 24. QKD vs PQC

| Feature | QKD / Quantum Cryptography | PQC |
|---|---|---|
| Basic idea | Uses quantum physics | Uses new mathematical constructions |
| Hardware | Specialized quantum hardware | Ordinary classical hardware |
| Security basis | Physical properties | Computational hardness |
| Deployment | Specialized links | Software/protocol upgrades |
| Scalability | More difficult | Much more practical for internet systems |
| Practical role | Dedicated high-assurance links | Broad internet and enterprise migration |

### The core idea

For most systems students and engineers will actually build:

```text
PQC
```

is the practical focus.

---

# 25. Building New Mathematical Foundations

The lecture describes the NIST PQC standardization process as an eight-year global competition.

The timeline presented is:

```text
2016 → 2024
```

Researchers submitted:

```text
82 candidate algorithms
```

The candidates went through:

- public cryptanalysis
- attack attempts
- performance evaluation
- standardization analysis

The lecture groups the surviving approaches into three major mathematical families:

```text
Lattice-based
Hash-based
Code-based
```

---

# 26. Lattice-Based Cryptography

Lattice-based cryptography is the dominant family highlighted in the lecture.

The underlying ideas include difficult problems such as:

```text
Shortest Vector Problem
Learning With Errors (LWE)
```

You do not need to understand the full mathematics yet.

A useful mental model is:

> Imagine a huge, high-dimensional grid where a hidden structure is buried inside carefully designed noise. Recovering the hidden information is believed to be difficult even for quantum computers.

---

# 27. Why Industry Likes Lattice-Based Cryptography

The lecture highlights:

- relatively small keys and ciphertexts
- fast computation
- practical deployment
- suitability for protocols such as TLS

The two major standards built from this family in the lecture are:

```text
ML-KEM
ML-DSA
```

---

# 28. Hash-Based Cryptography

The lecture introduces:

> **SLH-DSA**

Standard:

```text
FIPS 205
```

Derived from:

```text
SPHINCS+
```

The basic security foundation comes from hash functions.

### Advantage

It relies on relatively conservative cryptographic assumptions.

### Trade-off

Signatures are much larger and slower than lattice-based alternatives.

The lecture mentions applications such as:

- firmware signing
- root certificate authorities

where the conservative security foundation can be valuable.

---

# 29. Code-Based Cryptography

The lecture discusses:

> **HQC**

as a code-based KEM selected by NIST in March 2025 as a backup KEM.

Code-based cryptography is based on problems related to decoding error-correcting codes without knowing their hidden structure.

The family has roots going back to the 1970s, including Classic McEliece.

### Main trade-off

Historically, code-based systems can have very large public keys.

### Why keep them?

Diversification.

The idea is:

```text
Do not rely on only one mathematical family.
```

If a major future breakthrough affects lattice-based cryptography, having a well-developed alternative family is valuable.

---

# 30. The Three Families to Remember

```text
Lattice-based
    ↓
ML-KEM
ML-DSA

Hash-based
    ↓
SLH-DSA

Code-based
    ↓
HQC
```

A useful mental model:

```text
Lattice = primary practical family

Hash = conservative backup

Code = diversification / backup
```

---

# 31. ML-KEM: The Quantum-Safe Key Exchange

Now we reach the first major algorithm.

> **ML-KEM**

Official standard:

```text
FIPS 203
```

It is based on:

```text
CRYSTALS-Kyber
```

ML-KEM is a **Key Encapsulation Mechanism**, or KEM.

---

# 32. What Is a KEM?

A KEM solves a very specific problem:

> **How can two parties who have never met establish a shared secret over an insecure channel?**

Once they share that secret, they can use fast symmetric encryption such as AES to protect the actual data.

Conceptually:

```text
Public network
      ↓
Key establishment
      ↓
Shared secret
      ↓
AES / symmetric authenticated encryption
      ↓
Actual data
```

This is the role that public-key mechanisms such as RSA-based key establishment and ECDH have historically played in protocols such as TLS.

---

# 33. ML-KEM Security Levels

The lecture presents three ML-KEM variants:

| Variant | Approximate security comparison |
|---|---|
| ML-KEM-512 | ≈ AES-128 |
| ML-KEM-768 | ≈ AES-192 |
| ML-KEM-1024 | ≈ AES-256 |

The lecture highlights:

```text
ML-KEM-768
```

as a practical default in many deployment discussions.

---

# 34. ML-KEM: Bob and Alice

The easiest way to understand ML-KEM is as a five-step story.

```text
Bob generates keys
        ↓
Bob publishes public key
        ↓
Alice encapsulates a secret
        ↓
Bob decapsulates
        ↓
Both have the same shared secret
```

Let's go through it.

---

# 35. Step 1: Bob Generates Keys

Bob generates:

```text
Public key
Private key
```

using the ML-KEM algorithm.

The mathematical foundation is lattice-based cryptography.

---

# 36. Step 2: Bob Shares His Public Key

Bob can send his public key openly.

It is okay for an attacker to see it.

```text
Bob
 │
 │ Public key
 ▼
Alice
```

The security depends on the difficulty of recovering the private information from the public information.

---

# 37. Step 3: Alice Encapsulates

Alice uses Bob's public key to encapsulate a randomly generated secret.

The result is essentially:

```text
Shared secret
+
Ciphertext
```

Alice sends the ciphertext to Bob.

An attacker can see the ciphertext but should not be able to recover the secret.

---

# 38. Step 4: Bob Decapsulates

Bob uses his private key to process the ciphertext.

He recovers:

```text
Same shared secret
```

that Alice generated.

---

# 39. Step 5: Shared Secret

Now:

```text
Alice ─────────────── Shared Secret
                       │
Bob   ─────────────── Shared Secret
```

The shared secret can become the key for symmetric authenticated encryption.

For example:

```text
ML-KEM
   ↓
Shared secret
   ↓
AES
   ↓
Encrypted conversation
```

---

# 40. ML-KEM Analogy

Imagine Bob creates a special box.

His:

```text
Public key
```

describes the public locking mechanism.

His:

```text
Private key
```

contains the secret information needed to open it efficiently.

Alice:

```text
Creates secret
     ↓
Locks it using Bob's public key
     ↓
Sends ciphertext
```

Bob:

```text
Uses private key
     ↓
Opens / decapsulates
     ↓
Recovers same secret
```

The mathematical security comes from lattice problems rather than physical locks.

---

# 41. ML-KEM Runs on Normal Computers

This is extremely important.

You do **not** need a quantum computer to use PQC.

ML-KEM is:

```text
Post-quantum
```

not:

```text
Quantum hardware cryptography
```

It runs using classical computing hardware.

That is why it can be integrated into existing systems.

---

# 42. ML-KEM Is Already Being Deployed

The lecture gives several industry examples.

It discusses:

- Chrome / Google
- Cloudflare
- AWS

and describes hybrid post-quantum key exchange already being used in parts of the web ecosystem.

The migration pattern is generally:

```text
Classical ECDH
      +
ML-KEM
      ↓
Hybrid key establishment
```

This allows systems to gain quantum resistance while retaining compatibility with existing infrastructure.

---

# 43. Why Hybrid Deployment?

Instead of immediately removing classical cryptography:

```text
Classical
   ↓
delete
   ↓
PQC
```

organizations can use:

```text
Classical
    +
PQC
    ↓
Combined protection
```

This provides a gradual migration path.

The exact security property depends on the construction and protocol, but the goal is that the connection remains protected as long as the relevant hybrid construction retains at least one sound security component.

---

# 44. ML-DSA: Quantum-Safe Digital Signatures

The second major algorithm is:

> **ML-DSA**

Official standard:

```text
FIPS 204
```

It is based on:

```text
CRYSTALS-Dilithium
```

Its purpose is different from ML-KEM.

Remember:

```text
ML-KEM
→ Key establishment

ML-DSA
→ Digital signatures
```

---

# 45. What Does a Digital Signature Do?

A digital signature provides three important properties in the lecture:

## 1. Proves WHO

It helps establish who signed a message or file.

## 2. Proves INTACT

It detects whether the signed content has been modified.

## 3. Non-repudiation

It provides evidence associated with the signer so that the signer cannot simply deny having signed it, subject to the surrounding legal and technical context.

---

# 46. Why Do We Need Quantum-Resistant Signatures?

Current digital signature systems commonly use algorithms such as:

```text
RSA
ECDSA
```

These are vulnerable to Shor's algorithm in a sufficiently capable quantum setting.

Therefore we need quantum-resistant signature algorithms.

ML-DSA is one of the main standardized approaches.

---

# 47. ML-DSA: Signing and Verification

The process is:

```text
1. Generate key pair
        ↓
2. Sign message using private key
        ↓
3. Publish message + signature
        ↓
4. Verify using public key
```

---

# 48. Step 1: Generate Keys

The signer generates:

```text
Public key
Private key
```

using lattice-based mathematics.

---

# 49. Step 2: Sign

The signer uses:

```text
Private key
+
Message
+
Randomness
```

to generate a digital signature.

The signature is mathematically tied to the exact message.

---

# 50. Step 3: Share

The sender can publish:

```text
Message
Signature
Public key
```

These can travel together.

---

# 51. Step 4: Verify

Anyone with the public key can verify the signature.

If the message changes:

```text
Original message
        ↓
Modification
        ↓
Signature verification fails
```

Therefore the signature provides both authentication and integrity checking.

---

# 52. ML-DSA Analogy: The Wax Seal

Think of ML-DSA as a mathematical wax seal.

The signer has the private key.

They use it to create a seal on the exact message.

Anyone can inspect the seal using the public key.

But creating a valid new seal without the private key should be computationally infeasible, including for a sufficiently capable quantum attacker under the scheme's security assumptions.

---

# 53. Where Can ML-DSA Be Used?

The lecture gives several practical applications.

## TLS Certificates

Used for website and code authenticity.

Browser PKI is actively exploring and deploying post-quantum certificate approaches.

## Software and Firmware Signing

Important for:

- IoT
- embedded systems
- industrial devices
- operating-system updates

A device can verify that an update has not been tampered with.

## Documents and Contracts

Useful for signed agreements where authenticity and integrity matter.

## Government and Defense PKI

The lecture discusses CNSA 2.0 requirements involving ML-DSA.

---

# 54. ML-DSA Has a Trade-Off

PQC is not simply:

```text
Same thing
+
quantum resistance
```

There are engineering trade-offs.

The lecture highlights that ML-DSA signatures are much larger than today's ECDSA signatures.

Approximate sizes presented:

```text
ML-DSA signature
≈ 2.4–4.6 KB

ECDSA signature
≈ 64–100 bytes
```

This matters especially for:

- IoT
- mobile systems
- bandwidth-constrained environments

The larger size is an engineering trade-off, not automatically a flaw.

---

# 55. FN-DSA

The lecture also mentions:

> **FN-DSA / FIPS 206**

as a complementary lattice-based signature direction intended to provide smaller signatures.

The broader lesson is:

```text
PQC standardization is continuing
```

and different schemes can target different engineering constraints.

---

# 56. NIST PQC Timeline

The lecture gives this timeline:

```text
2016
↓
NIST PQC competition begins

2016–2024
↓
Global evaluation
82 candidates
Public cryptanalysis

August 2024
↓
FIPS 203 / 204 / 205 finalized

March 2025
↓
HQC selected as backup KEM

2026–27
↓
FN-DSA / FIPS 206 finalization discussed

Ongoing
↓
Real-world migration
```

The important point is that standardization is not the end of the story.

It is the beginning of large-scale deployment.

---

# 57. Industry Is Already Moving

The lecture gives examples across the technology industry.

## Google / Chrome

The lecture describes ML-KEM hybrid key exchange as enabled in Chrome's browser handshake and discusses Google's longer-term PQC migration target.

## Cloudflare

The lecture describes a growing portion of TLS 1.3 traffic using post-quantum key agreement.

## AWS

The lecture discusses hybrid PQ-TLS and integration of PQC into services such as:

- KMS
- Certificate Manager
- Secrets Manager

## Microsoft / Azure

The lecture discusses ML-KEM / ML-DSA integration into SymCrypt and Microsoft's migration plans.

## Apple

The lecture discusses Apple's PQ3 protocol for iMessage.

## Signal

The lecture describes Signal as already using post-quantum-secured messaging mechanisms.

---

# 58. Banking, Government and Regulation

The migration is not limited to technology companies.

The lecture highlights:

### Financial services

Examples include:

- G7 cyber guidance
- SWIFT planning
- Bank of Israel preparedness
- Hong Kong Monetary Authority preparedness work
- JPMorgan Chase quantum research

### Government and defense

The lecture discusses:

- NSA CNSA 2.0
- US federal requirements and timelines
- EU NIS Cooperation Group roadmap

The broader lesson:

> Regulatory and organizational pressure is increasingly turning PQC from a research topic into an infrastructure planning problem.

---

# 59. Why Organizations Should Not Simply "Switch to PQC"

The algorithms themselves are not necessarily the hardest part.

The hard part is discovering:

```text
Where is cryptography being used?
```

A large organization may have cryptography embedded in:

- applications
- TLS
- certificates
- APIs
- VPNs
- SSH
- PKI
- HSMs
- cloud services
- mobile apps
- IoT devices
- code signing
- databases
- third-party software

Therefore:

```text
PQC migration
=
Architecture problem
+
Inventory problem
+
Engineering problem
+
Governance problem
```

---

# 60. Hybrid Cryptography as the Migration Path

The lecture describes hybrid cryptography as:

> **Not a switch, but a blend.**

Instead of immediately replacing classical cryptography everywhere:

```text
Classical crypto
       +
PQC
       ↓
Hybrid protection
```

This allows organizations to:

- gain operational experience
- test interoperability
- measure performance
- reduce migration risk
- gradually move toward PQC-only configurations where appropriate

---

# 61. The PQC Migration Lifecycle

The lecture presents six stages.

```text
1. Discover
       ↓
2. Classify
       ↓
3. Prioritize
       ↓
4. Test
       ↓
5. Hybridize
       ↓
6. Migrate & Monitor
```

This is one of the most important frameworks in the lecture.

---

# 62. Stage 1: Discover

Find every use of vulnerable cryptography.

Look for:

```text
RSA
ECC
ECDH
ECDSA
DH
```

across:

- certificates
- PKI
- TLS
- SSH
- VPN
- HSM
- code signing
- APIs
- applications

The first question should NOT be:

> "Where do we install ML-KEM?"

It should be:

> **"Where are we using cryptography?"**

---

# 63. Stage 2: Classify

For each cryptographic dependency, ask:

```text
What data does it protect?
How sensitive is the data?
How long must it remain confidential?
Which vulnerable algorithms are involved?
```

This connects technical cryptography with business risk.

---

# 64. Stage 3: Prioritize

The lecture gives a useful risk model:

```text
Risk =
Cryptographic exposure
×
Data sensitivity
×
Data lifetime
×
Migration complexity
```

This helps determine what should be migrated first.

For example:

```text
Highly sensitive
+
long-lived data
+
vulnerable public-key crypto
=
high priority
```

---

# 65. Stage 4: Test

Pilot PQC in real systems such as:

- TLS
- APIs
- VPN
- PKI
- HSM
- cloud infrastructure
- application frameworks
- mobile
- IoT

Measure things such as:

- latency
- bandwidth
- compatibility
- key sizes
- signature sizes
- interoperability

---

# 66. Stage 5: Hybridize

Where appropriate:

```text
Classical
+
PQC
```

Run them together during the migration period.

This gives teams real-world experience without requiring an immediate irreversible switch.

---

# 67. Stage 6: Migrate and Monitor

Move toward a PQC-enabled architecture.

But migration does not end with deployment.

Continue monitoring because:

```text
Legacy crypto
will not disappear overnight.
```

New vulnerabilities, new standards and new implementation requirements can also appear.

---

# 68. Cryptographic Bill of Materials: CBOM

A major practical concept introduced in the lecture is:

> **Cryptographic Bill of Materials**

Think of a CBOM as:

```text
Software Bill of Materials
```

but specifically for cryptography.

Instead of simply knowing:

```text
Application X exists
```

you want to know:

```text
Application X
    ↓
Uses ECDSA P-256
    ↓
For signing
    ↓
High PQ risk
```

---

# 69. Example CBOM

| Application | Algorithm / Key Size | Purpose | PQ Risk |
|---|---|---|---|
| Internet banking | ECDSA P-256 | Web signing | High |
| API gateway | RSA-2048 | TLS | High |
| VPN | RSA / ECDH | Key exchange | High |
| Code signing | RSA-2048 | Software integrity | High |
| Database | AES-256 | Data encryption | Low |

The important point is:

> **You cannot migrate what you cannot find.**

---

# 70. Crypto-Agility

Another major engineering concept is:

> **Crypto-agility**

Crypto-agility means being able to change:

- cryptographic algorithms
- protocols
- keys
- certificates

without redesigning the entire application.

---

# 71. Bad Architecture

Imagine:

```text
Application
     ↓
Hard-coded RSA
     ↓
Database
```

If RSA must be replaced, the application itself may need substantial redesign.

That is expensive and risky.

---

# 72. Better Architecture

Instead:

```text
Application
     ↓
Crypto abstraction layer
     ↓
Crypto provider
     ↓
Algorithm
```

Then:

```text
RSA
 ↓
ECC
 ↓
PQC
```

can become largely a configuration or provider-level change rather than a complete application redesign.

---

# 73. Why Crypto-Agility Matters Beyond PQC

Even after the current PQC migration finishes:

```text
Algorithms will change again.
```

Therefore the real engineering lesson is not:

> "Build a system specifically for ML-KEM."

It is:

> **Build systems where cryptographic algorithms are replaceable components.**

---

# 74. PQC Is an Architecture Problem

PQC affects far more than cryptographic libraries.

It touches:

## Applications and Networks

- application architecture
- network architecture
- IAM
- PKI

## Cloud and DevSecOps

- cloud services
- CI/CD pipelines
- container signing
- secrets management

## Hardware and Data

- HSMs
- embedded devices
- data protection
- long hardware refresh cycles

## Governance

- vendor management
- compliance
- incident response
- contracts
- security policies

Therefore PQC migration needs to be treated as a broader transformation rather than a simple library upgrade.

---

# 75. What Should an Application Security Engineer Ask?

During a security assessment, ask:

### Cryptography and TLS

```text
What asymmetric algorithms are being used?

Where is RSA embedded?

Where is ECC embedded?

What symmetric algorithms are being used?

What key sizes are being used?

Which TLS version is in use?

Which key-exchange algorithm is being used?

Which signature algorithms are being used?

How are certificates issued?

Where are private keys stored?

What is the certificate lifecycle?
```

---

# 76. Questions Teams Often Forget

Also ask:

```text
Does the application framework support PQC?

Do the libraries support algorithm agility?

Does the cloud provider support PQC?

Which cryptographic operations does the cloud provider manage?

How long must this data remain confidential?
```

That final question is particularly important because it connects directly to:

```text
Harvest Now, Decrypt Later
```

---

# 77. A Simple PQC Reference Architecture

The lecture proposes a conceptual architecture:

```text
Users / Internet
       ↓
API Gateway
       ↓
Application
       ↓
Crypto / KMS / HSM
       ↓
Protected Data
```

With:

```text
API Gateway
    ↓
Hybrid TLS

Application
    ↓
Crypto services

ML-KEM
    ↓
Key establishment

ML-DSA
    ↓
Signing / authentication

KMS / HSM
    ↓
Central key management
```

---

# 78. Why Centralize Cryptographic Decisions?

If every application implements cryptography independently:

```text
1000 applications
      ↓
1000 places to migrate
```

But if cryptographic decisions are centralized:

```text
Applications
      ↓
Crypto abstraction / central service
      ↓
KMS / HSM / Gateway
```

then migration can happen at fewer architectural layers.

The lecture summarizes the idea as:

> Change two important layers instead of thousands of applications.

---

# 79. Real Industry Challenges

The lecture highlights four major challenges.

## 1. Crypto-agility

Legacy applications may have cryptographic algorithms hard-coded into protocols and applications.

## 2. Performance and Size

PQC keys and signatures can be much larger.

ML-DSA signatures are significantly larger than ECDSA signatures.

This matters for:

- IoT
- mobile
- constrained networks

## 3. Supply Chain

Your organization may be ready, but:

```text
Vendor
↓
Library
↓
Framework
↓
Dependency
```

may not support PQC.

Therefore the entire software supply chain needs attention.

## 4. Compliance and Regulation

Organizations may face sector-specific requirements and government timelines.

---

# 80. Example: Bank Migration Scenario

The lecture gives a simple scenario.

Suppose a bank currently uses:

```text
RSA-2048 for TLS
ECDSA for signatures
```

A sample migration plan is:

### Months 1–3

```text
Assess
+
Inventory
```

Find where cryptography is used.

### Months 4–9

```text
Pilot
+
Hybrid certificates
+
ML-KEM on public endpoints
```

### Months 10–24

```text
Full migration
+
Third-party integrations
+
Continuous monitoring
```

The important point is that migration is a process, not a one-day algorithm replacement.

---

# 81. Build a Quantum-Safe Chat Application

The lecture proposes a practical student project.

## Objective

Build an end-to-end encrypted chat application using:

```text
ML-KEM
+
ML-DSA
```

for quantum-resistant key establishment and authentication.

---

## Tools

The lecture suggests:

- Open Quantum Safe / liboqs
- Python or JavaScript
- TLS
- hybrid PQC certificates

---

## Deliverables

A good project should include:

```text
Working chat application
+
Documented GitHub repository
+
Performance benchmarks
+
Security analysis
```

Benchmark things such as:

- handshake latency
- message size
- classical vs PQC overhead

---

# 82. What Makes the Project Good?

Use hybrid key exchange so the application can remain interoperable with classical clients where needed.

Measure:

```text
Classical handshake
vs
PQC / hybrid handshake
```

Document:

- key rotation
- failure handling
- fallback behavior
- what happens if PQC negotiation fails
- performance impact

This is a much stronger project than simply calling a PQC library once.

---

# 83. Where PQC Is Relevant

The lecture identifies several sectors.

## Internet

Browsers and CDNs are already deploying hybrid PQC.

## Banking

Long-lived transaction data can be attractive to harvest-now-decrypt-later attacks.

## Healthcare

Medical information may require confidentiality for decades.

## IoT / Industrial

A particularly difficult migration domain because devices can have:

```text
10–20 year lifecycles
```

and constrained hardware.

## Cloud

Major cloud providers are integrating PQC into:

- TLS
- key management
- security infrastructure

## Telecom

Network infrastructure and signaling protocols also need consideration.

---

# 84. Career Opportunities

The lecture highlights several emerging roles.

## Cryptographic Inventory and Discovery

Find where RSA/ECC and other cryptography are embedded.

## PQC Migration Architect

Design hybrid and post-quantum migration strategies across:

- TLS
- VPN
- PKI
- applications

## Crypto-Agility Engineer

Build systems where cryptographic algorithms can be replaced without full redesign.

## Quantum-Safe Security Architect

Work across:

- cryptography
- systems architecture
- risk assessment
- compliance
- migration planning

Other areas include:

- compliance and regulatory work
- standards and protocol engineering
- banking quantum-readiness
- healthcare security
- OT/IoT security

---

# 85. How to Start Learning

The lecture suggests starting with free and practical resources.

## Step 1: Understand the Standards

Start with:

```text
NIST FIPS 203
NIST FIPS 204
NIST FIPS 205
```

Understand what:

```text
ML-KEM
ML-DSA
SLH-DSA
```

actually do.

---

## Step 2: Get Hands-On

Explore:

```text
Open Quantum Safe
liboqs
OQS OpenSSL provider
```

The objective is to move from:

```text
"I know the names"
```

to:

```text
"I can actually use and benchmark these algorithms."
```

---

## Step 3: Learn the Foundations

Build knowledge in:

```text
Cryptography
     ↓
Number theory
     ↓
Linear algebra
     ↓
Probability
     ↓
Quantum computing
     ↓
PQC
     ↓
Secure systems
```

---

# 86. What You Should Remember About PQC

PQC does **not** mean:

```text
Using a quantum computer to encrypt information
```

Instead:

```text
Classical computer
      ↓
New cryptographic mathematics
      ↓
Designed to resist quantum attacks
```

This is why PQC can run on today's computers.

---

# 87. A Simple Mental Model of the Whole Lecture

Remember these four layers.

## Layer 1: The Threat

```text
Quantum computing
      ↓
Shor + Grover
```

## Layer 2: The Replacement

```text
PQC
      ↓
Lattice
Hash
Code
```

## Layer 3: The Main Algorithms

```text
ML-KEM
      ↓
Key establishment

ML-DSA
      ↓
Digital signatures

SLH-DSA
      ↓
Hash-based signatures

HQC
      ↓
Backup KEM / diversification
```

## Layer 4: The Migration

```text
Discover
   ↓
Classify
   ↓
Prioritize
   ↓
Test
   ↓
Hybridize
   ↓
Migrate & Monitor
```

---

# 88. One Diagram for the Entire Lecture

```text
                  QUANTUM COMPUTING
                         │
             ┌───────────┴───────────┐
             │                       │
           SHOR                    GROVER
             │                       │
             ▼                       ▼
     Public-key crypto       Symmetric crypto
             │                       │
      RSA / ECC / DH          AES / hashing
             │                       │
             ▼                       ▼
      Major replacement       Security weakening
             │                       │
             └───────────┬───────────┘
                         │
                         ▼
                 QUANTUM RISK
                         │
                         ▼
          Harvest Now, Decrypt Later
                         │
                         ▼
                 POST-QUANTUM
                  CRYPTOGRAPHY
                         │
          ┌──────────────┼──────────────┐
          │              │              │
       Lattice         Hash           Code
          │              │              │
     ML-KEM          SLH-DSA          HQC
     ML-DSA
          │
          ▼
       HYBRID
    CLASSICAL + PQC
          │
          ▼
     ENTERPRISE
      MIGRATION
          │
    ┌─────┼─────┐
    │     │     │
 Discover  Test  Migrate
    │
    ▼
   CBOM
    │
    ▼
CRYPTO-AGILITY
```

---

# 89. Most Important Concepts to Memorize

| Concept | Simple meaning |
|---|---|
| Qubit | Quantum information unit |
| Superposition | Quantum combination of possible states |
| Entanglement | Quantum correlation between qubits |
| Interference | Reinforces useful outcomes and suppresses others |
| Shor | Quantum algorithm threatening factoring/discrete-log systems |
| Grover | Quantum search algorithm with quadratic speed-up |
| HNDL | Capture encrypted data now and decrypt it later |
| QKD | Quantum physics-based key distribution |
| PQC | Classical cryptography designed to resist quantum attacks |
| Lattice-based | Main PQC family used by ML-KEM and ML-DSA |
| Hash-based | Conservative signature approach such as SLH-DSA |
| Code-based | Alternative family such as HQC |
| KEM | Mechanism for establishing a shared secret |
| ML-KEM | NIST FIPS 203, quantum-resistant KEM |
| ML-DSA | NIST FIPS 204, quantum-resistant signature |
| SLH-DSA | NIST FIPS 205, hash-based signature |
| HQC | Code-based backup KEM discussed in the lecture |
| Hybrid crypto | Classical + PQC used together |
| CBOM | Inventory of cryptographic usage |
| Crypto-agility | Ability to change cryptography without redesign |
| Q-Day | Informal term for availability of a cryptographically relevant quantum computer |

---

# 90. Common Confusions

## Confusion 1: Quantum computing = faster classical computing

**Wrong.**

Quantum computers use a fundamentally different computational model and provide advantages only for certain problems.

---

## Confusion 2: PQC uses quantum computers

**Wrong.**

PQC runs on ordinary classical hardware.

```text
Quantum-resistant
≠
Quantum-powered
```

---

## Confusion 3: QKD and PQC are the same

**Wrong.**

```text
QKD
→ Quantum physics + specialized hardware

PQC
→ Mathematical algorithms + ordinary hardware
```

---

## Confusion 4: Shor and Grover do the same thing

**Wrong.**

```text
Shor
→ Attacks important public-key mathematical assumptions

Grover
→ Speeds up unstructured search
```

---

## Confusion 5: AES is completely broken by quantum computers

**Wrong.**

Grover weakens the effective security level of brute-force search.

It does not break AES in the same way Shor threatens RSA.

The lecture therefore emphasizes AES-256 as a practical stronger choice.

---

## Confusion 6: We can wait until quantum computers exist

**Not for long-lived sensitive data.**

Harvest-now-decrypt-later means encrypted data can be collected today and attacked later.

---

## Confusion 7: PQC migration is just changing one library

**Wrong.**

Cryptography can be embedded throughout:

```text
Applications
Networks
PKI
Certificates
Cloud
HSM
IoT
Code signing
APIs
Vendors
```

Therefore PQC is an architecture and migration problem.

---

# 91. Exam-Oriented Short Answers

## What is Post-Quantum Cryptography?

Post-Quantum Cryptography is a class of cryptographic algorithms designed to remain secure against attacks from sufficiently capable quantum computers while running on ordinary classical computing hardware.

## What is Shor's algorithm?

Shor's algorithm is a quantum algorithm that efficiently solves integer factoring and related discrete-logarithm problems, threatening public-key systems such as RSA and ECC-based cryptography.

## What is Grover's algorithm?

Grover's algorithm provides a quadratic speed-up for unstructured search, changing approximately `O(N)` search complexity to `O(√N)`.

## What is Harvest Now, Decrypt Later?

It is an attack strategy where encrypted information is captured and stored today so that it can potentially be decrypted later when sufficiently capable quantum computers become available.

## What is QKD?

Quantum Key Distribution uses quantum physics to distribute cryptographic keys and requires specialized quantum communication infrastructure.

## What is PQC?

PQC uses new mathematical cryptographic constructions designed to resist both classical and quantum attacks and can run on ordinary computers.

## What is ML-KEM?

ML-KEM is a lattice-based Key Encapsulation Mechanism standardized as NIST FIPS 203 and derived from CRYSTALS-Kyber. It is used for quantum-resistant key establishment.

## What is ML-DSA?

ML-DSA is a lattice-based digital signature scheme standardized as NIST FIPS 204 and derived from CRYSTALS-Dilithium.

## What is SLH-DSA?

SLH-DSA is a stateless hash-based digital signature scheme standardized as NIST FIPS 205 and derived from SPHINCS+.

## What is a KEM?

A Key Encapsulation Mechanism allows parties to establish a shared secret over an insecure channel, after which symmetric cryptography can protect the actual data.

## What is a CBOM?

A Cryptographic Bill of Materials is an inventory of cryptographic algorithms, keys, certificates and cryptographic dependencies used across an organization's systems.

## What is crypto-agility?

Crypto-agility is the ability to replace cryptographic algorithms, protocols, keys and certificates without redesigning the entire application.

## Why is hybrid cryptography used?

Hybrid cryptography combines classical and post-quantum mechanisms to reduce migration risk and maintain compatibility while organizations transition toward PQC.

---

# 92. Final Revision: The 10 Things You Should Be Able to Explain

Before considering this lecture complete, make sure you can explain these without looking at the notes:

1. **Why quantum computers are different from classical computers.**
2. **What superposition, entanglement and interference mean at a basic level.**
3. **Why Shor threatens RSA, ECC and related public-key systems.**
4. **How Grover affects symmetric cryptography.**
5. **What "Harvest Now, Decrypt Later" means.**
6. **The difference between QKD and PQC.**
7. **Why lattice-based cryptography is important.**
8. **What ML-KEM does and how Alice and Bob establish a shared secret.**
9. **What ML-DSA does and how digital signatures work.**
10. **How an organization actually migrates to PQC using discovery, CBOM, hybridization and crypto-agility.**

---

# 93. The Final Story

The lecture can ultimately be reduced to one chain:

```text
Today's Internet
       ↓
RSA / ECC / DH protect important systems
       ↓
Quantum computers introduce new algorithms
       ↓
Shor threatens public-key foundations
       ↓
Grover weakens brute-force security
       ↓
Long-lived encrypted data creates HNDL risk
       ↓
We need quantum-resistant cryptography
       ↓
NIST standardizes PQC
       ↓
Lattice-based schemes become central
       ↓
ML-KEM → key establishment
ML-DSA → digital signatures
SLH-DSA → hash-based signatures
HQC → code-based backup
       ↓
Industry begins deploying hybrid PQC
       ↓
Organizations inventory cryptography
       ↓
CBOM + risk classification
       ↓
Pilot + hybridize
       ↓
Migrate + monitor
       ↓
Crypto-agile architecture
       ↓
Quantum-safe digital infrastructure
```

> **One-line summary:**  
> **Quantum computing threatens important classical public-key cryptography through algorithms such as Shor, while Grover weakens brute-force security; PQC provides new quantum-resistant mathematical foundations, with ML-KEM for key establishment and ML-DSA for signatures, while hybrid deployment, cryptographic inventories and crypto-agile architecture make the real-world migration manageable.**

---

# 94. Final Takeaway

The most important mindset from this lecture is:

```text
Don't ask:
"When will quantum computers break us?"

Ask:
"Where are we vulnerable,
how long must our data remain secure,
and how easily can we change our cryptography?"
```

That changes quantum security from a futuristic prediction into an engineering problem that can be worked on today.
