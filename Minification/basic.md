# Basics
For minification, arrow functions (ECMAScript 6) are preferred over traditional functions (ECMAScript 5), unless a new scope is needed.
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
Do not forget that in standard browsers, most objects can also be accessed as properties of `self` or `window`. `globalThis` can be used in many environments. Although not as efficient as storing objects in variables, they allow access to the global scope in strict mode:
```js
Object===self['Object'];
//true

"use strict";globalThis.variable=Math.PI;
variable===Math.PI;
//true
```
Brackets are useful for working with strings and `BigInt` values. They can be useful when a number, `undefined`, or `null` is involved, but still function as arrays if not concatenated.
```js
//prevent null or undefined from affecting the string
a+(b||'');
a+[b];

element.value=a||b||'';
element.value=a||[b];

//return '33'
1+2+''+3;
1+2+[3];

//convert BigInt to integer
Number(3n);
+(3n+'');
+[3n];

//use BigInt like a number
1/(3n+'');
1/[3n];
```
In many cases, `if...else` statements include curly brackets (`{}`) to group statements, and not merely to group expressions. Some statements cannot be represented as expressions altogether, such as looping with the `while` or `for` keywords. However, for minification, it is advised to consider that statements which involve `if`, `while`, `for`, or other keywords can often be expressed without the curly brackets.
```js
if(x){y='true';console.log(x)}else{y='false'}
if(x)y='true',console.log(x);else y='false';
```
`if` statements themselves tend to be impractical; use ternary/conditional operators, Boolean logic, math, and bitwise operations instead. With bitwise, `Number|0` is often used to round toward `0`. It differs from `Math.floor` by rounding upward for negative integers, and reversing their sign after `2**31-1`.
```js
if(a==3)a=0;
//0 is falsy
a-3||(a=0);
//better
a-3?0:a=0;
a=a-3&&a;

if(a==3)a=0;else a=3;
a=a-3&&3;

if(a=='c')a='b';else a='c';
a=a=='c'?'b':'c';
a='cb'[a=='c'|0];

a=Math.round(5.76);
a=5.76+.5|0;
```
If the return value is irrelevant, ternary operators can minify even further in specific situations.
```js
a+''&&(b+=a);
a+''?b+=a:0;

b=console.log(b.value)||(a=>b=a);
b=console.log(b.value)?0:a=>b=a;
```
Bitwise operators `&` and `|` can perform similarly to `&&` and `||`, under much stricter circumstances. Unlike logical operators, functions and assignments will run regardless. Avoid having numbers greater than `7`, or even anything other than `1` or `0`, if they are to serve as consistent Booleans. Bitwise also negates decimals.
```js
if(a>3&&a<6)a=0;
a>3&a<6&&(a=0);
//even better
a>3&a<6?a=0:a;
a=a<4|a>5&&a;
a=a-4.5|0&&a;

/*
Order:
0. Left-hand assignment or expression
1. (Parentheses) and [brackets]
2. Chaining (.,?.,a[a]) and calling (a(),a``)
3. Postfix (a++,a--), prefix (++a,--a), unary (!,~,+,-) and exponent (**)
4. Multiplication, division, and remainder (*,/,%)
5. Addition and subraction (+,-)
6. Bit shift (<<,>>,>>>)
7. Comparison (<,<=,>,>=)
8. Equality (==,!=,===,!==)
9. Bitwise AND (&)
10. Bitwise XOR (^)
11. Bitwise OR (|)
12. Logic (&&,||) and nullish coalescing (??)
13. Ternary (condition?true:false)
14. Assignment
15. Destructuring (...)
16. Comma (,)
*/
```
Some switch statements may be substituted with arrays:
```js
switch(a){case'dog':a='cat';break;case'water':a='ice';break;default:a='dog'}
a=['dog','cat','ice'][['dog','water'].indexOf(a)+1];
```
Other switch statements cannot be replaced by arrays. Chains of ternary operators can handle the more obnoxious switch statements.
```js
switch(a){case'dog':delete a;break;case'water':test1();break;case'ice':a=test2;break;default:test2()}

//easy to continue
//turn the condition into a number; iterate with ++ or --
(b=>b--?b--?b?a=test2:test1():delete a:test2())(['dog','water','ice'].indexOf(a)+1);
```
The grave accent, or backtick, can be used for both expression interpolation and function parsing:
```js
'A random number: '+Math.random()+'\nIs it a lucky one?';
`A random number: ${Math.random()}\nIs it a lucky one?`;
//expression interpolation

(a=>a+a)('Help me! ');
(a=>a+a)`Help me! `;
//parsing function via tagged template


//use with caution
console.log('test');
console.log`test`;
console.log((a=>a.split`|`)('te|st'));
console.log((a=>a.split`|`)`te|st`);
/*
Logged list:

"test"
["test",raw:["test"]]
["te","st"]
Uncaught TypeError: a.split is not a function
*/
```
With the tagged templates, arrays of strings can be replaced by the `.split` function after **5** strings.
```js
['1','2','3','4','5'];
'1,2,3,4,5'.split`,`;

//numbers are much faster and no larger
['ab','cd','ef','gh','ij'];
'ab0cd0ef0gh0ij'.split(0);
```
