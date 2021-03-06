
# A RSA Key Binary Encoder/Decoder

                                                            Titaniumcore Project

Atsushi Oka [ http://oka.nu/ ]                                        Jan 8,2009

RSAKeyFormat.js is a library to encode/decode a RSA key into specific
format.  There can be a lot of expressions which expresses A RSA key.
This library defines a schema to express a RSA key as following :

            PRIVATE KEY                PUBLIC KEY          
                                                           
            NAME       TYPE            NAME       TYPE     
    0x0000  ------------------         ------------------  
            keysize    int             keysize    int      
    0x0004  ------------------         ------------------  
            sizeof(n)  int             sizeof(n)  int      
    0x0008  ------------------         ------------------  
                                                           
               n       byte[]             n       byte[]   
                                                           
            ------------------         ------------------  
            sizeof(e)  int             sizeof(e)  int      
            ------------------         ------------------  
                                                           
               e       byte[]             e       byte[]   
                                                           
            ------------------         ------------------  
            sizeof(d)  int                                 
            ------------------                             
                                                           
               d       byte[]                              
                                                           
            ------------------                             

All integer values are stored in big endian byte order.

This RSAKeyFormat.js defines the RSAKeyFormat class. The RSAKeyFormat class
implements KeyFormat interface. For further information, see
[KeyFormat.interface.md](KeyFormat.interface.md).

## Link
```html
<script src="./tools/packages.js"></script>
<script src="./cipher/BigInteger.init1.js"></script>
<script src="./tools/binary.js"></script>
<script src="./cipher/SOAEP.js"></script>
<script src="./cipher/RSAKeyFormat.js"></script>
```

## Import
```javascript
var RSAKeyFormat = __import( this, "titaniumcore.crypto.RSAKeyFormat" );
```

## Note

This class is a static class. Do not instantiate this class.

// vim:expandtab:
