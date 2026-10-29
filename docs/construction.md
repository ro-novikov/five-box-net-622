# Construction of the five-box nets

Labels: **[proved]** has a proof in the working notes of this project, **[computed]** was checked by computation on the
stated range, **[open]** is not settled.

## 1. Strip foldings and pillowcases

A *strip* (blank) here is a polyomino whose boundary is: end 1, bottom edge, end 2, top edge. A *folding* glues the
boundary to itself piece by piece, each piece either to a translated copy (a translate pair) or to itself by a half
turn (a reflection of the boundary word, a palindrome). With one translate pair and four palindromes the boundary
closes into a flat doubled rectangle, a *pillowcase* A × B (Conway's criterion; [1], [4]). Notches and tabs at its four
corners turn it into a box 2 × (A − 2) × (B − 2) of surface area 2AB − 8.

The four **strip foldings** glue end 1 to end 2 by the translation τ along the strip and split each long
edge into two palindromes in one of two ways. Two splits on one edge make the edge periodic; with q_b periods on the
bottom and q_t on the top, the four translation lattices are

    L(c) = ⟨τ, s + c·τ⟩,   c ∈ {0, c₁, c₂, c₁ + c₂},  c₁ = i/q_b,  c₂ = j/q_t  (i, j integers),

all of covolume 4AB. One edge takes at most two reflections (stated in [2]), and the corners fit only with odd q_b and
q_t (argued in [4], docs/construction.md section 5) **[proved]** in the notch-and-tab model. So one translate pair
gives at most four boxes.

## 2. The fifth folding

The fifth folding of these nets (and of the 2128 net [2]) has this form:

- it keeps one palindrome of a strip folding on the bottom edge and one on the top edge;
- the translate pair moves from the ends onto the edges: a bottom piece of length a just before the kept bottom split,
  glued to a top piece of length a just before the kept top split;
- the rest forms two new palindromes, end 1 with a piece of the bottom edge and end 2 with a piece of the top edge.

If ⟨τ, s⟩ is the lattice of the kept strip folding, the fifth folding has the lattice

    L₅ = ⟨τ + ρ, s⟩,   ρ = λ·s,

its translation is shifted by λ·s. The covolume is the same, so the fifth pillowcase has the same A·B.

That every further folding of a strip net has this form was shown under a shortness hypothesis on the new pieces
**[proved under that hypothesis]**. That a strip net has at most one such folding, and so at most five boxes, is **[open]**:
in every computation two of them never fitted together (exhaustive extensions of the four strip foldings by one and two
further foldings on several hundred strips; 12 480 six-folding candidates, all infeasible in the exact model).

**The rule for the shift [computed].** About 15 000 lattice designs (pillowcase areas A·B from 100 to 1500) were sorted by λ
and tested for corner fit (notch/tab labelling of all five foldings at once):

- λ = 1/3, 2/5, 3/7, 4/9, 5/11, i.e. λ = k/(2k + 1): every design passes;
- λ = 1/2 (2 496 designs, about 300 000 parameter tuples), 1/4, 1/6, 3/8, 1/10: none passes;
- some designs pass for other ratios (1/5, 2/7, 5/13).

Every five-box net found has λ = 1/3, and so has the 2128 net [2]. Four designs at areas 1588, 1768, 1858 and 1948
were chosen in advance by the λ = 1/3 rule; all four gave five-box nets.

## 3. The 622 nets

| | net A | net B | net C |
|---|---|---|---|
| strip direction | (7, −3) | (7, −3) | (7, −3) |
| τ (along the strip, in squares) | (210, −90) | (210, −90) | (210, −90) |
| s of the kept strip folding | (126, −60) | (126, −60) | (126, −60) |
| ρ = s/3 | (42, −20) | (42, −20) | (42, −20) |
| edge length L (unit steps), periods | 300; bottom 3, top 5 | 300; bottom 3, top 5 | 300; bottom 3, top 5 |
| bottom splits b₁, b₂ | 8, 108 | 8, 108 | 8, 108 |
| top splits t₁, t₂ | 176, 56 | 178, 58 | 176, 56 |
| end length e | 61 | 60 | 61 |
| moved translate pair a | 1 | 2 | 1 |

det(τ, s) = 1260 = 4 · 315, so every pillowcase has A·B = 315: 3×105, 7×45, 9×35, 15×21 (strip foldings) and 5×63 (fifth folding).
These are all the boxes of area 622 with a side 2 (622 has 19 boxes in all). Net C has the parameters of net A and
another boundary found by the same solver.

## 4. How the strip was computed

The direction (7, −3) has p + q even. A straight band in such a direction, resampled to the grid as in [1] and [4],
puts the corner points of the pillowcases on edge midpoints instead of grid vertices, so it has no box foldings.
The strip is therefore not cut from a band; its boundary is computed:

1. **Lattice design.** τ, s, the periods and λ are chosen so that the four strip-folding lattices and the lattice of the fifth folding are
   rectangular lattices of the right sizes.
2. **Boundary.** The boundary is a word of unit steps (bottom and end 1 monotone in one direction, top and end 2 in
   the other). Every gluing of every folding makes two pieces of the word equal (translate pair) or a piece equal to
   its own reverse (palindrome). A CP-SAT model looks for a word that satisfies all five foldings at once, keeps each
   folding's designed lattice, closes into a simple polygon, and maximises the thickness of the strip near the
   corners.
3. **Corners.** The exact integer program of [4] decides, square by square, which squares to remove (notches) and add
   (tabs), so that every folding covers its box exactly once:

       x[q_F(z)] − Σ_{y : F(y) = z} y = [z lies in a corner square of the pillowcase]

   for every folding F and every square z of its pillowcase (x = removed, y = added squares).
4. **Check.** Both checkers in `verify/`, against every box of the area.

Below 622, designs at 352, 496, 532 and 592 (several hundred parameter tuples) gave no boundary of the required
shape (**[computed]**, within this boundary model and these tuples); 622 gave the two nets.

## 5. What these foldings are not

In every folding of nets A, B and C the net is glued into a pillowcase whose corners are cut, with no square wrapping
around a box edge; the corner pieces of the 2128 net [2] wrap around box edges in every folding. Nets A, B and C also have
a common box side 2 in all five boxes; the boxes of [2] have a common side 4.

References are those of the [README](../README.md).
