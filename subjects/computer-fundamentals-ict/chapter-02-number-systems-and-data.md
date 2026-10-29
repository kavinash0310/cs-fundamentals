# Chapter 2 — Number Systems, Arithmetic, and Data Representation

**Course:** Computer Fundamentals and ICT  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Chapter 1

## What you will learn

You will convert numbers among bases 2, 8, 10, and 16; perform binary arithmetic; distinguish unsigned and signed representation; understand fixed-point and floating-point ideas; and explain common character and media representations.

---

## 2.1 Positional number systems

In a positional base-b system, each digit is multiplied by a power of b according to its place.

~~~text
(d3 d2 d1 d0)base-b
= d3 × b^3 + d2 × b^2 + d1 × b^1 + d0 × b^0
~~~

| System | Base | Digits | Common use |
|---|---:|---|---|
| Binary | 2 | 0, 1 | Hardware-level representation |
| Octal | 8 | 0–7 | Compact grouping of binary |
| Decimal | 10 | 0–9 | Everyday counting |
| Hexadecimal | 16 | 0–9, A–F | Compact representation of binary |

For hexadecimal, A=10, B=11, C=12, D=13, E=14, and F=15.

### Binary to decimal

~~~text
101101.101₂
= 1×2^5 + 0×2^4 + 1×2^3 + 1×2^2 + 0×2^1 + 1×2^0
  + 1×2^-1 + 0×2^-2 + 1×2^-3
= 32 + 8 + 4 + 1 + 0.5 + 0.125
= 45.625₁₀
~~~

The point separates positive powers from negative powers.

---

## 2.2 Decimal to binary

### Integer part: repeated division

Divide by two, record each remainder, then read the remainders from last to first.

**Convert 45₁₀:**

~~~text
45 ÷ 2 = 22 remainder 1
22 ÷ 2 = 11 remainder 0
11 ÷ 2 =  5 remainder 1
 5 ÷ 2 =  2 remainder 1
 2 ÷ 2 =  1 remainder 0
 1 ÷ 2 =  0 remainder 1
Read upward: 101101₂
~~~

Check: 32 + 8 + 4 + 1 = 45.

### Fractional part: repeated multiplication

Multiply the fraction by two. Record each integer part in order until the fraction is zero or the required precision is reached.

**Convert 0.625₁₀:**

~~~text
0.625 × 2 = 1.25   bit 1; keep .25
0.25  × 2 = 0.50   bit 0; keep .50
0.50  × 2 = 1.00   bit 1; stop
Therefore 0.625₁₀ = 0.101₂
~~~

A decimal fraction may repeat indefinitely in binary. State the precision or rounding when it does.

---

## 2.3 Octal and hexadecimal shortcuts

Binary aligns naturally with octal and hexadecimal:

- 3 binary bits correspond to one octal digit because 2³ = 8.
- 4 binary bits correspond to one hexadecimal digit because 2⁴ = 16.

Group from the binary point outward. Pad with zeros at the outside of the integer or fraction group; do not change the value.

~~~text
Binary 101101₂
Octal groups: 101 101 = 5 5 = 55₈
Hex groups:   0010 1101 = 2 D = 2D₁₆

Binary 1101.101₂
Octal groups: 001 101 . 101 = 15.5₈
Hex groups:   1101 . 1010 = D.A₁₆
~~~

### Cross-check conversion

~~~text
45₁₀ = 101101₂ = 55₈ = 2D₁₆
55₈ = 5×8 + 5 = 45
2D₁₆ = 2×16 + 13 = 45
~~~

---

## 2.4 Binary arithmetic

### Addition rules

~~~text
0 + 0 = 0
0 + 1 = 1
1 + 0 = 1
1 + 1 = 10  (write 0 and carry 1)
1 + 1 + 1 = 11 (write 1 and carry 1)
~~~

**Worked addition:**

~~~text
  101101₂
+ 011011₂
---------
 1001000₂
~~~

Check in decimal: 45 + 27 = 72; 1001000₂ = 64 + 8 = 72.

### Subtraction and borrow

~~~text
  10110₂  (22)
- 01101₂  (13)
--------
  01001₂  (9)
~~~

When subtracting 1 from 0, borrow from the nearest higher 1. In binary, a borrowed 1 represents 10₂ in the current column.

### Multiplication

Binary multiplication uses the same shift-and-add idea as decimal multiplication:

~~~text
      1011₂   (11)
   ×   101₂   (5)
   --------
      1011
     0000
+  101100
   --------
    110111₂  (55)
~~~

A left shift by one position multiplies an unsigned binary integer by 2 if overflow is not considered.

---

## 2.5 Unsigned integers

An n-bit unsigned number represents values from 0 to 2ⁿ − 1.

| Width | Minimum | Maximum |
|---:|---:|---:|
| 4 bits | 0 | 15 |
| 8 bits | 0 | 255 |
| 16 bits | 0 | 65,535 |

