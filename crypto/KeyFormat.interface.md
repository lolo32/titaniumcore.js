
# interface KeyFormat

                                                            Titaniumcore Project

Atsushi Oka [ http://oka.nu/ ]                                      Jan 15,2009

This document describes about KeyFormat interface.
For further information of the interface files, see [readme.interface.md](readme.interface.md).

```cpp
/**
 * KeyFormat interface.
 * Converts RSA public/private keys into byte arrays and vice versa.
 */
interface KeyFormat {
    byte[] encodePublicKey( BigInteger n, int e, int ksize );
    byte[] encodePrivateKey( BigInteger n, int e, BigInteger d, int ksize );
    Key decodePublicKey( byte[] value );
    Key decodePrivateKey( byte[] value );
}

interface Key {
    BigInteger n;
    int e;
    BigInteger d;
    int ksize;
}
```

// vim:expandtab:
