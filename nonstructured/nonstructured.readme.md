
# A Framework for Non-Structure Oriented Programming

[nonstructured.js](nonstructured.js)

                                                            Titaniumcore Project

Copyright(c) 2008 Atsushi Oka [ http://oka.nu/ ]

## Introduction

This script [nonstructured.js](nonstructured.js) is a simple framework to write programs without
structure-oriented method.  It helps you to write programs only with simple
statements, that is, without many structure-oriented things such as nested
loop, function calling and so on. It might sound very ridiculous.  You might
ask "what the hell is it for!?".

Nowadays JavaScript/ActionScript has become very important. JavaScript is
heavily applied in various situations such as mathematical calculation, complex
data conversion, etc. The problem is its execution speed.

JavaScript has a strict limitation of execution speed. When a function
calling takes long time to return, most JavaScript's execution environments
such as web-browsers will look frozen. And after a certain period of time,
it show a message box that warns that the browser is running slower and ask
if the user want to quit the script or not.  This is not necessary when you
know that you are executing heavy process and this will look very awkward.

If you are using Java, you can use Thread object,too. Thread is a solution
for such situation.  But JavaScript does not have Thread.

My solution is to break-up your function. See below examples.

Ordinary way :
```javascript
for ( var i=0; i<10; i++ ) {
    for ( var j=0; j<10; j++ ) {
        for ( var k=0; k<10; k++ ) {
            trace( "i=" + i + " j=" + j + " k=" + k );
        }
    }
}
```

My idea :
```javascript
var i=0;
return function() {
    if ( i++<10 ) {
        var j=0;
        return function() {
            if ( j++<10 ) {
                var k=0;
                return function() {
                    if ( k++<10 ) {
                        trace( "i="+i + " j="+j + " k="+ k );
                        return true;
                    } else {
                        return false;
                    }
                }
            } else {
                return false;
            }
        }
    } else {
        return false;
    }
};
```

These two codes above have loosely same logical meaning.  The former example
is written in normal style.  The latter is written by using multi-nested
closure. There are some differences there. The former code will be fully
executed by one function calling so that it takes several minutes to return.
In the latter example, it will return immediately as it finishes only one
step.  And it is also necessary to call the closure until the function
returns false in order to finish entire procedure.

I named the latter style as "Nested Closure Oriented Programming".

In fact, nested closure oriented code looks very complicated and ugly but it
has some interesting advantages.  Because each function calling returns in a
very small amount of time so you will not get "slower script" alert anymore.
Once you convert your logic as nested closure oriented, it does not matter
how much time a function calling takes to finish its execution.  Your
program is done without slow-downing or freezing.

The purpose of this library ``nonstructured.js'' is to present some simple
protocol to the closures and free them from problems that you will face when
you try to implement common tasks such as accessing to shared scope, calling
function, passing parameters, returning a value and so on. They are usually
very complicated to implement in Nested Closure Oriented Programming. This
nonstructured.js manages these problems.

In fact, I wrote this framework in order to generate a 4096bit length RSA
key which requires a few hours to finish its entire process and luckily it
works for me.

I hope it works for you, too!

## DOWNLOAD

The newest version of nonstructured.js is available at the website.

http://oka.nu/titaniumcore/js/nonstructured/nonstructured.readme.txt

http://oka.nu/titaniumcore/js/nonstructured/nonstructured.20081230131226.zip

## LICENCE

nonstructured.js and its related files are distributed under the LGPL.

# ABOUT TITANIUMCORE.JS PROJECT

This script is a part of Titaniumcore.js project.
See http://ats.oka.nu/titaniumcore/js/readme.txt for further information.

## USAGE

Simply define a closure. Then call `ready()` method and call `go()` method.

Ex.1)
```javascript
var f=function() {
    trace( "hello" );
    return false;
};
f.ready().go();
```

This example will print "hello" and exit.

`ready()` method is available on every Function objects.  The `ready()` method
wraps the closure by an newly generated Nonstructured object and returns the
object. `go()` method is defined in the Nonstructured class.  When you call
`go()` method the Nonstructured object will automatically call the closure
until the closure returns a "`false`".  Nonstructured class uses `setInterval()`
JavaScript standard global function so it is executed asynchronously. You
can execute any other process.

Ex.2)
```javascript
var i=10;
var f=function() {
    trace( "hello" );
    return (i--<0);
};
f.ready().go();
```

The above method will be called 10 times.

>    ----------------------------------------------------------------------
>    Column : "What is Closure?" --- The term "closure" has various meanings
>    depends on the situation.  Some of articles use it as a scope, some
>    articles use it as a dynamically generated function.  This document
>    mainly uses the term "closure" as a Function object. Incidentally, it is
>    also called "method" especially when it is belonged to any specific
>    object.  This term originally came from Object-Oriented Programming. In
>    JavaScript, a function, a closure and a method each term signifies same
>    thing - an instance of JavaScript's Function class.
>    ----------------------------------------------------------------------

Ex.3)
```javascript
var f = [
    function() {
        trace( "hello!" );
        return false;
    },
    function() {
        trace( "hello!" );
        return false;
    },
    EXIT
];
f.ready().go();
```

You can also call `ready().go()` to an Array object. The closures in the Array
object will be called next to next as if it is a multi-statement in the
analogy of structured programming.


Basic idea of Nested-Closure-Oriented Programming is based on a new way to
implement statements. In this library, each closure is a single-statement
and each array is a multi-statement.

    Statement       -> A Function object
    Multi-Statement -> An Array object that contains multiple Function objects.

Nesting multiple-statement is also available.

```javascript
var f = [
    function() {
        trace( "hello!" );
        return false;
    },
    [
        function() {
            trace( "hello!" );
            return false;
        },
        function() {
            trace( "hello!" );
            return false;
        },
        EXIT
    ],
    function() {
        trace( "hello!" );
        return false;
    },
    EXIT
];
f.ready().go();
```

Don't forget to place the constant value EXIT to the end of a multi-statement. A
multi-statement will back to the first statement after the last statement
is executed unless you explicitly place the constant value EXIT.

## Closure Specification

This section describes "What A Closure Should be" in this framework.  The
protocol which a closure has to follow in this framework has two categories
returning values and parameters :

### 1. Returning Value

#### 1.1 Returning Value - Simple Values

```javascript
var f = function() {
    trace( "hello!" );
    return true; // << (A)
};
f.ready().go();
```

The returning value (A) can be anything. Boolean, Number, Function and so
on. The type of the returned value affects the flow of execution. This
framework defines simple protocol : returning boolean value "`false`"
declares that the current function need to be called more, and returning
boolean value "`true`" declares that current function does not need any
further calling anymore.

See this simplified example:

```javascript
// you create a closure as :
var i =0;
var f = function() {
    return i++ == 10;
}
```

```javascript
// the framework processes it as :
while ( ! f() ){
}
```

This framework processes closures loosely in this way.

Available returning values are :

    Boolean Values ( please refer a column "Boolean value in JavaScript" below )
        false     : Requires to continuously process the current closure.
        true      : Requires to quit processing the current closure.

    Constant Values
        CONTINUE  : Same as false.
        BREAK     : Same as true.

        EXIT      : Same as true. Used for Multi-Statement-Flow-Controlling.
                    Requires to exit from current multi-statement.

        AGAIN     : Same as true. Used for Multi-Statement-Flow-Controlling.
                    Requires to restart current multi-statement.

        LABEL(id) : Same as true. Used for Multi-Statement-Flow-Controlling.
                    It works as same as EXIT works unless label names match.
                    This is used for controlling nested multi-statement.

        * Multi-Statement-Flow-Controlling will be described later.

There are two categories for values to return.  User should use constant
values rather than Boolean values for readability unless you have any proper
reason not to use them for the sake of good manner programming.  Though it
is not responsible.

SHOULD NOT
```javascript
function(){
    return true;
},
function(){
    return true;
},
EXIT
```

SHOULD
```javascript
function(){
    return BREAK;
},
function(){
    return BREAK;
},
EXIT
```

SHOULD NOT
```javascript
function(){
    return i++<10 ? CONTINUE : BREAK;
}
```

SHOULD
```javascript
function(){
    return i++<10;
}
```

Just select simpler one.

#### 1.2 Returning Value - Functions / Arrays

Returning an Function object / an Array object has special meaning in this
framework.  When the closure returns an closure or an array, it will be
executed recursively.  This behavior is implemented in the analogy of
calling function in Structure-Oriented programming.  The flow will come back
to the caller when callee returns false value.

