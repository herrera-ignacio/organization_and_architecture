# Data Representation

The organization of any computer depends considerably on how it represents numbers, characters, and control information.

## Glossary

### Bit

The most basic unit of information in a digital computer is called a _bit_, which is a contraction of _binary digit_.

A Bit is nothing more than a state of "_on_" or "_off_" within a computer circuit.

### Byte

In 1964, designers of IBM System/360 mainframe computer established a convention of using groups of 8 bits as the basic unit of addressable computer storage, called a _byte_.

### Nibbles

Eight-bit bytes can be divided into two 4-bit halves called __nibbles__.

Because each bit of a byte has a value within a positional numbering system, the nibble containing the least-valued bit is called the low-order nibble, and the other hald the high-order nibble.

### Words

Computer _words_ consist of two or more adjacent bytes that are sometimes addressed and almost always are manipulated collectively.

#### Word size

Word size represents the data size that is handled most efficently by a particular architecture.

Words can be 16 bits, 32 bits, 64 bits, or any other size that makes sense within the contet of a computer's organization.

## Positional Numbering Systems

A numeric value is represented through increasing powers of a _radix_ (or base). This is often referred to as a _weighted numbering system_ because eaxh position is weighted by a power of the radix.

The set of valid numerals for a positional numbering system is equal in size to the radix of that system. For example, there are 10 digits in the decimal system, 0 through 9, and 3 digits in for the ternary (base 3) system, 0, 1, and 2. The largest valid number in a radix system is one smaller than the radix.

```
241.51_10 = 2 x 10^2 + 4 x 10^1 + 3 x 10^0 + 5 x 10^-1 + 1 x 10^-2
212_3 = 2 x 3^2 + 1 x 3^1 + 2 x 3^0 = 23_10
```

The two most important radices in computer science are __binary__ (base 2), digits 0 and 1, and __hexadecimal__ (base 16), digits 0 through 9 with A, B, C, D, E, and F being used to represent the numbers 10 through 15.

## Binary System

### Unsigned Whole to Binary

* Repeated Substraction method
* Division-Remainder method

#### Division-Remainder method

![division method](https://lh4.googleusercontent.com/proxy/TiYkEVjwMsS_FDCy7XP6wK9cjsz_egi2MwGhGT0lDnKOG8ZPLQT--8GSU0TAONssPbOV9psGM8zTXF0tQXRLovKxSJmDUdeZw41ViSmbc5g0gZk4JqVf3H6IQVwo_Nziidk)

![division method](https://admin.indepth.dev/content/images/2019/08/tmp.jpg)

### Fractions to Binary

Fractions can be approximated in any other base system using negative powers of a radix.

![fraction to binary](https://image.slidesharecdn.com/l2-10-121011053919-phpapp02/95/l-210-6-728.jpg)

## Signed Integer Representation

By convention, a "1" in the high-order bit indicates a negative number, the remaining bits are used to represent the number itself.

### Signed Magnitude

The most intuitive, uses remaining bits to represent magnitude of the number.

For example, in an 8-bit word, `-1` would be represented as `1000 0001`, and `+1` as `0000 0001`. Having 7 bits that can be used for the actual representation of the magnitude of the number means that the largest integer in an 8-bit word can represent is `2^7 - 1` or 127, and the smallest is 8 ones, or -127.

Therefore, _N_ bits can represent `-2^(N-1)` to `2^(N-1)-1`.

#### Arithmetic

Signed-magnitude arithmetic is carried out using essentially the same methods as humans use with pencil and paper, but it can get confusing very quickly. As an example, consider the rules for addition:

* If the signs are the same, add the magnitudes and use that same sign for the result.
* If the signs differ, you must determine which operand has the largest magnitude.
* If there is a carry in the high-order bit, we say that we have an _overflow_ condition and the carry is discarded, resulting in an incorrect sum.

#### Overflow

Overflow (and tuhs an erroneous result) in signed numbers occurs when the sign of the result is incorrect.

In signed magnitude, the sign bit is used only for the sign, so we can't _carry into_ it. If there is a carry emitting from the seventh bit, our result will be truncated as the seventh bit overflows, giving an incorrect sum.

#### Simple Rule for Detecting Overflow

If carry into the sign bit equals the carry out of the bit, no overflow has occurred, else, overflow (and thus an error) has occurred.

### Complements

The advantage is that there is no need to process sign bits separately, but we can still easily check the sign of a number by looking at its high-order bit.

This is inspired in the method of finding _diminished radix complement_ of the subtrahend, or "_casting out 9s_", that has been extended to binary operations.

Let's say we wanted to find 167-52, taking the difference of 999 - 52 we have 947. Thus in nine's complement arithmetic we have 167 - 52 = 167 + 947 = 114, and adding the "carry" from the hundreds column is added back to the units place, giving us a correect 115. The numbers 501-999 represent the _radix complements_ of the numbers 001-500 and are used to represent negative magnitudes.

__Complement notation allows us to simplify subtraction by turning it into addition__, but it also gives us a method to represent negative numbers.

Because we do not wish to use a special bit to represent the sign, we need to remember that if a number is negative, we should convert it to its complement. The result shoud have a 1 in the leftmost bit position to indicate the number is negative, whereas all positive numbers should have a zero in the leftmost bit position.

#### One's Complement (diminished radix complement)

Given a number N in base r having d digits, the diminished radix complement of N is defined to be `r^d - 1 - N`.

For decimal numbers, r = 10, and the diminished radix is 10 - 1 = 9. For example, the nine's complement of 2468 is 9999 - 2476 = 7531.

For an equivalent operation in binary, we substract from one less the base 2, which is 1. For example, the one's complement of `0101` is `1111 - 0101 = 1010`. There's an easy trick, this is the same than _switching all of the 1s with 0s and vice versa_.

Suppose we wish to subtract 9 from 23:

1. First express subtrahend (9) in one's complement.
2. Add to minuend (23), we are doing -9 + 23.
3. High order bit will have a 1 carry, which is added to the low-oder bit of the sum (carry-around from using the diminished radix complement), so then last carry is added to the sum.

If we subract 23 from 9:

1. Repeat steps.
2. Last carry is 0, so we are done.
3. We end up with a binary number starting with 1, meaning its negative.
4. If we want to know the absolute value, we take the one's complement of this binary number.

#### Two's Complement (radix complement)

Given a number N in bvase r having d digits,the radix complement of N is defined to be `r^d - N` for N != 0 and 0 for N = 0.

As example, ten's complement of negative two is 10^2 - 2 = 998.

Similarly, in binary, two's complement of the 4-bit number 0011 is 2^4 - 0011 = 10000 - 0011 = 1101. Remember, __only negative numbers need to be converted to two's complement notation__.

Upon closer examination, you will discover that two's complement is nothing more than one's complement incremented by 1. So you can simply flip bits and add 1.

## Floating Point Representation

### Scientific Notation

We can inspire in the _scientific notation_:

* Fractional part (_Mantissa_)
* Exponential part: indicates the power of ten to which the mantissa should be raised obtain the value we need.

### Simple Model

* Sign bit
* Exponent part (on a power of 2)
* Fractional part (significand or mantissa)

The number of bits used for the exponent and significand depends on whether we would like to optimize for __range (more bits in the exponent)__ or __precision (more bits in the significand)__.

### Bias

Simple model doesn't provide for negative exponents. We could fix this by adding a sign bit to the exponent, but it turns out that is more efficient to use a _biased_ exponent.

The idea before a bias value, is to convert every integer in the range into a non-negative integer, which is then stored as a binary numeral, the integers in the desired range of exponents are first adjusted by adding this fixed bias value to each exponent. The bias value is a number near the middle of the range of possible values that we select to represent zero.

For example, suppose we have an exponent with 5 bits, thus allowing for numbers between 0 and 31, we can select 16. Any value less than 16 will indicate negative values, and any number larger than 16 in the exponent field will represent a positive value. This is called an __excess-16__ representation, because we have to substract 16 to get the true value of the exponent.

So for example, if we want to store 17, we calculate 17 = 0.10001 * x^5. The biased exponent is now 16+5=21.

#### Normalization

One large problem with this system is that __we do not have a unique representation for each number__.

Because synonymous forms are not well-suited for digital computers, a __convention__ has been established where the __leftmost bit of the significand will always be a 1__. This is called __normalization__. 

As an example, if we want to express 0.03125 in normalized floating-point form with excess-16 bias:

0.03125_10 = 0.000001_2 ^ 2^0 = 0.1 * 2^4. Applying the bias, the exponent field is 16-4 = 12.

Result: 0 | 01100 | 1000 0000

If we wanted to use the normalization notation implying the 1: 0 | 01100 | 0000 0000

## Floating Point Arithmetic

If we wanted to add two decimal numbers that are expressed in scientific notation, such as `1.5 x 10^2 + 3.5 x 10^3`, we would change one of the numbers so that both of them are expressed in the same power of the base. Floating-point addition and subtraction work the same way.

For example:

```
  0 | 10010 | 1100 1000
+ 0 | 10000 | 1001 1010
```

We retain the larger exponent, renormalizing and trucanting the low-order bit:

```
  11.001000
+  0.10011010
------------
  11.10111010

0 | 10010 | 1110 1110
```

### Floating-Point Errors

When computers carry out floating point calculations, we are modeling the infite system of real numbers in a finite system of integers. What we have, in truth, is an _approximation_ of the real number system. The more bits we use, the better the approximation.

Floating point errors can ve blatant, subtle or unnoticed. Blatant errors, such as numeric __overflow or underflow__ are the ones that cause programs to crash. Subtle errors can lead to widly erroneous results that are often hard to detect before they cause real problems.

We can compute the __relative error__ in our representation by taking the ratio of the absolute value of the error to the true value of the number.

For example, in an 9 bits system, if we want to to store 128.5, we would have `1000 0000.1` (128.1). Typically the low-order bit is dropped or rounded into the next bit. If we are not careful, suchj errors can propage through a lengthy calculation, causing substantian loss of precision. 

### IEE-754 Standard

#### Single Precision (32 bit)

Single precision standard uses a 32-bit word:

* Excess 127 bias
* 8 bit exponent
  * When exponent is 255 and significand is 0, quantitiy represented is +- infinity
  * When exponent is 255 and significand is not 0, represents NaN (error indicator)
* 23 bit significand
* 1 sign bit

#### Double Precision (64 bit)

Double precision standard uses a 64-bit word:

* Excess 1023 bias
* 11 bit exponent
* 52-bit significand

## Character Codes

### Binary-Coded Decimal

Numeric coding system used primarily in IBM mainframe and midrange systems. It encodes each digit of a decimal number to a 4-bit binary form.

### EBCDIC

Extended Binary Coded Decimal Interchange Code.

### ASCII

Defines codes for 32 control characters, 10 digits, 52 letters (upper and lowercase), 32 special characters, and the space character.

### Unicode

ASCII were built around the Latin Alphabet, so in 1991 a consortium of industry and public leaders was formed to establish a new international information exchange code called Unicode.

Unicode is a 16-bit alphabet that is downward compatible with ASCII and the Latin-1 character set. It has the capacity to encode the majority of characters used in every language of the world.
