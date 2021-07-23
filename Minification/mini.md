### Standard JavaScript constraints for **shortening** property and object references:
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
# Frequent Mishaps
Common Usage | Function | Replacement
------------ | ------- | -----------
`(.\|\n)` or `[\s\S]` | Represent any character | `[^]`
!isFinite(c)?a():b() | Execute `a` if true, or `b` if not | (isFinite(c)?b:a)()

## Minor Adjustments
Ternary operators and arrow functions often need sets of parentheses in order to execute statements in groups. In these situations, code such as `(isFinite(c)?b:a)()` is preferable to `isFinite(c)?b():a()`, even though they are the same size. This is because it creates a set of parentheses at no cost to size, and their singular return value can be abused.
```js
isFinite(c)&&(console.log('c is infinite'),isNaN(c)?b():a());

//parentheses abused
isFinite(c)&&(console.log('c is infinite'),isNaN(c)?b:a)();
```
The same can also be done with bracket notation (e.g. `array1&&array1[delete array1,4]`), although some cases may interfere with its return value.
# Major Redesigns
Some methods may outshine the usual ones at specific tasks. For example, a self-invoked function may serve as a shorter alternative to a temporary for loop, or `Array.forEach`, `Array.map`, or a while loop. The `.split` function can even be used to split every 2,000 characters better than a for loop can:
```js
for(i='long_example'.repeat(40),e=0,a=[];e<i.length;)a.push(i.slice(e,e+=200));a;

'long_example'.repeat(40).split(/(?<=^(?:[^]{200})+)/);
```
# Mixing with Optimization
If a script needs to run thousands of times per minute, then avoid RegExp. `.indexOf`, when paired with bracket notation, is the most optimal searching method.
`.split`, as a replacement for arrays of strings, should also be avoided under those conditions.
