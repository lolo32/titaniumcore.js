
# JavaScript Cryptography Toolkit

                                                            Titaniumcore Project

Atsushi Oka [ http://oka.nu/ ]                                       Jan 10,2009

This library is an object oriented cryptography toolkit that implements several
fundamental cryptographic algorithms including TWOFISH, SERPENT, RIJNDAEL, RSA
with key-generation and SHA(SHA-1,224,256,384,512) for JavaScript. This library
works in ActionScript as well. The unique feature of this library is
asynchronous processing. A heavyweight process such as 4096bit RSA key
generation will be done asynchronously so that this library does not cause
problems such as freezing browsers, "slow-downing" warning dialogs, etc.

## Demonstration

Block-Cipher Demonstration
*  [Cipher.sample.html](Cipher.sample.html)

RSA Key-Generation and Encryption/Decryption
*  [RSA.sample1.html](RSA.sample1.html)
*  [RSA.sample2.html](RSA.sample2.html)

SHA Digest Calculation Demonstration
*  [SHA.sample.html](SHA.sample.html)

## Contents

* [Cipher.js](Cipher.js)

    Contains Cipher class which implements some block-cipher algorithms.  
    See [Cipher.readme.md](Cipher.readme.md)

* [RSA.init1.js](RSA.init1.js)
* [RSA.init2.js](RSA.init2.js)
* [RSA.init3.js](RSA.init3.js)

    Contains RSA class which implements RSA encryption,decryption and
    key-generation with asynchronous processing feature.  
    See [RSA.readme.md](RSA.readme.md)

* [BigInteger.init1.js](BigInteger.init1.js)
* [BigInteger.init2.js](BigInteger.init2.js)
* [BigInteger.init3.js](BigInteger.init3.js)

    Contains BigInteger class which implements calculation of variable
    length integer.  
    See [BigInteger.readme.md](BigInteger.readme.md)

* [SecureRandom.js](SecureRandom.js)

    Contains SecureRandom class which implements Arcfour pseudo random
    generator.  
    See [SecureRandom.readme.md](SecureRandom.readme.md)

* [BitPadding.js](BitPadding.js)

    Contains a class that implements "bit-padding" padding-scheme.
    See [BitPadding.readme.md](BitPadding.readme.md)

* [SOAEP.js](SOAEP.js)

    Contains a class that implements a padding-scheme SOAEP. SOAEP is an
    original scheme that I have designed for Titaniumcore project.  
    See [SOAEP.readme.md](SOAEP.readme.md)

* [RSAKeyFormat.js](RSAKeyFormat.js)

    Contains some methods that convert RSA key and a binary string.  
    See [RSAKeyFormat.readme.md](RSAKeyFormat.readme.md)

* [RSAMessageFormat.js](RSAMessageFormat.js)

    Defining facade methods for RSA encryption/decryption.  
    See [RSAMessageFormat.readme.md](RSAMessageFormat.readme.md)

* [SHA.js](SHA.js)

    Contains SHA class that calculates various SHA hash value.  
    See [SHA.readme.md](SHA.readme.md)

* [jsSHA.js](jsSHA.js)
* [jsSHA.class.js](jsSHA.class.js)

    Contains the core methods for SHA calculation.  
    See [jsSHA.readme.md](jsSHA.readme.md)

    SPECIAL THANKS to Brian Turek [ http://sourceforge.net/users/caligatio ]

## Notice

- Dependency

  Most of above scripts depend on "tools" scripts and "nonstructured.js".

    - "tools" scripts
    
      "tools" is a set of utility scripts.  See [readme.md](../tools/readme.md).

    - [nonstructured.js](../nonstructured/nonstructured.js)
    
      "nonstructured.js" is a framework for asynchronous execution.  
      See [nonstructured.readme.md](../nonstructured/nonstructured.readme.md) 


- Data Conversion 

  This library does not support utf-8 conversion nor base64
  conversion directly. Most of functions in this library only take byte
  arrays as their parameters and their result is as a byte array object.
  This library does not implement conversions.  This is intended to keep
  higher reusability. 

  When data conversion between utf-8 or base64 to binary data is
  necessary, use "[binary.js](../tools/binary.js)" or another conversion library.
  [binary.js](../tools/binary.js) is included in "tools" directory.

  See [binary.readme.md](../tools/binary.readme.md) for further information.

## Acknowledgment

The core algorithm of Cipher.js is originally written by
Michiel van Everdingen.

The original form can be referred at  
    http://home.versatel.nl/MAvanEverdingen/Code/code.html

Contact of the Author  
    Michiel van Everdingen  
    http://home.versatel.nl/MAvanEverdingen/index.html

See [Cipher.readme.md](Cipher.readme.md)

---

Following files were originally written by Tom Wu :

*   SecureRandom.js
*   BigInteger.init1.js
*   BigInteger.init2.js
*   BigInteger.init3.js
*   RSA.init1.js
*   RSA.init2.js
*   RSA.init3.js

   Copyright (c) 2005  Tom Wu
   All Rights Reserved.  
   http://www-cs-students.stanford.edu/~tjw/jsbn/

   See "LICENSE" for details.  
   http://www-cs-students.stanford.edu/~tjw/jsbn/LICENSE

   See [RSA.readme.md](RSA.readme.md)

---

jsSHA.js is written by Brian Turek [ http://sourceforge.net/users/caligatio ].

> A JavaScript implementation of the SHA family of hashes, 
> as defined in FIPS PUB 180-2
> Version 1.1 Copyright Brian Turek 2008
> Distributed under the BSD License
> See http://jssha.sourceforge.net/ for more information
>
> Several functions taken from Paul Johnson

See [jsSHA.readme.md](jsSHA.readme.md)

This library is distributed under LGPL.

// vim:expandtab:
