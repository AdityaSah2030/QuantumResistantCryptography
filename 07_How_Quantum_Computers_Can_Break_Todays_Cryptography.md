# Lecture 07: How Quantum Computers Can Break Today's Cryptography

> **Topic:** Quantum Computing & Cryptography  
> **Core story:** Cryptography → Quantum Computing → Shor → RSA → Grover → Quantum Risk → PQC → Hybrid Security → Migration

---

# 1. The Big Picture

Every time we:

- make a bank payment,
- log in to a website,
- send an encrypted message,
- download software,
- verify a digital signature,
- access sensitive records,

cryptography is working in the background.

The central question of this lecture is:

> **What happens when computers become powerful enough to challenge the mathematical assumptions protecting today's digital systems?**

The answer is not that **all cryptography suddenly becomes useless**.

Instead:

```text
Quantum computing
        ↓
Changes which mathematical problems are considered hard
        ↓
Some current public-key systems become vulnerable
        ↓
We need quantum-resistant alternatives
        ↓
Migration to Post-Quantum Cryptography (PQC)
```

The lecture therefore tells a story from **classical cryptography** to **quantum algorithms** and finally to **quantum-safe security**.

---

# 2. Where Cryptography Is Already Used

Cryptography is described in the lecture as an invisible security layer around everyday digital life.

## The Gatekeeper

Banking sessions, payments and identity checks rely on cryptographic protocols.

## The Bodyguard

Modern messaging uses encryption to protect conversations while they are in transit.

## The Vault

Sensitive records depend on:

- encryption
- access control
- key management

## The Witness

Digital signatures authenticate:

- software
- documents
- transactions
- people

### The important point

For decades, **public-key cryptography** has made large-scale digital trust practical.

Quantum computing changes the assumptions underneath some of these systems.

---

# 3. Classical Computing vs Quantum Computing

## Classical intuition

A classical computer manipulates definite states:

```text
0
1
```

Algorithms operate on these states using classical logic.

## Quantum intuition

A quantum computer uses **qubits**.

A qubit can exist in a quantum combination:

```text
|ψ⟩ = α|0⟩ + β|1⟩
```

where:

- `|0⟩` and `|1⟩` are the basis states
- `α` and `β` are probability amplitudes

For a two-qubit system, the computational basis is:

```text
|00⟩
|01⟩
|10⟩
|11⟩
```

More generally, `n` qubits have `2ⁿ` computational basis states.

---

# 4. Very Important: Quantum Computers Do NOT Simply "Try Everything"

A common explanation says:

> "A quantum computer tries every possible answer simultaneously."

The lecture explicitly corrects this idea.

Quantum computers can represent many possibilities using superposition, but **measurement does not simply reveal all of them**.

The useful mechanism is:

```text
Superposition
      ↓
Interference
      ↓
Useful amplitudes reinforced
Unwanted amplitudes cancelled
      ↓
Measurement
      ↓
Classical result
```

So the computational advantage comes from **carefully designed interference**, not from magically reading every possible answer at once.

---

# 5. Feynman's Quantum Insight

Richard Feynman's quantum-computing vision is an important starting point.

The basic idea is:

```text
Many possible paths
        ↓
Quantum amplitudes
        ↓
Interference
        ↓
Measurement
```

Suppose different computational paths have amplitudes:

```text
a₁, a₂, a₃, ...
```

The total amplitude can be represented as:

```text
A = a₁ + a₂ + ...
```

The probability of observing the corresponding outcome is:

```text
P = |A|²
```

This is important because amplitudes can:

- reinforce one another
- cancel one another

---

# 6. The Three-Step Quantum Computing Intuition

## 1. Superposition

The quantum system carries amplitudes for many possible alternatives.

## 2. Interference

Useful amplitudes can reinforce each other.

Unwanted amplitudes can cancel.

## 3. Measurement

Eventually, we obtain an ordinary classical result.

The result is probabilistic, with probabilities shaped by the preceding quantum interference.

### The key idea

Quantum computing is powerful not merely because many possibilities can exist in superposition.

