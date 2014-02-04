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

```
var a = [1,2,3],
  i = a.iterator();

i.next(); // {done: false, value: 1}
i.next(); // {done: false, value: 2}
i.next(); // {done: false, value: 3}
i.next(); // {done: true, value: undefined}
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
```small
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



## Useful links
* [http://kangax.github.io/es5-compat-table/es6/](http://kangax.github.io/es5-compat-table/es6/)
* [http://www.ecmascript.org/](http://www.ecmascript.org/)
* [https://developer.mozilla.org/en/docs/Web/JavaScript/ECMAScript_6_support_in_Mozilla](https://developer.mozilla.org/en/docs/Web/JavaScript/ECMAScript_6_support_in_Mozilla)

