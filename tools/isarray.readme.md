
# isarray.js

                                                            Titaniumcore Project

Atsushi Oka [ http://oka.nu/ ]                                       Jan 11,2009

[isarray.js](isarray.js) is a tool to distinguish an Array object from other objects.
"`typeof`" operator is insufficient because it tells only it is an object or
not.

It simply does :

```javascript
Array.prototype.isArray=true;
```

This is extremely simple. It make us feel that it is not necessary to be a
library.  But adding properties to prototype produces an impact to entire
program and it produces dependency.

Management of dependency is very important.  As programs becomes bigger,
dependency becomes more complicated, too.  If you do not manage this
dependency, the program becomes more difficult to modify as it grows larger.
Finally it will come to impossible to modify.

isarray.js uses packages.js to trace these dependency during the referrers
of this library.


## Author

Copyright(c) 2009 Atsushi Oka [ http://oka.nu/ ]  
This script file is distributed under the LGPL
