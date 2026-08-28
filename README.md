# Does Random Mean Secure?
### A Probability and Statistics Project on Randomness, Entropy and Cryptography

## Project Overview

This project studies the connection between probability, randomness and encryption.

Randomness is extremely important in cryptography. Encryption keys and many other security systems depend on numbers that should be difficult to predict. However, a sequence that looks random is not necessarily secure, and encrypted data that looks completely scrambled may still contain information about the original message.

The main idea of this project is to investigate two questions:

1. How can we use statistics to test whether something behaves randomly?
2. How does the quality of randomness affect the security of encryption?

---

## Main Research Question

The main question I want to investigate is:

> **How does the quality of randomness used in encryption affect the amount of information that the ciphertext reveals?**

A related question is:

> **If a sequence passes statistical tests for randomness, does that mean it is cryptographically secure?**

The project will show that these are not exactly the same thing.

---

# Part 1: Understanding Randomness

The first part of the project will study what a random binary sequence should look like.

A binary sequence consists of zeros and ones, for example:

`101100101011001`

For an ideal random sequence:

P(0) = 0.5  
P(1) = 0.5

Each bit should also be independent of the previous bits.

I will generate different types of sequences and compare them.

These will include:

- fair random coin flips
- biased coin flips
- human-generated "random" sequences
- sequences from simple pseudorandom number generators
- sequences from stronger random or cryptographic generators

The goal is to see whether statistical methods can distinguish between them.

---

# Part 2: Statistical Tests for Randomness

Different statistical tests will be applied to the generated sequences.

## Frequency Test

This will check whether the number of zeros and ones is close to what would be expected from a fair random process.

If there are `n` bits and each bit has probability 0.5 of being 1, then:

X ~ Binomial(n, 0.5)

The expected number of ones is:

E[X] = n/2

and the variance is:

Var(X) = n/4

The observed values can then be compared with the theoretical values.

---

## Runs Test

A run is a consecutive sequence of the same value.

For example:

`0001110011`

can be divided into:

`000 | 111 | 00 | 11`

The number and length of runs can give information about whether a sequence behaves randomly.

This will also be useful when comparing human-generated sequences with computer-generated sequences.

---

## Chi-Square Test

A chi-square test can be used to compare observed frequencies with expected frequencies.

For example, for pairs of independent random bits, the patterns:

- 00
- 01
- 10
- 11

should each appear approximately 25% of the time.

Large differences may indicate some structure or bias in the sequence.

---

## Autocorrelation

Autocorrelation will be used to check whether values in one part of the sequence are related to values appearing later.

Ideally, independent random bits should have very little correlation with previous bits.

---

## Entropy

Entropy will be used as another way of measuring uncertainty.

For a random variable X:

H(X) = -Σ P(x) log2 P(x)

For a perfectly fair binary variable:

P(0) = P(1) = 0.5

and:

H(X) = 1 bit

If one value becomes much more likely than the other, the entropy decreases.

For example, a bit generator where:

P(1) = 0.9

is much easier to predict than one where:

P(1) = 0.5

This will become important later when studying encryption keys.

---

# Part 3: Human Randomness vs Computer Randomness

One interactive part of the project will allow people to create their own sequences of zeros and ones.

For example:

`101101001010110101`

The program will analyse the sequence using:

- number of zeros and ones
- number of runs
- longest run
- entropy
- autocorrelation
- statistical randomness tests

The results can then be compared with computer-generated random sequences.

This experiment will investigate whether humans are actually good at producing random sequences.

Humans often avoid patterns such as:

`111111`

because they do not look random, even though long runs can naturally occur in real random data.

---

# Part 4: Random Numbers vs Pseudorandom Numbers

The project will also look at pseudorandom number generators.

A pseudorandom generator produces numbers using a deterministic algorithm.

One simple example is a Linear Congruential Generator:

X(n+1) = (aX(n) + c) mod m

The output may look random, but if someone knows the algorithm and its internal state, future values may be predictable.

This leads to an important distinction:

**Statistical randomness**  
Does the output behave like random data?

versus

**Cryptographic unpredictability**  
Can someone predict future values?

