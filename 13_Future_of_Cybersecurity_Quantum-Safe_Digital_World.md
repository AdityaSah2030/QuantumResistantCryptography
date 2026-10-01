# 13 · Future of Cybersecurity: Quantum-Safe Digital World

## Lecture Overview

**Theme:** Future of Cybersecurity and the transition to a quantum-safe digital world  
**Deck:** *Quantum-Safe Digital World*  
**Focus:** From CIA-T and Harvest Now, Decrypt Later to post-quantum standards, real deployments, enterprise migration, careers, labs and project work.

This lecture connects three layers:

```text
SECURITY THEORY
      ↓
What quantum computing changes
      ↓
POST-QUANTUM TOOLKIT
      ↓
ML-KEM · ML-DSA · SLH-DSA · Hybrid crypto
      ↓
INDUSTRY
      ↓
Internet · Banking · Healthcare · IoT · Cloud
      ↓
ENTERPRISE MIGRATION
      ↓
Inventory · Risk · Crypto-agility · Migration · KPIs
      ↓
CAREERS + PROJECTS
```

The deck's central message is:

> **Every security control has a clock. The important question is not only "Is it secure?" but "Secure until when?"**

The presentation explicitly frames this as an engineering and risk-management subject. No quantum physics is required. fileciteturn14file0L71-L91

---

# 1. The Eight-Part Story

The entire bootcamp deck is organised into eight parts:

| Part | Topic |
|---|---|
| 1 | Why security has a clock |
| 2 | What quantum breaks |
| 3 | The new toolkit |
| 4 | Who sets the deadlines |
| 5 | Where it already runs |
| 6 | The enterprise playbook |
| 7 | Careers |
| 8 | Your turn: quiz, lab, group activity and project |

The first three parts establish the technical foundation.

Parts 4–6 explain how industry and regulators are responding.

Parts 7–8 translate the subject into career preparation and practical work. fileciteturn14file0L39-L68

---

# 2. Learning Outcomes

By the end of the lecture, the intended outcomes are:

### 1. Explain the risk

Be able to explain:

- why RSA and ECC become vulnerable,
- why harvested data can already be at risk,
- why migration must begin before a cryptographically relevant quantum computer exists.

### 2. Name the standards

Know:

- ML-KEM,
- ML-DSA,
- SLH-DSA,

and understand where each is used.

### 3. Read the deadlines

Understand the migration timelines discussed for:

- NIST,
- EU,
- UK,
- India's NQM,
- SEBI,
- browser and certificate ecosystems.

### 4. Run the enterprise playbook

Be able to:

```text
Build crypto inventory
        ↓
Rank risk
        ↓
Design for crypto-agility
        ↓
Migrate
        ↓
Measure
```

### 5. Place yourself

Identify which cybersecurity role fits your skills and interests. fileciteturn14file0L71-L91

---

# PART 1 — SECURITY HAS A CLOCK

# 3. The CIA Triad

Before discussing quantum computing, the lecture revisits the three classic security objectives.

## C — Confidentiality

Only authorised people should be able to read the information.

Example:

> Your exam paper before the exam.

---

## I — Integrity

Nobody should be able to modify the information without authorisation.

Example:

> Your marks in the university record.

---

## A — Availability

The system should work when legitimate users need it.

Example:

> The results portal on results day.

---

# 4. CIA-T: Adding Time

The traditional CIA triad is:

```text
Confidentiality
Integrity
Availability
```

The lecture adds an explicit time dimension:

> **CIA-T**

Conceptually:

```text
C(t)
I(t)
A(t)
```

The key insight is:

> Security controls do not remain effective forever.

The deck states that availability already has recognised time measures such as:

- RTO,
- RPO,
- MTTR.

The post-quantum problem forces us to explicitly think about time for confidentiality and integrity as well. fileciteturn14file0L102-L138

---

# 5. The Security Question We Usually Ask

Traditional security questions include:

```text
Is the data encrypted?
Is the file signed?
Is the server up?
```

These tend to produce:

```text
YES / NO
```

But the lecture argues that the better question is:

```text
Unreadable until when?
Trusted until when?
Restored within how long?
```

The answer becomes:

> **A number of years.**

---

# 6. Security Has a Validity Horizon

Consider a cryptographic lock.

It is not simply:

> "Secure."

It is:

> **Secure for as long as the attacker cannot obtain the key or otherwise defeat the protection.**

Therefore every control has a:

> **Validity horizon**

Examples:

| Control | Question |
|---|---|
| Encryption | How long must the data remain confidential? |
| Digital signature | How long must the signature remain trustworthy? |
| Certificate | How long can the certificate remain valid? |
| Password | How long until it is leaked or guessed? |
| Server patch | How long until a vulnerability becomes exploitable? |

---

# 7. Harvest Now, Decrypt Later

One of the most important concepts in the entire lecture is:

> **Harvest Now, Decrypt Later (HNDL)**

Imagine an attacker intercepts encrypted traffic today.

```text
2026
 ↓
Attacker copies encrypted traffic
 ↓
Stores it
 ↓
2032
 ↓
Data is still sitting in storage
 ↓
2038
 ↓
Sufficient quantum capability exists
 ↓
Attacker decrypts it
```

The critical point is:

> **The attack begins when the encrypted data is collected, not when the quantum computer becomes available.**

The deck identifies particularly sensitive long-lived information such as:

- health records,
- KYC data,
- financial information,
- defence traffic,
- trade secrets.

It also notes that SEBI's 2025–26 annual report names HNDL and "trust now, forge later" as threats relevant to India's securities market. fileciteturn14file0L141-L154

---

# 8. Why HNDL Is So Dangerous

Suppose information needs to remain confidential for:

```text
25 years
```

If the data is captured today and a quantum attack becomes practical before those 25 years expire, the original confidentiality requirement has failed.

You cannot travel back in time and:

```text
Delete the attacker's copy
```

Therefore:

```text
Long secrecy life
        +
Future quantum threat
        ↓
HNDL risk today
```

---

# 9. Trust Now, Forge Later

HNDL mainly concerns:

> **Confidentiality**

But quantum attacks also create a long-term:

> **Integrity / authenticity**

problem.

Today:

```text
Phone receives software update
        ↓
Checks digital signature
        ↓
Signature verifies
        ↓
Firmware trusted
```

After sufficiently capable quantum attacks against vulnerable signature systems:

```text
Attacker
   ↓
Creates forged signature
   ↓
Looks legitimate
   ↓
Fake firmware / certificate / contract
   ↓
System may accept it
```

The lecture calls this:

> **Trust now, forge later.** fileciteturn14file0L160-L173

---

# 10. Long-Lived Signatures

Some signatures need to remain trustworthy for decades.

Examples from the lecture:

| Signed object | Required trust lifetime |
|---|---:|
| Firmware in a car, meter or medical device | 10–20 years |
| Root Certificate Authority | 10–25 years |
| Signed contract, degree or land record | Decades or lifetime |

This creates an important asymmetry:

> You may be able to re-encrypt old data, but you cannot easily modify the verification logic in millions of devices that have already been shipped.

That is why signature infrastructure needs early planning. fileciteturn14file0L166-L173

---

# 11. Mosca's Rule

The lecture introduces a simple planning equation:

```text
X + Y > Z
```

Where:

```text
X = years the data must remain secret
Y = years required for migration
Z = years until quantum technology can break the protection
```

If:

```text
X + Y > Z
```

then the organisation is already late.

---

# 12. Mosca's Rule Example

Consider a hospital record:

```text
X = 25 years
Y = 5 years
Z = 10–15 years
```

Therefore:

```text
X + Y
= 25 + 5
= 30
```

Even using the larger estimate:

```text
30 > 15
```

Therefore the data is already exposed to the planning risk.

The exact Q-Day date does not need to be known to justify migration planning. fileciteturn14file0L176-L193

---

# 13. Mosca's Rule: Examples

| Data / asset | X: required lifetime | Exposed today? in lecture's model |
|---|---:|---|
| Session tokens, OTPs | <1 year | No |
| Customer PII / KYC | 10 years | Yes |
| Health / genomic data | 25+ years | Yes |
| Firmware signing keys | 15 years | Yes |

The important lesson is:

> **The longer the protection must remain valid, the earlier migration must begin.**

---

# 14. The Rule to Remember

The lecture summarises the principle as:

> **Every control has a validity horizon.**

A system is secure when:

```text
Life of protection
        >
Life of the secret
```

Examples:

| Control | Horizon / threat |
|---|---|
| RSA-2048 encryption | Until a sufficiently capable quantum computer; Shor's algorithm is the threat |
| Public TLS certificate | Certificate lifetime |
| Password | Until leaked or guessed |
| Unpatched server | Until exploit becomes available |
| AES-256 | Still considered resistant to the discussed quantum threat model |

The same thinking can be applied to:

- key rotation,
- patch cycles,
- data retention,
- compliance requirements. fileciteturn14file0L200-L210

---

# PART 2 — WHAT QUANTUM BREAKS

# 15. Quantum Does NOT Break Everything

A major misconception is:

> "Quantum computers break all cryptography."

The lecture explicitly rejects this.

The simplified picture is:

```text
Public-key cryptography
        ↓
Major quantum threat

Symmetric cryptography
        ↓
Reduced security margin, not the same type of break

Hashing
        ↓
Reduced security margin, but not destroyed
```

Therefore:

> **Quantum breaks the locks, not the safes.**

---

# 16. RSA, ECC and Diffie-Hellman

The lecture's table gives:

| Algorithm | Used for | Quantum attack |
|---|---|---|
| RSA-2048 / RSA-3072 | Certificates, signatures, key transport | Shor |
| ECC: ECDH, ECDSA, X25519 | HTTPS, apps, UPI, wallets | Shor |
| Diffie-Hellman | VPNs, IPsec | Shor |

The lecture treats these classical public-key mechanisms as needing replacement in the post-quantum migration. fileciteturn14file0L224-L237

---

# 17. AES and Grover's Algorithm

Symmetric encryption is affected differently.

The lecture describes:

```text
AES-128
   ↓
Grover
   ↓
Roughly halves effective brute-force security
```

Therefore the recommended migration direction is:

```text
AES-128
   ↓
AES-256
```

The lecture treats AES-256 as remaining secure against the discussed quantum threat.

---

# 18. Hash Functions

The deck lists:

```text
SHA-256
SHA-3
```

as surviving the discussed quantum threat model, with Grover providing a comparatively minor reduction rather than the complete break seen for RSA/ECC.

The key conceptual distinction is:

```text
Shor
→ destroys the relevant public-key hardness assumptions

Grover
→ quadratic speedup against search
```

---

# 19. The Migration Is Not About Re-encrypting Everything

This is an important practical point.

The major migration focus is:

```text
Key exchange
+
Digital signatures
```

rather than blindly replacing every cryptographic primitive.

The deck explicitly states that the migration is primarily about key exchange and signatures, not re-encrypting everything. fileciteturn14file0L224-L237

---

# 20. When Is Q-Day?

Q-Day means the point at which a sufficiently capable quantum computer can practically break important cryptographic systems.

The exact date is unknown.

The deck presents several planning indicators:

- estimates for the number of qubits needed have fallen,
- some surveyed experts assign serious odds to code-breaking machines within roughly 15 years,
- governments are targeting roughly the 2030–2035 period for removing vulnerable public-key cryptography from critical systems.

The deck's planning principle is:

> **Do not bet on the date. Bet on the migration time.**

The migration duration is something an organisation can actually measure. fileciteturn14file0L240-L253

---

# 21. Why Q-Day Estimates Keep Changing

The lecture notes that estimates can fall because of improvements in:

- quantum algorithms,
- error correction,
- hardware,
- engineering.

Therefore:

```text
Q-Day estimate
       ↓
Uncertain

Migration time
       ↓
Measurable
```

This is why an organisation should not wait for certainty about Q-Day.

---

# 22. Migration Timeline

The deck gives a planning sequence:

### 2024–2026

```text
Standardise algorithms
Build implementations
Start cryptographic discovery
Address HNDL
```

### 2026–2028

```text
Crypto inventories
Crypto-agility
Hybrid TLS
PQC testing
Vendor readiness
```

### 2028–2031

```text
High-value systems migrate
Authentication/signatures increasingly move to PQC
PQC becomes embedded in infrastructure
```

### 2031–2035

```text
Broad enterprise migration
Deprecation of vulnerable public-key cryptography
Legacy systems become the major problem
```

The deck highlights:

> **Data discovery and categorisation as the biggest challenge.** fileciteturn14file0L256-L277

---

# PART 3 — THE NEW TOOLKIT

# 23. NIST's Post-Quantum Toolkit

The lecture presents the following standards:

| Standard | Algorithm | Job | Mathematical basis | Status / use |
|---|---|---|---|---|
| FIPS 203 | ML-KEM / Kyber | Key exchange | Lattice | Final Aug 2024 |
| FIPS 204 | ML-DSA / Dilithium | Signatures | Lattice | Final Aug 2024 |
| FIPS 205 | SLH-DSA / SPHINCS+ | Signatures | Hash-based | Final Aug 2024 |
| FIPS 206 | FN-DSA / Falcon | Signatures | Lattice | Draft in deck |
| Pending | HQC | Key exchange | Code-based | Selected Mar 2025 as ML-KEM backup |
| SP 800-208 | LMS / XMSS | Signatures | Hash-based | Approved; firmware signing |

The deck's memory aid is:

```text
KEM = key exchange
DSA = signatures
ML = module lattice
```

The status is presented as of mid-2026 in the lecture. fileciteturn14file0L291-L304

---

# 24. ML-KEM

ML-KEM is the NIST-standardised KEM based on the Kyber algorithm family.

Its job:

> **Key establishment / key exchange**

Typical uses presented include:

- TLS,
- VPN,
- messaging.

Think:

```text
ML-KEM
   ↓
Shared secret
   ↓
Symmetric session encryption
```

---

# 25. ML-DSA

ML-DSA is a lattice-based post-quantum digital-signature scheme.

Its job:

> **Digital signatures**

Applications include:

- certificates,
- software signing,
- authentication,
- document signing.

---

# 26. SLH-DSA

SLH-DSA is the hash-based signature standard derived from SPHINCS+.

Its mathematical basis is:

> **Hash functions**

The deck positions it as:

- conservative,
- useful as a backup,
- particularly relevant for root keys and other high-assurance situations.

---

# 27. FN-DSA

FN-DSA corresponds to the Falcon algorithm family.

The deck describes it as:

