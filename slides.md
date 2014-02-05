## What we will cover

* Why ECMAScript6?
* Iterators and Generators
* ...
* Browser support



#### Next...
## Iterators



An **iterator** lets you iterate over the contents of an object.

In ES6, an iterator is an object with a `next()` method which returns `{done, value}` tuples.

An **iterable** is an object which can return an iterator.

Note: 
* Browser: Only Firefox Nightly 29 for now. Firefox stable supports older syntax and semantics.
* Node: `for-of` partially supported



Arrays are iterable:

```small
var a = [1,2,3],
  i = a.iterator();

console.log(i.next()); // {done: false, value: 1}
console.log(i.next()); // {done: false, value: 2}
console.log(i.next()); // {done: false, value: 3}
console.log(i.next()); // {done: true, value: undefined}
```



The `for-of` loop can be used to simplify iterations:

```
var a = [1,2,3];

for (var num of a) {
  console.log(num); // 1, 2, 3
}
```



We can make any object iterable:
```
function ClassA() {
  this.elements = [1, 2, 3];
}
```



By adding the `@@iterator` method:
```smaller
ClassA.prototype['@@iterator'] = function() {
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
```



```
var col = new ClassA();

for (var num of col) {
  console.log(num); // 1, 2, 3
}
```



#### Next...
## Generators



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
```small
var helloWorld = function*() {
  yield 'hello';
  yield 'world';
}

var hw = helloWorld();
console.log( hw.next() ); // { value: 'hello', done: false }
console.log( hw.next() ); // { value: 'world', done: false }
console.log( hw.next() ); // { value: undefined, done: true }
```



Passing values back to generator:
```small
var helloWorld = function*() {
  var nextWord = yield 'hello';
  yield nextWord;
}

var hw = helloWorld();

console.log( hw.next() );        // { value: 'hello', done: false }
console.log( hw.next('world') ); // { value: 'world', done: false }
console.log( hw.next() );        // { value: undefined, done: true }
```



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
    <p>_What about `throw()`-ing errors?</p>
  </div>
</div>


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
console.log( hw.throw('Voldemort') );
console.log( hw.next() );
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
console.log( hw.throw('Voldemort') );
console.log( hw.next() );
</pre>
<pre class="output">
Yield 1...
{ done: false, value: 'hello' }
Error: Voldemort
</pre>



How do Generators make asynchronous programming easier?



The old-school way:

```smaller
var readFile = function(fileName, cb) { ... };

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
```



Improved using Promises:

```smaller
var readFile = Promise.promisify(function(fileName, cb) { ... });

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
```

Note:
* Only need to handle errors in one place
* Much more readable code. Control flow more obvious.



We can do better with generators.

But first we need a function which will automatically handle the `yield`-ed values and call `next()` on the generator...


<!-- .slide: id="runGenerator", -->
Automatically resolve Promises and call `next()`:
```smallest
var runGenerator = function(generatorFunction) {
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
```

Note:
* This assumes that the generator always yields either Promises or values which can be wrapped as a Promise.



Now we can rewrite `main()` as a generator function:

```smaller
var readFile = Promise.promisify(function(fileName, cb) { ... });

var main = function*() {
  try {
    console.log( yield readFile('file1') );
    console.log( yield readFile('file2') );
  } catch (err) {
    console.error(err);
  }
}

runGenerator(main);
```

Note:
* The whole method is much cleaner-looking and easier to understand.
* We can now use `try-catch` to catch your errors. Nice.
* Using generators allows you to write clean-looking asynchronous code which almost looks synchronous.



You don't need to write `runGenerator()` yourself.

* [https://github.com/visionmedia/co](https://github.com/visionmedia/co) - similar to runGenerator but more powerful.

* [https://github.com/petkaantonov/bluebird](https://github.com/petkaantonov/bluebird) - kick-ass Promise library and provides runGenerator-like methods.

Note:
* **co** is one of the key building blocks for Koa, the successor to Express.



Native Promises in ES6...yay!

**Generators + Promises = future of JS asynchronous programming.**

[http://spion.github.io/posts/why-i-am-switching-to-promises.html](http://spion.github.io/posts/why-i-am-switching-to-promises.html)

Note:
* Native Promises - http://www.html5rocks.com/en/tutorials/es6/promises/



To try out generators in Node.js grab **v0.11.2 (or higher)** and run with the `--harmony` flag.

All of the previous Generator code examples should work!



 
## Useful links
* [http://kangax.github.io/es5-compat-table/es6/](http://kangax.github.io/es5-compat-table/es6/)
* [http://www.ecmascript.org/](http://www.ecmascript.org/)
* [https://developer.mozilla.org/en/docs/Web/JavaScript/ECMAScript_6_support_in_Mozilla](https://developer.mozilla.org/en/docs/Web/JavaScript/ECMAScript_6_support_in_Mozilla)