The crucial part is being able to **make those possibilities interfere in a useful way**.

---

# 7. Feynman → Shor

The lecture presents a historical progression:

```text
1981
Feynman
Quantum simulation vision

        ↓

1994
Shor
Quantum factoring algorithm

        ↓

1996
Grover
Quantum search algorithm

        ↓

2024
PQC standards
Quantum-resistant cryptography
```

The important transition is from:

```text
"What could quantum computing do?"
```

to:

```text
"Here is a concrete quantum algorithm that threatens an important
cryptographic problem."
```

That algorithm is **Shor's algorithm**.

---

# 8. Feynman's Contribution

Feynman's broader vision was that quantum systems could themselves be used as computational resources.

His key ideas included:

- quantum systems naturally use superposition
- quantum systems have amplitudes
- amplitudes can interfere
- quantum machines could efficiently simulate quantum physics

This established the broader vision of computation based on quantum mechanics.

---

# 9. Shor's Breakthrough

Peter Shor later developed a concrete quantum algorithm with major implications for number theory and cryptography.

Shor's algorithm can factor large integers efficiently in polynomial time on a sufficiently capable, error-corrected quantum computer.

The important transformation is:

```text
Factoring
   ↓
Period finding
   ↓
Quantum Fourier Transform
   ↓
Recover period
   ↓
Classical mathematics
   ↓
Factors
```

### Why this matters

Some important public-key cryptographic systems rely on mathematical problems believed to be difficult for classical computers.

Integer factorization is one of those problems.

RSA is the major example discussed in this lecture.

---

# 10. Shor's Algorithm: The Core Idea

Suppose we want to factor:

```text
N
```

The algorithm does not directly search through every possible factor.

Instead, it converts the factoring problem into a **period-finding problem**.

The key function is:

```text
f(x) = aˣ mod N
```

For an appropriate value of `a`, this function is periodic.

The quantum computer is used to find that hidden period.

Then classical mathematics uses that period to recover factors of `N`.

---

# 11. Shor's Algorithm Step by Step

## Step 1: Choose a random `a`

Choose:

```text
1 < a < N
```

Calculate:

```text
gcd(a, N)
```

If:

```text
gcd(a,N) > 1
```

then a factor has already been found.

Otherwise, continue.

---

## Step 2: Define the periodic function

Define:

```text
f(x) = aˣ mod N
```

Its values repeat.

The repetition has some period:

```text
r
```

such that:

```text
f(x+r) = f(x)
```

for the relevant sequence.

The challenge is to discover `r`.

---

## Step 3: Create a quantum superposition

The quantum computer represents many possible `x` values coherently.

This allows the quantum circuit to work with the periodic structure.

---

## Step 4: Apply the Quantum Fourier Transform

The **Quantum Fourier Transform (QFT)** extracts information about the periodic structure.

The lecture describes this as producing strong peaks at frequencies related to the hidden period.

---

## Step 5: Measure and infer `r`

Measurement gives classical information.

Classical mathematical processing, including **continued fractions**, can then be used to reconstruct the period `r`.

---

## Step 6: Extract the factors

If `r` is suitable and even, calculate:

```text
gcd(a^(r/2) - 1, N)
```

and

```text
gcd(a^(r/2) + 1, N)
```

These can reveal non-trivial factors of `N`.

---

# 12. Shor's Algorithm in One Diagram

```text
                Choose N
                   │
                   ▼
              Choose a
                   │
                   ▼
           f(x) = aˣ mod N
                   │
                   ▼
          Find hidden period r
             using quantum
           interference + QFT
                   │
                   ▼
             Measure result
                   │
                   ▼
        Classical post-processing
          using continued fractions
                   │
                   ▼
           Recover period r
                   │
                   ▼
     gcd(a^(r/2) − 1, N)
     gcd(a^(r/2) + 1, N)
                   │
                   ▼
              Factors of N
```

---

# 13. Worked Example: Factoring 15

The lecture gives a simple example:

```text
N = 15
```

Choose:

```text
a = 2
```

Then:

```text
f(x) = 2ˣ mod 15
```

The sequence is:

```text
1, 2, 4, 8, 1, 2, 4, 8, ...
```

Therefore:

```text
r = 4
```

The period is 4.

Since `r` is even:

```text
r/2 = 2
```

Therefore:

```text
2^(r/2) = 2² = 4
```

Now calculate:

```text
gcd(4 - 1, 15)
= gcd(3,15)
= 3
```

and:

```text
gcd(4 + 1, 15)
= gcd(5,15)
= 5
```

Therefore:

```text
15 = 3 × 5
```

### What the example teaches

The quantum computer is used to find the hidden period.

Classical mathematics then converts that period into the factors.

---

# 14. Why RSA Matters

RSA is one of the major public-key cryptosystems discussed in the lecture.

Its basic idea is:

```text
Choose two large primes
        ↓
p and q
        ↓
n = p × q
        ↓
Create public/private exponents
        ↓
Publish public key
        ↓
Protect private key
```

The important mathematical assumption is that:

```text
Multiplying large primes
        ↓
is easy
```

while:

```text
Given n = p × q
        ↓
Recover p and q
        ↓
is classically hard at cryptographic sizes
```

This asymmetry is central to RSA's security model.

---

# 15. RSA Example from the Lecture

The final slides give a small RSA example.

Choose:

```text
p = 3
q = 11
```

Then:

```text
n = p × q
  = 3 × 11
  = 33
```

Calculate Euler's totient:

```text
φ(n) = (p−1)(q−1)

     = (3−1)(11−1)

     = 2 × 10

     = 20
```

Choose:

```text
e = 3
```

because:

```text
gcd(3,20) = 1
```

Now find `d` such that:

```text
3d ≡ 1 (mod 20)
```

Since:

```text
3 × 7 = 21
21 ≡ 1 (mod 20)
```

we obtain:

```text
d = 7
```

Therefore:

```text
Public key  = (e,n) = (3,33)

Private key = (d,n) = (7,33)
```

This is only a **toy example**. Real RSA uses cryptographically large parameters.

---

# 16. Why Shor Threatens RSA

RSA publishes information based around:

```text
n = p × q
```

Computing `n` from `p` and `q` is easy.

Recovering `p` and `q` from a sufficiently large `n` is believed to be hard for classical computers.

Shor changes the situation:

```text
RSA modulus
     ↓
Shor period finding
     ↓
Quantum Fourier Transform
     ↓
Recover period
     ↓
Classical conversion
     ↓
Factors p and q
```

Once the factors are recovered, the security foundation of RSA is compromised.

### Important qualification

The lecture emphasizes:

> Shor's algorithm is not "instant factoring."

Its practical cryptographic impact requires sufficiently large, error-corrected quantum hardware.

---

# 17. Quantum Computing Does Not Break All Cryptography Equally

This is a critical point.

The lecture states:

> Quantum does not invalidate all cryptography. It specifically changes which mathematical hardness assumptions remain trustworthy.

So the correct mental model is:

```text
Quantum computing
        ↓
Different effect on different algorithms
        ↓
Some public-key assumptions become vulnerable
        ↓
Other cryptographic mechanisms are affected differently
```

This is why the migration to PQC focuses strongly on replacing vulnerable public-key mechanisms.

---

# 18. Grover's Algorithm

After Shor, the lecture introduces another major quantum algorithm:

> **Grover's algorithm**

Grover addresses a different problem.

### Shor

```text
Goal:
Factor large integers

Technique:
Period finding + QFT

Impact:
Major implication for RSA
```

### Grover

```text
Goal:
Search an unstructured space

Technique:
Oracle + amplitude amplification

Speed-up:
O(N) → O(√N)
```

---

# 19. What Does "Unstructured Search" Mean?

Suppose there are:

```text
N possible items
```

and exactly one is the target.

There is no useful ordering or structure that lets us quickly eliminate large groups of possibilities.

A classical worst-case search may require approximately:

```text
O(N)
```

checks.

Grover reduces this to approximately:

```text
O(√N)
```

oracle queries.

This is a **quadratic speed-up**, not an exponential speed-up.

---

