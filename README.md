# QuantumResistantCryptography

Notes, examples, and key takeaways from the **Build Secure Futures: Bootcamp on Quantum-Resistant Cryptography**.

## About the Bootcamp

**Build Secure Futures: Bootcamp on Quantum-Resistant Cryptography** was organized by **Techno Main Salt Lake in collaboration with the Indian Statistical Institute, Kolkata** from **21 to 25 September 2026**.

The bootcamp covers cybersecurity, cryptography, quantum computing, quantum-resistant cryptography, post-quantum algorithms, and practical quantum-safe security.

## Bootcamp Schedule

| Date | Session |
|---|---|
| **21 Sep 2026** | Introduction to Cybersecurity |
| | Understanding Cryptography |
| **22 Sep 2026** | Public-Key & Secret-Key Cryptography |
| | Digital Signatures and Hashing |
| | Beyond Post Quantum Cryptography: Privacy Enhancing Technologies & Autonomous AI Defence for a Quantum-Safe Digital World |
| **23 Sep 2026** | What is Quantum Computing? |
| | How Quantum Computers Can Break Today's Cryptography |
| | Introduction to Quantum-Resistant Cryptography |
| **24 Sep 2026** | ML-DSA (Dilithium): Quantum-Safe Digital Signatures |
| | Understanding Modern Post-Quantum Algorithms |
| | ML-KEM (Kyber): A Simple Introduction |
| **25 Sep 2026** | Building a Simple Quantum-Safe Security System |
| | Future of Cybersecurity: Quantum-Safe Digital World |
| | Valedictory |

## Notes

### Day 1: Introduction to Cybersecurity

**Speaker:** Padma Shri Bimal Kumar Roy  
**Position:** Former Director, Indian Statistical Institute, Kolkata

The introductory lecture used statistical and everyday examples to explain ideas related to information extraction, randomization, privacy, bias, data obfuscation, and hashing.

### 1. Estimating Refugee Population Using Food Supplies

Following the communal riots after Indian independence in 1947, thousands of displaced people took shelter in a refugee camp inside the Red Fort.

Officials could not physically enter the fort to conduct a census. Private contractors supplied rice, pulses, and salt, but the government suspected that the contractors were inflating their bills.

The ISI team used historical data on average daily per-capita consumption to estimate the population indirectly.

**Formula:**

> Estimated Population = Total Quantity Supplied Per Day / Average Daily Intake Per Person

Three estimates were obtained:

- Rice: very high population estimate
- Pulses: moderate estimate
- Salt: lowest estimate

Salt was considered the more useful measurement because it was cheap, provided little financial incentive for inflation, and had tighter physiological limits on human consumption.

**Key idea:** Use an indirect measurement that is difficult to manipulate and has relatively predictable behavior.

---

### 2. Randomized Response

Randomized Response demonstrates how sensitive information can be collected while protecting individual privacy.

Suppose a teacher wants to estimate how many students drink alcohol.

Each student secretly flips a fair coin:

- **Heads:** Answer "Do you drink alcohol?"
- **Tails:** Answer "Were you born between 1 January and 30 June?"

The teacher only sees Yes/No responses and cannot determine which question an individual student answered.

#### Example

Given:

- Total students = **400**
- Total Yes answers = **134**
- Probability of Heads/Tails = **0.5**

Approximately:

- 200 students answer the alcohol question
- 200 students answer the birthday question

Since 1 January to 30 June represents approximately half the year:

> Expected birthday Yes answers = 200 × 0.5 = 100

Therefore:

> Alcohol Yes answers = 134 - 100 = 34

Scaling this to the entire class:

> Estimated Alcohol Drinkers = 34 × (400 / 200) = 68

Therefore:

> **Estimated number of alcohol drinkers ≈ 68**

This corresponds to approximately:

> **17% of the class**

**Key idea:** Randomization can protect individual privacy while allowing useful aggregate information to be obtained.

---

### 3. Randomization Can Reduce Bias

A simple example involved a mother dividing an item between two children.

If the mother divides the item herself, both children may feel that the other received the better piece.

A randomized procedure can reduce this perceived bias:

- Heads: Child A divides
- Tails: Child B divides
- The child who did not divide chooses their piece first

The divider therefore has an incentive to make the two pieces as equal as possible.

**Key idea:** Randomization can reduce systematic bias by preventing one person from always receiving the advantage.

---

### 4. Worker Numbers and the Idea of Hashing

Another historical example discussed workers being represented by assigned numbers rather than repeatedly using their complete identity.

This was presented as an early form of the idea behind **one-way hashing**.

In modern computing:

> Input Data → Hash Function → Hash Value

Hashing is used in areas such as:

- Password storage
- Data integrity
- Digital signatures
- File verification
- Blockchain systems
- Identifiers and indexing

**Important:** The historical numbering example is an analogy for the concept of hashing. An assigned number is not necessarily a cryptographic hash in the modern technical sense.

---

### 5. Estimating Student Spending Without Revealing Individual Spending

The final example focused on privacy-preserving data collection.

Students were asked to choose a random number between 1 and 10, add it to their actual spending, and report the resulting sum.

Example:

> Actual spending = ₹5  
> Random number = 7  
> Reported value = ₹12

If the random number is uniformly selected from 1 to 10, its expected value is:

> (1 + 2 + 3 + ... + 10) / 10 = 5.5

Therefore:

> Average Reported Value = Average Actual Spending + 5.5

So:

> **Average Actual Spending = Average Reported Value - 5.5**

For example, if the average reported value is ₹10.5:

> 10.5 - 5.5 = ₹5

The estimated average actual spending is therefore ₹5.

**Key idea:** Controlled random noise can hide individual values while preserving useful aggregate information.

---

## Core Concepts from Day 1

The examples connect several ideas:

1. **Statistical estimation**  
   Information can sometimes be estimated indirectly without directly observing the target.

2. **Randomization**  
   Randomness can be deliberately introduced to reduce bias and protect privacy.

3. **Privacy-preserving data collection**  
   Aggregate information can be extracted without revealing the corresponding information of an individual.

4. **Data obfuscation**  
   Controlled noise can hide sensitive individual values while preserving useful statistical information.

5. **Hashing**  
   A derived representation can be used instead of directly exposing original information.

## Central Takeaway

> Security is not simply about hiding all information. It is about designing systems where useful information can be obtained while unnecessary exposure of sensitive information is minimized.

## Progress

- [x] Day 1: Introduction to Cybersecurity
- [ ] Day 1: Understanding Cryptography
- [ ] Day 2: Public-Key & Secret-Key Cryptography
- [ ] Day 2: Digital Signatures and Hashing
- [ ] Day 2: Privacy Enhancing Technologies & Autonomous AI Defence
- [ ] Day 3: Quantum Computing
- [ ] Day 3: Quantum Threats to Current Cryptography
- [ ] Day 3: Introduction to Quantum-Resistant Cryptography
- [ ] Day 4: ML-DSA (Dilithium)
- [ ] Day 4: Modern Post-Quantum Algorithms
- [ ] Day 4: ML-KEM (Kyber)
- [ ] Day 5: Quantum-Safe Security System
- [ ] Day 5: Future of Quantum-Safe Cybersecurity

---

## Bootcamp Information

**Organized by:** Techno Main Salt Lake  
**In collaboration with:** Indian Statistical Institute, Kolkata  
**Duration:** 21–25 September 2026  
**Theme:** Quantum-Resistant Cryptography
