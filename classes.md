# Creating Class in JavaScript

## Constructors

Since `function`s are also `Object`s in JavaScript, so you can add **properties** to it.  
In convention, we capitalize the first character to indicate this function is a 
constructor of a class, and should be used with a `new` keyword.

```javascript
function Bear(options) {
  if (!(this instanceof Bear)) {
    return new Bear(options);
  }
  options = options || {};
  this.type = options.type || 'normal';
  this.xxxx = options.xxxx || 'some default value';
}
```

The `if (!(this instanceof Bear))` lets the user create a instance of this class with two ways:
1. `var polarBear = new Bear({xxxx: 'polar'});`
1. `var grizzlyBear = Bear({xxxx: 'grizzly'});`

## Add methods to class

Do not write `Bear.growl = function() {}`, this will only affect the Bear object, not the created instances.  
Instead, use `prototype`s!

```javascript
Bear.prototype.growl = function() {
  console.log('The' + this.type + 'bear says' + this.says);
}
```

But what if we want to chain methods like `polarBear.growl().eats().walks();`?  
Simply `return this;`

## Prototypal Inheritance

Lets make a Grizzly class that inherits the Bear class.

```javascript
function Grizzly() {
  // to call Bear's constructor
  Bear.call(this, 'grizzly');
}

Grizzly.prototype = Object.create(Bear.prototype);

var grizzly = new Grizzly();
```

Now, if you call `grizzly.growl()`, how does it know which one to call?
Well, this is known as the **Prototype chain**.
It checks from the child up! Which means:

1. Does the instance have that method? `grizzly.growl = function() {};`
1. If not, if the Grizzly class (prototype) has that method? `Grizzly.prototype.growl = function() {}`
1. If not, just keep going up the chain, is it on Bear's prototype?


