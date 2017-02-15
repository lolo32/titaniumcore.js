
# Titaniumcore RSA Encryption Scheme

                                                            Titaniumcore Project

Atsushi Oka [ http://oka.nu/ ]                                        Jan 8,2009

This script defines RSAMessageFormat class that implements an RSA message
encryption scheme - Titaniumcore RSA Encryption Scheme.

## Link
```html
<script src="./tools/packages.js"></script>
<script src="./cipher/SecureRandom.js" ></script>
<script src="./cipher/BigInteger.init1.js" ></script>
<script src="./cipher/BigInteger.init2.js" ></script>
<script src="./cipher/RSA.init1.js" ></script>
<script src="./cipher/RSA.init2.js" ></script>
<script src="./cipher/SOAEP.js" ></script>
<script src="./cipher/BitPadding.js" ></script>
<script src="./tools/binary.js" ></script>
<script src="./cipher/RSAMessageFormat.js"></script>
```

## Import
```javascript
var RSAMessageFormat = __import( this, "titaniumcore.crypto.RSAMessageFormat" );
```

## Constructor
*    `new RSAMessageFormat( paddingScheme )`

        `paddingScheme` : a PaddingScheme object.
        See [PaddingScheme.interface.md](PaddingScheme.interface.md)

## Interface

RSAMessageFormat implements MessageFormat interface.
See [MessageFormat.interface.md](MessageFormat.interface.md)

// vim:ts=8:expandtab:
