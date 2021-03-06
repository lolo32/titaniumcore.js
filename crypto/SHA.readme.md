
# SHA

                                                            Titaniumcore Project

Atsushi Oka [ http://oka.nu/ ]                                       Jan 11,2009

SHA class is a class to calculate SHA hash value.

## Linking

HTML
```html
<script src="./tools/packages.js"></script>
<script src="./tools/binary.js"></script>
<script src="./cipher/jsSHA.js"></script>
<script src="./cipher/SHA.js"></script>
```

ActionScript
```actionscript
#include "./tools/packages.js"
#include "./tools/binary.js"
#include "./cipher/jsSHA.js"
#include "./cipher/SHA.js"
```

## Import

```javascript
var SHA = __import( this,"titaniumcore.crypto.SHA" );
```

## Example

```javascript
var sha = SHA.create( "SHA-1" );
var result = sha.hash( [ 0x00, 0x00, 0x00, 0x00 ] );
alert( result.join(" ") );
```

## Reference

- `SHA.create( type_name )`

  A factory method.  The parameter `type_name` Specifies hash name.
  Available names are :

      "SHA-1", "SHA-224", "SHA-256","SHA-384" and "SHA-512"

  Otherwise throws an error. Returns an SHA object.

- `SHA.prototype.hash( message );`

  Calculate a hash value. The parameter message specifies an array
  object that contains byte values to be hashed. Returns an array object.

## Acknowledgment

The core calculation routines of this class use "jsSHA.js".
See [jsSHA.readme.md](jsSHA.readme.md) for further information.

Special Thanks to Mr.Brian Turek!  
http://sourceforge.net/users/caligatio

// vim:ts=8:expandtab:
