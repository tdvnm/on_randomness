# Does Random Mean Secure?
## A Statistical Investigation of Randomness, Entropy, and Perfect Secrecy in Cryptography

---

## Overview

Modern cryptography depends heavily on randomness.

Random numbers are used to generate encryption keys, nonces, initialization vectors, passwords, authentication tokens, and many other security-critical values. At first glance, it may seem that if a sequence of numbers "looks random", it should also be secure.

However, these are not the same thing.

A sequence may pass several statistical tests for randomness while still being predictable. Similarly, an encrypted message may appear completely scrambled while still leaking statistical information about the original message.

This project investigates the relationship between:

- probability,
- statistical randomness,
- entropy,
- information leakage,
- pseudorandom number generation,
- and cryptographic secrecy.

The central idea of the project is to explore the following question:

> **To what extent does statistical randomness contribute to cryptographic secrecy, and does something that appears random necessarily provide security?**

The project combines classical probability and statistics with foundational ideas from cryptography and information theory.

Rather than simply implementing an encryption algorithm, the aim is to use statistical tools to study what makes randomness useful for encryption and what it mathematically means for encrypted information to remain secret.

---

# Central Research Question

The main research question is:

> **How does the quality of randomness used in an encryption system affect the amount of statistical information revealed by its ciphertext?**

This leads to several smaller questions:

1. How can randomness be measured statistically?
2. Can statistical tests distinguish human-generated, biased, pseudorandom, and genuinely random-looking sequences?
3. What does it mathematically mean for an encrypted message to reveal no information?
4. How does randomness affect the security of encryption?
5. What happens when the randomness used for encryption is biased or predictable?
6. Can a sequence pass statistical randomness tests while still being cryptographically insecure?
7. Is statistical randomness sufficient for cryptographic security?

The project ultimately investigates the distinction between:

> **Random-looking**

and

> **Unpredictable**

These concepts are related, but they are not equivalent.

---

# Why This Topic?

Randomness appears naturally in probability theory, while secrecy appears naturally in cryptography.

The connection between the two is surprisingly deep.

For example, consider a message \(M\) and its encrypted ciphertext \(C\).

Before observing the ciphertext, an attacker may believe that:

\[
P(M=m)=0.3
\]

for some possible message \(m\).

After observing the ciphertext, the attacker may update this probability:

\[
P(M=m \mid C=c).
\]

If the ciphertext reveals absolutely nothing about the message, then observing it should not change the attacker's belief.

Therefore,

\[
P(M=m \mid C=c)=P(M=m).
\]

This simple probability equation is the basis of **Claude Shannon's concept of perfect secrecy**.

The project begins with ordinary probability and gradually builds toward this idea.

---

# Main Themes of the Project

The project will be divided into several connected parts.

---

# Part 1 — What Does Randomness Mean?

The first part of the project investigates randomness from a statistical perspective.

A binary random sequence can be represented as:

\[
X_1,X_2,\ldots,X_n
\]

where each variable may take the value

\[
X_i \in \{0,1\}.
\]

For an ideal sequence of independent fair random bits,

\[
P(X_i=0)=P(X_i=1)=\frac12.
\]

If

\[
S=\sum_{i=1}^{n} X_i
\]

represents the total number of ones in the sequence, then:

\[
S \sim \text{Binomial}\left(n,\frac12\right).
\]

Therefore,

\[
E[S]=\frac n2
\]

and

\[
\operatorname{Var}(S)=\frac n4.
\]

This provides one simple statistical way of investigating whether a sequence behaves as expected under a random model.

However, having approximately equal numbers of zeros and ones is not enough.

For example,

```text
01010101010101010101
