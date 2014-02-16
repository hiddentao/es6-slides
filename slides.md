## What we will cover

* Why ECMAScript6?
* Iterators and Generators
* ...
* Browser support



## Scoping



## Block scoping

Finally !



## Before
```javascript
for(var i=10; i>0 ; i--) {
  // do stuff with i
}
console.log(i); // 0
```




## `let`
```javascript
for(let i=10; i>10; i--) {
}
console.log(i); // `i is not defined`
```




## Works with if too
```
var log = function(msg) {};
if(someCond) {
  let log = Math.log;
  // do some stuff
}
log("I'm done here");

```



## Easier closures

broken example

```javascript
var a = ['rhum', 'banana', 'nutella'];
for(var i = 0, n=a.length; i<n; i++) {
  var nomnom = a[i];
  setTimeout(function() {
    console.log(nomnom);
  }, 500*(i+1))
}
```



## Easy fix

```javascript
var a = ['rhum', 'banana', 'nutella'];
for(var i = 0, n=a.length; i<n; i++) {
  let nomnom = a[i];
  setTimeout(function() {
    console.log(nomnom);
  }, 500*(i+1))
}
```



## let('s) recap

* works like var
* scope is defined by the current block ({ })



## const

Like `let`, but define read-only constant declarations.

```javascript
'use strict';
const i = 10;
i = 5; // throw error
```



## Destructuration



## With array

```javascript
// Assignment
var [day, month, year] = [19, 02, 2014];

// Swap two values.
var x=3, y=4;
[x, y] = [y, x];
```



## Array with functions

```javascript
var now = function() { return [19, 02, 2014]; }
var [day, month] = now();
var [, , year] = now();
```



## With objects

```javascript
var now = function() { return {
  d: 19,
  m: 2,
  y: 2014
}}

var {d: day, m: month} = now();
// day: 19
// month: 2
```



## As argument of a function

```javascript
recipes = [
  { name: 'burger', calorie: 215 },
  { name: 'pizza', calorie: 266 } ];

recipes.forEach(function
  ({ name: name, calorie: calorie }) {
    console.log(name, calorie);
  }
);
```



## Default parameters
Ability to define default value for functions paramaters.
No more:

```javascript
function(a) {
  if(!a) { a = 10; }
  // do stuff with a
}
```



## Now

```javascript
function(a=10) {
  // do stuff with a
}
```



## Undefined and null

`undefined` will trigger the evaluation of the default value, not `null`

```javascript
function point (x, y=1, z=1) {
  return console.log(x, y, z);
}

point(10, null); // 10, null, 1

```



## Arity
Number of parameters without default value

```javascript
(function(a){}).length // 1
(function(a=10){}).length // 0
(function(a=10, b){}).length // 1
```



## Rest parameters
Better than `arguments`

```javascript
function(name) {
  console.log(name);
  arguments[0] = 'ME !';
  console.log(name); // ME !
  Array.isArray(arguments); // false
}
```



## Now
```javascript
function(...args) {
  Array.isArray(args); // true
  // do some stuff
}

function(name, ...more) {

}
```



## Example
```javascript
var humblify = function(name, ...qualities) {
  console.log('Hello %s', name);
  console.log('You are '+qualities.join(' and '));
}

humblify('Greg', 'awesome', 'a genius', 'the master of the universe');
// Hello Greg
// You are awersome and a genius and the
// master of the universe
```




## Restrictions
Rest parameters can only be the last parameter

```javascript
// incorrect
function(...args, callback) {
}
```



## Details
Rest parameter is *always* an array

```javascript
function f(name, ...more) {
  Array.isArray(more); // always true
  return more.length;
}
f(); // returns 0
```



## Arity
Does not include the rest parameter
```javascript
(function(a) {}).length // 1
(function(a, ...rest) {}).length // 1
```



## Spread
Expand expression where multiple arguments or multiple element are needed



## Spread with functions
#### A better `apply`

```javascript
var f = function(one, two, three) {}
var a = [1, 2, 3];
f.apply(null, a);
```


## Apply ?

* `Function.prototype.apply`
* `fun.apply(thisArg, [argsArray])`


## Apply example

```
function f() {
  for(let i=0; i<arguments.length; ++i)
    console.log(arguments[i]);
}

f.apply(this, ['one', 2, 'foo']);
// one
// 2
// foo
```




