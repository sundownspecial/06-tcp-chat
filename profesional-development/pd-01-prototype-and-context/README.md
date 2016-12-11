# 401 JS -- Prototype and Context

# Context resources
* Read [MDN this]
* Watch the Video [Javascript Context Tutorial]

# Prototype resources
* Skim [MDN new]
* Skim [MDN Object prototype]
* Read [MDN inheritance and the prototype chain]

# Learning Objectives
* understand the difference between context and scope
* learn how to configure a functions context
* understand the roll context plays in constructor functions

# Overview
* when a function has a `this` we say that `this` is the functions context
* unlike scope a functions context can be configured
* If a function is not a property on an object, by default it has no context
* If a function is a property on an object, by default that object is the context for that function
* `call`, `bind`, and `apply` are function prototype methods that allow us to change the context of a function
* `call` is a methods on a function that invokes a function with a specified context and argments  
 * `call` passes comma seporated argments
``` javascript
// invoke the function getCoolStuff
// set the object state to be getCoolStuff's context
// pass 'hello' and 'wolrd' as getCoolStuff's argments
var state = {};

getCoolStuff.call(state, 'hello', 'world');
```  
* `apply` is a methods on a function that invokes a function with a specified context and argments  
 * `apply` passes argments from an array  
``` javascript
// invoke the function getCoolStuff
// set the object state to be getCoolStuff's context
// pass 'hello' and 'wolrd' as getCoolStuff's argments
var state = {};

getCoolStuff.apply(state, ['hello', 'world']);
```   
* `bind` is a methods on a function that returns a new function with a specified context
``` javascript
// create a function getCoolState that acts 
// like getCoolStuff thke state as its context

var state = {};
var getCoolState = getCoolStuff.bind(state);

getCoolState('hello', 'world');
```   
* Arrow functions inherit their context from their parent context
``` javascript
describe('lulwat', function(){
  this.example = 'some data';

  // it's arrow function has the context of the callback from the describe block
  it('example should be "some data"', (done) => {
    expect(this.example).to.equal('some data');
    done();
  })
})
```
* variables can be assigned to arrow functions  
``` javascript
var doubleIt = num => num * 2;

doubleIt(16);
// 32
```
* The `new` keyword creates a empty object and invokes a function with that object as its context
* For constructor ABC to inherit constructor XYZ's property it should `XYZ.call(this)` 
* For constructor ABC to inherit constructor XYZ's methods it should `ABC.prototype = Object.create(XYZ.prototype)`
``` javascript
function XYZ(shape){
  this.name = 'XYZ';
  this.size = 10;
  this.shape = shape;
}

XYZ.prototype.describe = function(){
  return this.name + ': ' + this.shape;
}

function ABC(shape, color){
  XYZ.call(this, shape);
  this.name = 'ABX'; // overwrite XYZs inherited name
  this.color = color;
}

ABC.prototype = Object.create(XYZ.prototype);
ABX.prototype.constructor = ABC;

ABC.prototype.changeColor = function(color){
  this.color = color;
}

let redBall = new ABC('ball', 'red');

// {
//   name: 'ABC',
//   size: 10',
//   shape: 'ball',
//   color: 'red'
//   __proto__: {
//     changeColor: function(color){
//       this.color = color;
//     },
//     __proto__: {
//       describe: function(){
//         return this.name + ': ' + this.shape;
//       }
//       __proto__: Object.prototype
//     }
//   }
// }

```
* dont mess with `__proto__` its slow **not even to read a property**
* if you want to determin an objects prototype use `Object.getPrototypeOf(someObject)`
``` javascript
var prototype = {};
var color = Object.create(prototype);
Object.getPrototypeOf(color) === prototype; // true
```
* don't nest a lot of prototype's it will have a speed impact on your code
* if you look up a property that is not on any object on the prototype chain it will still look through the whole prototype chain

# Exercise
* Write out the bdd framework from the class lecture
* Add `beforeEach` and `afterEach` functionality
* **SUPER BONUS**: 
 * refactor your bdd framework to support async `it`, `before`, `after`, `beforeEach`, and `afterEach` callbacks using a `done` callback, just like mocha!

[MDN new]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new

[MDN Object prototype]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype 

[MDN inheritance and the prototype chain]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain

[MDN this]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this

[Javascript Context Tutorial]: https://www.youtube.com/watch?v=fjJoX9F_F5g
