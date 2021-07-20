# Basics
For minification, the ECMAScript 6 arrow functions are preferred over ECMAScript 5 functions, unless a new scope is needed.
```js
//ES5
function name(a,b,c){return a+b+c}(1,2,3);
//ES6
self.name=(a,b,c)=>a+b+c;name(1,2,3);
//or
(self.name=(a,b,c)=>a+b+c)(1,2,3);

//outermost layer as an anonymous function
((a,b,c)=>a+b+c)(1,2,3);
```
In standard browsers, most objects can also be accessed as properties of `self` or `window`.
```js
Object===self['Object']
//true
```
Additionally, `if` statements should usually be avoided; use ternary/conditional operators, boolean logic, math, and bitwise operations instead. With bitwise, `Number|0` is often used to round toward 0 (almost a complete replacement for `Math.round`).
```js
if(a==3)a=0;
a-3||(a=0);
//better
a=a-3&&a;

if(a==3){a=0}else{a=3}
a=a-3&&3;

if(a=='c'){a='b'}else{a='c'}
a=a=='c'?'b':'c';

if(a>3&&a<6)a=0;
a>3&a<6&&(a=0);
//even better
a=a>3&a<6?0:a;
a=a<4|a>5&&a;
a=a-4.5|0&&a;

/*
Order:
0. Left-hand assignment
1. (Parentheses) and [Brackets]
2. Chaining (.,?.)
3. Unary (!,~,-) and Exponent (**)
4. Multiplication, division, and remainder (*,/,%)
5. Addition and subraction (+,-)
6. Bit shift (<<,>>)
7. Comparison (<,<=,>,>=,==,!=,===,!==)
8. Bitwise (^,&,|)
9. Logic (&&,||) and Nullish Coalescing (??)
10. Ternary (condition?true:false)
11. Assignment
12. Destructuring (...)
*/
```
Some switch statements may also be replaced with arrays.
```js
switch(){}
```
The grave accent, or backtick, can be used for both expression interpolation and function parsing.
```js
'A random number: '+Math.random()+'\nIs it a lucky one?'
`A random number: ${Math.random()}\nIs it a lucky one?`
//expression interpolation

(a=>a.repeat(2))('Help me! ')
(a=>a.repeat(2))`Help me! `
//parsing function via tagged template


//use with caution
console.log('test')
console.log`test`
console.log((a=>a.split`|`)('te|st'))
console.log((a=>a.split`|`)`te|st`)
/*
Logged list:

"test"
["test",raw:["test"]]
["te","st"]
Uncaught TypeError: a.split is not a function
*/
```
## Standard JavaScript constraints for shortening property and object references:
## Via Default Parameters/Arguments
```js
//example
(a,b,c)=>a+b+c+String.hasOwnProperty()+String.hasOwnProperty();
//shortened object
(a,b,c,s=String)=>a+b+c+s.hasOwnProperty()+s.hasOwnProperty();
//shortened property
(a,b,c,h='hasOwnProperty')=>a+b+c+String[h]()+String[h]();
//shortened
(a,b,c,s=String,h='hasOwnProperty')=>a+b+c+s[h]()+s[h]();
```
Property | Min. | Examples
-------- | ---- | --------
`.1234567890+` | 2 | `.codePointAt`, `.charCodeAt`
`.123456(789)` | 3 | `.replace`, `.length`, `.filter`
`.12345` | 4 | `.split`, `.slice`, `.round`
`.1234` | 5 | `.join`, `.ceil`, `.bind`
`.123` | 9 | `.raw`, `.map`

Object | Min. | Examples
------ | ---- | --------
`123456+` | 2 | `String`, `Function`, `Math.ceil`
`1234(5)` | 3 | `Math`, `Array`, `self`
`123` | 4 | `URL`, `Set`, `Map`, `CSS`, `top`
`12` | 6

For shortening, destructuring should be used after combining at least **7** variables.
```js
a='1',b='2',c='3',d='4',e='5',f='6',g='7'
[a,b,c,d,e,f,g]='1,2,3,4,5,6,7'.split`,`
```
Every additional variable included will save 2 bytes each, regardless of string length. This is important because **additional properties now follow the table for Variable Reassignment.**
## Via Variable Reassignment
```js
(a,b='meaningless')=>a[b]+a.length+a.length+a.length;
(a,b='meaningless')=>a[b]+a[b='length']+a[b]+a[b];
```
Property | Minimum Uses
-------- | ------------
`.12345678+` | 2
`.12345(67)` | 3
`.1234` | 4
`.123` | 7

Reassignments for object (not property) references have the same range as with the default parameters. This also applies to destructuring.
```js
(a,b='meaningless')=>a[b]+123456+123456;
(a,b='meaningless')=>a[b]+(b=123456)+b;
```
# Managing RegExp
Common Usage | Function | Replacement
------------ | ------- | -----------
`(.\|\n)` or `[\s\S]` | Represent any character | `[^]`
# Minor Adjustments
# Mixing with Optimization
If a script needs to run thousands of times per minute, then avoid RegExp. `.indexOf`, when paired with bracket notation, is the most optimal searching method.
