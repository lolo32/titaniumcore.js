
# A JavaScript RSA public-key Cryptography Implementation With RSA Key-Generation

                                                            Titaniumcore Project

Atsushi Oka [ http://oka.nu/ ]                                        Jan 4,2009

This is a RSA public-key cryptography implementation which is written in
JavaScript. This library can asynchronously process encryption, decryption
and RSA key-generation.

## What This Library Can

There are two full functioning sample :

- [RSA.sample1.html](RSA.sample1.html)

  An example for RSA key generation.

- [RSA.sample2.html](RSA.sample2.html)

  An example for encryption and decryption.

## What You Have To Do :

```javascript
// Example ( Public Key Encryption and Decryption )

// Import classes.
var RSA = __import( this,"titaniumcore.crypto.RSA" );
var BigInteger = __import( this,"titaniumcore.crypto.BigInteger" );

// Create an RSA engine.
var rsa = new RSA();

// Generate new RSA key.
rsa.generate( 128,65537 );

// Create a message.
var message = new BigInteger( "DEADBEAFDEADBEAFDEADBEAFDEADBEAF",16 );

// Encrypt by a public key.
var encrypted = rsa.processPublic( message );

// Decrypt by a private key.
var decrypted = rsa.processPrivate( encrypted );
```

There is a simple example:

- [RSA.example1.html](RSA.example1.html)
- [RSA.example2.html](RSA.example2.html)

## Link

There are three levels for linking. You can choose one of these levels,
depending your necessity.

- init1 :

  Defines only functions for decryption.

- init2 :

  Defines functions for decryption, encryption and key-generation.

- init3 :

  Defines functions for asynchronous encryption,decryption and
  key-generation.


Followings are examples of linking in HTML.

- init1 :

  ```html
  <script src="./cipher/BigInteger.init1.js"></script>
  <script src="./cipher/RSA.init1.js"></script>
  ```

- init2 :

  ```html
  <script src="./cipher/BigInteger.init1.js"></script>
  <script src="./cipher/RSA.init1.js"></script>
  <script src="./cipher/SecureRandom.js"></script>
  <script src="./cipher/BigInteger.init2.js"></script>
  <script src="./cipher/RSA.init2.js"></script>
  ```

- init3 :

  ```html
  <script src="./cipher/BigInteger.init1.js"></script>
  <script src="./cipher/RSA.init1.js"></script>
  script src="./cipher/SecureRandom.js"></script>
  <script src="./cipher/BigInteger.init2.js"></script>
  <script src="./cipher/RSA.init2.js"></script>
  <script src="./nonstructured/nonstructured.js"></script>
  <script src="./cipher/BigInteger.init3.js"></script>
  <script src="./cipher/RSA.init3.js"></script>
  ```

## Import
```javascript
var RSA = __import( this,"titaniumcore.crypto.RSA" );
```

## CONSTRUCTOR

- `function RSA()`

  Creates new RSA object.
  There is no parameter.

## FIELDS
- `n`     : BigInteger

  the modulus for both the public and private keys.

- `e`     : Number

  the public key exponent.

- `d`     : BigInteger

  the private key exponent.

- `ksize` : Number

  the key size.

- `keyFormat` : KeyFormat

  Specifies a key formatter that encodes RSA keys to or decodes RSA keys
  from byte arrays. The object in keyFormat must implement the KeyFormat
  interface.

  See [KeyFormat.interface.md](KeyFormat.interface.md).

- `messageFormat` : MessageFormat

  Specifies a message formatter. A message format is a message encryption
  scheme. The object in messageFormat must implement the MessageFormat
  interface.

  See
  - [RSAMessageFormat.implementation.readme.md](RSAMessageFormat.implementation.readme.md)
  - [RSAMessageFormat.readme.md](RSAMessageFormat.readme.md)
  - [MessageFormat.interface.md](MessageFormat.interface.md)

- `tolerantlyGenerate` : boolean

  If this field is true, this RSA object will generate a RSA key that has
  one or two more bits than specified. When this option is true, the RSA
  key-generation will be slightly faster.

## METHODS

### Defined in init1

- `function publicKey(n,e)`

  Set a public key.

  - `n` : A BigInteger object. Specifies N.
  - `e` : A BigInteger object or A Number object. Specifies an exponent.

- `function processPublic(message)`

  Encrypts/Decrypts by the public key.

  - `message` : Specifies a BigInteger object that contains a message.

- `function publicKeyBytes(keybytes)`

  Set a public key in binary represention to the object.  Before call
  this function, a KeyFormat object must be set to the rsa.keyFormat
  property.

- `function publicEncrypt(message)`  
  `function publicDecrypt(message)`  
  `function publicEncryptMaxSize()`

  Encrypts/decrypts a message by a specified encryption scheme with the
  public key.  Before call these function a MessageFormat object must be
  set to the rsa.messageFormat property.

  `publicEncryptMaxSize()` function returns a number of the maximum message
  size that the encryption scheme can encrypt/decrypt.

### Defined in init2

- `function privateKey(n,e,d)`

  Set a private key.

  - `n` : A BigInteger object. Specifies a modulo N.
  - `e` : A BigInteger object or A Number object. Specifies a public exponent.
  - `d` : A BigInteger object. Specifies a private exponent D.

- `function processPrivate(message)`

  Encrypts/Decrypts by the private key.

  - `message` : Specifies a BigInteger object that contains a message.

- `function generate(b,e)`

  Generates a RSA key and set to this object.

  - `b` : Specifies bit-length of RSA key.
  - `e` : A BigInteger object or A Number object. specifies a public exponent.

- `function privateKeyBytes(keybytes)`

  Set a private key in binary representation to the object.  Before call
  this function, a KeyFormat object must be set to the rsa.keyFormat
  property.

- `function privateEncrypt(message)`  
  `function privateDecrypt(message)`  
  `function privateEncryptMaxSize()`

  Encrypts/decrypts a message by a specified encryption scheme with the
  private key.  Before call these function a MessageFormat object must be
  set to the `rsa.messageFormat` property.

  `privateEncryptMaxSize()` function returns a number of the maximum message
  size that the encryption scheme can encrypt/decrypt.

### Defined in init3

- `function generateAsync( keylen, exp, progress, result, done )`

  Generates a RSA key asynchronously.

  - `keylen` : Same as generate() function.
  - `exp` : Same as generate() function.
  - `progress` : Specifies a callback closure.
  - `result` : Specifies a closure that receives the newly generated key.
  - `done` : Specifies a callback closure.

  Callback closures will be called when each process step is done.

- `function processPublicAsync( message, progress, result, done )`

  Encrypts/Decrypts by the public key asynchronously.

  - `message` : Specifies a BigInteger object that contains a message.
  - `progress` : Specifies a callback closure.
  - `result` : Specifies a closure that receives the encrypted/decrypted value.
  - `done` : Specifies a callback closure.

- `function processPrivateAsync( message, progress, result, done )`

  Encrypts/Decrypts by the private key asynchronously.

  - `message` : Specifies a BigInteger object that contains a message.
  - `progress` : Specifies a callback closure.
  - `result` : Specifies a closure that receives the encrypted/decrypted value.
  - `done` : Specifies a callback closure.

## SPECIFICATION OF CALLBACK CLOSURES

- `function progress( stepCount )`

  - `stepCount` : A number of current step count will be passed to this parameter.

- `function result( value1, value2 )`

  - in processPublicAsync/processPrivateAsync
    - `value1` : the encrypted/decrypted value in a byte array.
    - `value2` : null

  - in generateAsync
    - `value1` : The newly generated key in a byte array.
    - `value2` : A RSAKey object.

- `function done( succeeded, count, time ,startTime, finishTime )`

  - `succeeded` : true if no problem was occured.
  - `count` : A number of total step count.
  - `time` : Elapsed time in millisec.
  - `startTime` : A Date object of start time.
  - `finishedTime` : A Date object of finished time.

## ACKNOWLEDGMENT

Following files were originally written by Tom Wu :

- SecureRandom.js
- BigInteger.init1.js
- BigInteger.init2.js
- BigInteger.init3.js
- RSA.init1.js
- RSA.init2.js
- RSA.init3.js

Copyright (c) 2005  Tom Wu
All Rights Reserved.  
http://www-cs-students.stanford.edu/~tjw/jsbn/

See "LICENSE" for details.  
http://www-cs-students.stanford.edu/~tjw/jsbn/LICENSE

Additionally Atushi Oka has done following works :
- Packaged all classes
- Added asynchronous execution feauture
- Fixed bugs
- Revised ambiguous class interface on constructors/methods.
- Adapted to Flash ActionScript
  ( "add" is a reserved identifier in ActionScript. etc.)

// vim:expandtab:
