---
author: "UmmIt"
title: "Caesar Cipher encryption and decryption"
decryption: "Caesar Cipher is a type of substitution cipher in which each letter in the plaintext is shifted a certain number of places down the alphabet."
tags: ["Caesar Cipher", "encryption", "decryption", "mathematics"]
date: 2025-01-15T10:58:16+0800
---

## Introduction

Caesar Cipher is a type of substitution cipher in which each letter in the plaintext is shifted a certain number of places down the alphabet. For example, with a shift of 1, A would be replaced by B, B would become C, and so on. Here is a quick example of the Caesar Cipher encryption and decryption.

- Encryption: that is shifting the letters to the right (Add)
- Decryption: that is shifting the letters to the left (Subtract)

In a Caesar Cipher, each letter is shifted by a certain number of places down the alphabet. For example, with a shift of 3, A becomes D, B becomes E, and so on.

## Table of A-Z letters

Before we start, let's create a table of A-Z letters and their corresponding numbers.

| Letter | Number |
|--------|--------|
| A      | 1      |
| B      | 2      |
| C      | 3      |
| D      | 4      |
| E      | 5      |
| F      | 6      |
| G      | 7      |
| H      | 8      |
| I      | 9      |
| J      | 10     |
| K      | 11     |
| L      | 12     |
| M      | 13     |
| N      | 14     |
| O      | 15     |
| P      | 16     |
| Q      | 17     |
| R      | 18     |
| S      | 19     |
| T      | 20     |
| U      | 21     |
| V      | 22     |
| W      | 23     |
| X      | 24     |
| Y      | 25     |
| Z      | 26     |

## Encryption

Let assume that we have a plaintext `HELLO` and we want to encrypt it using Caesar Cipher with a shift of 3. The encryption process would be as follows:

1. Convert each letter of the plaintext to its corresponding number.
2. Shift each number by the specified shift value.
3. Convert each shifted number back to its corresponding letter.

For example, to encrypt the plaintext `HELLO` with a shift of 3:

- H -> 8
- E -> 5
- L -> 12
- L -> 12
- O -> 15

After shifting each number by 3:

- H -> 8 + 3 = 11 -> K
- E -> 5 + 3 = 8 -> H
- L -> 12 + 3 = 15 -> O
- L -> 12 + 3 = 15 -> O
- O -> 15 + 3 = 18 -> R

Therefore, the encrypted text would be `KHOOR`.

## Decryption

To decrypt the encrypted text `KHOOR` with a shift of 3:

1. Convert each letter of the encrypted text to its corresponding number.
2. Shift each number back by the specified shift value.
3. Convert each shifted number back to its corresponding letter.

For example, to decrypt the encrypted text `KHOOR` with a shift of 3:

- K -> 11
- H -> 8
- O -> 15
- O -> 15
- R -> 18

After shifting each number back by 3:

- K -> 11 - 3 = 8 -> H
- H -> 8 - 3 = 5 -> E
- O -> 15 - 3 = 12 -> L
- O -> 15 - 3 = 12 -> L
- R -> 18 - 3 = 15 -> O

Therefore, the decrypted text would be `HELLO`.

## Overflow positions

Overflow positions is the situation when the shift value is greater than 26. For example, if the shift value is 30. In this case, we can simply subtract 26 from the shifted number to get the correct position.

As the example, Using 25 as the shift value of `E`: (encrypt)

- E -> 5 + 25 = 30 -> 30 - 26 = 4 -> D

so the encrypted text would be `D`.

This method only need to when the value is greater than 26, which is out of the range of A-26 (1-26).

## Conclusion

Caesar Cipher is a simple encryption technique that can be easily broken by brute force. I wrote this only for taking notes of my school. Please dont use this on your real project. If you want to encrypt your data, you should use a more secure encryption algorithm like AES, RSA, etc.
