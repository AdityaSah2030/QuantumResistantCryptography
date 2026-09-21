# Build Secure Futures: Bootcamp on Quantum-Resistant Cryptography

## Day 1: Introduction to Cybersecurity

**Date:** 21 September 2026  
**Lecture:** Introduction to Cybersecurity  
**Speaker:** Padma Shri Bimal Kumar Roy, Former Director, Indian Statistical Institute, Kolkata

> **Note:** These notes cover the introductory cybersecurity lecture and the examples discussed during the session.

---

## 1. Story: Estimating Refugee Population Using Food Supplies

### Background

Immediately after Indian independence in 1947, communal riots broke out in Delhi. Thousands of displaced people took shelter in a makeshift refugee camp inside the Red Fort.

The newly formed government under Jawaharlal Nehru had to arrange food for the refugees. Because the situation was volatile, officials could not physically enter the fort to conduct a census or count the people.

Instead, private contractors supplied daily rations:

- Rice
- Pulses (Dal)
- Salt

The government noticed that the contractors were submitting extremely high bills and suspected that the quantities were being inflated to obtain extra money.

The government approached **Professor P. C. Mahalanobis and the Indian Statistical Institute (ISI)** for help.

### The Problem

> How can we accurately estimate the number of people inside a sealed location using only the food bills submitted by contractors?

### The Statistical Approach

The ISI team had historical data regarding the average daily per-capita intake of an Indian adult for rice, pulses, and salt.

For each commodity:

**Estimated Population = Total Quantity Supplied Per Day / Average Daily Intake Per Person**

They calculated three independent estimates:

1. **Rice estimate:** Suggested a very high population.
2. **Pulse estimate:** Suggested a moderate population.
3. **Salt estimate:** Suggested the lowest population.

### Why Was Salt the Useful Measurement?

#### Rice

Rice was relatively expensive, so contractors had a financial incentive to inflate the quantity.

Also, human consumption of rice is flexible. A person who is hungry can consume substantially more rice.

#### Salt

Salt was:

- Very cheap
- Less attractive to inflate for financial gain
- Subject to much tighter physiological limits on consumption

A person cannot simply consume two or three times their normal salt requirement without making the food extremely difficult to eat.

### Key Insight

The salt supply was therefore treated as a more reliable indicator of the actual number of people.

> **Use an indirect measurement that is difficult to manipulate and has relatively predictable behavior.**

This is an example of extracting useful information without directly observing the thing being measured.

---

# 2. Story: Randomized Response

The next example demonstrated how sensitive information can be collected while protecting individual privacy.

### The Problem

Suppose a teacher wants to determine:

> **How many students drink alcohol?**

If students are asked directly, some may not answer truthfully because the question is sensitive.

The teacher therefore needs a way to obtain useful aggregate information without knowing the answer of any individual student.

## Randomized Response

Each student secretly flips a fair coin.

### If Heads

Answer:

> "Do you drink alcohol?"

### If Tails

Answer:

> "Are you born between 1 January and 30 June?"

The teacher only receives a **Yes/No** answer.

The teacher cannot determine which question a particular student answered.

Therefore, an individual student's "Yes" does not reveal whether that student drinks alcohol.

---

## Example Calculation

### Given

- Total students:

**N = 400**

- Total "Yes" answers received:

**Y = 134**

- Probability of Heads/Tails:

**P(Heads) = P(Tails) = 0.5**

Approximately half the students answer each question.

Therefore:

- Approximately **200 students** answer the alcohol question.
- Approximately **200 students** answer the birthday question.

### Step 1: Expected Birthday "Yes" Answers

The period from 1 January to 30 June represents approximately half of the year.

Therefore:

**P(Birthday Yes) = 0.5**

Expected birthday Yes answers:

**200 × 0.5 = 100**

So approximately **100 Yes answers** are expected from the birthday question.

### Step 2: Isolate the Alcohol "Yes" Answers

Total Yes answers:


134


Expected birthday Yes answers:


100


Therefore:

**Alcohol Yes = 134 - 100 = 34**

Approximately **34 of the 200 students** who answered the alcohol question said Yes.

### Step 3: Estimate the Entire Class

Since approximately half the class answered the alcohol question:

**Estimated Alcohol Drinkers = 34 × (400 / 200) = 68**

Therefore:

Estimated number of alcohol drinkers ≈ 68

This corresponds to approximately:

17% of the class.


### Key Insight

The teacher cannot identify the response of any particular student, but can still estimate the aggregate proportion of students exhibiting the sensitive behavior.

> **Randomization can protect individual privacy while allowing useful information to be obtained from a population.**

---

# 3. Story: Randomization Can Reduce Bias

Another example showed how randomization can make a process fairer.

## The Problem: Dividing Something Between Two Children

Suppose a mother has to divide an item between two children.

If the mother divides it herself, both children may believe that the other child received the larger or better piece.

Even when the division is objectively fair, the process may be perceived as biased.

## The Randomization Solution

Use a coin toss:

- **Heads:** Child A divides the item.
- **Tails:** Child B divides the item.

The child who did not divide the item gets to choose their piece first.

### Why Does This Work?

Suppose Child A is the divider.

If Child A creates unequal pieces and makes one piece larger, Child B gets to choose first and will naturally take the larger piece.

Therefore, Child A has an incentive to divide the item as equally as possible.

The same reasoning applies if Child B is the divider.

### Key Insight

> **Randomization can reduce systematic bias by preventing one person from always receiving the advantage.**

---

# 4. Story: Worker Numbers and the Idea of Hashing

Another historical example discussed workers being assigned numbers so that a number could be used to represent their identity.

Instead of repeatedly using the person's complete identity, a number served as an identifier.

This was presented as an early form of the idea behind **one-way hashing**.

## Hashing Concept

In modern computing, a hash function transforms input data into a derived value:

**Input Data → Hash Function → Hash Value**

Conceptually:

```text
Person's information
        |
        v
   Hash function
        |
        v
   Derived identifier
```

The security-related idea is that a derived representation can be used instead of repeatedly exposing the original information.

### Modern Connections

Hashing is used in areas such as:

- Password storage
- Data integrity
- Digital signatures
- File verification
- Blockchain systems
- Identifiers and indexing

> **Note:** The historical numbering example is an analogy for the concept of hashing. A simple assigned number is not necessarily a cryptographic hash in the modern technical sense.

---

# 5. Story: Estimating Students' Spending Without Revealing Individual Spending

The final example focused on privacy-preserving data collection.

### Background

During school, students were not supposed to keep money with them. However, some students still carried and spent money.

The teachers wanted to know:

> **How much money were students spending on average?**

But directly asking every student how much they spent would reveal individual spending.

The challenge was therefore:

> **How can we estimate average spending without revealing any individual student's actual spending?**

---

# Data Obfuscation

The students were asked to introduce a random value into their spending amount.

Each student:

1. Randomly chooses a number between **1 and 10**.
2. Adds the random number to their actual spending.
3. Reports the resulting sum to the teacher.

### Example

Suppose:

```text
Actual spending = ₹5
Random number   = 7

Reported value  = ₹12
```

The teacher sees ₹12 but does not know the student's actual spending.

---

## Recovering the Average

If the random number is selected uniformly from 1 to 10, its expected value is:

**(1 + 2 + 3 + … + 10) / 10 = 5.5**

Therefore:

**Average Reported Value = Average Actual Spending + 5.5**

So:

**Average Actual Spending = Average Reported Value - 5.5**

### Example

If the average reported value is ₹10.5:


10.5 - 5.5 = ₹5


Therefore, the estimated average actual spending is **₹5**.

### Key Insight

The random number acts as **noise** that hides the individual's actual value.

When many students are considered together, the expected contribution of the random noise can be accounted for mathematically.

> **Controlled noise can protect individual data while preserving useful aggregate information.**

This is an example of **data obfuscation**.

---

# 6. Connecting the Stories

Although the examples appear different, they demonstrate a common idea:

## Extract useful information without unnecessarily exposing individual information.

| Story | Problem | Technique / Idea | Main Lesson |
|---|---|---|---|
| Refugee population | Cannot physically count people | Indirect statistical estimation | Use reliable indirect measurements |
| Randomized response | Sensitive personal question | Randomization + probability | Protect individual privacy while estimating a population |
| Dividing between children | Perceived unfairness | Randomization | Randomization can reduce bias |
| Worker numbering | Representing identity | Hashing analogy | Use a derived representation instead of directly exposing original information |
| Student spending | Need average without revealing individuals | Data obfuscation / random noise | Add controlled noise to protect individual data while preserving aggregate information |

---

# 7. Overall Takeaways from the Lecture

The examples progressively connect several ideas:

### 1. Statistical Estimation

Something does not always have to be directly observed to be estimated.

### 2. Randomization

Randomness can be deliberately introduced into a system to:

- Reduce bias
- Protect privacy
- Produce statistically useful results

### 3. Privacy

It is possible to obtain information about a population without knowing the corresponding information about every individual.

### 4. Data Obfuscation

Adding controlled random noise can hide sensitive individual values while still allowing aggregate statistics to be calculated.

### 5. Hashing

A derived representation can be used instead of directly exposing the original data.

---

## Central Principle

> **Security is not simply about hiding all information. It is about designing systems where useful information can be obtained while unnecessary exposure of sensitive information is minimized.**

These statistical and privacy concepts provide an introduction to the broader ideas of **cybersecurity and cryptography** that follow in the bootcamp.
