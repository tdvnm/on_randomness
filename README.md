# Does Random Mean Secure?
### Randomness, Predictability and Information Leakage

A Probability and Statistics project. The question it asks:

> If the randomness used to generate an encryption key becomes easier to
> predict, how much more information does the ciphertext reveal about the
> message?

**Where this is right now:** the 20-minute presentation is written
(`slides/presentation.pdf`) and the exact leakage curve for the main
experiment has been calculated. The simulation that checks the exact
curve has not been run yet.

## Repository layout

```
slides/  Beamer deck: presentation.tex, presentation.pdf, PRESENTATION_NOTES.md
code/    simulation (planned; empty for now)
```

## Building

Requires a TeX distribution with `latexmk`. The deck uses the CM Bright
font, which ships with TeX Live.

```sh
cd slides && latexmk -pdf presentation.tex
```

## The argument

The project is one line of reasoning rather than a survey. Each step
creates the need for the next.

1. **What does random mean?** Model a source as bits
   $B_1,\ldots,B_n$ that are fair, $P(B_i=1)=\tfrac12$, and independent.
   It can fail by *bias* (wrong probabilities) or by *dependence* (bits
   influence each other).
2. **How do we test for it?** The frequency test counts ones,
   $X\sim\mathrm{Bin}(n,\tfrac12)$ under the ideal model. The runs test
   counts switches, $R-1\sim\mathrm{Bin}(n-1,\tfrac12)$. The two tests ask
   different questions: balance versus pattern.
3. **What can the tests miss?** A two-state Markov chain with switching
   probability $s$ keeps the counts at 50/50 but changes the runs,
   $E[R]=1+(n-1)s$. Balanced does not mean independent. The chain is used
   only as a model of dependence; nothing more of Markov-chain theory is
   needed.
4. **Random-looking is not unpredictable.** A p-value says how unusual a
   statistic would be under the ideal model. A large one means the test
   found nothing; it does not prove security. A deterministic generator
   $S_{n+1}=f(S_n)$ can pass tests on a finite sample while its future is
   fixed once the state is known. Cryptographic generators are
   deterministic after seeding too; their goal is that prediction is
   infeasible.
5. **Randomness as a key.** Two-bit messages and keys, $C=M\oplus K$.
   Four messages, four keys, four ciphertexts, so the whole probability
   model is exact.
6. **Perfect secrecy.** $P(M=m\mid C=c)=P(M=m)$. With a uniform key every
   message explains every ciphertext equally well, and Bayes' rule leaves
   the prior unchanged.
7. **Measuring leakage.** Entropy is uncertainty in bits. Mutual
   information $I(M;C)=H(M)-H(M\mid C)$ is how much the ciphertext reduces
   uncertainty about the message. Zero means perfect secrecy.
8. **The experiment.** Keep the cipher and the message distribution
   fixed, $P(M=00,01,10,11)=(0.4,0.3,0.2,0.1)$, and change only the key:
   $K_1,K_2$ independent $\mathrm{Bern}(q)$ for $q=0.50,0.55,\ldots,0.95$.
   This isolates bias while keeping the key bits independent; it is a
   different failure from the Markov example.

## Main result (exact)

For XOR with a key independent of the message, $H(C\mid M)=H(K)$, so
$I(M;C)=H(C)-H(K)$ with $P(C=c)=\sum_m P(M=m)\,P(K=m\oplus c)$.

| $q$  | $H(K)$ bits | $I(M;C)$ bits |
| ---- | ----------- | ------------- |
| 0.50 | 2.000       | 0.000         |
| 0.60 | 1.942       | 0.052         |
| 0.70 | 1.763       | 0.214         |
| 0.80 | 1.444       | 0.503         |
| 0.90 | 0.938       | 0.966         |
| 0.95 | 0.573       | 1.304         |

$H(M)\approx1.85$ bits. At $q=0.95$ the key $11$ occurs with probability
$0.9025$, and seeing $C=11$ moves the attacker's belief in $M=00$ from
$0.40$ to about $0.94$.

The curve is monotone for this cipher, this message distribution and this
family of biased independent keys. It is not a claim that key entropy
alone determines the security of every scheme.

## Planned validation

For each $q$: sample messages from $(0.4,0.3,0.2,0.1)$, sample independent
$\mathrm{Bern}(q)$ key bits, compute $C=M\oplus K$, repeat about 10,000
times, estimate $I(M;C)$ from the counts, and compare with the exact curve.
The plug-in estimate has a small upward bias, so it should sit slightly
above zero at $q=0.5$ and shrink with sample size.

## Course topics used

Bernoulli and Binomial distributions, independence, conditional
probability, Bayes' rule, the law of total probability, expected value,
and the Markov property with a transition matrix. Entropy and mutual
information come from Shannon.

## References

- J. K. Blitzstein and J. Hwang, *Introduction to Probability*, 2nd ed.
  The course textbook.
- C. E. Shannon, *A Mathematical Theory of Communication*, 1948. Entropy
  and mutual information.
- C. E. Shannon, *Communication Theory of Secrecy Systems*, 1949. Perfect
  secrecy.
- NIST SP 800-22 Rev. 1a, 2010. Statistical tests for random and
  pseudorandom generators; the frequency and runs tests come from here.
