# Does Random Mean Secure? — speaking guide

Use [`presentation.pdf`](presentation.pdf) for the talk. Slides 1–16 are the
presentation; slide 17 is references and backup. The timings target 18½ minutes,
leaving roughly 1½ minutes of margin in a 20-minute slot. Speak in your own
words; these are cues, not text to read aloud.

| Slide | Target | What to say |
| --- | --- | --- |
| 1. Title | 0:00–0:20 | “I’m studying what happens when an encryption key looks random but becomes easier to guess.” |
| 2. Problem | 0:20–1:20 | “We often focus on the encryption rule. Here I keep that rule fixed and vary the randomness used for the key. My question is how much more the ciphertext tells an attacker.” |
| 3. Route | 1:20–2:20 | Give the audience the map: test a bit source, use it as a key, measure leakage, then explain why statistical tests have limits. Say this is a **proposal** with one exact calculation already completed. |
| 4. Ideal source | 2:20–3:30 | “Independent fair bits” means each bit is 0 or 1 with equal probability and previous bits do not affect the next. Bias and dependence are different failures. |
| 5. Two tests | 3:30–5:30 | Explain the statistics in words before the formulas. Frequency counts ones. Runs counts groups of equal bits. Give a verbal example: `010101...` has balanced zeros and ones but switches too often. Under the ideal model, both counts have known binomial distributions. |
| 6. Test meaning | 5:30–6:30 | “A small p-value makes the ideal model doubtful. A large one means this test did not find a problem; it does not certify security.” The source comparison on the slide is planned work. |
| 7. Generator gap | 6:30–7:30 | “A deterministic generator can make output that looks convincing in a short sample. Its state still controls every future bit. If that state is recovered, the output is predictable.” Say **can**, not **will**, pass your tests. |
| 8. XOR cipher | 7:30–8:40 | Work through `01 XOR 10 = 11` slowly. Then show why XORing `11` with the same key returns `01`. The attacker knows the method but lacks the actual key. |
| 9. Perfect secrecy | 8:40–10:00 | “Perfect secrecy means the ciphertext never changes my belief about the message.” Explain the proof: for any proposed message and ciphertext, exactly one key connects them; a uniform key makes every such key equally likely. The independence and one-use assumptions matter. |
| 10. Leakage | 10:00–11:10 | “Entropy is uncertainty measured in bits. Conditional entropy is uncertainty after I see the ciphertext. Their difference is what I learned.” Zero is perfect secrecy. |
| 11. Setup | 11:10–12:30 | Point to the fixed and changed columns. The lopsided message distribution is fixed. At q = 0.5 keys are uniform; at q = 0.95 the key `11` is very likely. Only q changes. |
| 12. Exact calculation | 12:30–13:45 | “Because there are just four messages, I can calculate every ciphertext probability by adding four cases. This gives a shortcut: leakage equals ciphertext entropy minus key entropy.” Do not derive each logarithm on the slide. |
| 13. Graph | 13:45–15:50 | **Slow down here.** Blue is leaked information; red is remaining key uncertainty. At q = 0.5 blue is zero. At q = 0.95 red is about 0.57 bits and blue is about 1.30 of the message’s 1.85 bits. Say the graph is an **exact calculation**, not a measured simulation result. |
| 14. Simulation | 15:50–17:00 | Describe the sampling and count table. The purpose is to verify the exact curve and see finite-sample error. An estimate slightly above zero at q = 0.5 is expected; it should approach zero as sample size grows. |
| 15. Scope | 17:00–18:00 | “This two-bit model lets me calculate leakage precisely. The tests find particular statistical defects; they cannot prove that a real generator resists an attacker.” |
| 16. Takeaway | 18:00–18:30 | Repeat the question and answer in one sentence: “In this model, as the key becomes easier to guess, the ciphertext reveals more; passing output tests alone is not enough to call a key secure.” Stop. |

## Rehearsal priorities

1. Run through the deck once with a timer. If over 20 minutes, shorten slides 4–7; keep time for slide 13.
2. Practice explaining slides 5, 9, 10, and 13 without reading them. Those carry the graded mathematics.
3. Keep slide 17 hidden unless asked for sources. Have the longer `main.pdf` available for questions about Caesar cipher, human randomness, or the generator attack.

## Likely questions

**Why only two bits?** Every probability and leakage value can be calculated
exactly, so the simulation has a clear reference answer. This is a model for
the relationship, not a claim about the size of real encryption keys.

**Does a high p-value mean a secure generator?** No. It means a particular
statistic did not reveal a departure from the chosen null model on that sample.

**Why must the key be used once?** Reusing a one-time-pad key gives an attacker
relationships between messages even if the key was originally uniform.

**Why is the simulated leakage at q = 0.5 sometimes positive?** The plug-in
estimate from a finite table has sampling error and a small upward bias.

**Does lower key entropy always imply more leakage?** The rising curve is for
this fixed message distribution, XOR cipher, and family of biased key sources.
Entropy alone does not determine the leakage of every possible cipher.