```text
Lattice-based
Compact signatures
```

It is presented as a draft in the deck.

A key trade-off is that compact signatures come with implementation complexity and safety considerations.

---

# 28. HQC

HQC is a:

> **Code-based key-exchange mechanism**

The deck identifies it as a selected backup to ML-KEM.

The strategic reason is important:

```text
ML-KEM
   ↓
Lattice assumption

HQC
   ↓
Different mathematical family
```

Using different mathematical families provides algorithmic diversity.

If one family suffers a major cryptanalytic breakthrough, an independent family may provide an alternative.

---

# 29. LMS and XMSS

LMS and XMSS are:

> **Stateful hash-based signature schemes**

The lecture notes that they are useful for:

> **Firmware signing**

but state management must be handled carefully.

The core issue:

```text
Signature key state
       ↓
Must be tracked correctly
       ↓
Incorrect state management can be dangerous
```

---

# 30. Industry Defaults

The deck identifies:

```text
ML-KEM-768
ML-DSA-65
```

as the industry defaults discussed in the lecture.

It also explains why HQC exists:

> The ecosystem should not depend entirely on one mathematical family. fileciteturn14file0L301-L304

---

# 31. Hybrid Key Exchange

One of the most important deployment concepts is:

> **Hybrid cryptography**

The deck presents:

```text
X25519
   +
ML-KEM-768
   ↓
Combined key material
   ↓
Key schedule
   ↓
Session key
```

The name shown in logs is:

```text
X25519MLKEM768
```

---

# 32. Why Use Hybrid Cryptography?

Post-quantum algorithms are relatively young compared with classical cryptography.

Hybrid deployment attempts to preserve protection from both sides:

```text
Classical component
       +
Post-quantum component
```

Conceptually:

```text
If ML-KEM is broken
        ↓
Classical component still contributes protection

If quantum breaks X25519
        ↓
ML-KEM still contributes protection
```

The lecture describes the combined session as safe if either half retains its intended security property.

---

# 33. Where Hybrid TLS Appears

The deck states that hybrid mechanisms are already being shipped across major software ecosystems, including:

- Chrome,
- Firefox,
- Safari,
- OpenSSL,
- OpenSSH.

It gives:

```text
X25519MLKEM768
```

as the hybrid key-exchange group students should recognise. fileciteturn14file0L307-L321

---

# 34. PQC Is Fast, But It Is Big

A major engineering trade-off is:

> **Post-quantum cryptography can be computationally practical while producing much larger keys and signatures.**

The lecture gives these illustrative sizes:

| Algorithm | Public key | Ciphertext / signature |
|---|---:|---:|
| X25519 | 32 B | 32 B |
| ML-KEM-768 | 1,184 B | 1,088 B |
| ECDSA P-256 | 64 B | 64 B |
| RSA-2048 | 256 B | 256 B |
| ML-DSA-65 | 1,952 B | 3,309 B |
| FN-DSA-512 | 897 B | ~666 B |
| SLH-DSA-128s | 32 B | 7,856 B |

The lecture emphasises that ML-KEM-768 is roughly 35× larger than X25519 in the compared key/ciphertext dimensions. fileciteturn14file0L324-L339

---

# 35. Why Signatures Are Harder to Migrate

Key exchange can often be changed at the connection layer.

Digital signatures can appear inside:

- certificates,
- smart cards,
- IoT devices,
- payment messages,
- firmware,
- trust chains.

Large signatures therefore create:

```text
Buffer pressure
+
Certificate size increases
+
Network overhead
+
Hardware limitations
```

The deck explicitly calls signatures the harder part of the migration. fileciteturn14file0L324-L339

---

# 36. PQC vs QKD

A common confusion is:

> Post-quantum cryptography = quantum cryptography

They are different.

| PQC | QKD |
|---|---|
| New mathematical problems | Quantum behaviour of photons |
| Runs on existing CPUs | Requires specialised physical infrastructure |
| Can scale through software updates | Point-to-point and distance constrained |
| NIST FIPS standards | Technology/standard ecosystem still developing |
| Web, apps, cloud, IoT | High-assurance specialised links |

The deck's key phrase is:

> **PQC is software. QKD is physics.** fileciteturn14file0L342-L355

---

# 37. Can PQC and QKD Be Used Together?

Yes.

The lecture gives examples where organisations combine:

```text
PQC
+
Crypto-agility
+
QKD
```

for particularly high-assurance environments.

The important distinction remains:

```text
PQC
→ cryptographic algorithms

QKD
→ physical quantum communication technology
```

---

# PART 4 — WHO SETS THE DEADLINES?

# 38. Governments and the Browser Ecosystem

The deck presents two different forces:

```text
Governments
   ↓
Set strategic deadlines

Browsers + Certificate Authorities
   ↓
Set practical Internet deployment pace
```

This distinction is important for enterprise planning.

---

# 39. Regulatory Timeline

The lecture presents:

| Region / organisation | Timeline in deck |
|---|---|
| USA / NIST IR 8547 | RSA/ECC deprecated after 2030, disallowed after 2035 |
| USA / NSA CNSA 2.0 | PQC transition from 2025; completion around 2033–2035 |
| EU | Start by end-2026; critical infrastructure by 2030; broad migration by 2035 |
| UK / NCSC | Discovery by 2028; priority migration by 2031; completion by 2035 |
| Australia / ASD | Stop using classical asymmetric cryptography by 2030 |
| India / NQM Task Force | Critical infrastructure from 2027; enterprises from 2028; nationwide target by 2033 |

These are the timelines presented in the source deck and should be rechecked before using them as current regulatory advice. fileciteturn14file0L366-L379

---

# 40. India's Quantum-Safe Roadmap

The deck gives:

```text
2027
↓
Critical infrastructure begins formal migration

2028
↓
Non-critical enterprises begin structured preparation

2033
↓
Full nationwide adoption target
```

Critical infrastructure examples include:

- defence,
- power,
- telecom,
- space,
- core government.

The deck attributes the roadmap to the NQM Task Force and notes that it includes:

- cryptographic inventories,
- crypto-agility,
- product certification,
- indigenous development.

---

# 41. SEBI's Role

The deck says SEBI's Annual Report 2025–26 expects regulated entities to address:

```text
Crypto inventory
       ↓
PQC / QKD assessment
       ↓
Crypto-agility
       ↓
Transition roadmap
       ↓
Workforce upskilling
```

The approach is summarised as:

> **Discover → Observe → Transform**

This connects regulation directly to the enterprise migration playbook. fileciteturn14file0L383-L397

---

# 42. CA/Browser Forum

Another important actor is the:

> **CA/Browser Forum**

It includes browser vendors and Certificate Authorities.

The lecture explains that the forum determines important aspects of how public TLS certificates work.

A major change discussed is the reduction of certificate lifetime.

---

# 43. Certificate Lifetime Schedule

The deck gives:

```text
Today
398 days

15 March 2026
200 days

15 March 2027
100 days

15 March 2029
47 days
```

Validation reuse is also reduced.

The important engineering implication is:

> A 47-day certificate cannot realistically be renewed manually at scale.

Therefore:

```text
Short certificate lifetime
        ↓
Automation
        ↓
Automated certificate lifecycle
        ↓
Crypto-agility capability
```

The deck treats this as an important indirect driver of crypto-agility. fileciteturn14file0L400-L420