```javascript
// 1. define a subroutine as a closure.
var subroutine = function(scope,param,subparam) {
    trace( "hello,world! (subroutine)" );

    // 4. Retrieve the value 1729 on "ramanujan" field on the param object.
    var value = param.ramanujan;

    // 5. Set any value on the param object for returning value to callee.
    param.hardie = value * value;

    return BREAK;
};

func = [
    function( scope, param, subparam ) {
        trace( "hello,world! (calling)" );

        // 2. Set any value on the subparam object.
        subparam.ramanujan = 1729;

        // 3. Return the closure to be recursively called.
        return subroutine;
    },
    function( scope, param, subparam ) {
        trace( "hello,world! (returned)" );

        // 6. Retrieve the value 2989441 (==1729*1729) on the subparam object.
        var result = subparam.hardie;

        trace( result );
        return BREAK;
    },
    EXIT
];
```

The code above is an complete example of calling subroutine, passing
parameters, retrieving parameters and returning values.  More detail of
retrieving parameters and returning values are described in the next
section.

### 2. Parameters

A closure can have three parameters "scope", "param" and "subparam".

ex)
```javascript
var i =0;
var f = function( scope, param, subparam ) {
    return i++<10;
}
```

scope, param and subparam are references to objects.

    scope    -> Scope Object
    param    -> Parameter Object
    subparam -> Sub-Parameter Object

- Scope Object

  A Scope Object is a object which exists uniquely during the processing
  session. It is used for implementing global scope. Closures are able to
  share various data by setting / getting values on this Scope object.

- Parameter object

  A Parameter object is a object which keeps informations from the caller.
  If any values are set to this object by callee, they will be able to be
  referred by caller.


- Sub-Parameter object

  A Sub-Parameter object is a object which keeps informations which will be
  passed to callees. If any values are set to this object before returning a
  closure / an array as a subroutine, these value will be able to be referred
  from callees.


#### Column : Boolean value in JavaScript

JavaScript is not a strongly typed language. Values are
automatically converted by its context.  That is, "true" "false"
described above are not necessarily be Boolean object.

http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf

>     (quote)
>        9.2 ToBoolean - The operator ToBoolean converts its argument to
>        a value of type Boolean according to the following table :
>
>            Undefined
>               false
>            Null
>               false
>            Boolean
>               The result equals the input argument (no conversion).
>            Number
>               The result is false if the argument is +0, -0, or NaN;
>               otherwise the result is true.
>            String
>               The result is false if the argument is the empty string (its
>               length is zero); otherwise the result is true.
>            Object
>               true
>     (quote end)



## INJECTIONS

An injection is a method which is always available on any arbitrary object
and modifying its role as if it is another object.

A calling injection method on an object alters the role of the object as if
the object is a constant value which name is same as the injection's name.

For example,
```javascript
[
    function(){
        trace("EXIT");
        return ({}).EXIT();
    }
]
```

The example above works exactly same as the following example.
```javascript
function(){
    trace("EXIT");
    return EXIT;
}
```

Injections can be used in such situation :
```javascript
[
    function() {
        trace( "WANNA QUIT AFTER EXECUTING A SUBROUTINE" );
        return function() {
            trace( "EXECUTED" );
            return BREAK;
        }; // (1) <<<
    },
    function(){
        trace( "DON'T WANT EXECUTE HERE" );
        return EXIT;
    }
]
```

In this case, you will return a function in (1) but at the same time, you
also want to return constant EXIT. The function object that you return will
be treated as if it is a boolean value true so it will work as if it is
BREAK. (Please refer to ``Returning Value'') In such case, you can use
injection like below :
```javascript
[
    function() {
        trace( "WANNA QUIT AFTER EXECUTING A SUBROUTINE" );
        return function() {
            trace( "EXECUTED" );
            return BREAK;
        }.EXIT(); // This function object will be treated as if this
                  // object is a constant value EXIT.
    },
    function(){
        trace( "DON'T WANT EXECUTE HERE" );
        return EXIT;
    }
]
```

Injection methods are available on these five objects:

- `CONTINUE()`

  `CONTINUE()` method alter its object's role as if the object is a constant value `CONTINUE`.

- `BREAK()`

  `BREAK()` method alter its object's role as if the object is a constant value `BREAK`.

- `AGAIN()`

  `AGAIN()` method alter its object's role as if the object is a constant value `AGAIN`.

- `EXIT()`

  `EXIT()` method alter its object's role as if the object is a constant value `EXIT`.