# 20. Grover's Algorithm: The Basic Story

The lecture gives this sequence:

```text
Create superposition
        ↓
Oracle marks target
        ↓
Diffusion operator
        ↓
Amplitude of target grows
        ↓
Repeat
        ↓
Measure
```

---

# 21. Step 1: Create a Superposition

Suppose there are `N` candidates.

The quantum system starts with all candidate states having equal amplitude.

Conceptually:

```text
Candidate 1   ─┐
Candidate 2   ─┤
Candidate 3   ─┤ → Equal superposition
...            ┤
Candidate N   ─┘
```

Each candidate initially has the same probability.

---

# 22. Step 2: Oracle Marks the Target

The oracle knows how to recognize the desired item.

It does not directly output the answer.

Instead, it changes the **phase/sign** of the target state.

For example:

```text
Target amplitude:

+1/2
 ↓
−1/2
```

The target is now marked through phase.

---

# 23. Step 3: Diffusion Operator

The diffusion operator reflects amplitudes around their average.

This causes the marked state's amplitude to become larger while the others become smaller.

Conceptually:

```text
Oracle
  ↓
Target phase flipped
  ↓
Diffusion
  ↓
Target amplitude amplified
```

This is **amplitude amplification**.

---

# 24. Step 4: Repeat

The lecture states that for one target, the oracle + diffusion process is repeated roughly:

```text
π/4 × √N
```

times.

The exact implementation depends on the problem, but the important idea is:

```text
Repeat interference
        ↓
Target amplitude increases
```

---

# 25. Step 5: Measure

After enough iterations:

```text
Target probability
        ↑
        ↑
        ↑
```

The target now has a high probability of being observed.

Measurement finally gives the classical answer.

---

# 26. Grover Example: One Target in 1,000,000

Suppose there is:

```text
N = 1,000,000
```

possibilities.

### Classical search

Worst case:

```text
~1,000,000 checks
```

### Grover

Approximately:

```text
√1,000,000
= 1,000
```

oracle queries.

Therefore:

```text
Classical: ~1,000,000
Grover:    ~1,000
```

This illustrates the quadratic speed-up.

### Remember

```text
O(N)
   ↓
O(√N)
```

Not:

```text
O(N)
   ↓
O(log N)
```

and not an exponential speed-up.

---

# 27. Grover Worked Example: Four States

Suppose we have four possible states:

```text
|00⟩
|01⟩
|10⟩
|11⟩
```

Suppose:

```text
Target = |10⟩
```

Initially each state has amplitude:

```text
+1/2
```

because:

```text
(1/2)² = 1/4
```

and there are four equally likely states.

---

## After the Oracle

The oracle flips only the target's phase:

```text
|00⟩ → +1/2

|01⟩ → +1/2

|10⟩ → −1/2

|11⟩ → +1/2
```

The target has not yet become more probable.

It has simply been **marked by phase**.

---

## After Diffusion

The diffusion operation amplifies the marked state.

The lecture's ideal example gives:

```text
|00⟩ → 0
|01⟩ → 0
|10⟩ → 1
|11⟩ → 0
```

Therefore:

```text
P(|10⟩) = 1
```

The target is obtained with probability 1 in this ideal four-item example after one Grover iteration.

---

# 28. Shor vs Grover

| Feature | Shor | Grover |
|---|---|---|
| Main problem | Integer factoring | Unstructured search |
| Main technique | Period finding + QFT | Amplitude amplification |
| Speed-up | Polynomial-time quantum factoring | Quadratic |
| Major cryptographic implication | RSA and related factoring-based systems | Search/key-space implications |
| Core mechanism | Hidden periodicity | Oracle + diffusion |

### Important

Grover does not replace Shor.

They solve different computational problems.

---

# 29. Which Secrets Deserve Priority?

The lecture emphasizes that different information has different:

- lifetimes
- sensitivity
- consequences

The categories highlighted are:

## State & Defense

Long confidentiality lifetime and potentially high strategic impact.

## Health & Genomic Data

Highly sensitive and potentially relevant for decades.

## Financial Information

Risks include:

