# Verification

## Standalone script (Python 3, standard library only)

```
python3 verify/verify_folds.py data/net-622-a-cells.txt            # every box of area 622
python3 verify/verify_folds.py data/net-622-a-cells.txt 2x3x61     # only the listed boxes
```

It checks that the net is one simple polyomino (connected, Euler characteristic 1, no corner-only contacts) and, for
each box, places the first square on every surface square in all 8 orientations and rolls the net over the box; a box
folds when every square lands on a different surface square and every pair of joined squares lands on squares that
meet along the same box edge. Expected output: `expected-output-a.txt`, `expected-output-b.txt`, `expected-output-c.txt`.

## The hidny checker

`ValidNetSolutionChecker` from [hidny/CuboidSimplePhaseNetSearch](https://github.com/hidny/CuboidSimplePhaseNetSearch),
called for each box through `CheckNet.java` (from Silverstein's repository, MIT). For all three nets it reports the same five
boxes among the 19 boxes of area 622 (1x1x155 takes about 15 s, the others a few seconds). Tested with Java 21; run
from the repository root:

```
git clone https://github.com/hidny/CuboidSimplePhaseNetSearch
javac -d classes -sourcepath CuboidSimplePhaseNetSearch/src verify/CheckNet.java
java -Xss512m -cp classes CheckNet data/net-622-a-cells.txt 1,1,155 1,2,103 1,3,77 1,5,51 1,7,38 1,11,25 1,12,23 2,3,61 2,5,43 2,7,33 2,13,19 3,5,37 3,7,29 3,13,17 5,7,23 5,9,19 5,11,16 7,8,17 7,11,13
```

The checker prints a long diagnostic log; the result lines have the form `2x3x61 : FOLDS` or `3x5x37 : does not fold`
(filter with `| grep " : "`).
