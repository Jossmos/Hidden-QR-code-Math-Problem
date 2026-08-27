# The Hidden Matrix / The Hidden QR Code

A hand-solvable puzzle in linear algebra over **GF(2)** (the finite field with two elements) that hides a real, scannable **QR code** behind four phases of computation: a convergent series, a linear-feedback shift register, an XOR-masking step, and Gauss-Jordan elimination modulo 2.

No calculator shortcuts, no computer required — every step is designed to be worked out by hand with pen and paper (though nothing stops you from scripting it).

## Why would you do this to someone you love

Because the best gifts take effort — theirs, not just yours. This is what happens when "I made you something" meets "I made you suffer, a little, on purpose."

Feed someone a convergent series, a shift register, and a few thousand XOR operations, and hours (realistically, days) later they're rewarded with... a QR code. We're talking roughly **22–40 hours of hand computation** — around 1,682 shift-register steps and over 24,000 individual XOR operations in the final elimination alone. Pointing at whatever you want: a wedding website, a proposal video, a heartfelt message, or — let's be honest, this is also extremely funny — a picture of a gorilla doing something rude. The math doesn't care what's on the other end. That's the beauty of it.

Good excuses to inflict this on someone:
- **A wedding gift or proposal reveal** with a bit more ceremony than just handing over a link.
- **A running rivalry** between friends: first one to correctly reconstruct `X` wins bragging rights (and finally gets to see the payoff).
- **Revenge** for the birthday card they got you that played a kazoo sound on loop.
- Genuinely just because someone you know **really likes linear algebra** and deserves a proper workout.

Whatever the reason, they will not see the ending coming.

## What's inside

| File | Description |
|---|---|
| `The_Hidden_Matrix_Exercise.tex` / `.pdf` | The puzzle only — no solution, no hints beyond the method itself. This is what you hand to the person solving it. |
| `The_Hidden_QR_Code_Solution.tex` / `.pdf` | The exact same puzzle, with every intermediate result and the final solution worked out in full, ending in a rendered, verified-scannable QR code. |

Both `.tex` files are **fully self-contained** — every grid and figure is inlined directly in the source, so there are no external dependencies or missing includes. Clone the repo, compile, done.

## How it works

The puzzle unfolds in four phases:

1. **The key number** — solve a closed-form equation (a convergent alternating series evaluated via Abel's theorem) to obtain an integer seed `S`.
2. **The LFSR** — feed `S` into a 29-bit linear-feedback shift register (tap polynomial `x^29 + x^2 + 1`, primitive over GF(2)) and generate, bit by bit, two 29×29 matrices: `A` and a mask matrix `M`.
3. **Unmasking** — you're given an XOR-encrypted matrix `B_enc`. Using your self-generated `M`, recover the real matrix `B = B_enc ⊕ M`.
4. **Gauss-Jordan elimination mod 2** — solve `A·X ≡ B (mod 2)` by row-reducing the augmented matrix `[A | B]` to `[I | X]`. The resulting `X`, rendered as black-and-white cells with a white quiet-zone border, is a real QR code.

Every step includes a fully worked mini-example (a 4-bit LFSR, a 3×3 linear system) so the method is clear before tackling the full 29×29 version, plus self-check values (expected number of 1s in each matrix) to catch transcription errors early.

## Requirements

Any standard TeX distribution (TeX Live, MiKTeX, MacTeX) with the following packages, all of which ship in the default/full install:

```
amsmath, amssymb, xcolor, mdframed, enumitem, tikz,
pdflscape, hyperref, titlesec, fancyhdr
```

## Building

```bash
pdflatex The_Hidden_Matrix_Exercise.tex
pdflatex The_Hidden_Matrix_Exercise.tex   # run twice for the table of contents
```

Same for `The_Hidden_QR_Code_Solution.tex`. Both compile cleanly with no warnings.

## Solving it

Print the exercise PDF (13 pages) and work through it phase by phase. Here's what you're actually signing someone up for:

| Phase | What it involves | Operations | Estimated time |
|---|---|---|---|
| 1. Key number | Derive and evaluate a convergent series | — | 1–3 hours |
| 2. LFSR → `A`, `M` | Run the shift register by hand, bit by bit | 1,682 steps | 10–19 hours |
| 3. Unmasking | XOR `B_enc` with `M`, cell by cell | 841 XORs | 2–3 hours |
| 4. Elimination | Gauss-Jordan on the 29×58 augmented matrix | 415 row-XORs, ~24,000 individual cell flips | 9–15 hours |
| **Total** | | | **≈ 22–40 hours** |

At a couple of focused hours per evening, that's realistically **2–3 weeks**. Phase 2 is the real test of patience — and the most dangerous: a single flipped bit 50 steps in silently corrupts everything after it, and you won't find out until the self-check count comes up wrong, possibly hundreds of steps later. The self-check boxes exist specifically to catch this before it wastes an entire evening's work.

Once you have the final matrix `X`, color the 1s black and the 0s white, add a white border of at least 4 cells around the grid, and scan it with any phone camera.

## Making your own version

The QR code encoded in this repo's puzzle points to a specific (joke) URL baked into the matrices at generation time. To build your own version with a different destination URL:

1. Generate a QR code matrix for your target URL/text (error-correction level `Q` recommended — keeps the matrix at a manageable 29×29 for a reasonably short URL, and the ~25% redundancy forgives small hand-transcription mistakes).
2. Pick a seed, run it through the same LFSR (or your own tap polynomial) to get matrices `A` and `M`.
3. Compute `B = A·X mod 2` and `B_enc = B ⊕ M`.
4. Drop the new `A`, `B_enc` grids (and update the self-check counts) into the `.tex` template.

The generation was originally done in Python (`qrcode`, `numpy`) — open an issue or PR if you'd like the generation script included in this repo as well.

## Why this works (the short version)

Arithmetic modulo 2 (where `1+1=0`) turns addition into XOR. Matrices of 0s and 1s under this rule live in the finite field `GF(2)`, where every ordinary rule of linear algebra — invertibility, Gaussian elimination, matrix multiplication — still holds. This is the same mathematics underlying QR code error correction, CRC checksums, and stream ciphers, which is why an LFSR, an XOR mask, and a QR code all fit naturally into one puzzle.

## License

MIT — do whatever you like with this, attribution appreciated but not required.