- fraud
- identity compromise
- transaction integrity issues

## Intellectual Property

Commercial value may persist for years.

## Keys & Credentials

A compromised key can unlock multiple systems.

## Personal Communications

Sensitivity varies, but bulk collection creates privacy concerns.

---

# 30. The "Harvest Now, Decrypt Later" Idea

The lecture's discussion of long-lived sensitive information leads to an important migration concern:

```text
Sensitive encrypted data
        ↓
Collected today
        ↓
Quantum computer becomes capable later
        ↓
Old encryption may become decryptable
```

Therefore, the question is not only:

> "When will a sufficiently powerful quantum computer exist?"

It is also:

> "How long does the information need to remain confidential?"

This is why migration can be necessary **before** a cryptographically relevant quantum computer exists.

---

# 31. The Great Migration to PQC

The lecture presents migration as five major stages:

```text
1. Discover
       ↓
2. Prioritize
       ↓
3. Pilot
       ↓
4. Deploy
       ↓
5. Retire
```

---

# 32. Step 1: Discover

Inventory:

- cryptographic algorithms
- certificates
- protocols
- libraries
- data lifetimes

The goal is to understand:

```text
Where is cryptography being used?
What algorithms are being used?
What data does it protect?
How long must that data remain secure?
```

---

# 33. Step 2: Prioritize

Systems should be evaluated based on factors such as:

- sensitivity
- exposure
- dependencies
- replacement difficulty

Not every system has the same urgency.

---

# 34. Step 3: Pilot

Test:

- PQC mechanisms
- hybrid mechanisms
- performance
- interoperability

The point is to discover practical problems before a large-scale deployment.

---

# 35. Step 4: Deploy

Roll out quantum-safe configurations.

The lecture emphasizes:

- rollback capability
- observability

This matters because cryptographic migration affects real systems and can create compatibility or performance problems.

---

# 36. Step 5: Retire

Remove deprecated algorithms only after the surrounding ecosystem is ready.

This means:

```text
New system working
        ↓
Dependencies migrated
        ↓
Interoperability confirmed
        ↓
Old algorithm removed
```

Do not simply switch off an old algorithm without understanding its dependencies.

---

# 37. Crypto-Agility

One of the most important engineering concepts in the lecture is:

> **Crypto-agility**

Crypto-agility means:

> The ability to replace cryptographic algorithms without redesigning the entire system.

A system with poor crypto-agility might have cryptographic algorithms deeply embedded throughout the architecture.

A crypto-agile system treats cryptographic components more like replaceable modules.

Conceptually:

```text
Application
     ↓
Crypto interface
     ↓
 ┌───────────────┐
 │ Algorithm A   │
 │ Algorithm B   │
 │ Algorithm C   │
 └───────────────┘
```

The goal is to make future cryptographic transitions much easier.

---

# 38. Post-Quantum Cryptography

The lecture introduces several PQC families.

The important NIST standards listed are:

```text
ML-KEM
ML-DSA
SLH-DSA
```

It also discusses:

```text
Classic McEliece
```

as a code-based alternative.

---

# 39. ML-KEM

**ML-KEM** is a:

> Lattice-based Key Encapsulation Mechanism.

The lecture identifies it as:

```text
NIST FIPS 203
```

and notes that it is derived from:

```text
CRYSTALS-Kyber
```

### What does a KEM do?

A Key Encapsulation Mechanism is used to establish a shared secret between parties over a public channel.

Conceptually:

```text
Alice
  │
  │ KEM operation
  ▼
Shared secret
  ▲
  │
  │ KEM operation
  │
Bob
```

The shared secret can then be used with symmetric authenticated encryption for the actual data.

---

# 40. ML-DSA

**ML-DSA** is a:

> Lattice-based digital signature scheme.

The lecture identifies it as:

```text
NIST FIPS 204
```

and notes that it is derived from:

```text
CRYSTALS-Dilithium
```

Its purpose is to provide quantum-resistant digital signatures.

---

# 41. SLH-DSA

**SLH-DSA** is a:

> Stateless hash-based digital signature scheme.