**Example:** 8-bit 11111111₂ = 255₁₀. Adding 1 in fixed 8-bit unsigned arithmetic wraps to 00000000₂; the mathematical value 256 is outside the representable range.

---

## 2.6 Signed integer representations

Historically, computers have used several encodings for signed values. The most common general-purpose representation is two's complement.

### Sign-magnitude

The leading bit indicates sign; remaining bits represent magnitude. It has positive and negative zero, which complicates arithmetic.

### One's complement

A negative number is formed by inverting every bit of the positive value. It also has two zero encodings.

### Two's complement

To encode a negative number in a fixed width:

1. Write the positive value in n bits.
2. Invert each bit.
3. Add 1.
4. Keep n bits.

**Example: −13 in 8 bits**

~~~text
+13        = 00001101
Invert     = 11110010
Add 1      = 11110011
Thus −13   = 11110011₂ (8-bit two's complement)
~~~

For n-bit two's complement, the range is −2ⁿ⁻¹ through 2ⁿ⁻¹ − 1.

For 8 bits: −128 through +127.

### Interpreting a two's-complement pattern

If the most significant bit is 0, interpret it as a non-negative binary value. If it is 1, invert, add one, and apply a negative sign.

~~~text
11110110₂
Invert: 00001001
Add 1:  00001010 = 10
Value: −10
~~~

### Signed overflow

In fixed-width signed addition, overflow occurs when the mathematical result is outside the representable range. A quick sign test: adding two positive values and getting a negative result, or two negative values and getting a positive result, indicates overflow.

**Example in 8-bit two's complement:**

~~~text
  01111111   (+127)
+ 00000001   (+1)
----------
  10000000   (interpreted as −128)
~~~

The bit addition is valid modulo 2⁸, but +128 cannot be represented as an 8-bit signed value.

---

## 2.7 Fixed-point representation

A fixed-point format reserves a fixed number of bits for the fractional part. If there are k fractional bits, the stored integer is scaled by 2ᵏ.

**Example:** In a format with 4 fractional bits, stored integer 53 represents:

~~~text
53 / 2^4 = 53/16 = 3.3125
~~~

Fixed-point arithmetic can be fast and predictable, but its range and precision are limited. Scaling must be handled consistently, especially for multiplication and division.

---

## 2.8 Floating-point representation

Floating point represents a value approximately as:

~~~text
(-1)^sign × significand × base^exponent
~~~

For binary normalized form, a nonzero value is commonly written as 1.fraction × 2^exponent.

IEEE 754 single precision uses 32 bits:

| Field | Bits | Meaning |
|---|---:|---|
| Sign | 1 | Positive or negative |
| Exponent | 8 | Biased exponent; bias is 127 |
| Fraction | 23 | Fraction bits after the implicit leading 1 for normal values |

### Worked representation: 13.25

~~~text
13.25₁₀ = 1101.01₂
         = 1.10101₂ × 2^3

Sign = 0
Stored exponent = 3 + 127 = 130 = 10000010₂
Fraction field = 10101000000000000000000
Pattern = 0 | 10000010 | 10101000000000000000000
~~~

This normalised finite value is exactly representable because 0.25 is a power-of-two fraction.

Special encodings include zero, subnormal values, infinity, and NaN (not a number). Details depend on the format and operation.

### Precision and rounding

Many decimal fractions cannot be represented exactly in a finite binary fraction. For example, 0.1₁₀ repeats in binary. Floating-point arithmetic therefore rounds.

~~~python
# Illustrative: a typical binary floating-point result is not exact.
print(0.1 + 0.2)
~~~

For money or exact decimal accounting, use an appropriate decimal or integer-based representation rather than assuming binary floating point is exact.

### Absolute and relative error

- **Absolute error:** |approximate value − exact value|.
- **Relative error:** absolute error divided by the magnitude of the exact value.

Numerical questions may ask for rounding, significant bits, range, or representation error. Always state what representation is being used.

---

## 2.9 Character and numeric codes

- **ASCII:** original 7-bit code with 128 values, including control codes and common English letters, digits, and punctuation.
- **Extended ASCII:** an informal label for multiple incompatible 8-bit code pages; not one universal encoding.
- **Unicode:** assigns code points to characters across writing systems and symbol sets.
- **UTF-8:** variable-length Unicode encoding using 1–4 bytes; ASCII values use the same byte values in UTF-8.
- **UTF-16:** Unicode encoding using one or two 16-bit code units per code point.
- **EBCDIC:** an 8-bit character encoding associated with some IBM systems.
- **BCD:** each decimal digit is encoded separately using four bits. Decimal 59 is BCD 0101 1001, but ordinary binary 59 is 00111011.
- **Gray code:** adjacent values differ in one bit, useful in some position encoders and transition-sensitive systems.

Do not confuse a Unicode code point with its UTF-8 byte sequence. One character can use multiple bytes.

---

## 2.10 Image, audio, video, and compression

### Images

A raster image is a grid of **pixels**. Resolution gives the pixel dimensions. Colour depth indicates bits used per pixel; greater depth can represent more colours. Uncompressed storage is approximately:

~~~text
width × height × bits per pixel
~~~

**Worked size:** A 1920×1080 image at 24 bits per pixel uses:

~~~text
1920 × 1080 × 24 = 49,766,400 bits
49,766,400 / 8 = 6,220,800 bytes ≈ 6.22 MB (decimal), before file overhead
~~~

### Audio

A digital audio signal samples an analogue signal repeatedly. Important properties include sample rate, bit depth, and number of channels. Higher values usually increase uncompressed size.

Approximate uncompressed audio bit rate:

~~~text
sample rate × bits per sample × number of channels
~~~

For 44,100 samples/second × 16 bits × 2 channels = 1,411,200 bits/second.

### Video

Video is a timed series of frames, often with audio. Resolution, frame rate, colour information, and compression affect its size.

- **Lossless compression:** reconstructs original data exactly.
- **Lossy compression:** removes some information to achieve a smaller representation.
- Never assume a compressed file is smaller by one fixed ratio; content and codec matter.

---

## 2.11 Solved practice

**1. Convert 156₁₀ to hexadecimal.**

~~~text
156 ÷ 16 = 9 remainder 12
12 = C
Answer: 9C₁₆
Check: 9×16 + 12 = 156
~~~

**2. Convert 7B₁₆ to decimal.**

~~~text
7×16 + 11 = 112 + 11 = 123₁₀
~~~

**3. Convert 11010110₂ to hexadecimal.**

~~~text
Group: 1101 0110
1101 = D; 0110 = 6
Answer: D6₁₆
~~~

**4. Find the 8-bit two's-complement representation of −27.**

~~~text
+27 = 00011011
Invert = 11100100
Add 1 = 11100101
Answer: 11100101
~~~

**5. A 24-bit colour image is 800×600 pixels. Find its raw size in bytes.**

~~~text
800 × 600 × 24 / 8 = 1,440,000 bytes = 1.44 MB (decimal)
~~~

---

## 2.12 Questions and answers

1. **What is the base of binary?**  
   Two; its digits are 0 and 1.

2. **Why can one hexadecimal digit represent four binary bits?**  
   Because 16 possible hexadecimal values equal 2⁴ possible four-bit patterns.

3. **What is 45₁₀ in binary, octal, and hexadecimal?**  
   101101₂, 55₈, and 2D₁₆.

4. **What is the 8-bit unsigned range?**  
   0 through 255.

5. **What is the 8-bit two's-complement range?**  
   −128 through +127.

6. **How is a negative two's-complement number formed?**  
   Invert the fixed-width positive pattern and add one.

7. **Why does signed overflow occur?**  
   The exact arithmetic result lies outside the representable signed range.

8. **How does BCD differ from ordinary binary?**  
   BCD encodes each decimal digit separately; ordinary binary encodes the whole number.

9. **Why can floating-point 0.1 be approximate?**  
   Its binary fractional expansion repeats and must be rounded to finite bits.

10. **What is the difference between Unicode and UTF-8?**  
    Unicode defines code points; UTF-8 is one encoding of those code points into bytes.

### GATE/BPSC practice

1. Convert 101101.101₂ to decimal.  
   **Solution:** 32 + 8 + 4 + 1 + 1/2 + 1/8 = 45.625.

2. Convert 0.375₁₀ to binary.  
   **Solution:** .375×2=.75 bit 0; .75×2=1.5 bit 1; .5×2=1 bit 1. Result 0.011₂.

3. Add 011011₂ and 101101₂.  
   **Solution:** 27 + 45 = 72 = 1001000₂.

4. Interpret 11110110₂ as an 8-bit two's-complement value.  
   **Solution:** invert to 00001001; add one to get 10; answer −10.

5. A value is stored in an 8-bit unsigned register. What happens to 250 + 10 if the result is kept in 8 bits?  
   **Solution:** mathematical sum 260; 260 mod 256 = 4, so the stored bit pattern represents 4 unsigned. Carry/overflow information depends on the operation and architecture.

### Multiple-choice check

1. Base 16 uses digits through: A. 9 B. A C. F D. G  
2. 11111111₂ as unsigned 8-bit is: A. 127 B. 128 C. 255 D. 256  
3. The two's-complement range for 8 bits is: A. −127..127 B. −128..127 C. 0..255 D. −255..255  
4. UTF-8 is: A. a floating-point format B. a variable-length Unicode encoding C. an image format D. a CPU instruction  
5. BCD representation of decimal 59 is: A. 111011 B. 0101 1001 C. 0011 1011 D. 1001 0101  
6. One octal digit groups how many binary bits? A. 2 B. 3 C. 4 D. 8

**Answers:** 1-C, 2-C, 3-B, 4-B, 5-B, 6-B.

### Draw-from-memory tasks

- Draw a 32-bit floating-point word and label sign, exponent, and fraction.
- Show binary integer grouping into octal and hexadecimal.
- Draw a sampling diagram: continuous wave → samples → quantised levels → bits.
