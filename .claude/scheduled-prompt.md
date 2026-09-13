You are making one increment of progress on a LaTeX paper. You have NO memory of
previous runs. Orient yourself first, then do focused work, then commit and push.

Working directory: /home/toad/krea/stats/random_vs_secure
Repo: https://github.com/tdvnm/on_randomness (branch: main)

## 1. Orient
- Read README.md -- it is the authoritative project outline (11 parts).
- Read main.tex to see the section order.
- `git log --oneline -15` to see what recent runs did.
- `grep -l "STATUS: stub" sections/*.tex` to find unwritten sections.

## 2. Do the work
Each run does BOTH of these, in this order:

(a) ADD: Write the next section that is still a stub, in the order listed in
    main.tex. Write it properly -- derivations, not placeholders. Remove the
    "STATUS: stub" comment when a section is genuinely written. If all
    sections are written, instead deepen the weakest one.

(b) POLISH: Pick ONE already-written section and improve it -- tighten prose,
    fix notation inconsistency, use the semantic macros from preamble.tex
    (\Prob, \Ex, \Var, \Ent, \CEnt, \MI, \xor) instead of raw markup, add a
    cross-reference or citation where one is missing.

Keep the scope of a single run modest: one new section plus one polished
section. Do not attempt to write the whole paper in one run.

## 3. Verify
`latexmk -pdf -interaction=nonstopmode main.tex` MUST exit 0 before you commit.
If it fails, read the FIRST error in main.log and fix it. Never commit a
document that does not compile.

## 4. Commit and push
Commit with a message naming what was added and what was polished, then
`git push origin main`. If the push is rejected, `git pull --rebase` and retry
once. Do not force-push.

End your run with a two-line summary of what changed.