A generator may perform well in statistical tests but still be unsafe for cryptography.

I may also briefly study the Blum-Blum-Shub pseudorandom generator as an example of a generator designed with cryptographic unpredictability in mind.

---

# Part 5: Introducing Encryption

The next part of the project will investigate how randomness affects encryption.

A few simple encryption methods will be used so that their statistical behaviour can be studied clearly.

These may include:

- Caesar cipher
- XOR encryption
- XOR encryption using weak or biased keys
- One-Time Pad

The purpose is not to compare modern encryption algorithms, but to understand how probability and randomness affect secrecy.

---

# Part 6: Statistical Weakness of Simple Ciphers

A Caesar cipher shifts every letter by the same amount.

For example:

`HELLO`

could become:

`KHOOR`

Although the letters have changed, the statistical structure of the original language remains.

Common letters in English remain common after encryption.

This means that frequency analysis can be used to gain information about the plaintext.

This part of the project will demonstrate that:

> Data can look scrambled without actually hiding its statistical structure.

Letter-frequency graphs for plaintext and ciphertext can be compared visually.

---

# Part 7: The One-Time Pad

The One-Time Pad will be the main encryption example used in the project.

If:

M = message  
K = random key  
C = ciphertext

then encryption can be written as:

C = M XOR K

For the One-Time Pad to provide perfect secrecy, the key must be:

- completely random
- uniformly distributed
- independent of the message
- as long as the message
- used only once

The project will simulate this system and study what happens when these conditions are satisfied.

---

# Part 8: Shannon's Perfect Secrecy

The project will use Claude Shannon's idea of perfect secrecy.

Let:

M = plaintext message  
C = ciphertext

If observing the ciphertext gives no new information about the message, then:

P(M = m | C = c) = P(M = m)

This means that seeing the ciphertext does not change the probability of any particular plaintext message.

This is a direct connection between conditional probability and cryptographic secrecy.

---

# Part 9: Measuring Information Leakage

The project will use conditional entropy and mutual information to measure how much information an encrypted message reveals.

Before observing the ciphertext, uncertainty about the message is:

H(M)

After seeing the ciphertext, the remaining uncertainty is:

H(M | C)

Mutual information is:

I(M ; C) = H(M) - H(M | C)

If:

I(M ; C) = 0

then the ciphertext reveals no statistical information about the message.

This is what we expect from a correctly implemented One-Time Pad.

If:

I(M ; C) > 0

then the ciphertext is leaking some information.

---

# Part 10: Main Experiment — Changing the Quality of Randomness

This will be the main experiment of the project.

A small message space will be created, for example:

- 00
- 01
- 10
- 11

The messages will deliberately not have equal probabilities.

For example:

P(00) = 0.40  
P(01) = 0.30  
P(10) = 0.20  
P(11) = 0.10

Thousands of messages will then be generated using this distribution.

They will be encrypted using XOR with different kinds of keys.

---

## Experiment A: Uniform Random Key

First, the key will be selected uniformly:

P(00) = 0.25  
P(01) = 0.25  
P(10) = 0.25  
P(11) = 0.25

The experiment should show that the ciphertext does not reveal the original message distribution.

The estimated mutual information should approach:

I(M ; C) ≈ 0

as the number of simulations increases.

---

## Experiment B: Biased Random Key

The key distribution will then be deliberately changed.

For example:

P(00) = 0.70

with the remaining probability distributed among the other keys.

The encryption algorithm will still be exactly the same:

C = M XOR K

Only the source of randomness changes.

The experiment will measure how much information now leaks through the ciphertext.

The expectation is:

I(M ; C) > 0

---

## Experiment C: Gradually Increasing Bias

Rather than testing only one biased key, I plan to gradually change the probability.

For example:

P(K = 1) =

0.50  
0.55  
0.60  
0.65  
0.70  
0.75  
0.80  
0.85  
0.90  
0.95

For every level of bias I can calculate:

- entropy of the key
- statistical test results
- ciphertext distribution
- conditional entropy
- mutual information between plaintext and ciphertext

This should allow me to produce graphs showing how decreasing randomness affects information leakage.

