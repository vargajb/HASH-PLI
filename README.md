# HASH-PLI
[PL/I](https://en.wikipedia.org/wiki/PL/I) implementation of some hash functions.<br>
- [CRC-32](https://en.wikipedia.org/wiki/Cyclic_redundancy_check#CRC-32_algorithm) is an error-detecting code commonly used in digital networks and storage devices to detect accidental changes to digital data.
- [SipHash](https://en.wikipedia.org/wiki/SipHash) is used in hash table implementations of [various software](https://en.wikipedia.org/wiki/SipHash#Usage) (such as [Python](https://en.wikipedia.org/wiki/Python_(programming_language)))<br>
It is a [non-collision-resistant](https://en.wikipedia.org/wiki/Collision_resistance) 
[keyed cryptographic hash function](https://en.wikipedia.org/wiki/List_of_hash_functions#Keyed_cryptographic_hash_functions).

- [SHA-2](https://en.wikipedia.org/wiki/SHA-2) ([unkeyed cryptographic hash function](https://en.wikipedia.org/wiki/List_of_hash_functions#Unkeyed_cryptographic_hash_functions)) family hash functions: SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, SHA-512/256.

I performed a parallel test of these hash algorithms on [IBM mainframe](https://en.wikipedia.org/wiki/Mainframe_computer) and [PC](https://de.wikipedia.org/wiki/Personal_Computer). I calculated the CRC-32 and SHA-2 codes of files with FreeCommander and Total Commander (SipHash was checked [online](https://duzun.me/playground/hash#siphash=)) on PC. After uploading the files to Mainframe, I repeated the hash calculation there as well. The algorithm implemented in PL/I gives the same result.

 If you have a license for the C compiler on the mainframe, you have an easier task. :)

## What is the PL/I programming language?
[PL/I](https://en.wikipedia.org/wiki/PL/I) is one of the oldest programming languages still used today on [IBM mainframe](https://en.wikipedia.org/wiki/Mainframe_computer) computers.

## Tested with:
CPU: [IBM z15](https://en.wikipedia.org/wiki/IBM_z15_(microprocessor)) 8561-710 ([big-endian](https://en.wikipedia.org/wiki/Endianness))<br>
Operating system: [z/OS](https://en.wikipedia.org/wiki/Z/OS) 2.5<br>
Compiler: [IBM(R) Enterprise PL/I for z/OS V5.R3.M0](https://www.ibm.com/support/pages/enterprise-pli-zos-documentation-library)

## Performance:
Tested 100 times with 1 MB data.
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

## How to call?
More sample calls can be found in the HASH.PLI main program.
### Sample PL/I code to call SHA-512:
```
      DECLARE
 (ADDR,STG,NULL, UTF8,UTF8TOCHAR,HEX)  BUILTIN;
 %INCLUDE O(PPMMAC);    /* GENERAL PL/I PREPROCESSORS MACROS          */
 %INCLUDE O(SHA2);      /* TYPE DEFINITIONS FOR THE SHA2 ALGORITHM    */
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

 %INCLUDE U(SHA2);      /* PL/I IMPLEMENTATION OF THE SHA2 ALGORITHM  */
```
#### Sample output in SYSPRINT:
[It gives the same result that was written in Wikipedia.](https://en.wikipedia.org/w/index.php?title=SHA-2&oldid=587073545#Examples_of_SHA-2_variants)
```
TEST SHA512
INPUT      : "The quick brown fox jumps over the lazy dog"
HASH VALUE : 07E547D9586F6A73F73FBAC0435ED76951218FB7D0C8D788A309D785436BBB642E93A252A954F23912547D1E8A3B5ED6E1BFD7097821233FA0538F3DB854FEE6
```

## Description of sources
| Program name | Description                                  |
|--------------|----------------------------------------------|
| CRC32.IPU    | PL/I implementation of the CRC-32 algorithm  |
| HASH.PLI     | Main program for calling the HASH functions  |
| PPMMAC.IPO   | General purpose PL/I preprocessors macros    |
| SIPHASH.IPU  | PL/I implementation of the SipHash algorithm |
| SHA2.IPO     | Type definitions for the SHA2 functions      |
| SHA2.IPU     | PL/I implementation of the SHA2 functions    |
| SYSPRINT.txt | Sample output                                |