---

# 44. Why Certificate Automation Matters

Suppose a company has thousands of certificates.

Manual renewal:

```text
Certificate
 ↓
Human reminder
 ↓
Manual replacement
```

does not scale.

Automation:

```text
Certificate
 ↓
ACME / automated lifecycle
 ↓
Automatic issuance
 ↓
Automatic rotation
```

creates an infrastructure in which changing cryptographic parameters becomes easier.

Therefore certificate automation is useful beyond certificates themselves.

---

# 45. The Browser Signature Problem

The deck distinguishes:

```text
Key exchange
→ Already deploying

Certificates
→ Much harder
```

The lecture reports that major browsers have adopted hybrid key exchange while public post-quantum certificates remain a harder problem.

It also discusses Google's work on:

> **Merkle Tree Certificates**

The concept is to avoid simply adding very large post-quantum signatures to every certificate chain.

---

# 46. Why Merkle Tree Certificates Matter

The basic idea presented is:

```text
Millions of certificates
        ↓
One signed tree head
        ↓
Small inclusion proof
        ↓
Browser verifies membership
```

This is intended to address the size problem associated with post-quantum signatures.

---

# PART 5 — WHERE IT ALREADY RUNS

# 47. Internet

The lecture describes post-quantum key exchange as already deployed in the web ecosystem.

The deck's 2026 measurements include:

```text
2 in 3
human traffic to Cloudflare
```

using post-quantum key exchange by April 2026.

It also cites:

```text
49%
of measured domains
```

supporting hybrid ML-KEM.

At the same time:

```text
0
post-quantum certificates
```

were reported as being in public use in the deck.

These measurements are source-deck claims and should be treated as time-specific measurements. fileciteturn14file0L447-L463

---

# 48. Internet Deployment Stack

The deck lists:

- Chrome,
- Edge,
- Firefox,
- Safari,
- OpenSSL,
- BoringSSL,
- Go,
- AWS-LC,
- CDNs,
- cloud load balancers.

The deployment pattern is:

```text
Browser
   ↓
Hybrid key exchange
   ↓
CDN / load balancer
   ↓
Application server
```

The edge is therefore one of the easiest places to start migration.

---

# 49. Messaging

The lecture gives several examples.

## Signal

The deck mentions:

```text
PQXDH handshake
+
Post-quantum ratchet
```

The post-quantum ratchet protects ongoing conversations.

## Apple iMessage

The deck identifies:

> **PQ3**

as the post-quantum protocol introduced for iMessage.

## OpenSSH

The deck describes:

```text
mlkem768x25519
```

as the default key-exchange configuration in OpenSSH 10.

---

# 50. The General Deployment Pattern

The deck identifies a common pattern:

> Whoever controls both ends of the connection can migrate first.

For example:

```text
Company server
        ↕
Company client
```

is easier to migrate than:

```text
Company
   ↕
Millions of external users
```

Therefore:

> **Service-to-service traffic can be an early migration win.**

---

# 51. Banking and Payments

The lecture uses banking as a major case study because financial and KYC data can have long confidentiality lifetimes.

Examples discussed include:

- BIS Project Leap,
- JPMorgan Q-CAN,
- SWIFT,
- Mastercard,
- AWS Payment Cryptography,
- SEBI.

---

# 52. BIS Project Leap

The deck describes central-bank testing of post-quantum signatures in:

> **TARGET2 settlement**

The experiment demonstrated that the cryptography can work, but also exposed engineering issues:

- verification can be slower,
- messages can become larger,
- fixed-size buffers can become a problem.

This is a recurring lesson:

> **Cryptography migration is also a systems-engineering problem.**

---

# 53. JPMorgan Q-CAN

The deck describes:

> **Q-CAN**

as a quantum-secured and crypto-agile network between data centres.

The cited figure is:

```text
45 days
continuous operation
at 100 Gbps
```

The key lesson is not the specific number but the combination of:

```text
High throughput
+
Quantum-safe security
+
Crypto-agility
```

---

# 54. SWIFT

The deck describes:

> **SwiftNet 8.0**

as targeted for post-quantum enablement for global bank messaging.

This matters because financial institutions do not operate independently.

A payment ecosystem includes:

```text
Bank
+
Payment network
+
Central bank
+
Vendor
+
Customer
```

Migration therefore requires ecosystem coordination.

---

# 55. AWS Payment Cryptography

The deck cites hybrid ML-KEM support in payment APIs with an approximately:

```text
0.05%
```

throughput cost in the cited example.

Again, this is a source-specific figure, not a universal performance guarantee.

---

# 56. Inside a Bank

The deck provides an asset-level view:

| Asset | Current crypto | Quantum risk | Industry action |
|---|---|---|---|
| Net banking / mobile app | ECDHE + RSA certs | Harvested sessions | Hybrid TLS at CDN/load balancer |
| Card chip | RSA offline authentication | Forgery | Follow scheme roadmap |
| HSM / PIN / key exchange | AES + RSA key transport | Key transport exposure | PQC-capable HSM firmware |
| SWIFT / RTGS messaging | PKI signatures | Forged instructions | Track PQC messaging roadmap |
| e-KYC / e-sign / contracts | RSA signatures | Long legal validity | ML-DSA / SLH-DSA; re-timestamp |
| Backups / databases | AES-256 | Low in this model | Keep AES-256; protect key wrapping |

The recurring pattern is:

```text
Asset
 ↓
Current crypto
 ↓
Quantum risk
 ↓
Migration action
```

---

# 57. Healthcare

Healthcare is especially important because:

```text
Records
→ stored for decades

Genomic data
→ can remain sensitive for a lifetime

Medical devices
→ may remain deployed for 10–20 years
```

The lecture describes healthcare as a strong HNDL concern.

Migration examples include:

- hybrid TLS,
- post-quantum firmware signatures,
- PQC requirements in procurement,
- post-quantum-ready identity and consent mechanisms.

---

# 58. IoT, OT and Automotive

IoT and operational technology create a different problem:

```text
Small hardware
+
Long device lifetime
+
Limited patchability
```

Examples include:

- cars,
- smart meters,
- PLCs,
- industrial systems.

The deck suggests:

```text
Optimised ML-KEM
+
PQC accelerators
+
Post-quantum secure boot
+
Authenticated firmware updates
```

---

# 59. Why Firmware Signing Is Critical

Suppose an IoT device trusts:

```text
RSA-signed firmware
```

and the RSA signing infrastructure becomes forgeable.

An attacker could potentially create malicious firmware that the device accepts.

If millions of devices are already deployed:

```text
Future cryptographic break
        ↓
Firmware signature forged
        ↓
Malicious update
        ↓
Huge fleet exposed
```

Therefore:

> **The update channel must be designed for cryptographic migration before deployment.**

---

# 60. Cloud

The deck gives examples from:

### AWS

- hybrid PQ TLS,
- KMS,
- ACM,
- Secrets Manager,
- ML-DSA keys,
- CloudFront ML-KEM negotiation.

### Microsoft

- ML-KEM and ML-DSA through SymCrypt and Windows cryptographic APIs.

### Google Cloud

- quantum-safe signatures in Cloud KMS,
- hybrid key exchange within Google's traffic.

### Cloudflare

- post-quantum protection for visitors and origins,
- zero-trust tunnels,
- IPsec.

---

# 61. The Shared Responsibility Model

