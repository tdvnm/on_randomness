# Does Random Mean Secure? — speaking guide

Use [`presentation.pdf`](presentation.pdf) for the talk. Slides 1–15 are the
presentation; slides 16–20 are backup and references. The plan targets about
19 minutes in a 20-minute slot. Speak in your own words; these are cues, not
text to read aloud.

Every slide has two names. The small grey label above the title says where
you are (section · topic). The title says why the slide matters.

| Slide | Target | What to say |
| --- | --- | --- |
| 1. Title | 0:00–0:20 | “I want to ask one question: if the randomness behind an encryption key becomes easier to predict, how much more does the ciphertext reveal?” |
| 2. Can we actually make something random? | 0:20–1:30 | Ask the room to write 10–15 bits. Show the example. Ask the three questions. Do not go into psychology; the point is that “looks random” is a feeling, not a model. |
| 3. So what should a random sequence actually do? | 1:30–2:45 | Fairness and independence in words first, then the notation. Bias and dependence are different ways to fail, and a test built for one can miss the other. |
| 4. Half zeros and half ones is not enough | 2:45–4:00 | Both strings have eight ones. The frequency statistic is Binomial because it counts successes in independent Bernoulli trials. The test asks whether the count is unusual; it cannot see order. |
| 5. Two tests ask two different questions | 4:00–5:15 | Define a run with `00 | 111 | 00`. Runs count switches, and each neighbouring pair switches independently, so again Binomial. Frequency measures balance; runs measure pattern. |
| 6. What if randomness has memory? | 5:15–6:45 | The chain is here only as a clean model of dependence: the next bit depends only on the current bit. At s = 0.5 it is the ideal source; at s = 0.8 it flips too often. Balance stays 50/50; E[R] = 1 + (n−1)s moves. **Say explicitly that this is not the key model used later.** |
| 7. Looking random is not the same as being unpredictable | 6:45–8:45 | A p-value is a statement under the ideal model. Large p means this test found nothing. Then the two questions: statistical versus security. A generator is a rule on hidden state; recover the state and the future is fixed. Deterministic is not the problem; cryptographic generators are deterministic too, and their goal is that prediction is infeasible. |
| 8. What happens when randomness becomes a key? | 8:45–9:45 | Transition: “so far predictability was a property of a sequence; now the bits are a key.” Work through `01 XOR 10 = 11` and back. Two bits means four of everything, so every number later is exact. |
| 9. When does the ciphertext tell us nothing? | 9:45–11:30 | Read the big equation in words first. Uniform key: exactly one key links each (m, c), so the likelihood is 1/4 for every m and Bayes cancels it. Perfect secrecy. |
| 10. How do we measure what gets revealed? | 11:30–13:15 | Entropy is uncertainty in bits: certain 0, fair 1, biased 0.47. H(M|C) is what remains after the ciphertext. I(M;C) is the reduction; zero means nothing leaked. |
| 11. What if I change only the randomness? | 13:15–14:30 | Point at the two boxes. Fixed: message distribution, cipher, independence. Changed: only q. **This is bias, not dependence; the Markov source is a different failure.** |
| 12. As the key becomes predictable, secrecy begins to disappear | 14:30–16:00 | At q = 0.95 the key `11` shows up 90% of the time. Then the concrete case: before the ciphertext the attacker gives “00” 40%; after seeing `11`, 94%. That change of belief is the leakage. |
| 13. The weaker the key, the more the ciphertext reveals | 16:00–18:00 | **Slow down here.** Purple line is leakage; the falling line is key entropy; dashed is H(M). At q = 0.5 leakage is zero. At q = 0.95 the key has 0.57 bits left and 1.30 of the 1.85 message bits leak. This is exact, not simulated. It is a result for this cipher, message distribution and key family; do not claim more. |
| 14. Checking the curve by simulation | 18:00–18:40 | Planned, not done. Sample, encrypt, count, estimate, compare. Say the plug-in estimate sits slightly above zero at q = 0.5 and shrinks with sample size. |
| 15. Does random mean secure? | 18:40–19:20 | Three levels: looks random, hard to predict, protects information. Three sentences. Then the closing line and stop. |

## Rehearsal priorities

1. Run through with a timer. If over 20 minutes, cut words on slides 3, 7
   and 11; keep the time on slides 9, 12 and 13.
2. Practice slides 6, 9, 10 and 13 without reading them. They carry the
   mathematics that will be graded.
3. Keep slides 16–20 hidden unless asked. Backups: exact meaning of a
   p-value, Markov details, the full q = 0.95 posterior table, a small
   LCG, references.

## Likely questions

**Why only two bits?** Every probability and leakage value is exact, so a
simulation has a reference answer. It is a model of the relationship, not a
claim about real key sizes.

**Does a high p-value mean a secure generator?** No. One statistic did not
find a departure from the null model on that sample.

**Where is the Markov chain used?** Only on slide 6, as a model of a source
with a switching habit. The leakage experiment uses independent biased bits.

**Why must the key be used once?** Reusing a one-time-pad key gives an
attacker relationships between messages even if the key was uniform.

**Why is a simulated leakage at q = 0.5 positive?** The plug-in estimate from
a finite table has sampling error and a small upward bias.

**Does lower key entropy always mean more leakage?** The rising curve is for
this fixed message distribution, XOR cipher and family of biased independent
keys. Entropy alone does not determine the leakage of every cipher.
