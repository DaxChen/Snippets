# Safe and Secure JavaScript

Things from Douglas Crockford:
1. https://www.youtube.com/watch?v=DwYPG6vreJg&feature=youtu.be&t=27m13s


## Singleton
Global variables are eval. How to have private variables and private functions?

```javascript
var singleton = function() {
   var privateVar;
   function privateFunc() {
      ...privateVar...
   }

   return {
      firstMethod: function(a, b) {
         ...privateVar...
      },
      secondMethod: function(c) {
         ...privateFunc()...
      }
   };
}();
```

Notice the var `singleton` is not holding a function, but the result (return) of the self-executing function.

These are **Privilaged Methods**, they are functions thay have access to secret informations, private things.

## Power Constructor

Put the singleton module pattern in constructor function, and we have a power constructor pattern.

1. Make a new object somehow.
1. Augment it.
1. Return it.

```javascript
function powerConstructor() {
   // just use anything that will make/return a object, eg. that = {}
   var that = object(oldObject), 
      privateVariable;
   function privateFunction() {}

   that.firstMethod = function(a, b) {
      ...privateVariable...
   }
   that.secondMethod = function(c) {
      ...privateFunction()...
   }
   return that;
}

myObject = powerConstructor();
```


