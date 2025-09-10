# Diffie-Hellman Key Exchange Simulator

## Project Description

This project implements a comprehensive Diffie-Hellman key exchange protocol simulator with interactive capabilities. The simulator demonstrates the secure exchange of cryptographic keys between two parties over an insecure channel, while also illustrating the potential vulnerability to man-in-the-middle attacks. It incorporates advanced mathematical concepts including Miller-Rabin primality testing and efficient primitive root discovery using Carmichael's theorem.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Features](#features)
- [Methodology](#methodology)
- [Examples](#examples)
- [References](#references)
- [Dependencies](#dependencies)
- [Algorithms/Mathematical Concepts Used](#algorithmsmathematical-concepts-used)
- [License](#license)
- [Acknowledgments](#acknowledgments)
- [Note](#note)

---

## Installation

The simulator requires Python 3.6 (or higher), and Jupyter Notebook.
* Simply run the diffie-hellman_key_exchange_simulator.ipynb file via Jupyter Notebook:
     ```bash
     jupyter notebook
     ```

## Usage

The simulator provides an interactive notebook interface that guides users through the Diffie-Hellman Key Exchange process:

1. Select or enter a prime number
2. Choose a primitive root from the available options
3. Input private keys for both parties (Ram and Shyam)
4. Observe the computed public keys and shared secret
5. Optionally simulate a man-in-the-middle attack

## Features

- **Secure Prime Generation**: Select from sample primes or enter custom primes
- **Primitive Root Discovery**: Automatically identifies valid primitive roots for the selected prime
- **Comprehensive Miller-Rabin Primality Testing**: Uses 40 rounds of testing for high-confidence primality verification
- **Complete Protocol Simulation**: Demonstrates key generation, exchange, and verification
- **Man-in-the-Middle Attack Simulation**: Shows how an attacker might compromise the protocol without authentication
- **Educational Output**: Provides detailed information about each step of the process
- **Input Validation**: Ensures all user inputs are valid and within appropriate ranges
- **Efficient Implementation**: Uses techniques like memoization and early termination for performance

## Methodology

The simulator follows this process flow:

1. **Prime Selection**:
   - User specifies the desired number of digits for the prime
   - The program generates sample primes of that length
   - User selects from the samples or enters a custom prime (p)
   - Miller-Rabin primality testing confirms the selection

2. **Primitive Root Identification**:
   - The program finds primitive roots for the selected prime using Carmichael's theorem
   - User selects one primitive root (g) from the available options

3. **Key Exchange**:
   - Users enter private keys for both parties (Ram (a) and Shyam (b))
   - The program calculates public keys using modular exponentiation:
     - Ram's public key: g^a mod p
     - Shyam's public key: g^b mod p
   - The program computes shared secrets:
     - Ram's computation: (g^b)^a mod p
     - Shyam's computation: (g^a)^b mod p
   - The program verifies that both parties derive the same shared secret

4. **Man-in-the-Middle Attack Simulation** (optional):
   - User enters the attacker's (Hari's) private key
   - The program demonstrates how Hari can establish separate keys with Ram and Shyam
   - The simulation shows whether the attack succeeds or fails based on the computed keys

## Examples

### Example 1: Different Forged Secret Keys

**Inputs:**
- Prime: 23
- Primitive Root: 5
- Ram's Private Key: 4
- Shyam's Private Key: 3
- Hari's Private Key: 3

**Outputs:**
- Ram's Public Key: 5^4 mod 23 = 4
- Shyam's Public Key: 5^3 mod 23 = 10
- Legitimate Shared Secret: 4^3 mod 23 = 10^4 mod 23 = 18
- Forged Key (Hari to both parties): 5^3 mod 23 = 10
- Ram's Compromised Secret: 10^4 mod 23 = 18
- Shyam's Compromised Secret: 10^3 mod 23 = 11
- Hari's Computed Secret from Ram: 4^3 mod 23 = 18
- Hari's Computed Secret from Shyam: 10^3 mod 23 = 11

In this scenario, the forged keys lead Ram and Shyam to compute different shared secrets (18 ≠ 11). This mismatch could potentially make them suspect an attack due to encryption/decryption failures.

### Example 2: Same Forged Secret Keys

**Inputs:**
- Prime: 23
- Primitive Root: 5
- Ram's Private Key: 4
- Shyam's Private Key: 2
- Hari's Private Key: 11

**Outputs:**
- Ram's Public Key: 5^4 mod 23 = 4
- Shyam's Public Key: 5^2 mod 23 = 2
- Legitimate Shared Secret: 4^2 mod 23 = 2^4 mod 23 = 16
- Forged Key (Hari to both parties): 5^11 mod 23 = 22
- Ram's Compromised Secret: 22^4 mod 23 = 1
- Shyam's Compromised Secret: 22^2 mod 23 = 1
- Hari's Computed Secret from Ram: 4^11 mod 23 = 1
- Hari's Computed Secret from Shyam: 2^11 mod 23 = 1

In this scenario, the forged keys lead Ram and Shyam to compute the same shared secret (1), and Hari can compute this same value. This makes the attack completely undetectable, demonstrating the vulnerability of unauthenticated Diffie-Hellman key exchange.

## References

1. Diffie, W., & Hellman, M. E. (1976). New directions in cryptography. IEEE Transactions on Information Theory, 22(6), 644-654.
2. Miller, G. L. (1975). Riemann's Hypothesis and Tests for Primality. Journal of Computer and System Sciences, 13(3), 300-317.
3. Rabin, M. O. (1980). Probabilistic algorithm for testing primality. Journal of Number Theory, 12(1), 128-138.
4. Stinson, D. R. (2005). Cryptography: Theory and Practice (3rd ed.). Chapman and Hall/CRC.
5. Menezes, A. J., van Oorschot, P. C., & Vanstone, S. A. (1996). Handbook of Applied Cryptography. CRC Press.

## Dependencies

The simulator uses only Python standard libraries:
- `math`: For mathematical operations
- `random`: For generating random numbers in primality testing
- `functools`: For `lru_cache` decorator to implement memoization
- `typing`: For type hints
- `dataclasses`: For the `DHParameters` class

## Algorithms/Mathematical Concepts Used

1. **Diffie-Hellman Key Exchange Protocol**:
   - Based on the discrete logarithm problem in modular arithmetic
   - Uses the property that (g^a)^b mod p = (g^b)^a mod p

2. **Miller-Rabin Primality Test**:
   - Probabilistic primality testing algorithm
   - Uses Fermat's little theorem with witness values
   - Complexity: O(k log³n) where k is the number of rounds

3. **Primitive Root Discovery**:
   - Uses Carmichael's theorem to efficiently check if a number is a primitive root
   - Requires factorization of φ(p) = p-1 (for prime p)
   - Validates g^((p-1)/q) mod p ≠ 1 for all prime factors q of p-1

4. **Modular Exponentiation**:
   - Fast computation of a^b mod n using the square-and-multiply method
   - Implemented via Python's built-in `pow(a, b, n)` function
   - Complexity: O(log b)

5. **Prime Factorization**:
   - Trial division algorithm with optimizations
   - Uses memoization for efficiency
   - Wheel factorization approach

6. **Man-in-the-Middle Attack Simulation**:
   - Demonstrates the protocol's vulnerability without authentication
   - Shows how an attacker can establish separate keys with both parties

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- The fundamental work of Whitfield Diffie and Martin Hellman in public-key cryptography
- Gary L. Miller and Michael O. Rabin for their contributions to primality testing
- The cryptographic community for continued research in secure communications

## Note

| AI was used to generate most of the docstrings and inline comments in the code. |
|:--:|
