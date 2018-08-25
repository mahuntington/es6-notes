# ES6 - The Future Is Now! (Sort of)

## Lesson Objectives

1. What is ES6
1. strict mode
1. understanding MDN
1. polyfills and shims
1. babel/transpiling
1. IFFE
1. Block level scoping with let
1. const
1. arrow functions
1. Classes
1. for...of
1. default value setting
1. Array.isArray()
1. argument object
1. spread and rest operators
1. Trailing commas
1. Template Literals
1. Object literal extensions
1. Destructuring
1. swap (desconstucturing method)
1. Merge objects
1. Array functions

<!--  other options:

1. symbols
1. helper functions (trunc, entries, values, keys)
1. unicode
1. Maps/Sets, WeakMaps/WeakSets
1. proxies
1. promises
1. generators
1. async/await

-->

## What is ES6   

ECMAScript is the standard version of JavaScript. ECMA stands for European Computer Manufacturers Association. ES5 became standard in 2009. ES6 (or ES2015), became standard in 2015. But just because it is made standard, it doesn't mean that all browsers/environments are ready/compliant.

## Strict Mode

Adding 'use strict' to the top of your code, (see Babel screenshot) will let you opt in to some newer features of JS! [More info](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)

## understanding MDN

- Lets you know which methods/syntaxes are standard, experimental, depreciated, when things were added and more including shims/polyfills.
    - [forEach on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
- Quickly see if a method is standard[no notation], experimental[flask], depreciated (removed from standards, may be removed)[thumbs down], or obsolete (likely removed altogether in newer environments)[trash can]

    ![Screenshot left bar, objects](https://i.imgur.com/nSqSXwf.png)

- See more information about when the method was defined implemented:

    ![Screenshot- forEach Specifications section](https://i.imgur.com/7bPC29H.png)

## Polyfills/Shims

Some people call them polyfills, some call them shims; either way: It is code that is a fallback that provides the functionality when the expected functionality is not available (because old browser, etc. ). Many entries in MDN have polyfills. This is also a great way to look at how the ECMAScript standard code is written (a lot of examples are directly from the official ECMAScript docs)

An example Polyfill:

![Screenshot- forEach polfill section](https://i.imgur.com/e0G8ZCB.png)

## babel/transpiling

Now! But it depends where. Node.js supports most/nearly all new things. [Chrome, Firefox, Edge, and Safari have been keeping up well](http://caniuse.com/#search=es6). But new features are being added and tested. To be sure that code will run across many platforms (never forget the people who still use IE8), many people have come to rely on transpilers/compilers. Transpilers/Compilers take 'dialects' of standard code (incuding ES6, coffeescript, typescript and more) and convert the code for you. A popular one for ES6 is [Babel](https://babeljs.io/), they have both repl (Read, Evaluate, Print, Loop) and an npm module/gem.

 ![babel repl](https://i.imgur.com/SKLMkIU.png)

 You can also run babel from within the command line, but most often it's used a build tool like [webpack](https://webpack.js.org/).  First install:

 ```
 npm init
 npm install babel-cli
 npm install babel-preset-es2015
 ```

 Now in your `package.json` add this into `"scripts"`:

 ```
 "scripts": {
   "build": "babel --presets es2015 test.js -o compiled.js"
 },
 ```

Now create a `test.js` file:

```
touch test.js
```

In it, write some ES6:

```JavaScript
class Car {
    run(){
    }
}
```

Now run `npm run build` and take a look at compiled.js:

```JavaScript
"use strict";

var _createClass = function () { function defineProperties(target, props) { for (var i = 0; i < props.length; i++) { var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false; descriptor.configurable = true; if ("value" in descriptor) descriptor.writable = true; Object.defineProperty(target, descriptor.key, descriptor); } } return function (Constructor, protoProps, staticProps) { if (protoProps) defineProperties(Constructor.prototype, protoProps); if (staticProps) defineProperties(Constructor, staticProps); return Constructor; }; }();

function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

var Car = function () {
	function Car() {
		_classCallCheck(this, Car);
	}

	_createClass(Car, [{
		key: "run",
		value: function run() {}
	}]);

	return Car;
}();
```

That's a spicy meatball!

If you want, you can adjust `package.json` to re-compile every time you make a change:

```
"scripts": {
  "build": "babel --watch --presets es2015 test.js -o compiled.js"
},
```


## IIFE

Normally, variable declarations, are "hoisted" up to the top of whatever function they are declared in (or global if no function exists).  This can lead to some weird moments:

```JavaScript
var a = 2;

if(true){
    var a = 4;
}

console.log(a); //in many programming languages, this would be 2
```

and a famous interview question:

```JavaScript
for(var i=0; i<5; i++){
    setTimeout(function(){
        console.log(i) //you'd expect 0,1,2,3,4
    },500 * i);
}
```

IIFE - Immediately Invoked Function Expression - often used to create a block of scope. This is not an ES6 thing, but rather an example of an old way to get block scoping.

```JavaScript
var a = 2;

(function(){
    var a = 4;
    console.log ('Inside the IFFE, the value of a is', a ); //"Inside the IFFE, the value of a is 4"
})();

console.log ( 'Outside the IFFE, the value of a is', a ); //"Outside the IFFE, the value of a is 2"
```

And now the loop with setTimeout:

```JavaScript
for(var i=0; i<5; i++){
  (function(i){
    setTimeout(function(){
      console.log(i)
    },500 * i);
  })(i)
}
```

## Block Scoping with let

A new way to get block level scoping is with the keyword let, and the use of `{}` as in any `{}`, not just tied to a function! Note: `let` is NOT meant to completely replace `var`!

```JavaScript
var a = 2

{
    let a = 4
    console.log( 'the value of b inside the `{}` is', a); //"block level scope"
}

console.log ('the value of b outside of the `{}` is', a) //"global scope"
```

And now the loop with setTimeout:

```JavaScript
for(let i=0; i<5; i++){
    setTimeout(function(){
        console.log(i) //you'd expect 0,1,2,3,4
    },500 * i);
}

console.log(i); //notice i is not defined outside of the loop
```

## Const

Another new way to declare a variable. This will block the value from being reassigned.

```JavaScript
const pi = 3.14;
 pi = "pie" //TypeError: Assignment to constant variable.

const whatsForLunch = [ "Nuts", "Coffee" ]
whatsForLunch.push( "Yogurt" ) //you can still call object methods that alter the variable
console.log( whatsForLunch)
whatsForLunch.shift();  //you can still call object methods that alter the variable
console.log( whatsForLunch )
whatsForLunch = ["salad"]; //TypeError: Assignment to constant variable.
```

## Arrow Functions

A new way to write a function expression (arrow functions are always anonymous functions), with fun shortcuts:

### Shorthands

Basic:

```JavaScript
var basicES5 = function(){
    return 'oh hai';
}

//take out the keyword 'function' and add in '=>'
var basicES6 = () => {
    return 'oh hai'
}
```

If the body of the function is just one expression that gets returned, we can remove the `{}` and ES6 knows to return whatever comes after the `=>`

```JavaScript
var basicES6 = () => 'oh hai'
```

Multiple arguments:

```JavaScript
//ES5 - Multiple arguments
var sumES5 = function (a, b){
    return a+b;
}

console.log('ES5 sum:',sumES5(5,5));

//ES6 - Multiple arguments
var sumES6 = ( a, b ) => a + b;

console.log('ES6 sum:', sumES6(6,6));
```

If we only have one argument to the function, we can leave out the `()`

```JavaScript
//ES5 - one argument
var squareES5 = function (c){
    return c*c;
}
console.log('ES5 square:', squareES5(5));

//ES6 - one argument
var squareES6 = c => c*c
console.log('ES6 square:',squareES6(6));
```

### Binding

with callbacks, `this` can get redefined:

```JavaScript
function Person(name){
    this.name = name;
    this.logName = function(){
        setTimeout(function(){
            console.log(this.name);
        },500)
    }
}
var me = new Person('Matt');
me.logName();
```

arrow functions fix this:

```javascript
function Person(name){
    this.name = name;
    this.logName = function(){
        setTimeout(()=>{
            console.log(this.name);
        },500)
    }
}
var me = new Person('Matt');
me.logName();
```

## Support for Classes

```JavaScript
class Cat {
    static getTypes(){
        return ['large', 'small'];
    }
    constructor(name) {
        this.name = name;
    }
    makesNoises() {
        return (this.name + 'meows');
    }
    purrs(){
        return (this.name + 'purrs');
    }
}

class Lion extends Cat {
    makesNoises() {
        return (this.name + 'roars!');
    }
    eatHuman(){
        return 'yum';
    }
}

var myCat = new Cat("Kitty");
console.log(myCat.makesNoises());
console.log(myCat.purrs());

var myLion = new Lion("Simba");
console.log(myLion.makesNoises());
console.log(myLion.purrs());
console.log(myLion.eatHuman());

console.log(Cat.getTypes());
```

This makes inheritance much easier than how it used to be:

```javascript
var Person = function(){
    this.greet = function(){
        console.log('oh hai!');
    }
}

var SuperHero = function(){
    Person.call(this)
    this.fly = function(){
        console.log('up, up, and away!');
    }
}
SuperHero.prototype = Object.create(Person.prototype);
SuperHero.prototype.constructor = SuperHero;
superman.greet();
```

## for...of

Until now, the best way to loop through an array was either:

```JavaScript
var array = [1,4,6];
for(var i =0; i < array.length; i++){
    console.log(array[i]);
}
```

Or:

```JavaScript
var array = [1,4,6];
array.forEach(function(element){
    console.log(element);
});
```

But now we can do:

```JavaScript
var array = [1,4,6];
for(let element of array){
    console.log(element);
}
```

## Default values

When creating a constructor and you wanted a default value, you previously had to write something like this:

```JavaScript
var multBy2 = function(value){
    var defaultValue = (value)?value:0;
    return defaultValue * 2
}

console.log(multBy2());
```

Now you can do this in ES6

```JavaScript
var multBy2 = function(value = 0){
    return value * 2
}

console.log(multBy2());
```

## Array.isArray()

If you have tried to confirm if something is an array with ES5, you may have found it frustrating:

```JavaScript
var arr = [1,2,3];

console.log(typeof arr); //'object'
```

You could do

```JavaScript
arr instanceof Array; // true
```

But now there is a new method!

```JavaScript
Array.isArray(arr); //true
```

## Argument Object

This isn't an ES6 thing, but should help expand your knowledge of arguments in JS.

The arguments object is an array-like object (but not fully an array, so you cannot always call array methods on it). It allows for a lot of flexibility in the number of arguments a function can take:

```JavaScript
function sum (){
    var sum = 0;
    for(var i = 0; i < arguments.length; i++ ){
        sum += arguments[i];
    }
    return sum;
}

console.log( sum(1,2,3,4) ); //10
```

## Spread and Rest Operators

### Spread operator:

Spread Operator will take an array, and convert each element into its own value.  It "spreads" the elements out into values

Let's look at `Math.max()` which takes any number of arguments and finds the maximum value

```JavaScript
//Standard syntax
console.log(Math.max(1,5,-6)); // 5
```

- But what if you have an array of values?
- Converting each to a variable and then adding them as arguments is a pain, until now:

```JavaScript
var z = [1,5,-2,8,-9,17,-22];
console.log(Math.max(...z)); // 17
```

### Rest operator:

The rest operator gathers many values into an array.  It's basically the opposite of spread.

```JavaScript
function returnOnlyNums(...arrayParam){
    var nums = arrayParam.filter(arg => typeof arg === 'number');
    return nums;
}

console.log( returnOnlyNums(44, false, 'pizza', 45, {season: "winter"}, [1,2,3,4,5,], 2, 9) ); // [ 44, 45, 2, 9 ]
```

## Trailing commas

You can add trailing commas to make copy/paste easier later on:

```javascript
var arr = [
  1,
  2,
  3, //the comma here will not cause issues
];

console.log(arr);
```

If you have multiple commas, it will create holes in the array:

```javascript
var arr = [
  1,
  2,
  3,,,,
];

console.log(arr);
console.log(arr.length); //logs 6
```

Trailing commas are okay in objects too:

```javascript
var object = {
  foo: "bar",
  baz: "qwerty",
  age: 42,
};

console.log(object);
```

## Template Literals (String Interpolation)

Template literals allow you to insert variables into strings with out having to use `+`

The old way:

```JavaScript
var name = "Matt"
console.log ('Hello '+ name);
```

Now we can do this:

```JavaScript
var name = "Matt"
console.log (`Hello ${name}.`);
```

You can even evaluate JavaScript within the Template Literal:

```JavaScript
var name = "Matt";

function yell(value){
    return value.toUpperCase();
}

console.log (`Hello ${yell(name)}!`);
```

Multiple lines used to suck:

```JavaScript
var adjective = 'awesome'
var template = 'I wrote a haiku\n'+
'This is so very ' + adjective + '\n'+
'Now I am finished';

console.log(template);
```

Now they're easy!

```JavaScript
var adjective = 'awesome';
var template = `I wrote a haiku
This is so very ${adjective}
Now I am finished`;

console.log(template);
```

## Object literal upgrades

Instead of:

```JavaScript
var a = 8;
var b = 9;
var c = 10;

var es5obj = {
    a:a,
    b:b,
    c:c,
    d : function (){
        console.log(this.a, this.b, this.c);
    }

}
es5obj.d();
```

Now do:

```JavaScript
var a = 8;
var b = 9;
var c = 10;

var es6obj = {
    a,
    b,
    c,
    d(){
        console.log(this.a, this.b, this.c);
    }
}
es6obj.d();
```

## Destructuring

Destructuring pulls variables out of their "structure" (array or object)

### Array Destructuring

You can assign values an array to variables easily:

```JavaScript
var a, b;
[a,b] = [10,12]
console.log(a);
console.log(b);
```

You can also declare in the same line as assignment:

```JavaScript
var [a,b] = [10,12]
console.log(a);
console.log(b);
```

You can ignore certain values:

```JavaScript
var a, b;
[a,,b] = [1,2,3,4];
console.log(a);
console.log(b);
```

You can also use a rest operator to gather variables into an array after a certain point:

```JavaScript
var a, b, rest;
[a,b,...rest] = [10, 20, 30, 40, 50];
console.log(a);
console.log(b);
console.log(rest);
```

You can set defaults as well:

```JavaScript
var a, b;

[a=5, b=7] = [1];
console.log(a); // 1
console.log(b); // 7
```

### Object Destructuring

Pull variables out of an object

```JavaScript
var p, q;

{p, q} = {p: 42, q: true};

console.log(p); // 42
console.log(q); // true
```

You can also change the names of the variables:

```JavaScript
var o = {p: 42, q: true};
var {p: foo, q: bar} = o;

console.log(foo); // 42
console.log(bar); // true
```

You can also do default values:

```JavaScript
var {a = 10, b = 5} = {a: 3};

console.log(a); // 3
console.log(b); // 5
```

You can pull values from a function argument that's an object:

```JavaScript
var user = {
    id: 42,
    displayName: 'jdoe',
    fullName: {
        firstName: 'John',
        lastName: 'Doe'
    }
};

function userId({id}) {
    return id;
}

console.log('userId: ' + userId(user)); // "userId: 42"
```

Use this for default options arguments:

```JavaScript
function drawES2015Chart({size = 'big', cords = {x: 0, y: 0}, radius = 25} = {}) {
    console.log(size, cords, radius);
}

drawES2015Chart({
    cords: {x: 18, y: 30},
    radius: 30
});
```

If you have a property name which is not a valid variable name, you can reassign it:

```JavaScript
const foo = { 'fizz-buzz': true }
const { 'fizz-buzz': fizzBuzz } = foo
console.log(fizzBuzz);
```

Lastly, you can compute a variable name:

```JavaScript
let key = 'z';
let {[key]: foo} = {z: 'bar'};

console.log(foo); // "bar"
```

## Swap values: File under `Destructuring Assignment`

If you wanted to swap the value of x and y with es5, you had to do

```JavaScript
var x = true;
var y = false;
var temp;

temp = x;
x = y;
y = temp;
console.log(x,y);
```

The new way will return x and y to original values in this case

```JavaScript
var x = true;
var y = false;

[x,y] = [y,x];

console.log(x,y);
```

## Merge objects

You can use `Object.assign()` to merge properties of two or more objects:

```javascript
var a = {
    foo:'bar'
}

var b = {
    awesome:true
}

Object.assign(a, b);

console.log(a);
```

This is extremely useful when you need to create a brand new object that is a clone of another one with some minor changes (happens all the time with Redux):

```javascript
var bob = {
    name:'Bob',
    age: 42
}

var sally = Object.assign({}, bob, {name:'Sally'})

console.log(sally);
```

## Array functions

There are a bunch of cool new array helper functions:

```javascript
var nums = [1,3,5,7,8];
var largeNums = nums.filter(function(currentNum){
    return currentNum > 5
});
console.log(largeNums);

var sum = nums.reduce(function(accumulatedValue, currentNum){
    return accumulatedValue + currentNum;
});
console.log(sum);

var doubledValues = nums.map(function(currentNum){
    return currentNum * 2;
})
console.log(doubledValues);

var found = nums.find(function(currentNum) {
  return currentNum > 5;
});
console.log(found);

var found = nums.findIndex(function(currentNum) {
  return currentNum > 5;
});
console.log(found);

var doesFiveExist = nums.includes(5);
console.log(doesFiveExist);

var doubleArray = nums.concat(nums);
console.log(doubleArray);

var section = nums.slice(2,4)
console.log(section);
```