- `LABEL(id)`  
  `IDENTIFY(id)`

  These two injections are used for labeling. Call `LABEL(id)` method to
  set an identifier on the specific object to indicate which
  statement/multi-statement will be flow-controlled by this injected
  object.  This is called "labeling". Please refer to 1.2.3 Labeling.

## MULTI-STATEMENT

You can define a multi statement in this way.
```javascript
var f = [
    function() {
        return BREAK;
    },
    function() {
        return BREAK;
    }
];
f.ready().go();
```

When you define a multi statement, there are some points you have to notice.

### 1. Multi-Statement-Flow-Controlling

#### 1.1 BREAK AND CONTINUE

```javascript
var f = [
    function() {
        trace( "1" );
        return CONTINUE;
    },
    function() {
        trace( "2" );
        return BREAK;
    }
];
f.ready().go();
```

In a multi statement, the execution statement will stay in the same
statement when the closure returns `CONTINUE`.  This example outputs only "1"
and will not stop. If a closure returns `BREAK`, the execution statement will
advance to the next statement.


#### 1.2 EXIT, AGAIN

```javascript
var f = [
    function() {
        trace( "1" );
        return BREAK;
    },
    function() {
        trace( "2" );
        return BREAK;
    }
];
f.ready().go();
```

When you execute this example, you will get a result that 1,2,1,2... and
will not stop. This means that in nonstructured.js, multi-statement is
repeatedly executed unless a closure returns a constant value EXIT
explicitly.

##### 1.2.1 EXIT

```javascript
var f = [
    function() {
        trace( "1" );
        return EXIT;
    },
    function() {
        trace( "2" );
        return BREAK;
    }
];
f.ready().go();
```

Returning `EXIT` requests the multi-statement to exit to any upper statement.
This example outputs only "1" and stop.

##### 1.2.2 AGAIN

```javascript
var f = [
    function() {
        trace( "1" );
        return BREAK;
    },
    function() {
        trace( "2" );
        return AGAIN;
    }
    function() {
        trace( "3" );
        return BREAK;
    }
];
f.ready().go();
```

Returning `AGAIN` requests the multi-statement to restart from the first statement.
This example outputs "1","2","1","2"...  and will not stop.

##### 1.2.3 Labeling

```javascript
var f = [
    function() {
        trace( "1.1" );
        return BREAK;
    },
    [
        function() {
            trace( "2.1" );
            return BREAK;
        },
        [
            function() {
                trace( "3.1" );
                return BREAK;
            },
            function() {
                trace( "EXIT" );
                return EXIT;
            }
            function() {
                trace( "3.2" );
                return BREAK;
            }
        ],
        function() {
            trace( "2.2" );
            return BREAK;
        }
    ],
    function() {
        trace( "1.2" );
        return BREAK;
    }
];
f.ready().go();
```

This example returns `EXIT` in the third nested multi statement. The example
outputs "1.1", "2.1", "3.1", "2.2", "1.2" and stop. Though in some
situation, it is required to exit from nested multiple statement. In such
case, you can use labeling.

```javascript
var f = [
    function() {
        trace( "1.1" );
        return BREAK;
    },
    [
        function() {
            trace( "2.1" );
            return BREAK;
        },
        [
            function() {
                trace( "3.1" );
                return BREAK;
            },
            function() {
                trace( "EXIT" );
                return LABEL("SECOND").EXIT(); // this specifies label name to the returning value .
            }
            function() {
                trace( "3.2" );
                return BREAK;
            }
        ],
        function() {
            trace( "2.2" );
            return BREAK;
        }
    ].IDENTIFY("SECOND"), // this specifies label name on the multi-statement.
    function() {
        trace( "1.2" );
        return BREAK;
    }
];
f.ready().go();
```

This example outputs "1.1", "2.1", "3.1", "1.2" and stop.

###### 1.2.3.1 LABEL(id)

`LABEL(id)` is a global function. It is also available as an injection. It
give the returning value an identifier that specifies which multi-statement
will be affected by the returning value. This returning value will be
treated as if it is a constant value EXIT when the identifier does not match
with the identifier of a multi-statement. If and only if these label names
are identical, it will be treated as a normal returning value.

###### 1.2.3.2 IDENTIFY(id)

`IDENTIFY(id)` is only available as an injection. It specifies an identifier on
the multi-statement.

## REFERENCE

### GLOBAL

- `Nonstructured`

  The core class for this framework.
  Though user usually do not have to generate or refer this class.

