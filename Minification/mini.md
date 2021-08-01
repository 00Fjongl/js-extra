### Standard JavaScript constraints for **shortening** property and object references:
## Via Default Parameters/Arguments
```js
Object.length;  //accessed property
Object['length'];  //via bracket notation
l='length';Object[l];  //stored property name
(l='length')=>Object[l];  //via default parameters


(a,b,c)=>a+b+c+String.hasOwnProperty()+String.hasOwnProperty();  //example to shorten
(a,b,c,s=String)=>a+b+c+s.hasOwnProperty()+s.hasOwnProperty();  //object
(a,b,c,h='hasOwnProperty')=>a+b+c+String[h]()+String[h]();     //property
(a,b,c,s=String,h='hasOwnProperty')=>a+b+c+s[h]()+s[h]();     //both
```
Property | Minimum Uses | Examples
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
Every additional variable included will save 2 bytes each, regardless of string length. This is important because **additional properties will now follow the table for Variable Reassignment.**
## Via Variable Reassignment
```js
(a,b='meaningless')=>a[b]+a.length+a.length+a.length;
(a,b='meaningless')=>a[b]+a[b='length']+a[b]+a[b];
```
Variables are reassigned, or reused, to avoid creating new ones.
Property | Min.
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
`/(.\|\n)/` or `/[\s\S]/` | Represent any character | `/[^]/`
`!isFinite(c)?a():b()` | Execute `a` if not true, or `b` if true | `(isFinite(c)?b:a)()`
`undefined` | Return a false-y or nearly empty placeholder | `0[0]` or `(statement).$` or `mangled_variable.$`
`false` and `true` | Return a value that can be used for logic | `!1` or `0`, and `!0` or `1`
`['a','b','c'][2]` | Return a character from a list | `'abc'[2]`
`Math.floor(a/2)-a/2` or `(a/2|0)-a/2` | Check for an even number | `a%2`

## Minor Adjustments
Logic, ternary operators, and arrow functions often need sets of parentheses in order to execute statements in groups. In these situations, code such as `(isFinite(c)?b:a)()` is preferable to `isFinite(c)?b():a()`, even though they are the same size. It creates a set of parentheses at no cost to size, and their singular return value can be abused to make more room.
```js
isFinite(c)||(console.log('c is infinite'),isNaN(c)?b():a());

//parentheses abused
isFinite(c)||(console.log('c is infinite'),isNaN(c)?b:a)();
```
The same can also be done with bracket notation (e.g. `array1&&array1[delete array1,4]`), although some cases may interfere with its return value. Beyond function returns and conditionals, the method is applicable to working with keywords, default parameters, passing arguments, and efficiently reassigning variables. Unused function arguments also serve as space.  
  
### Synonymous Code
Unfortunately, some pieces of code have optimizations so situational that the challenge lies in recognizing it, rather than developing a method. While some of the more applicable techniques are shared, not all can be covered. Further minification may involve locating niche "synonyms."  
  
Functions such as `setTimeout` and `Array.prototype.push` return varying integers. As they cannot return `0`, any logic that happens to include them may be shortened.
```js
Math.random()*2|0&setTimeout(console.log,5e3,'test');
Math.random()*2|!setTimeout(console.log,5e3,'test');
```
`**0` and all bitwise operators will parse `NaN` (or `undefined`, `'abc'`, etc.) as `0`. This results in a difference when using `a!=3`, `a-3`, or `a^3` for a condition. Operations that deal with numbers are commonplace in minification. As such, taking note of those operators should reduce bugs and potential length.  
  
Is a value inconsistent? Is it causing issues with logic? Use `!value`, `!!value`, `[value]`, or `![value]` when in a pinch!
```js
self.onkeydown=e=>(e.keyCode-43?onkeydown=console.log(e.key)||e=>e:alert`Press the correct key.`,!1);
self.onkeydown=e=>![e.keyCode-43?onkeydown=console.log(e.key)||e=>e:alert`Press the correct key.`];
```
# Major Redesigns
Some methods may outshine the usual ones at specific tasks. For example, a self-invoked function may serve as a shorter alternative to a temporary `for` loop, `while` loop, `Array.forEach`, or `Array.map`. The `.split` function can even be used to split every 2,000 characters better than `for` loops can:
```js
for(i='long_example'.repeat(40),e=0,a=[];e<i.length;)a.push(i.slice(e,e+=200));a;

'long_example'.repeat(40).split(/(?<=^(?:[^]{200})+)/);
```
A complete redesign of the entire system may occur if a better function or combination of methods appears.  
  
Grouping commonly repeated code throughout a script may drastically reduce its length. Functions are the most flexible method to group statements, although execution speed and memory space dip when called often. As mentioned before, they can be modified to call themselves, and even replace finite loops.
```js
for(e=0;e<255;e++)[a[b],a[c]]=[a[c],a[b]];for(e=0;e<255;e++)[d[b],d[c]]=[d[c],d[b]];

//grouped
(f=(a,e=0)=>e++<255?f(a,e,[a[b],a[c]]=[a[c],a[b]]):f)(a)(d);
//better
(f=a=>{for(e=0;e++<255;)[a[b],a[c]]=[a[c],a[b]]})(a);f(d);
//not better
for(e=0;e++<255;)[a[b],a[c],d[b],d[c]]=[a[c],a[b],d[c],d[b]];
```
# Mixing with Optimization
If a script needs to run thousands of times per minute, then avoid the use of RegExp, and avoid any function-group-minifying. `.split`, as a replacement for arrays of strings, should also be avoided under those conditions.  
  
`.indexOf`, when paired with bracket notation, is likely the most optimal searching method (between `.includes`, `.startsWith`, `.endsWith`, `.search`, etc.). `.includes` is only the fastest when used with arrays.  
  
Unnecessary parentheses create more return values, which may cause slight amounts of lag. Turning arrays into strings, or from one type to another, results in some lag. This is apparent through tagged templates in place of strings, for arguments.  
  
Properties accessed through bracket notation leave minimal differences in performance, even if the string is replaced by a variable. However, large variable assignments will consume memory.  
