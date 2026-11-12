HOW THE CUT LENGTH IS CALCULATED
================================

Notation: L = pipe length, v = speed of sound, c = pitch change in cents.

Goal
----
Given a pipe of length L (mm) and a wanted pitch increase of c cents, find how
much to cut off the open end.


Step 1: a shorter pipe sounds higher
------------------------------------
An open pipe resonates at

    f = v / (2L)

where v is the speed of sound. Since v does not change, the frequency and the
length are inversely proportional: f * L = constant. If the frequency is
multiplied by some factor, the length must be divided by the same factor.


Step 2: what a cent is
----------------------
Musicians divide an octave (a doubling of frequency) into 1200 equal steps
called cents. Each step multiplies the frequency by the same number, so 1200
steps together must give 2. One cent is therefore the 1200th root of 2:

    one cent:    f2 / f1 = 2^(1/1200)

Raising the pitch by c cents repeats that multiplication c times:

    c cents:     f2 / f1 = 2^(c/1200)


Step 3: new length
------------------
Because length varies inversely with frequency, the new length is the old
length divided by the frequency factor:

    L2 = L1 / 2^(c/1200) = L1 * 2^(-c/1200)


Step 4: length to cut
---------------------

    cut = L1 - L2 = L1 * (1 - 2^(-c/1200))


Worked example: L1 = 381 mm, c = 70 cents
-----------------------------------------
    2^(-70/1200) = 0.960373
    L2           = 381 * 0.960373 = 365.9 mm
    cut          = 381 - 365.9    = 15.1 mm


Sanity checks
-------------
  - c = 0 gives 2^0 = 1, so the cut is 0.
  - c = 100 (one semitone) gives 0.943874, a cut of 5.6126% of the length.
  - c = 1200 (one octave) gives 1/2, so the pipe is halved.
  - A negative c lowers the pitch, so the pipe must be lengthened instead.


One semitone as a percentage
----------------------------
A semitone is 100 cents, so c = 100:

    new length / old length = 2^(-100/1200) = 2^(-1/12)
                            = 1 / (12th root of 2)
                            = 0.9438743127
                            = 94.3874%

The pipe keeps 94.3874% of its length, so the cut is

    100% - 94.3874% = 5.6126% of the original length.

Seen from the frequency side, the same semitone multiplies the frequency by
the 12th root of 2 = 1.0594631, an increase of 5.9463%. The two percentages
differ because they are taken against different starting values (the old
frequency and the old length).

The 5.6126% figure is exact only for exactly one semitone. For any other
offset use the full formula, because successive steps compound.


Why the cut is not simply proportional to c
-------------------------------------------
Each cent multiplies the length by 0.99942254, and successive cents compound,
so the exponent carries c. A straight-line rule (c times a fixed percentage)
is only approximately right and drifts for larger offsets.


Assumptions and limits
----------------------
  - The tube is uniform and open at both ends, and the temperature and
    blowing conditions are the same before and after the cut.
  - This calculation uses the length only. The effective acoustic length is
    slightly longer than the physical length (the "end correction", about
    0.61 * the inner diameter in total). It adds a small offset to the cut,
    typically a few tenths of a millimetre. pipecut.py includes it when given
    the inner diameter; the simpler pipecut_cents.py leaves it out.