- `CONTINUE`

  This is a constant value for result of a closure.  Any closure should
  return this value when the closure still needs to be called
  continuously.

- `BREAK`

  This is a constant value for result of a closure.  Any closure should
  return this value when the closure does not need to be called anymore.

- `AGAIN`

  This is a constant value for flow-controlling on a multi-statement.
  When a closure returns this value, the multi-statement that belongs the
  closure restarts from the first statement. If a closure returns this
  value when it is not belonged by a multi-statement, it works same as
  `BREAK`.

- `EXIT`

  This is a constant value for flow-controlling on a multi-statement.
  When a closure returns this value, the multi-statement that belongs the
  closure exits from execution. If a closure returns this value when it is
  not belonged by a multi-statement, it works same as BREAK.

- `LABEL(id)`

  This is not a constant value but a function.  This function is used for
  flow-controlling of nested-multi-statement.

  It generates a newly created object for flow-controlling.  When a
  closure returns the value, it will affect only for the specific
  multi-statement which has the same label name.  It is always used with
  "injections" that is described below.

- `FOR()`

  This is not a constant value but a function. The purpose of this
  closure is  for implementing a for statement in the Nested Closure
  Oriented Programming.

  `FOR()` method takes three parameters "variable", "condition" and "loop".

  - `variable`  : give an initialized object to this parameter.
  - `condition` : give a closure with one parameter that checks loop condition.
  - `loop`      : give a closure with one parameter that processes increment/decrement task.

### INJECTIONS

- `CONTINUE()`

  `CONTINUE()` method alter its object's role as if the object is a constant value CONTINUE.

- `BREAK()`

  `BREAK()` method alter its object's role as if the object is a constant value BREAK.

- `AGAIN()`

  `AGAIN()` method alter its object's role as if the object is a constant value AGAIN.

- `EXIT()`

  `EXIT()` method alter its object's role as if the object is a constant value EXIT.

- `LABEL(id)`

  Give the object an identifier for labeling.

- `IDENTIFY(id)`

  Give the object an identifier for labeling.

### WRAPPER

- `Function.ready()`  
  `Array.ready()`

    This `ready()` method is always available on Function object and Array
    object.  It wraps the object by a newly generated Nonstructured
    object. Returns the newly generated Nonstructured object.

### THE METHODS OF NONSTRUCTURED OBJECT

- `go()`

  This method executes the closure asynchronously. Execution is
  implemented by using timer. Nonstructured object calls a closure in
  every specific period is passed. When frequency property is equal or
  less than zero, it will synchronously execute and will not return
  until entire process is finished.

- `limit( value )`

  Manage "`limit`" property. This method sets maximum call count to the
  object. It returns the object itself.  If no parameter is specified,
  it will return current value.

- `frequency( value )`

  Manage "`frequency`" property. This method sets execution
  frequency by millisecond to the Nonstructured object and returns this
  object.  If no parameter is specified, it will return the current
  value.

- `timeout( value )`

  Manage "`timeout`" property. This method sets maximum elapse time
  of each timer calling  to the Nonstructured object and returns this
  object.  If no parameter is specified, it will return the current
  value.

- `done( closure )`

  Manage "`done`" property. This method specifies the procedure that
  will be executed when the whole process is done. If no parameter is
  specified, it will return the current value.

  ```javascript
  var f = function() {
      trace( "1" );
      return BREAK;
  };
  var doneProc = function( succeeded, count, elapse, startTime, finishTime ) {
      trace( "done!" );
  };

  f.ready().done( doneProc ).go();
  ```

  This example outputs  "1", "done!" and stops.

  The closure can have parameters below :

  - `succeeded`

    `True` if the process successfully finished. `False` if the
    process terminated by any error.

  - `count`

    The count how many steps was executed to finish the whole process.

  - `elapse`

    Elapsed time in milliseconds.

  - `startTime`

    A Date object contains the time when the process was started.

  - `finishTime`

    A Date object contains the time when the process was finished.


- `progress( closure )`

  Manage "`progress`" property. This method specifies the procedure that
  will be executed when a step is done. If no parameter is specified,
  it will return the current value.

  The closure can have a parameter below :

  - `count`

    The current count which the object executed so far.

## CONTACT

The author of this nonstructured.js is Atsushi Oka http://oka.nu/.
Please check the site to get the newest version of this library.

// vim:ts=8:sw=4:expandtab:
