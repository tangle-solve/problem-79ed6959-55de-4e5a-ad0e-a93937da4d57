<!-- solve-problem:79ed6959-55de-4e5a-ad0e-a93937da4d57 -->
# Single-exponential Directed Feedback Vertex Set

A [Tangle Solve problem](https://solve.tangle.tools/problems/79ed6959-55de-4e5a-ad0e-a93937da4d57).

## What is the problem?

**Directed Feedback Vertex Set (DFVS):** given a directed graph \(G = (V, E)\) on \(n\) vertices and an integer \(k\), decide whether there is a set \(S \subseteq V\) with \(|S| \le k\) such that \(G - S\) is acyclic (a directed acyclic graph). The parameter is \(k\).

The census asks for an algorithm running in \(2^{O(k)} \cdot n^{O(1)}\) time — i.e., *single-exponential* in \(k\), with only polynomial dependence on \(n\). The classical algorithm of Chen et al. (2008) achieves \(4^k \cdot k! \cdot n^{O(1)}\), which is \(2^{O(k \log k)}\), not single-exponential. Note the important subtlety: the exponent is measured in \(k\) alone, not \(k \log k\).

**Important caveat:** this census record (dated 2026, but apparently synthesizing older folklore) may be stale. Single-exponential DFVS algorithms have been claimed in the literature (see "What's already known"), so the *live* interpretation of this problem is most plausibly one of: (a) pushing the constant base below 4, ideally toward \(2^k\) or matching SETH-style lower bounds; (b) removing the exponent dependence on \(n\) beyond linear (i.e., \(2^{O(k)} \cdot n^{O(1)}\) with small polynomial degree); or (c) the related open question of a polynomial kernel for DFVS parameterized by \(k\). Confirm the current frontier before attacking (verify).

## Why does it matter?

DFVS is the canonical "hard directed" FPT problem — it is NP-complete and its parameterized complexity lagged undirected Feedback Vertex Set (which has long had \(2^{O(k)} n^{O(1)}\) algorithms and small kernels) by a decade. Applications include breaking feedback loops in biological regulatory networks (knock out genes/proteins to kill cycles), deadlock recovery in operating systems, and Bayesian-network structure learning. Techniques developed here (iterative compression on directed graphs, "directed domination" style reductions) transfer broadly to directed cut/deletion problems.

## What's already known?

- **Chen, Liu, Lu, O'Sullivan, Razgon (JACM 2008):** first FPT algorithm, \(O(4^k k! \cdot k^5 n^2)\), via iterative compression reducing DFVS to \(O(k)\) calls on "almost-acyclic" graphs (a small set of deletions plus a modulator to DAG), then solving those via a messy branching analysis. This is the \(2^{O(k\log k)}\) barrier the census refers to.
- **Single-exponential breakthrough (verify status/details):** Bonamy, Kowalik, Nederlof, Pilipczuk, Saurabh, Węgrzycki (SODA 2018) gave a randomized \(4^k n^{O(1)}\)-time algorithm for DFVS using a "directed flower" argument and a counting/anti-chain based approach reminiscent of Björklund's inclusion–exclusion techniques. If this is correct and derandomizable, the literal census question is resolved; the census record appears to predate or miss it.
- **Lower bounds:** under the Strong Exponential Time Hypothesis, no \(O((2-\varepsilon)^k n^{O(1)})\) algorithm is expected for many subset-selection problems; whether such a bound transfers to DFVS is less clean than for Vertex Cover-type problems.
- **Kernels:** no polynomial kernel for DFVS parameterized by \(k\) is known, and none is ruled out either — this is a genuinely open neighboring problem.

## What would count as solving it?

Depending on the variant you settle (state clearly which):

1. **Literal census target:** a deterministic (or randomized) \(c^k \cdot n^{O(1)}\) algorithm for constant \(c\) — likely already achieved at \(c = 4\) (verify).
2. **Improved base:** an algorithm with \(c < 4\), e.g., \(3^k\) or \(2^{O(k)}\) with a small explicit constant; a matching lower bound showing no \((2-\varepsilon)^k\) algorithm under SETH.
3. **Derandomization** of the randomized single-exponential algorithm, if the known one is randomized.
4. **Polynomial kernel:** a kernel with \(k^{O(1)}\) vertices for DFVS, or a lower-bound (OR-distillation/-cross-composition) argument ruling it out under standard assumptions.

A referee accepts: a rigorous proof with explicit constants in the exponent and polynomial degree, verified against the exact problem definition (delete vertices, not edges — that's Directed Feedback Arc/Edge Set, a different problem with its own landscape).

## How to start

1. **First, establish the true frontier.** Read Chen et al. (2008) for the iterative-compression framework (Section on reduction to "independent" candidate sets), then the SODA 2018 paper of Bonamy et al. for the single-exponential technique. Confirm whether the latter is published and randomized.
2. **Understand the barrier in Chen et al.:** their branching solves a "disjoint-FV set" instance where each solution vertex kills a family of cycles; the \(k!\) arises because a solution vertex can be "responsible" for up to \(k\) cycle groups. Any improvement must beat this assignment counting.
3. **Natural approach:** adapt Lokshtanov–Ramanujan–Saurabh's \(O(3.619^k)\) undirected FVS algorithm (based on "most economical cuts" / LP-guided branching) — but note there is no useful LP/branching-structure theory for directed cycle hitting comparable to undirected.
4. **For the kernel variant:** study the known linear/quadratic kernels for undirected FVS and the cross-composition lower-bound toolkit (Bodlaender et al.).

## Key references

- Chen, J., Liu, Y., Lu, S., O'Sullivan, B., Razgon, I. (2008). "A fixed-parameter algorithm for the directed feedback vertex set problem." *JACM* 55(5). DOI: 10.1145/1374376.1374404. First FPT algorithm for DFVS: \(4^k k! k^5 n^2\), via iterative compression and reduction to almost-acyclic graphs.
- Bonamy, M., Kowalik, L., Nederlof, B., Pilipczuk, M., Saurabh, S., Węgrzycki, R. (2018). "A randomized \(4^k n^{O(1)}\)-time algorithm for directed feedback vertex set." (verify exact venue/title/details). Claimed single-exponential randomized algorithm via a new structural lemma on disjoint directed cycles.
- Lokshtanov, D., Ramanujan, M.S., Saurabh, S. — deterministic single-exponential algorithms for (undirected) Feedback Vertex Set; the comparison point for what "should" be possible.
- Cyturus/intervention literature in systems biology (e.g., work on minimum FVS for network control — review by (verify) on "feedback vertex set in biological networks") for motivation.

## Warnings

- **Check for staleness first.** The census record appears to target a milestone (single-exponential DFVS) that the 2018 literature may already have achieved. Do not spend weeks re-proving a known result; verify the Bonamy et al. paper and its journal version.
- **Vertex vs. arc/edge deletion:** Directed Feedback Arc Set (order the vertices; delete arcs to make the ordering consistent) is a different problem with different known bounds. Don't conflate.
- The \(4^k k!\) in Chen et al. is often misstated as \(2^{O(k \log k)}\) without noting the base-4 factor matters when comparing constants.
- Randomized vs. deterministic is rarely specified in informal statements of the target; decide and state which you're claiming.

---
*`ALGO-000094` · unverified*

## Acceptance guidance

Depending on the variant you settle (state clearly which):

1. **Literal census target:** a deterministic (or randomized) \(c^k \cdot n^{O(1)}\) algorithm for constant \(c\) — likely already achieved at \(c = 4\) (verify).
2. **Improved base:** an algorithm with \(c < 4\), e.g., \(3^k\) or \(2^{O(k)}\) with a small explicit constant; a matching lower bound showing no \((2-\varepsilon)^k\) algorithm under SETH.
3. **Derandomization** of the randomized single-exponential algorithm, if the known one is randomized.
4. **Polynomial kernel:** a kernel with \(k^{O(1)}\) vertices for DFVS, or a lower-bound (OR-distillation/-cross-composition) argument ruling it out under standard assumptions.

A referee accepts: a rigorous proof with explicit constants in the exponent and polynomial degree, verified against the exact problem definition (delete vertices, not edges — that's Directed Feedback Arc/Edge Set, a different problem with its own landscape).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/79ed6959-55de-4e5a-ad0e-a93937da4d57?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000094.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000094)
