# Chapter 1 — Number Systems, Codes, and Boolean Algebra

## Learning goals
Convert between common bases, reason about signed representations and overflow, simplify Boolean expressions, and distinguish numeric representation from logic encoding.

## 1. Positional number systems
In base b, digits range from 0 through b-1. The value of digits dₖ...d₀ is Σdᵢbⁱ. Binary uses base 2, octal base 8, decimal base 10, hexadecimal base 16.

Example: (101101)₂=32+8+4+1=45. To convert 45 to binary, repeatedly divide by 2 and read remainders upward: 101101. Hex groups binary in fours: 0010 1101 = 2D₁₆.

For fractions, multiply the fraction repeatedly by the target base and read integer parts in order. Decimal 0.625 becomes .101₂ because .625×2=1.25, .25×2=.5, .5×2=1.

## 2. Signed integers
For n-bit two's complement, range is -2^(n-1) through 2^(n-1)-1. To negate, invert bits and add 1. Example in 8 bits: +13=00001101; invert 11110010; add one →11110011, which represents -13.

Addition uses ordinary binary addition; discard carry out. Overflow occurs when same-sign operands produce a result with the opposite sign. Carry out alone does not identify signed overflow.

Sign magnitude and one's complement have two zeros; two's complement has one zero and makes addition/subtraction efficient.

## 3. Boolean algebra
Boolean variables take values 0/1. Basic identities:
- Identity: x+0=x, x·1=x.
- Domination: x+1=1, x·0=0.
- Idempotent: x+x=x, xx=x.
- Complement: x+x'=1, xx'=0.
- De Morgan: (x+y)'=x'y'; (xy)'=x'+y'.

Here + means OR, multiplication means AND, prime means NOT. Absorption x+xy=x and x(x+y)=x often shortens expressions.

Example: F=AB+AB' = A(B+B')=A.

## 4. Gates and universal gates
AND, OR, NOT, NAND, NOR, XOR, XNOR are standard gates. XOR is 1 when inputs differ; it is not ordinary OR. NAND and NOR are functionally complete: using only NAND, build NOT by tying inputs together, AND by NAND followed by inversion, and OR through De Morgan.

## 5. Questions and practice
**Q1.** Convert 110101₂ to decimal. **A:** 53.
**Q2.** Represent -5 in 8-bit two's complement. **A:** 11111011.
**Q3.** When does signed addition overflow? **A:** Same-sign inputs yield opposite-sign result.
**Q4.** Simplify X+XY. **A:** X.
**Q5.** What input makes XOR equal 1? **A:** Unequal inputs.
Practice: convert 2F₁₆ to binary/decimal; subtract using two's complement; prove one Boolean identity with a truth table; build OR from NAND gates.

## Notebook work
Draw a place-value expansion, a two's-complement number line, and a truth table for XOR and De Morgan's law.


## Signed arithmetic and floating-point worked examples

Eight-bit two's-complement range is -128..127. Add 70+80: binary result has sign bit 1, although both operands were positive, so signed overflow occurred. Add -70+-80 similarly overflows below -128. Add 100+(-50)=50 and no overflow occurs.

For fixed point with 4 fractional bits, stored integer 28 represents 28/16=1.75. Resolution is 1/16; range depends on total signed bits. Multiplication needs enough product bits before rescaling.

Floating-point example: 13.25₁₀ = 1101.01₂ = 1.10101₂×2³. In IEEE single precision, sign=0, biased exponent=127+3=130 (10000010₂), fraction stores 10101 followed by zeros to 23 bits. This is exact because the binary fraction terminates.

Rounding examples matter: decimal 0.1 repeats in binary, so stored value is approximate. Comparing floating values for equality can fail after arithmetic; use an error tolerance chosen from scale and numerical method.

**Practice:** Convert -18 to 8-bit two's complement: 11101110. Identify overflow for 120+20 in 8 bits: yes. Explain why carry-out and signed overflow differ.

## Extended lesson: signed arithmetic and code conversion

In n-bit two's complement, values range from -2^(n-1) to 2^(n-1)-1. To negate x: invert all bits and add 1. Thus in 8 bits, +13=00001101; invert gives 11110010; add 1 gives 11110011, which represents -13.

**Overflow is not the carry flag.** For signed addition, overflow occurs when equal-sign operands produce a result with the opposite sign. In 4 bits:
~~~text
  0111  (+7)
+ 0001  (+1)
= 1000  (-8 if interpreted signed)
~~~
The mathematical result +8 is outside the 4-bit signed range, so signed overflow occurred. For unsigned arithmetic, the carry-out is meaningful instead. Adding operands of different signs cannot overflow in two's complement.

Subtraction uses A-B = A + (two's-complement of B) = A + ~B + 1. This allows one adder to do both addition and subtraction by XORing B with a SUB control and using SUB as carry-in.

### BCD and Gray code

8421 BCD stores each decimal digit separately in four bits. After adding two BCD digits, if the 4-bit result exceeds 1001 or has carry-out, add 0110 to correct it. Example: 8+7=15; binary 1000+0111=1111, which is invalid as a BCD digit. Add 0110: 1 0101, producing carry 1 and digit 0101, i.e. decimal 15.

Gray code changes one bit between adjacent values, reducing transition ambiguity in sensors. Binary-to-Gray: keep the most significant bit; each next Gray bit is XOR of adjacent binary bits. For binary 1011, Gray=1110. To decode, copy the first Gray bit and cumulatively XOR it with following Gray bits.

### Boolean laws in circuit design

De Morgan: (AB)'=A'+B' and (A+B)'=A'B'. The consensus theorem can remove a redundant term: AB + A'C + BC = AB + A'C. These identities preserve the truth table while changing gate count, fan-in, or timing.

**Check:** In 5-bit two's complement, what is the range? -16 to +15. Does 0100+1100 overflow? No signed overflow: +4 plus -4 is 0; carry-out alone does not signal signed overflow.
