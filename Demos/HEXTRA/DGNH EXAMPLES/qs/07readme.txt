
***********************************************************************

                         QS and MPQS
      Quadratic Sieve and Multiple Polynomial Quadratic Sieve
      -------------------------------------------------------



Introduction
------------

It's all very well reading learned articles on QS and MPQS, but there
is no substitute for looking at code to see what happens. Both QS and
MPQS have been split into several programs here and these refer to
17 procedures written in assembly code which are briefly described in
appendix A. I hope the reader can easily follow this structure.

In fact QS consists of the programs FBASE, QS1, QS4 and GAUSS2, run
in that order, while MPQS consists of FBASE, QS1, QS3, QS2 and GAUSS,
also run in that order.

FBASE calculates the factor base; QS1 works out the quadratic
congruences; QS4 sieves on a single interval while QS2 sieves using
multiple polynomials provided by QS3; GAUSS2 and GAUSS solve the
equations and have identical code although different data.
The naming reflects the order in which the pieces were written.

The treatment mostly follows the description given in chapter 8 of the
book, Factorization and Primality Testing by David M. Bressoud, 
Springer-Verlag 1989.



Outline of QS and MPQS
----------------------

There are several restrictions on numbers to be factorised which are
discussed in the book. Briefly, they must not contain a repeated
factor, be at least 13 decimal digits long and be equal to 1 modulo 8,
ie leave a remainder of 1 when divided by 8. This last condition can
be met by multiplying the given number by a pre-multiplier, for
example if the number is equal to 3 modulo 8 it can be multiplied by 3
which makes the result have the right form.

This multiplication affects the values of the primes in the factor
base. If there are many small primes generated there, then the
sieving goes faster, so it is worth considering whether a larger
pre-multiplier should be used; instead of 3 one could have 11, 19, 27
and so on, all of which make the result equal to 1 modulo 8. The
UBASIC package UBIBM32.EXE/MPQSX selects 33 as a pre-multiplier in
the case of the example number, 499 94860 12441, perhaps because the
largest prime in the factor base is only 257 instead of our value of
397 using a pre-multiplier of 1.

For a large pre-multiplier one might have to increase the number of
computer words to represent each long number, slowing things down.

Other choices have to be made. The factor base size (currently 29),
the sieving interval (currently 10000 for QS and 1000 for MPQS) and 
the Silverman factor (currently 1.5); these are discussed in the book.



903 Algol
---------

The 903 Algol here differs from that in
ftp://ftp.cs.man.ac.uk/pub/CCS-Archive/simulators/Elliott903.
LIBRARY.DAT contains the material for the 17 procedures listed
in appendix A. The procedure, copy, in LIBRARY.DAT has the same name
as one contained for a different purpose in the previous LIBRARY.DAT,
therefore the latter procedure has been renamed here to alcopy (Algol
copy).

A program expects its input data to follow immediately after the final
"end" rather than be located in a separate DOS file, for simplicity.
This means that the output of one program must be placed in a DOS file
using the '>' operator, and edited onto the end of the next program.
This has been done in all example cases.

Long numbers must be split into groups of five decimal digits and
must end with a letter. For example the number to be factorised
appears on the data tape as

  499 94860 12441P

If the procedure which reads long numbers, get, has to read two such
numbers in succession they must end in different letters or else the
second one will be read in as zero.



The programs
------------

Each program, eg QS1.DAT, is in a file with extension name DAT, and
its output is in the corresponding file, eg QS1.LIS, with extension
name LIS. The progress of running the set of programs can be seen in
the *.LIS files.

They take a long time to run because a double interpretation is being
done - SIM900AL interprets each 903 hardware instruction which, in
turn, executes an interpreter for the 903 Algol. On a 100MHz 486 the
times in minutes are roughly as follows:

FBASE  15
QS1     4
QS2    25
QS3     8
QS4    37
GAUSS  22
GAUSS2 31

Thus QS takes 15+4+37+31 = 87 minutes, and MPQS takes 15+4+8+25+22 =
74 minutes. MPQS is supposed to be faster than QS.



                           APPENDIX A
             Procedures written in SIR assembly code
             ---------------------------------------

These are all procedures except 'and' and 'max int' which are integer
procedures. Procedure names can have spaces in them because Algol 
symbols are surrounded by quote signs, eg "begin", and there is
therefore no confusion, but you can think of max int being maxint if
you wish.

The procedures find out what length each array has and act
appropriately. The procedure multiply is an exception to this rule
and has an integer parameter giving the length.

Parameters are numbered 1, 2, 3 and so on. The kind of parameter is
indicated by a two-letter shorthand before the parameter number, thus
the procedure mod2 is described as

mod2   sa1 := da2 mod sa3;

meaning that single length array, sa1, is given the value of double
length array, da2, modulo single length array, sa3.

add         sa1 := sa2 + sa3;
and         has the value of the logical and of in1 and in2;
copy        sa1 := sa2;
div         sa1 := sa1 / in2;
divide      sa1 := sa3 "div" sa4;
            sa2 := sa3 "mod" sa4;
if          "if" sa1 = sa2 "then" "goto" la3;
            It would have been better to have made a boolean
            procedure giving the value "true", for example, when
            the arrays are equal. In fact just such a boolean
            procedure has been created in GAUSS.
lead blank  arrange for leading blanks when printing integers.
lead zero   arrange for leading zeros when printing integers.
max int     takes the value 131071.
mod         sa1 := sa2 mod sa3;
mod2        sa1 := da2 mod sa3;
mpy         sa1 := sa1 * in2; (see note below)
multiply    da1 := sa2 * sa3; it uses in4 words for sa2, sa3.
revert      arrange for printing integers to have the default property.
qdm         a procedure body called by both divide and mod which
            has to be loaded somehow, see QS2.DAT code and note below.
sub         sa1 := sa2 - sa3;
unsigned    arrange for no sign to precede a printed integer.



                           NOTE
                           ----

The library has some peculiarities, but none of them affects the ordinary
use of procedures when the library is already established. What follows
applies only to the case where a translation requires a library scan, ie
the initial jump is to 12 rather than to 8.

The library is arranged as a sequence of SIR blocks, each of which usually
contains one procedure, eg arctan, and arctan names the block because it is
the first global SIR label in the block. There are two kinds of exception to
this model - one where more than one procedure is contained in a SIR block
because the procedures have shared code, and one where two SIR blocks each
call another SIR block which has the shared code.

The first exception has four examples:

(1) cos and sin are in a block called qatrig;
(2) instring and outstring are in a block called qastri;
(3) mpy, div, add, sub and neg are in a block called mpy.
(4) multiply and mod2 are in a block called multip.

When running a translation with a library scan, the use of any of the
above mentioned procedures requires one to use, or to pretend to use,
the procedure name of the block. For example to use cos one must have
the declaration

"code" "procedure" qatrig; "algol";

among the declarations, and the statement

"if" "false" "then" qatrig;

among the statements. In the case of add, say, it is almost certain 
that mpy will also be used, so all will be well. If it is not used 
then a loader error message, "FU   ADD", is given and the program
runs correctly until the call to add is encountered. This causes a
message, "NO PROGRAM", to be given. There was no room in the run time
system to adopt the modern type of error message in absolute hex.

The second exception applies only to the procedures divide and mod
which call a block named qdm. The code in QS2.DAT shows the pretence
of calling qdm. 



                           APPENDIX B
          Purpose of the main variables in the programs
          ---------------------------------------------

a           A polynomial coeff. eg 6343.
b           A polynomial coeff. eg 4150.
c           Equals (a^(-1)) modulo nb, eg 419 08035 83186.
e[j]        Exponents.
ks          Pre-multiplier such that ks*nb = 1 modulo 8.
L           Lower bound for polynomial search, eg -30.
m           Number of primes in factor base, eg 29.
n           Number of words per single length long number, eg 3.
nb          The number to be factorised, eg 499 94860 12441
one         Holds the value 1 as a long number.
ps[j]       The factor base, eg 2, 3, ... , 397.
s           Total sieve size, eg 1000 for QS2 and 10000 for QS4.
sn          entier(sqrt(nb))
t           Silverman's factor, eg 1.5
u           Upper bound for polynomial search, eg 30.
ts[j]       Quadratic congruences where ts[j]^2 = nb modulo ps[j]
zero        Holds the number 0 as a long number.



DGNH [ed CPB]  18-NOV-2005 (divide description changed)

***********************************************************************