## With es6's spread
```javascript
var f = function(one, two, three) {}
var a = [1, 2, 3];
f(...a);
```



## Sweet syntax
```javascript
var f = function(a, b, c, d, e, f) {};
var a = [1, 2];
f(-1, ...a, 3, ...[-3, -4]);
```



## More powerful array manipulation
Usecase: create new array with an existing one inside it:
```javascript
var from = [1, 2];
// wants: [0, 1, 2, 3] ie [0, from, 3]
```



## With es5

```
a.unshift(0);
a.push(3);
// and splice is here also
```

<div class="fragment">
<h2>With es6</h2>
<pre><code class="javascript">
var total = [0, ...from, 3];
</code></pre>
</div>



## Apply for `new`
With es5, one cannot use `apply` with `new`.

```javascript
var Constructor = function() {
  // do some stuff
}
var c = new Constructor.apply({}, []); //invalid
```

<div class="fragment">
But now:
<pre><code class="javascript">
var dataFields = readDateFields(database);
var d = new Date(...dateFields);
</pre></code>
</div>



## Better push
To push multiple elements:
```javascript
var a = [];
var toPush = [1, 2, 3];
a.push.apply(a, toPush);
```

<div class="fragment">
And now:
<pre><code class="javascript">
a.push(...toPush);
</code></pre>
</div>



## Converting any array-like


## Array like ???

* Object with a length property
* Can access elements with `[]`

```javascript
var fake = {
  0: 'I am',
  1: 'not',
  2: 'an aray',
  length: 3
};
```


## Array like in the wild

* Function's `arguments`
* All nodeList from the DOM



Before:
```javascript
var nodes = document.querySelectorAll('p');
var nodes = [].slice.call(nodes);
```

<div class="fragment">
And now:
<pre><code class="javascript">
nodes = [...nodes];
</code></pre>
</div>



## Array conversion
Better way:
```javascript
Array.from(document.querySelectorAll('p'));
```
Out of the scope of the talk.



#### Next...
## Iterators and Generators



An **iterator** lets you iterate over the contents of an object.

In ES6, an iterator is an object with a `next()` method which returns `{done, value}` tuples.

An **iterable** is an object which can return an iterator.

Note: 
* Browser: Only Firefox Nightly 29 for now. Firefox stable supports older syntax and semantics.
* Node: `for-of` partially supported



Arrays are iterable:
<pre><code class="javascript small">
var a = [1,2,3],
  i = a.iterator();

console.log(i.next()); // {done: false, value: 1}
console.log(i.next()); // {done: false, value: 2}
console.log(i.next()); // {done: false, value: 3}
console.log(i.next()); // {done: true, value: undefined}
</code></pre>



The `for-of` loop can be used to simplify iterations:
<pre><code class="javascript">
var a = [1,2,3];

for (var num of a) {
  console.log(num); // 1, 2, 3
}
</code></pre>



Array comprehensions:
```javascript
var a = [
  { color: 'red' },
  { color: 'blue' }
];

[ x.color for (x of a) if ('blue' === x.color) ]

// [ 'blue' ]
```

Note:
* Only in Firefox for now. Not even Node supports it.



We can make any object iterable:
<pre><code class="javascript">
function ClassA() {
  this.elements = [1, 2, 3];
}
</code></pre>



By adding the `@@iterator` method:
<pre><code class="javascript smaller">ClassA.prototype['@@iterator'] = function() {
  return {
    elements: this.elements,
    index: 0,
    next: function() {
      if (this.index >= this.elements.length)
        return { 
          done: true, 
          value: undefined 
        };
      else
        return { 
          done: false, 
          value: this.elements[this.index++] 
        };
}}};
</code></pre>



We can iterate over the Object:
<pre><code class="javascript">
var col = new ClassA();

for (var num of col) {
  console.log(num); // 1, 2, 3
}
</code></pre>



<div class="fragment">
  <p>A **generator** is a special type of iterator.</p>
  <p>A generator provides a `throw()` method. Its `next()` method accepts a parameter.</p>
  <p>A **generator function** acts as a constructor for a generator.</p>
  <div class="fragment">
    <p>_Generators offer a clean way of doing asynchronous programming!_</p>
  </div>
</div>

Note: 
* Browser: Only Firefox Nightly 29 for now. Firefox stable supports older syntax and semantics.
* Node: Generator functions and `yield` supported



Simple example:
<pre><code class="javascript small">
var helloWorld = function*() {
  yield 'hello';
  yield 'world';
}

var hw = helloWorld();
console.log( hw.next() ); // { value: 'hello', done: false }
console.log( hw.next() ); // { value: 'world', done: false }
console.log( hw.next() ); // { value: undefined, done: true }
</code></pre>



Passing values back to generator:
<pre><code class="javascript small">
var helloWorld = function*() {
  var nextWord = yield 'hello';
  yield nextWord;
}

var hw = helloWorld();

console.log( hw.next() );       // { value: 'hello', done: false }
console.log( hw.next('world') ); // { value: 'world', done: false }
console.log( hw.next() );       // { value: undefined, done: true }
</code></pre>



Let's take it step-by-step to see how code gets suspended...


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
var helloWorld = function*() {    
  console.log('Yield 1...');
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

var hw = helloWorld();

console.log( hw.next() );
console.log( hw.next('world') );
console.log( hw.next() );
</pre>

Note:
* Same logic as before, just with more logging


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld =</b> function*() {    
  console.log('Yield 1...');
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

var hw = <strong>helloWorld()</strong>;

console.log( hw.next() );
console.log( hw.next('world') );
console.log( hw.next() );
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld =</b> <strong>function*()</strong> {
  console.log('Yield 1...');
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

var hw = helloWorld();

console.log( hw.next() );
console.log( hw.next('world') );
console.log( hw.next() );
</pre>

Note:
* As soon as the VM sees that it's a generator function it returns without executing the function body.


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  console.log('Yield 1...');
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<strong>var hw</strong> = <b>helloWorld();</b>

console.log( hw.next() );
console.log( hw.next('world') );
console.log( hw.next() );
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  console.log('Yield 1...');
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

console.log( <strong>hw.next()</strong> );
console.log( hw.next('world') );
console.log( hw.next() );
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <strong>console.log('Yield 1...');</strong>
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

console.log( hw.next() );
console.log( hw.next('world') );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  var nextWord = <strong>yield 'hello';</strong>
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

console.log( hw.next() );
console.log( hw.next('world') );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  var nextWord = <b>yield 'hello'</b>;
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

<strong>console.log</strong>( <b>hw.next()</b> );
console.log( hw.next('world') );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  var nextWord = <b>yield 'hello'</b>;
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

<b>console.log( hw.next() );</b>
console.log( <strong>hw.next('world')</strong> );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  <strong>var nextWord</strong> = <b>yield 'hello'</b>;
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

<b>console.log( hw.next() );</b>
console.log( hw.next('world') );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  <b>var nextWord = yield 'hello';</b>
  <strong>console.log('Yield 2...');</strong>
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

<b>console.log( hw.next() );</b>
console.log( hw.next('world') );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
Yield 2...
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  <b>var nextWord = yield 'hello';</b>
  <b>console.log('Yield 2...');</b>
  <strong>yield nextWord;</strong>
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

<b>console.log( hw.next() );</b>
console.log( hw.next('world') );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
Yield 2...
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  <b>var nextWord = yield 'hello';</b>
  <b>console.log('Yield 2...');</b>
  <b>yield nextWord;</b>
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

<b>console.log( hw.next() );</b>
<strong>console.log</strong>( <b>hw.next('world')</b> );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
Yield 2...
{ done: false, value: 'world' }
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  <b>var nextWord = yield 'hello';</b>
  <b>console.log('Yield 2...');</b>
  <b>yield nextWord;</b>
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

<b>console.log( hw.next() );</b>
<b>console.log( hw.next('world') );</b>
console.log( <strong>hw.next()</strong> );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
Yield 2...
{ done: false, value: 'world' }
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  <b>var nextWord = yield 'hello';</b>
  <b>console.log('Yield 2...');</b>
  <b>yield nextWord;</b>
  <strong>console.log('No more yields...');</strong>
}

<b>var hw = helloWorld();</b>

<b>console.log( hw.next() );</b>
<b>console.log( hw.next('world') );</b>
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
Yield 2...
{ done: false, value: 'world' }
No more yields...
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  <b>var nextWord = yield 'hello';</b>
  <b>console.log('Yield 2...');</b>
  <b>yield nextWord;</b>
  <b>console.log('No more yields...');</b>
}

<b>var hw = helloWorld();</b>

<b>console.log( hw.next() );</b>
<b>console.log( hw.next('world') );</b>
<strong>console.log</strong>( <b>hw.next()</b> );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
Yield 2...
{ done: false, value: 'world' }
No more yields...
{ done: true, value: undefined }
</pre>



<div class="fragment">
  <p>The code in the generator doesn't start executing until you say so.</p>
  <p>When the `yield` statement is encountered it suspends execution until you tell it to resume.</p>
  <div class="fragment">
    <p>_What about `throw()`-ing errors?_</p>
  </div>
</div>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld =</b> function*() {    
  console.log('Yield 1...');
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

var hw = <strong>helloWorld()</strong>;

console.log( hw.next() );
console.log( hw.throw('Voldemort') );
console.log( hw.next() );
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld =</b> <strong>function*()</strong> {
  console.log('Yield 1...');
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

var hw = helloWorld();

console.log( hw.next() );
console.log( hw.throw('Voldemort') );
console.log( hw.next() );
</pre>

Note:
* As soon as the VM sees that it's a generator function it returns without executing the function body.


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  console.log('Yield 1...');
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<strong>var hw</strong> = <b>helloWorld();</b>

console.log( hw.next() );
console.log( hw.throw('Voldemort') );
console.log( hw.next() );
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  console.log('Yield 1...');
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

console.log( <strong>hw.next()</strong> );
console.log( hw.throw('Voldemort') );
console.log( hw.next() );
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <strong>console.log('Yield 1...');</strong>
  var nextWord = yield 'hello';
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

console.log( hw.next() );
console.log( hw.throw('Voldemort') );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  var nextWord = <strong>yield 'hello';</strong>
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

console.log( hw.next() );
console.log( hw.throw('Voldemort') );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  var nextWord = <b>yield 'hello'</b>;
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

<strong>console.log</strong>( <b>hw.next()</b> );
console.log( hw.throw('Voldemort') );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
</pre>


<!-- .slide: data-transition="none", class="step-code" -->
<pre class="program">
<b>var helloWorld = function*() {</b>
  <b>console.log('Yield 1...');</b>
  var nextWord = <b>yield 'hello'</b>;
  console.log('Yield 2...');
  yield nextWord;
  console.log('No more yields...');
}

<b>var hw = helloWorld();</b>

<b>console.log( hw.next() );</b>
console.log( <strong>hw.throw('Voldemort')</strong> );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
Error: Voldemort
</pre>



How do Generators make asynchronous programming easier?



The old-school way:
<pre><code class="javascript smaller">var readFile = function(fileName, cb) { ... };

var main = function(cb) {
  readFile('file1', function(err, contents1) {
    if (err) return cb(err);
    console.log(contents1);

    readFile('file2', function(err, contents2) {
      if (err) return cb(err);
      console.log(contents2);
      cb();
    });
  });  
}

main(console.error);
</code></pre>



Improved using Promises:
<pre><code class="javascript smaller">var readFile = Promise.promisify(function(fileName, cb) { ... });

var main = function() {
  return readFile('file1')
    .then(function(contents) {
      console.log(contents);
      return readFile('file2');
    })
    .then(function(contents) {
      console.log(contents);
    })
    .catch(console.error);
}

main();
</code></pre>

Note:
* Only need to handle errors in one place
* Much more readable code. Control flow more obvious.



We can do better with generators.

But first we need a function which will automatically handle the `yield`-ed values and call `next()` on the generator...


<!-- .slide: id="runGenerator", -->
Automatically resolve Promises and call `next()`:
<pre><code class="javascript smallest">var runGenerator = function(generatorFunction) {
  var gen = generatorFunction();

  var gNext = function(err, answer) {
    if (err) return gen.throw(err);

    var res = gen.next(answer);

    if (!res.done) {
      Promise.resolve(res.value)
        .then(function(newAnswer) {
          gNext(null, newAnswer);
        })
        .catch(gNext);
    }
  };
  gNext();
}
</code></pre>

Note:
* This assumes that the generator always yields either Promises or values which can be wrapped as a Promise.



Now we can rewrite `main()` as a generator function:
<pre><code class="javascript small">var readFile = Promise.promisify(function(fileName, cb) { ... });
var main = function*() {
  try {
    console.log( yield readFile('file1') );
    console.log( yield readFile('file2') );
  } catch (err) {
    console.error(err);
  }
}

runGenerator(main);
</code></pre>

Note:
* The whole method is much cleaner-looking and easier to understand.
* We can now use `try-catch` to catch your errors. Nice.
* Using generators allows you to write clean-looking asynchronous code which almost looks synchronous.



You don't need to write `runGenerator()` yourself.

* [https://github.com/visionmedia/co](https://github.com/visionmedia/co) - similar to runGenerator but more powerful.

* [https://github.com/petkaantonov/bluebird](https://github.com/petkaantonov/bluebird) - kick-ass Promise library and provides runGenerator-like methods.

Note:
* **co** is one of the key building blocks for Koa, the successor to Express.



<!-- .slide: class="side-by-side-code" -->
Generator delegation:
<pre><code class="javascript left smaller">var inner = function*() {
  try {
    yield callServer1();
    yield callServer2();  
  } catch (e) {
    console.error(e);
  }
};

var outer = function*() {
  yield* inner();
};

runGenerator(outer);
</code></pre>
<pre><code class="javascript right smaller">var outer = function*() {
  try {
    yield callServer1();
    yield callServer2();  
  } catch (e) {
    console.error(e);
  }
};

runGenerator(outer);
</code></pre>

Note:
* Generator delegation is a convenience notation which makes it easy to compose generators



<!-- .slide: class="side-by-side-code" -->
Generator comprehension:
<pre><code class="javascript left small">(for (x of a) 
  for (y of b) 
    x * y)
</code></pre>
<pre><code class="javascript right small">(function* () {
  for (x of a) {
    for (y of b) {
      yield x * y;
    }
  }
}())
</code></pre>

Note:
* Might seem weird, but has its reasons: http://esdiscuss.org/topic/why-do-generator-expressions-return-generators



**Generators = future of JS asynchronous programming.**

...and ES6 also has Promises!

[http://spion.github.io/posts/why-i-am-switching-to-promises.html](http://spion.github.io/posts/why-i-am-switching-to-promises.html)

Note:
* Native Promises - http://www.html5rocks.com/en/tutorials/es6/promises/



To try out generators in Node.js grab **v0.11.2 (or higher)** and run with the `--harmony` flag.

All of the previous Generator code examples should work!




#### Next...
## Collections



`Set` - no duplicates allowed
<pre><code class="javascript small">var items = new Set();
items.add(5);
items.add("5");
items.add(5);
console.log(items.size);    // 2

var items = new Set([1,2,3,4,5,5,5]);
console.log(items.size);    // 5
</code></pre>

Note:
* Note that value are not coerced - so `"5"` is considered different to `5`.
* Browsers: Firefox only. Chrome support is behind a flag.
* Node: Suppored behind `harmony` flag, though note that `add()` needs to be used - constructor parameter does nothing.
* http://www.nczonline.net/blog/2012/09/25/ecmascript-6-collections-part-1-sets/



Modifying a `Set`
<pre><code class="javascript small">var items = new Set([1,2,3,4,4]);
console.log( items.has(4) ); // true
console.log( items.has(5) ); // false

items.delete(4);
console.log( items.has(4) ); // false
console.log( items.size ); // 3

items.clear();
console.log( items.size ); // 0
</code></pre>



Iterate over a  `Set` using `for-of`
<pre><code class="javascript small">var items = new Set([1,2,3,4,4]);

for (let num of items) {
  console.log( num );     // 1, 2, 3, 4
}
</code></pre>



`Map` - map from key to value
<pre><code class="javascript small">var map = new Map();
map.set("name", "John");
map.set(23, "age");

console.log( map.has(23); ) // true
console.log( map.get(23) ); // "age"
console.log( map.size );    // 2
</code></pre>

Note:
* Browsers: Firefox only. Chrome support is behind a flag.
* Node: Suppored behind `harmony` flag.
* http://www.nczonline.net/blog/2012/10/09/ecmascript-6-collections-part-2-maps/



`Map` vs `Object`

* Maps can have non-string keys
* Maps don't have prototype leakage issues, i.e. no need to use `hasOwnProperty()`
* But 



Modifying a `Map`
<pre><code class="javascript small">var map = new Map([ ['name', 'John'], [23, 'age'] ]);
console.log( map.size );    // 2

map.delete(23);
console.log( map.get(23) ); // undefined

map.clear();
console.log( map.size ); // 0
</code></pre>

Note:
* Note that you can pass map key-valur pairs to the constructor



Iterating over a `Map`
<pre><code class="javascript smaller">var map = new Map([ ['name', 'John'], [23, 'age'] ]);

for (var value of map.values()) { ... }

for (var key of map.keys()) { ... }

for (var item of map.items()) {
  console.log('key: ' + item[0] + ', value: ' + item[1]);
}

for (var item of map) { // same as iterating map.items() }

map.forEach(function(value, key, map) { ... });
</code></pre>



`WeakMap` - similar to `Map`, but...

* Only allows `Object` keys
* Only holds a reference to the `Object` used as a key, so it doesn't prevent garbage collection
* Do no provide a `size`
* Cannot be iterated over



<pre><code class="javascript small">var weakMap = new WeakMap();
var key = {
  stuff: true
};

weakMap.set(key, 123); // weakMap contains 1 item

delete key;  // weakMap is now empty
</code></pre>



#### Next...
## API improvements

Note:
* Mozilla Developer docs explain all of these



New `Math` functions

`log10`, `log2`, `log1p`, `expm1`, `cosh`, `sinh`, `tanh`, `acosh`, `asinh`, `atanh`, `hypot`, `trunc`, `sign`

Note:
* Mozilla Developer docs explain all of these



`Number`

* `.isFinite()`

* `.isNaN()` - better than `isNan()`

* `.isInteger()`

`.EPSILON` - smallest values such that `1 + Number.EPSILON > 1`



`String`

* `.repeat(n)` - copy current string `n` times

* `.startsWith(str)`

* `.endsWith(str)`

* `.contains(str)`

* `.toArray()` - same as `.split('')`



#### Next...
## Modularity



## Classes



## In es5

* Classes doesn't exist natively
* Prototype based inheritance
* Framework and libraries implement their own class system



## New keyword

```javascript
class Laptop {
  constructor() {
    this.brand = 'asus';
  }

  on() { ... }
  off() { ... }
}
```



## Call the parent

```javascript
class SmashedLaptop extend Laptop {
  constructor() {
    super();
    this.pieces = [];
  }
}
```



## Key points

* `constructor` replace the function definition in es5
* No access to the `prototype` of the class
* Methods are defined the same way as objects
* Can call the parent with `super` (and perform initialization within the constructor)



## Modules

<pre><code language="javascript">&bull; import the default export of a module
import $ from "jquery";
</code></pre>

<div class="fragment">
<pre><code language="javascript small">&bull; binding an external module to a variable
module crypto from "crypto";
</code></pre>
</div>

<div class="fragment">
<pre><code language="javascript small">&bull; binding a module's exports to variables
import { encrypt, decrypt } from "crypto";
</code></pre>
</div>



## Modules

<pre><code language="javascript small">&bull; binding and renaming one of a module's exports
import { encrypt as enc } from "crypto";
</code></pre>

<div class="fragment">
<pre><code language="javascript small">&bull; re-exporting another module's exports
export * from "crypto";
</code></pre>
</div>

<div class="fragment">
<pre><code language="javascript small">&bull; re-exporting specified exports
from another module
export { foo, bar } from "crypto";
</code></pre>
</div>



## Why ?

* No need for the global object anymore
* Works well with existing modules system (AMD, CommonJS and node)
* Simplicity and usability
* Compatibility with browser and non-browser environments
* Easy asynchronous external loading



## Exporting and importing

```javascript
module "chocolate" {
  export let cocoa = 75;
}
```

In another file:

```
import { cocoa } from "chocolate";
// or
import { cocoa as c} from "chocolate";
```



## Default export

```javascript
module "foo" {
  export default function() {console.log("Hi!")}
}
```
<hr>

```javascript
import foo from "foo"; // no brackets
foo(); // Hi!
```



## Internals

* Top-level variables stay inside the module
* `export` make variables visible to the other modules
  * Can be read (get)
  * Cannot be changed (set)
  * Cannot be dynamically changed at runtime
* Modules are recursively instantiated before evaluation
* Modules' body is run after all dependencies are instantiated


<!-- Add some more stuff about modules here -->


## Useful links
* [http://kangax.github.io/es5-compat-table/es6/](http://kangax.github.io/es5-compat-table/es6/)
* [http://www.ecmascript.org/](http://www.ecmascript.org/)
* [http://www.esdiscuss.org/](http://www.esdiscuss.org/)
* [https://developer.mozilla.org/en/docs/Web/JavaScript/ECMAScript_6_support_in_Mozilla](https://developer.mozilla.org/en/docs/Web/JavaScript/ECMAScript_6_support_in_Mozilla)