One of the most important cloud lessons:

> **The cloud provider does not automatically make your application quantum-safe.**

A provider can secure:

```text
Provider endpoint
```

while your application still uses:

```text
Old library
Old certificate
Old key
Old protocol
```

Therefore:

```text
Cloud PQC readiness
≠
Application PQC readiness
```

Your organisation still owns:

- applications,
- libraries,
- keys,
- certificates,
- configuration.

The deck explicitly warns against assuming that "the cloud handles it." fileciteturn14file0L553-L567

---

# 62. Sector Summary

| Sector | Readiness in lecture | Biggest exposure | First move |
|---|---|---|---|
| Internet / web | High for key exchange | Certificates / signatures | Hybrid TLS |
| Cloud | High at provider level | Applications / libraries | PQ endpoints + application scanning |
| Banking | Medium to low | Long-lived data / payment rails | Crypto inventory |
| Healthcare | Low | Records / medical devices | Procurement + vendor requirements |
| IoT / OT | Low | Unpatchable fleets | PQ secure boot |

The deck describes this as a 2026 snapshot. fileciteturn14file0L570-L582

---

# PART 6 — THE ENTERPRISE PLAYBOOK

# 63. Governance

A successful migration needs ownership.

The deck identifies:

| Role | Responsibility |
|---|---|
| Board / risk committee | Risk acceptance and funding |
| CISO | Roadmap, exceptions, reporting |
| Crypto / enterprise architect | Algorithm standards, agility patterns, approved libraries |
| Programme office | Inventory, dependencies, sequencing, vendors, KPIs |
| Domain owners | Execute migration |
| Procurement / legal | Put PQC and agility requirements into contracts |
| Internal audit | Verify inventory completeness |

The major organisational lesson is:

> **Someone must be accountable for the migration.**

---

# 64. Why Separate PQC Budgets Are Not Always Needed

The deck notes that organisations often do not have a separate PQC budget.

Instead, migration can ride existing refresh cycles:

```text
HSM refresh
Certificate renewal
TLS termination upgrade
Library upgrade
Cloud migration
Hardware refresh
```

This turns PQC into part of normal technology lifecycle management.

---

# 65. Five-Phase Migration Roadmap

The deck presents:

```text
1. Discover
2. Prioritise
3. Build agility
4. Migrate
5. Operate
```

---

# 66. Phase 1 — Discover

Typical window:

```text
3–9 months
```

Goal:

> Create a live inventory covering most of the estate.

You cannot migrate cryptography that you do not know exists.

---

# 67. Phase 2 — Prioritise

Typical window:

```text
1–2 months
```

Goal:

```text
Inventory
 ↓
Risk register
 ↓
Dates
 ↓
Owners
```

---

# 68. Phase 3 — Build Crypto-Agility

Typical window:

```text
6–18 months
```

Goal:

> Make cryptographic algorithms configurable and replaceable.

This includes:

- centralised key management,
- automated certificates,
- shared crypto libraries,
- policy-based configuration.

---

# 69. Phase 4 — Migrate

Typical window:

```text
2–5 years
```

Migration begins after:

- pilots succeed,
- vendors commit,
- architecture is ready.

The deck recommends moving high-priority systems toward:

```text
Hybrid
or
Post-quantum
```

---

# 70. Phase 5 — Operate

This is permanent.

The goal is to make:

> **Changing an algorithm routine.**

A mature organisation should not treat cryptography as something changed once every decade.

Instead:

```text
Crypto change
=
Normal operational process
```

---

# 71. Three Rules That Matter More Than the Phase Names

The deck highlights three practical rules:

### Rule 1

Phases overlap.

Do not wait until discovery is finished before starting agility work.

### Rule 2

Pilot before large-scale rollout.

### Rule 3

Sequence:

```text
Key exchange first
        ↓
Signatures / certificates later
```

because signatures and certificates often take much longer to migrate. fileciteturn14file0L611-L625

---

# 72. CBOM — Cryptographic Bill of Materials

A central enterprise concept is:

> **CBOM**

Crypto Bill of Materials.

It answers:

```text
Where is cryptography used?
Which algorithm?
Where?
Who owns it?
How long does the data matter?
How urgent is migration?
```

---

# 73. Example CBOM

The deck gives:

| Asset | Algorithm | Location | Owner | Data life | Priority |
|---|---|---|---|---:|---|
| Customer portal TLS | ECDHE + RSA-2048 | Load balancer | Web | 10 years | P1 |
| Site-to-site VPN | IKEv2, DH group 14 | Firewall | Network | 7 years | P1 |
| Code signing | RSA-3072 | HSM | DevOps | 15 years | P1 |
| Internal mTLS | ECDSA P-256 | Service mesh | Platform | 2 years | P2 |
| Database backups | AES-256-GCM | Object storage | DBA | 7 years | P3 |

---

# 74. Four Ways to Discover Cryptography

No single discovery mechanism finds everything.

Use:

### 1. Network scans

Look at:

- TLS,
- SSH,
- VPN,
- network protocols.

### 2. Code and dependency scanning

Search source code and dependencies for:

- RSA,
- ECC,
- DH,
- cryptographic libraries,
- certificate handling.

### 3. Certificate, key and HSM inventories

Identify:

- certificates,
- private keys,
- HSMs,
- KMS systems.

### 4. Vendor and architecture review

Ask third parties:

```text
Which algorithms do you use?
Where?
Can they be changed?
When will PQC be supported?
```

The deck notes that discovery often finds significantly more cryptography than originally expected. fileciteturn14file0L628-L644

---

# 75. CBOM Must Stay Current

A spreadsheet created once will quickly become outdated.

A better model is:

```text
CBOM
 ↓
Scheduled regeneration
 ↓
CI/CD integration
 ↓
Continuous visibility
```

The deck recommends the:

> **CycloneDX CBOM**

format and stresses that owner and data-lifetime information make the inventory useful for management, not merely documentation.

---

# 76. Phase 2 Risk Factors

The lecture gives a prioritisation model:

| Factor | Weight |
|---|---:|
| Data secrecy life | 30% |
| Exposure | 25% |
| Signature longevity | 20% |
| System lifetime / patchability | 15% |
| Regulator / customer requirement | 10% |

The basic idea is:

```text
Long-lived secret
+
High exposure
+
Long-lived signature
+
Hard-to-patch system
+
Regulatory pressure
        ↓
Higher priority
```

---

# 77. P1, P2 and P3

### P1

Examples:

- Internet-facing TLS,
- VPN,
- code signing,
- firmware signing,
- root CAs,
- long-lived customer data,
- interbank messaging.

### P2

Examples:

- internal mTLS,
- identity and SSO,
- privileged access,
- database key wrapping,
- partner APIs.

### P3

Examples:

- short-lived internal traffic,
- AES-256 at rest,
- systems retiring before the migration deadline.

---

# 78. Why Signing Can Outrank Encryption

The lecture makes an important point:

> **A signature shipped today may be impossible to repair later.**

For example:

```text
Firmware signed today
       ↓
Device shipped
       ↓
Device cannot be patched
       ↓
Future signature break
       ↓
Trust problem
```

Encryption can sometimes be changed or data re-encrypted.

Embedded trust anchors are harder.

---

# 79. Crypto-Agility

The deck calls crypto-agility:

> **The real strategic target.**

The test is:

