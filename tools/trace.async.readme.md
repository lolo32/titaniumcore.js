
# trace.async.js

                                                            Titaniumcore Project

Atsushi Oka [ http://oka.nu/ ]                                       Jan 11,2009

## Introduction

[trace.async.js](trace.async.js) is designed to convey compatibility to ActionScript on
Flash.  ActionScript has `trace()` function. This function is very useful for
debugging. Web browser does not have it,

## Example

[trace.async.html](trace.async.html)

## Usage

```javascript
trace( "hello" );
```

This creates a new popup window and displays the message on it.

This method has less overhead since it process messages asynchronously.
Messages will be stored to a queue once and then will be processed
asynchronously with a timer that `setInterval()` function generates.

# Author

Copyright(c) 2009 Atsushi Oka [ <a href="http://oka.nu/">http://oka.nu/</a> ]

This script file is distributed under the LGPL
