# HASH-PLI
## [PL/I](https://en.wikipedia.org/wiki/PL/I) Implementation of [Hash Functions](https://en.wikipedia.org/wiki/Hash_function)
This repository showcases the implementation of several widely-used cryptographic hash functions using the **PL/I programming language**, demonstrating its capability to handle complex algorithms. By personally developing these implementations, I have proven that PL/I is not only a powerful language but also well-suited for high-performance tasks like cryptographic hashing.

Implemented Hash Functions:
- [CRC-32](https://en.wikipedia.org/wiki/Computation_of_cyclic_redundancy_checks#CRC-32_algorithm): A highly efficient, error-detecting code commonly used in digital networks and storage devices to verify data integrity.
- [SipHash](https://en.wikipedia.org/wiki/SipHash): A fast, [keyed cryptographic hash function](https://en.wikipedia.org/wiki/List_of_hash_functions#Keyed_cryptographic_hash_functions) designed for use in [hash table implementations](https://en.wikipedia.org/wiki/SipHash#Usage) (e.g., in [Python](https://en.wikipedia.org/wiki/Python_(programming_language))), and particularly effective against [hash-flooding attacks](https://en.wikipedia.org/wiki/Collision_attack#Hash_flooding).
- **[SHA-2 Family](https://en.wikipedia.org/wiki/SHA-2)**: A complete, custom-built implementation of the full SHA-2 family. This includes SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, and SHA-512/256, covering all variations. These functions are essential for modern cryptography, ensuring data integrity and supporting digital signatures.


By implementing all of these hash algorithms, I have demonstrated that PL/I can handle both high-performance and cryptographically secure tasks on IBM mainframes.

## What is PL/I?
PL/I (Programming Language One) is one of the oldest high-level programming languages, still actively used today on [IBM Mainframe systems](https://en.wikipedia.org/wiki/Mainframe_computer). Known for its versatility, PL/I is capable of handling scientific, engineering, and business applications, making it a reliable choice for a wide range of tasks in mainframe environments.

## Testing
The implemented hash algorithms underwent thorough testing both on an IBM Mainframe and a PC. On the PC, I used FreeCommander and Total Commander for CRC-32 and SHA-2, and an [online tool](https://duzun.me/playground/hash#siphash=) for SipHash. After transferring the files to the mainframe, I ran the same algorithms using the PL/I implementations. The matching results across both platforms validate the correctness and reliability of my implementation.

### Test Environment
**CPU:** [IBM z15 8561-710](https://www.topgun-tech.com/products/ibm-zsystems/ibm-z15-ibm-8561/) ([big-endian](https://en.wikipedia.org/wiki/Endianness))<br>
**Operating system:** [z/OS](https://en.wikipedia.org/wiki/Z/OS) 2.5<br>
**Compiler:** [IBM(R) Enterprise PL/I for z/OS V5.R3.M0](https://www.ibm.com/support/pages/enterprise-pli-zos-documentation-library)

### Performance Results
The following performance metrics demonstrate the efficiency of each algorithm on an IBM mainframe. These benchmarks illustrate that PL/I can achieve high throughput, making it a solid choice for performance-critical tasks in mainframe environments.
| Algorithm          | Speed (MB/sec)  |
| ------------------ |:---------------:|
| CRC-32             | 112,48          |
| SipHash-2-4 64 bit | 230,41          |
| SHA-224            | 223,21          |
| SHA-256            | 223,71          |
| SHA-384            |  64,30          |
| SHA-512            |  64,47          |
| SHA-512/224        |  64,35          |
| SHA-512/256        |  64,64          |

## How to Call the Functions?
For practical use, sample calls to each hash function are included in the `HASH.PLI` program. Users can refer to these samples to see how to invoke and test the hash functions within a PL/I environment.

### Sample PL/I code to call SHA-512
```
      DECLARE
 (ADDR,STG,NULL, UTF8,UTF8TOCHAR,HEX)  BUILTIN;
 %INCLUDE O(PPMMAC);    /* General PL/I preprocessor macros           */
 %INCLUDE O(SHA2);      /* Type definitions for the SHA2 algorithm    */
      DECLARE
 TEST_STR1      CHAR(43) STATIC
              INIT(UTF8('The quick brown fox jumps over the lazy dog')),
 SHA512_DIGEST  TYPE SHA512_DIGEST_TYPE;

    SHA512_DIGEST = SHA512(ADDR(TEST_STR1), STG(TEST_STR1));
    PUT EDIT('TEST SHA512',
             'INPUT      : "', UTF8TOCHAR(TEST_STR1), '"',
             'HASH VALUE : ' , HEX(SHA512_DIGEST))
            (SKIP, A,
             SKIP, A,A,A,
             SKIP, A,A);

 %INCLUDE U(SHA2);      /*  PL/I implementation of the SHA2 algorithm */
```
#### Sample output in SYSPRINT
The output produced matches the SHA-512 hash result as documented on [Wikipedia](https://en.wikipedia.org/w/index.php?title=SHA-2&oldid=587073545#Examples_of_SHA-2_variants).
```
TEST SHA512
INPUT      : "The quick brown fox jumps over the lazy dog"
HASH VALUE : 07E547D9586F6A73F73FBAC0435ED76951218FB7D0C8D788A309D785436BBB642E93A252A954F23912547D1E8A3B5ED6E1BFD7097821233FA0538F3DB854FEE6
```

## Description of sources
| Program name | Description                                  |
|--------------|----------------------------------------------|
| CRC32.IPU    | PL/I implementation of the CRC-32 algorithm  |
| HASH.PLI     | Main program for calling the hash functions  |
| PPMMAC.IPO   | General-purpose PL/I preprocessors macros    |
| SIPHASH.IPU  | PL/I implementation of the SipHash algorithm |
| SHA2.IPO     | Type definitions for the SHA-2 functions     |
| SHA2.IPU     | PL/I implementation of the SHA-2 functions   |
| SYSPRINT.txt | Sample output                                |

## Final Touch
I have optimized the implementation as much as I can, and at this point, I don't see any further opportunities for optimization. The PL/I code performs just as efficiently as the original C implementation of these algorithms, without any notable constraints from the language itself.

I also welcome contributions and suggestions for extending this repository with additional cryptographic functions or enhancements.