> If ML-KEM were weakened tomorrow, how quickly could the organisation move to another approved mechanism such as HQC?

A crypto-agile organisation should be able to answer:

```text
Policy change
+
Key rotation
+
Configuration change
```

rather than:

```text
Start another multi-year software project
```

---

# 80. Rules for Crypto-Agile Systems

The deck recommends:

### Do not hard-code algorithms

Bad:

```text
algorithm = RSA
```

inside application logic.

Better:

```text
algorithm = configuration / policy
```

### Centralise policy

```text
One crypto policy
        ↓
Shared libraries
```

### Automate certificates

Use short-lived certificates with automated lifecycle management.

### Centralise keys

Use:

- HSM,
- KMS,
- central key-management systems.

### Add headroom

Make sure:

- buffers,
- database columns,
- network fields,
- protocol structures

can accommodate larger post-quantum objects.

### Consolidate classical cryptography

Reduce unnecessary cryptographic diversity before introducing PQC.

---

# 81. Domain-by-Domain Migration

The deck provides:

| Domain | Standard move | Main blocker |
|---|---|---|
| External web/API | Hybrid X25519MLKEM768 at CDN/load balancer | Old TLS middleboxes |
| Remote access/VPN | Hybrid key exchange | Vendor firmware |
| SSH/admin | OpenSSH 10 default KEX | Old jump hosts |
| Internal PKI | PQ roots, dual chains, HSM upgrades | Trust distribution |
| Code/firmware signing | ML-DSA or LMS/XMSS | Hard-coded device algorithms |
| Data at rest | Keep AES-256; secure key wrapping | Misidentifying database encryption as main issue |
| Identity/SSO | PQ signatures | IdP vendor roadmap |
| IoT/OT | PQ secure boot; gateway termination | Unpatchable fleets |

The deck suggests a common sequence:

```text
Edge TLS
 ↓
Remote access
 ↓
Signing infrastructure
```

with PKI and OT progressing in parallel. fileciteturn14file0L687-L702

---

# 82. Third-Party Risk

A large amount of organisational exposure exists inside somebody else's product.

The deck recommends asking every critical vendor:

1. Which quantum-vulnerable algorithms do you use?
2. Where are they used?
3. What is your PQC roadmap?
4. Are implementations FIPS-validated?
5. Can algorithms change by configuration?
6. What happens to deployed hardware?
7. What happens to data already held?

---

# 83. Contract Requirements

PQC requirements should become procurement requirements.

Contracts can require:

- NIST algorithm support by a stated date,
- crypto-agility,
- notification when cryptographic components change,
- annual CBOM,
- validation evidence.

A useful procurement rule from the deck is:

> **No new system should be approved unless it is crypto-agile and its vendor has a dated roadmap.**

---

# 84. Beware "Quantum-Safe" Marketing

A vendor saying:

> "Quantum-safe"

does not automatically prove that it implements the required standards.

The deck recommends asking for:

```text
FIPS 203 / 204 support
+
Validation evidence
+
Migration roadmap
```

rather than accepting a generic marketing claim.

---

# 85. Measuring Migration

The board needs measurable KPIs.

The deck lists:

| KPI | Healthy direction |
|---|---|
| Inventory coverage | Toward 100%, refreshed quarterly |
| P1 assets still classical-only | Falling |
| External sessions using PQ groups | Rising |
| Certificates renewed automatically | Toward 100% |
| Time to change an algorithm | Weeks, not quarters |
| Critical vendors with dated roadmap | Rising |

---

# 86. Maturity Model

The deck provides six maturity states:

```text
0 — Unaware
    No owner, no inventory

1 — Aware
    Owner named, board briefed

2 — Informed
    Live CBOM and risk register

3 — Agile
    Central keys, automated certificates, hybrid pilots

4 — Migrating
    P1 estate moved, vendors committed

5 — Governed
    Algorithm change is routine
```

The deck's 2026 estimate places most large enterprises between levels 1 and 2, with a much smaller fraction broadly deployed with quantum-safe cryptography. This is a time-specific claim from the source deck. fileciteturn14file0L726-L746

---

# 87. Common Migration Mistakes

| Mistake | Better approach |
|---|---|
| Wait for a perfect inventory | Start agility and edge pilots in parallel |
| Treat PQC as a project with an end date | Build a permanent crypto lifecycle capability |
| Trust "quantum-safe" marketing | Demand standard/validation evidence |
| Assume cloud provider handles everything | Check your applications, keys and libraries |
| Ignore signatures | Start signing infrastructure early |
| Test only in isolated labs | Test real network paths |
| Ignore buffer sizes | Perform size testing |
| Write cryptography yourself | Use vetted libraries |

The common theme is:

> **Choosing an algorithm is not the finish line. Being able to change the algorithm is the strategic capability.** fileciteturn14file0L749-L763

---

# 88. A 12-Month Starting Plan

The deck proposes:

## Q1 — Establish

```text
Name accountable owner
Brief board
Apply procurement gate
Start Internet-facing discovery
```

## Q2 — See the Estate

```text
CBOM for external services
PKI
HSMs
Top applications
Risk register
Vendor questionnaire
Hybrid TLS pilot
```

## Q3 — Build the Base

```text
Expand CBOM
Certificate automation
Key management
HSM/PKI upgrade planning
Signing infrastructure plan
```

## Q4 — Commit to Dates

```text
Board-approved roadmap
P1 migration dates
Hybrid production deployment
Legacy crypto decommission register
Crypto-agility drill
```

The year-one test is:

> Can you say what cryptography you use, which parts matter most, and how quickly you can change them? fileciteturn14file0L766-L791

---

# PART 7 — CAREERS

# 89. Seven Roles

The deck identifies seven career paths:

| Role | Work | Core skills |
|---|---|---|
| PQC / cryptography engineer | Implement and optimise PQC | C, Rust, mathematics, side-channel security |
| Crypto migration consultant | Inventory, roadmap, vendor review | TLS, PKI, risk, regulation |
| PKI / identity engineer | PQ certificates, HSMs, code signing, automation | X.509, HSM, automation |
| Quantum-safe security architect | Crypto-agile cloud/OT design | Architecture, cloud, zero trust |
| Embedded / IoT security engineer | PQC on devices and secure boot | Embedded C, hardware security |
| GRC / quantum risk analyst | Map standards to controls and audits | ISO 27001, audit, policy |
| Researcher | Cryptanalysis and new schemes | Advanced mathematics, research |

The deck notes that migration, PKI, cloud and GRC create roles that do not necessarily require a PhD. fileciteturn14file0L804-L817

---

# 90. Student Roadmap

The deck proposes a progression.

## This Semester — Foundation

Learn:

- networking,
- TLS,
- Python,
- C,
- linear algebra,
- discrete mathematics,
- Linux.

---

## Next 6 Months — Core Security

Learn:

- PKI,
- OpenSSL,
- cloud security,
- security fundamentals,
- CTFs.

---

## Year 1 — Specialise

Study:

```text
FIPS 203
FIPS 204
OpenSSL 3.5
liboqs
Open Quantum Safe
```

Build:

> **A CBOM scanner**

---

## Launch — Get Hired

Build:

- internships,
- GitHub portfolio,
- technical writing,
- research/project work,
- cybersecurity community participation.

The deck specifically connects India's 2027–2033 migration window to workforce demand. fileciteturn14file0L820-L850