This will be one of the main results of the project.

---

# Part 11: Random-Looking Does Not Always Mean Secure

Another experiment will compare pseudorandom sequences with genuinely random sequences.

Some deterministic generators may produce sequences that:

- contain roughly equal numbers of zeros and ones
- have normal-looking runs
- have high entropy
- pass basic chi-square tests
- show very little autocorrelation

Statistically, they may look random.

However, if the generator's seed or internal state is known, the sequence may be completely predictable.

This shows that:

**Random-looking does not automatically mean secure.**

Statistical testing can identify some bad random generators, but passing statistical tests does not prove cryptographic security.

---

# Interactive Component

The project will include an interactive program or webpage.

The main features I plan to include are:

## Randomness Tester

The user enters a binary sequence.

The program calculates:

- number of zeros and ones
- percentage of ones
- entropy
- number of runs
- longest run
- autocorrelation
- results of statistical tests

---

## Human vs Machine Randomness

The user will try to generate a random sequence manually.

The program will compare it with computer-generated sequences and show differences in their statistical behaviour.

---

## Guess Which Sequence Is Random

Several sequences will be displayed and the user will guess which one was generated randomly.

The actual source and statistical results will then be revealed.

---

## Encryption Demonstration

The user will enter a message and see how it is encrypted using different methods.

Examples may include:

- Caesar cipher
- XOR with biased randomness
- XOR with a pseudorandom stream
- One-Time Pad

The program can then display the statistical properties of the resulting ciphertext.

---

## Information Leakage Visualisation

Graphs will show how the amount of information leakage changes as the randomness of the encryption key changes.

For example:

Key Bias vs Mutual Information

and:

Key Entropy vs Information Leakage

This should make the relationship between probability and security easier to understand visually.

---

# Probability and Statistics Topics Used

The project will cover several topics from probability and statistics, including:

- random variables
- Bernoulli trials
- binomial distribution
- expected value
- variance
- probability distributions
- conditional probability
- Bayes' theorem
- independence
- hypothesis testing
- chi-square tests
- p-values
- runs tests
- correlation
- autocorrelation
- entropy
- conditional entropy
- mutual information

The main aim is to apply these concepts rather than discuss them only theoretically.

---

# Research Background

The project will be based around three important ideas.

## Claude Shannon — Perfect Secrecy

Claude Shannon's work on the mathematical theory of secrecy will be used to understand the probability behind secure encryption.

The main idea used in this project is:

P(M = m | C = c) = P(M = m)

which defines perfect secrecy.

---

## Statistical Testing of Random Number Generators

The project will also look at methods similar to those used in statistical randomness testing, including ideas from the NIST Statistical Test Suite.

These tests examine whether generated sequences have statistical properties expected from random data.

---

## Cryptographic Pseudorandomness

The project will briefly look at the idea that cryptographic random generators need to be unpredictable, not simply statistically convincing.

Blum-Blum-Shub may be used as a historical example of a pseudorandom generator designed around this idea.

---

# Expected Results

I expect the experiments to show that:

1. Human-generated random sequences contain detectable patterns.

2. Simple statistical tests can detect biased or badly designed random generators.

3. High entropy generally makes values harder to predict.

4. Simple encryption systems may preserve statistical information from the plaintext.

5. A One-Time Pad using a uniformly random key should have approximately zero mutual information between plaintext and ciphertext.

6. Biasing the key distribution should increase the amount of information leaked.

7. As the entropy of the key decreases, information leakage should increase.

8. A sequence can pass basic statistical randomness tests while still being predictable.

9. Statistical randomness and cryptographic security are therefore related, but they are not the same thing.

---

# Final Objective

The final goal of the project is to understand the relationship between:

**randomness → uncertainty → information → secrecy**

Rather than simply implementing an encryption algorithm, I want to experimentally study what role probability actually plays in making encryption secure.

The main conclusion I want to investigate is:

> **Good randomness is necessary for many cryptographic systems, but looking statistically random is not enough to guarantee security.**

---

## Working Title

**Does Random Mean Secure?**  
*Randomness, Entropy and Perfect Secrecy Through Probability and Statistics*
