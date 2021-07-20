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
switch(a){case'dog':a='cat';break;case'water':a='ice';break;default:a='dog'}
a=['dog','cat','ice'][['dog','water'].indexOf(a)+1];
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