---

# 91. PART 8 — YOUR TURN

The final section turns the theory into practical work.

It contains:

```text
Quiz
 ↓
Hands-on lab
 ↓
Group activity
 ↓
Final project
```

---

# 92. Quiz: Questions 1–5

### Q1. Which is NOT broken by Shor's algorithm?

```text
A) RSA
B) ECDSA
C) AES-256
D) Diffie-Hellman
```

**Answer: C**

---

### Q2. ML-KEM is standardised in:

```text
A) FIPS 197
B) FIPS 203
C) FIPS 204
D) FIPS 186
```

**Answer: B**

---

### Q3. HNDL mostly threatens:

```text
A) Data with a long secrecy life
B) Hashed passwords
C) Public web pages
D) Live video
```

**Answer: A**

---

### Q4. X25519MLKEM768 is:

```text
A) Certificate type
B) Hybrid key-exchange group
C) Hash function
D) QKD protocol
```

**Answer: B**

---

### Q5. Mosca:

```text
X = 10
Y = 6
Z = 12
```

Then:

```text
X + Y = 16
16 > 12
```

**Answer: B — at risk.** fileciteturn14file0L861-L876

---

# 93. Quiz: Questions 6–10

### Q6. Which is a stateless hash-based signature?

```text
A) ML-DSA
B) FN-DSA
C) SLH-DSA
D) ML-KEM
```

**Answer: C**

---

### Q7. Biggest practical PQ signature challenge?

```text
A) Size
B) Requires quantum hardware
C) No standards
D) Slower AES
```

**Answer: A**

---

### Q8. First migration step?

```text
A) Buy QKD
B) Build crypto inventory
C) Replace AES
D) Wait for Q-Day
```

**Answer: B**

---

### Q9. Maximum public TLS certificate lifetime by March 2029 in the deck?

```text
A) 398 days
B) 200 days
C) 100 days
D) 47 days
```

**Answer: D**

---

### Q10. PQC migration does NOT require:

```text
A) New algorithms
B) Larger buffers
C) Quantum computers
D) Updated libraries
```

**Answer: C**

The source deck's answer key gives these answers and explanations. fileciteturn14file0L879-L910

---

# 94. Hands-On Lab

The deck proposes a practical lab titled:

> **See post-quantum cryptography on the wire**

## Step 1 — Browser

Open:

```text
Chrome
→ DevTools
→ Security
```

Look for:

```text
X25519MLKEM768
```

---

## Step 2 — OpenSSL

Check:

```bash
openssl version
```

The deck expects:

```text
OpenSSL 3.5+
```

Then:

```bash
openssl list -kem-algorithms
```

---

## Step 3 — Force Hybrid TLS

The deck gives:

```bash
openssl s_client -connect cloudflare.com:443 \
  -groups X25519MLKEM768
```

---

## Step 4 — Generate ML-DSA Key

The deck gives:

```bash
openssl genpkey -algorithm ML-DSA-65 \
  -out mldsa.key
```

---

## Lab Deliverable

Produce:

1. Screenshot of the negotiated group.
2. Table comparing ML-DSA and ECDSA key/signature sizes.

The deck also recommends generating an ECDSA key and signing the same file to compare sizes. fileciteturn14file0L916-L932

---

# 95. Group Activity: Quantum Risk Triage

Teams choose one organisation:

- Private bank with UPI app
- City hospital
- Smart-meter fleet
- SaaS startup on AWS
- State e-governance portal

Then:

```text
1. List five crypto assets
2. Estimate X and Y
3. Apply Mosca
4. Rank P1/P2/P3
5. Choose three first actions
6. Present in two minutes
```

The deck recommends using the bank inventory and CBOM examples as templates. fileciteturn14file0L935-L952

---

# 96. Final Project: Quantum-Safe Migration Blueprint

Teams of 3–5 select one organisation:

```text
Bank with UPI
Hospital network
IoT / smart-city fleet
Cloud SaaS company
College IT
```

---

# 97. Project Deliverables

The project requires:

### 1. CBOM

At least:

```text
10+ crypto assets
```

### 2. Mosca-based risk ranking

For each asset:

```text
X
Y
Z
Risk
```

### 3. Target architecture

Must include:

```text
Hybrid cryptography
+
Crypto-agility
```

### 4. Roadmap

```text
90-day pilot
+
3-year migration roadmap
```

### 5. Optional demo

Examples:

- hybrid TLS,
- ML-DSA.

---

# 98. Project Marking Scheme

| Component | Weight |
|---|---:|
| Inventory | 20% |
| Risk analysis | 20% |
| Architecture | 20% |
| Regulatory fit | 15% |
| Demo / evidence | 15% |
| Presentation | 10% |

The deck specifies:

```text
10-minute presentation
+
5-minute questions
+
8–10 slides
```

Every team member should speak.

If scanning an organisation's infrastructure, written permission is required. fileciteturn14file0L955-L985

---

# 99. Six Sentences to Remember

The conclusion of the deck can be condensed into six core ideas:

### 1.

> **Every control has a clock.**

Ask:

```text
What does it protect?
For how long?
```

### 2.

> **Harvest now, decrypt later means the attack has already started.**

Captured data can remain vulnerable for years.

### 3.

> **Quantum threatens public-key key exchange and signatures much more directly than AES-256 and hashing.**

### 4.

> **The standards exist.**

Know:

```text
ML-KEM
ML-DSA
SLH-DSA
```

### 5.

> **Automation creates agility.**

Especially:

```text
Automated certificates
+
Central key management
+
Configurable cryptography
```

### 6.

> **Inventory first, agility always.**

The strategic capability is not merely choosing the correct algorithm.

It is:

> **Being able to change the algorithm when necessary.** fileciteturn14file0L988-L1005

---

# 100. High-Value Exam Questions

## Q1. What is CIA-T?

CIA-T extends the traditional CIA security model by explicitly considering the time dimension:

```text
Confidentiality over time
Integrity over time
Availability over time
```

It asks how long a security control remains effective.

---

## Q2. What is Harvest Now, Decrypt Later?

It is an attack strategy in which adversaries collect encrypted data today and store it until future cryptographic capabilities, particularly quantum computing, allow the data to be decrypted.

---

## Q3. What is Trust Now, Forge Later?

It is the integrity analogue of HNDL. Data or software signed today may become forgeable in the future if the signature algorithm becomes vulnerable to quantum attacks.

---

## Q4. State Mosca's rule.

```text
X + Y > Z
```

where:

- `X` = required secrecy lifetime,
- `Y` = migration time,
- `Z` = estimated time until the relevant cryptographic break.

If `X + Y > Z`, migration planning is already late.

---

## Q5. Which public-key algorithms are threatened by Shor's algorithm?

The lecture identifies:

- RSA,
- Diffie-Hellman,
- ECC,
- ECDH,
- ECDSA,
- X25519.

---

## Q6. What happens to AES-128 and AES-256?

According to the lecture's simplified model:

```text
AES-128
→ Grover roughly halves the effective security

AES-256
→ remains at a security level considered adequate against the discussed quantum threat
```

---

## Q7. What is ML-KEM?

ML-KEM is a NIST-standardised post-quantum Key Encapsulation Mechanism based on the Kyber algorithm family and intended for quantum-resistant key establishment.

---

## Q8. What is ML-DSA?