The lecture identifies it as:

```text
NIST FIPS 205
```

and notes that it is derived from:

```text
SPHINCS+
```

This gives a different mathematical foundation from the lattice-based approaches.

---

# 42. Classic McEliece

Classic McEliece is a:

> Code-based Key Encapsulation Mechanism.

The lecture highlights:

- it is a long-studied alternative
- it has very large public keys
- it is not among the first three NIST finalized standards listed above

Its large key sizes are an important practical consideration.

---

# 43. Why Cryptography Needs Long-Term Scrutiny

The lecture also gives the historical example of **Rainbow**.

Rainbow was a multivariate signature candidate that was cryptanalytically broken.

The lesson is not simply:

```text
New algorithm = automatically secure
```

Instead:

```text
New mathematical construction
        ↓
Cryptanalysis
        ↓
Testing
        ↓
Academic scrutiny
        ↓
Standardization
```

Cryptographic algorithms need years of serious analysis before they can be trusted at large scale.

---

# 44. The Hybrid Shield

During migration, systems do not necessarily need to jump directly from:

```text
Classical
```

to:

```text
PQC only
```

The lecture introduces **hybrid key establishment**.

The basic idea is:

```text
Classical component
        +
PQC component
        ↓
Secure combiner
        ↓
Combined secret
        ↓
Session key
        ↓
Symmetric authenticated encryption
```

---

# 45. What Does Hybrid Mean?

Suppose we have:

```text
Classical key-establishment method
```

and:

```text
ML-KEM
```

The two mechanisms contribute secrets.

A secure combiner derives the final session key.

The goal is for the design to remain secure if at least one component retains its relevant security assumptions.

---

# 46. Hybrid Does NOT Mean Encrypting Everything Twice

This is an important clarification in the lecture.

Hybrid cryptography does **not** normally mean:

```text
Encrypt message with classical crypto
        ↓
Encrypt entire result again with PQC
```

Instead, hybrid key establishment combines key-establishment mechanisms.

The resulting shared secret is then used with symmetric authenticated encryption.

Conceptually:

```text
Classical KEM/key agreement ─┐
                             ├→ Secure combiner → Session key
PQC KEM ─────────────────────┘
                                      ↓
                          Symmetric authenticated encryption
                                      ↓
                                    Data
```

---

# 47. Why Symmetric Encryption Is Still Important

The lecture focuses strongly on public-key cryptography because Shor has major implications there.

But actual systems commonly use a combination of:

```text
Public-key / KEM
        ↓
Establish session secret
        ↓
Symmetric authenticated encryption
        ↓
Protect bulk data
```

This distinction matters.

Quantum computing does not mean:

```text
"All encryption becomes useless."
```

The impact depends on the cryptographic primitive and its underlying security assumptions.

---

# 48. What a B.Tech Student Can Do Today

The lecture gives a practical roadmap.

## LEARN

Study:

- linear algebra
- probability
- quantum circuits
- Shor's algorithm
- Grover's algorithm
- modern cryptography
- NIST PQC

---

## BUILD

Experiment with:

- Qiskit
- PQC libraries

Possible learning projects:

- toy quantum circuits
- simple Shor demonstrations
- Grover search experiments
- PQC key-generation experiments
- performance benchmarks

---

## SECURE

Learn:

- protocol design
- key management
- authenticated encryption
- certificates
- secure coding

---

## CONTRIBUTE

Potential contributions include:

- open-source documentation
- testing
- interoperability tools
- cryptographic inventory
- migration automation

---

## COMMUNICATE

The lecture recommends explaining quantum risk accurately.

The framing given is:

```text
Serious
     +
Uncertain in timing
     +
Solvable with disciplined migration
```

Avoid both extremes:

```text
"Quantum computing is irrelevant."
```

and:

```text
"Everything will be broken tomorrow."
```

---

## PREPARE

Potential career areas include:

- cybersecurity
- cryptography engineering
- quantum software
- standards

---

# 49. Recommended Learning Direction

The lecture recommends resources and areas including:

### Books

- *Quantum Computing for Everyone* by Chris Bernhardt
- *Cryptography Engineering* by Ferguson, Schneier & Kohno

### Standards and guidance

- NIST Post-Quantum Cryptography standards
- NIST migration guidance

### Practical work

- IBM Quantum / Qiskit learning materials
- toy quantum circuits
- PQC library benchmarking
- TLS
- PKI
- key management

### Communities / organizations

- NIST Computer Security Resource Center
- IETF post-quantum work
- Open Quantum Safe
- academic quantum information and cryptography groups

The lecture emphasizes using primary standards and current technical guidance because quantum security is an evolving area.

---

# 50. The Entire Lecture in One Story

The lecture can be remembered as one continuous chain:

```text
THE INTERNET
     ↓
Needs cryptography
     ↓
Public-key cryptography
     ↓
Relies on mathematical hardness
     ↓
Quantum computing develops
     ↓
Qubits + superposition + interference
     ↓
Feynman's quantum-computing vision
     ↓
Shor's algorithm
     ↓
Efficient quantum period finding
     ↓
Integer factoring
     ↓
RSA becomes vulnerable to a sufficiently capable
fault-tolerant quantum computer
     ↓
Grover's algorithm
     ↓
Quadratic speed-up for unstructured search
     ↓
Quantum risk becomes a migration problem
     ↓
Inventory current cryptography
     ↓
Prioritize sensitive systems
     ↓
Pilot PQC / hybrid designs
     ↓
Deploy
     ↓
Retire vulnerable algorithms
     ↓
Build crypto-agile systems
     ↓
Quantum-resistant digital infrastructure
```

---

# 51. The Most Important Concepts to Remember

## 1. Superposition

```text
|ψ⟩ = α|0⟩ + β|1⟩
```

A qubit can exist as a coherent combination of basis states.

---

## 2. Interference

Quantum amplitudes can reinforce or cancel one another.

This is what makes superposition computationally useful.

---

## 3. Measurement

Measurement converts the quantum state into a classical result.

You cannot simply read every amplitude directly.

---

## 4. Shor

```text
Factoring
→ Period finding
→ QFT
→ Classical recovery
```

Major implication:

```text
RSA
```

---

## 5. Grover

```text
Oracle
→ Phase marking
→ Diffusion
→ Amplitude amplification
→ Measurement
```

Speed:

```text
O(N) → O(√N)
```

---

## 6. PQC

Post-Quantum Cryptography aims to provide cryptographic mechanisms designed to remain secure against quantum attacks.

Important standards in the lecture:

```text
ML-KEM  → FIPS 203
ML-DSA  → FIPS 204
SLH-DSA → FIPS 205
```

---

## 7. Hybrid Cryptography

```text
Classical component
       +
PQC component
       ↓
Combined secret
       ↓
Symmetric encryption
```

---

## 8. Crypto-Agility

Design systems so that cryptographic algorithms can be replaced without redesigning the entire system.

---

# 52. Common Misconceptions

## Misconception 1

> "A quantum computer tries every answer and reads all the answers."

### Correct idea

Quantum algorithms manipulate amplitudes and use interference to increase the probability of useful outcomes.

---

## Misconception 2

> "Shor instantly breaks RSA."

### Correct idea

Shor provides a polynomial-time quantum algorithm for factoring, but practical cryptographic impact requires sufficiently large, error-corrected quantum hardware.

---

## Misconception 3

> "Quantum computing breaks all cryptography."

### Correct idea

Different cryptographic primitives rely on different mathematical assumptions.

Quantum computing particularly changes the security picture for important public-key systems based on assumptions such as integer factorization.

---

## Misconception 4

> "Grover gives exponential speed-up."

### Correct idea

Grover gives a quadratic speed-up:

```text
O(N)
→
O(√N)
```

---

## Misconception 5

> "Hybrid encryption means encrypting the whole message twice."

### Correct idea

Hybrid key establishment combines secrets from classical and PQC mechanisms, then typically uses the resulting session key for symmetric authenticated encryption.

---

## Misconception 6

> "PQC means quantum computers are used to encrypt the data."

### Correct idea