ML-DSA is a lattice-based post-quantum digital signature standard.

---

## Q9. What is SLH-DSA?

SLH-DSA is a stateless hash-based post-quantum signature scheme derived from SPHINCS+.

---

## Q10. Why is HQC important?

HQC provides a code-based alternative key-establishment mechanism and therefore gives the ecosystem mathematical diversity beyond lattice-based ML-KEM.

---

## Q11. What is hybrid key exchange?

Hybrid key exchange combines:

```text
Classical key exchange
+
Post-quantum KEM
```

For example:

```text
X25519
+
ML-KEM-768
=
X25519MLKEM768
```

---

## Q12. Why is hybrid deployment useful?

It can provide protection based on both classical and post-quantum mechanisms during the transition period.

---

## Q13. What is the main practical problem with PQC?

The lecture emphasises:

> **Size**

PQC keys, ciphertexts and especially signatures can be substantially larger than classical equivalents.

---

## Q14. What is a CBOM?

A:

> **Cryptographic Bill of Materials**

is an inventory of cryptographic assets across an organisation, including algorithms, locations, owners, data lifetime and migration priority.

---

## Q15. What is crypto-agility?

Crypto-agility is the ability to change cryptographic algorithms, keys, certificates and related implementations without requiring a large-scale redesign of the entire system.

---

## Q16. What are the five migration phases?

```text
1. Discover
2. Prioritise
3. Build agility
4. Migrate
5. Operate
```

---

## Q17. Why should signatures be migrated early?

Because signed objects such as:

- firmware,
- certificates,
- legal documents,
- root trust anchors

may need to remain trustworthy for many years and may be difficult or impossible to update after deployment.

---

## Q18. Does PQC require a quantum computer?

No.

PQC is designed to run on classical computing infrastructure.

---

## Q19. PQC vs QKD?

```text
PQC
→ software + mathematical cryptography

QKD
→ quantum physical communication
```

PQC can be deployed through software and infrastructure updates, while QKD requires specialised communication infrastructure.

---

## Q20. Why is cloud migration not automatically enough?

Because cloud providers may protect their own endpoints while an organisation's:

- applications,
- libraries,
- certificates,
- keys,
- protocols

remain vulnerable.

---

# 101. Important Tables for Revision

## Quantum Threat Summary

| Technology | Quantum effect | Migration direction |
|---|---|---|
| RSA | Shor | Replace |
| ECC / ECDSA / ECDH | Shor | Replace |
| Diffie-Hellman | Shor | Replace |
| AES-128 | Grover | Move toward AES-256 |
| AES-256 | Reduced margin but survives in lecture model | Keep |
| SHA-256 / SHA-3 | Minor Grover impact | Keep |

---

## PQC Standards

| Standard | Name | Purpose | Basis |
|---|---|---|---|
| FIPS 203 | ML-KEM | Key exchange | Module lattice |
| FIPS 204 | ML-DSA | Signatures | Module lattice |
| FIPS 205 | SLH-DSA | Signatures | Hash-based |
| FIPS 206 | FN-DSA | Signatures | Lattice |
| HQC | HQC | Key exchange | Code-based |
| SP 800-208 | LMS/XMSS | Signatures | Hash-based |

---

## Enterprise Priority

```text
P1
├── Internet-facing TLS
├── VPN
├── Code signing
├── Firmware signing
├── Root CAs
├── Long-lived customer data
└── Interbank messaging

P2
├── Internal mTLS
├── Identity / SSO
├── Privileged access
├── Key wrapping
└── Partner APIs

P3
├── Short-lived internal traffic
├── AES-256 at rest
└── Systems retiring before deadline
```

---

# 102. Final Mental Model

The entire lecture can be remembered as a story.

## Step 1 — Security has a clock

```text
Security
   ↓
Not just YES/NO
   ↓
How long?
```

## Step 2 — Quantum changes the clock

```text
RSA / ECC
   ↓
Shor
   ↓
Future break
```

## Step 3 — The attack may already have started

```text
Encrypted data
   ↓
Captured today
   ↓
Stored
   ↓
Decrypted later
```

## Step 4 — Replace vulnerable public-key mechanisms

```text
RSA / ECC
   ↓
ML-KEM
ML-DSA
SLH-DSA
```

## Step 5 — Do not migrate blindly

```text
Inventory
   ↓
Risk ranking
   ↓
Crypto-agility
   ↓
Hybrid deployment
   ↓
PQC migration
```

## Step 6 — Make cryptographic change routine

```text
Old algorithm
      ↓
Policy change
      ↓
Configuration
      ↓
New algorithm
```

That is the real destination:

> **Not one "perfect" quantum-safe algorithm, but an organisation that can continuously change its cryptography as the threat landscape changes.**

---

# 103. The Lecture in 20 Lines

```text
1. Security has a time dimension.
2. Confidentiality, integrity and availability all have validity horizons.
3. Harvest Now, Decrypt Later threatens long-lived confidential data.
4. Trust Now, Forge Later threatens long-lived digital signatures.
5. Mosca's rule compares secrecy life + migration time against expected quantum-break time.
6. Shor's algorithm threatens RSA, ECC and Diffie-Hellman.
7. Grover reduces the effective brute-force security of symmetric systems.
8. AES-256 remains the preferred symmetric choice in the lecture's model.
9. PQC runs on classical computers.
10. ML-KEM is the standardised Kyber-family KEM.
11. ML-DSA provides lattice-based signatures.
12. SLH-DSA provides stateless hash-based signatures.
13. HQC provides a code-based alternative to ML-KEM.
14. Hybrid X25519 + ML-KEM provides transitional protection.
15. PQC is computationally practical but often produces larger objects.
16. Certificates and signatures are harder to migrate than key exchange.
17. Governments and browser ecosystems are pushing migration timelines.
18. Enterprises begin with a CBOM and risk register.
19. Crypto-agility is the strategic goal.
20. The final objective is making cryptographic change routine.
```

---

# 104. Final Takeaways

1. **Think about security in time, not just in terms of secure/insecure.**
2. **HNDL means sensitive data captured today can become vulnerable later.**
3. **Long-lived signatures create a separate integrity problem.**
4. **RSA, ECC and Diffie-Hellman are the major public-key systems threatened by Shor's algorithm.**
5. **AES-256 remains the symmetric encryption choice highlighted by the lecture.**
6. **ML-KEM is for quantum-resistant key establishment.**
7. **ML-DSA and SLH-DSA are for post-quantum signatures.**
8. **HQC provides mathematical diversity as a code-based alternative.**
9. **Hybrid cryptography is an important transition strategy.**
10. **PQC does not require quantum hardware.**
11. **The main engineering challenge is often object size rather than raw computation.**
12. **Certificates and signatures can be much harder to migrate than key exchange.**
13. **The first enterprise step is cryptographic discovery.**
14. **CBOM turns cryptography into an inventory that can be managed.**
15. **Mosca's rule provides a simple way to prioritise long-lived data.**
16. **Crypto-agility is more important than permanently choosing one algorithm.**
17. **Vendor and cloud-provider claims must be verified against actual cryptographic usage.**
18. **IoT, healthcare, banking and other long-lived systems need particularly early planning.**
19. **Automation of certificates and key management is foundational to agility.**
20. **The quantum-safe future is an ongoing cryptographic lifecycle, not a one-time migration project.**