Post-Quantum Cryptography generally means **classical cryptographic algorithms designed to resist attacks from quantum computers**.

---

# 53. Exam-Oriented Short Answers

## What is Shor's algorithm?

Shor's algorithm is a quantum algorithm that can factor integers in polynomial time on a sufficiently capable fault-tolerant quantum computer. It reduces factoring to period finding and uses the Quantum Fourier Transform to extract the periodic structure.

## Why does Shor threaten RSA?

RSA's security is closely tied to the difficulty of factoring a large modulus `n = p × q`. Shor provides an efficient quantum approach to the underlying factoring problem, threatening this security assumption when sufficiently capable quantum hardware exists.

## What is Grover's algorithm?

Grover's algorithm is a quantum algorithm for unstructured search that provides a quadratic speed-up, reducing approximately `O(N)` oracle queries to `O(√N)`.

## What is amplitude amplification?

Amplitude amplification is the process used by Grover's algorithm to increase the amplitude, and therefore probability, of the desired state through repeated oracle and diffusion operations.

## What is the Quantum Fourier Transform?

The QFT is a quantum transformation used by Shor's algorithm to extract information about the periodic structure of a function.

## What is Post-Quantum Cryptography?

PQC refers to cryptographic algorithms designed to remain secure against attacks from sufficiently capable quantum computers.

## What is ML-KEM?

ML-KEM is a lattice-based Key Encapsulation Mechanism standardized as NIST FIPS 203 and derived from CRYSTALS-Kyber.

## What is ML-DSA?

ML-DSA is a lattice-based digital signature scheme standardized as NIST FIPS 204 and derived from CRYSTALS-Dilithium.

## What is SLH-DSA?

SLH-DSA is a stateless hash-based digital signature scheme standardized as NIST FIPS 205 and derived from SPHINCS+.

## What is crypto-agility?

Crypto-agility is the ability to replace cryptographic algorithms without redesigning the entire system.

## What is hybrid key establishment?

Hybrid key establishment combines a classical mechanism with a PQC mechanism and derives a combined session secret from them.

---

# 54. Quick Revision Table

| Concept | Remember it as |
|---|---|
| Qubit | Quantum version of the information unit |
| Superposition | Multiple basis states with amplitudes |
| Amplitude | Quantity whose squared magnitude gives probability |
| Interference | Amplitudes reinforce or cancel |
| Measurement | Converts quantum information into classical outcome |
| Feynman | Quantum computation vision |
| Shor | Factoring through period finding |
| QFT | Extracts periodic structure |
| RSA | Public-key system tied to factoring |
| Grover | Unstructured search |
| Oracle | Marks the target through phase |
| Diffusion | Amplifies target amplitude |
| PQC | Quantum-resistant cryptography |
| ML-KEM | Lattice-based KEM |
| ML-DSA | Lattice-based signatures |
| SLH-DSA | Hash-based signatures |
| Hybrid | Classical + PQC key establishment |
| Crypto-agility | Easy algorithm replacement |

---

# 55. Final Takeaway

The central story of this lecture is:

> **Today's cryptography is built on mathematical problems that are difficult for classical computers. Quantum computing changes the computational landscape by providing new algorithms for some of those problems.**

The most important example is:

```text
Shor
  ↓
Period finding
  ↓
Factoring
  ↓
RSA security assumption
```

A second important algorithm is:

```text
Grover
  ↓
Amplitude amplification
  ↓
Unstructured search
  ↓
Quadratic speed-up
```

The practical response is not panic. It is preparation:

```text
Discover
   ↓
Prioritize
   ↓
Pilot
   ↓
Deploy
   ↓
Retire
```

And the architectural principle that makes this transition easier is:

```text
CRYPTO-AGILITY
```

The lecture's final message is therefore not simply that quantum computers are a threat.

It is that security systems should be designed for **change**.

> **One-line summary:**  
> **Quantum computing threatens some current public-key cryptographic assumptions through algorithms such as Shor, while Grover provides a quadratic search speed-up; the practical response is disciplined migration toward PQC, hybrid designs, and crypto-agile systems.**
