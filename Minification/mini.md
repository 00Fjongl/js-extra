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
a='1',b='2',c='3',d='4',e='5',f='6',g='7';
[a,b,c,d,e,f,g]='1,2,3,4,5,6,7'.split`,`;
```
Every additional variable included will save 2 bytes each, regardless of string length. This is important because **additional properties will now follow the table for Variable Reassignment.**
## Via Variable Reassignment
```js
(a,b='meaningless')=>a[b]+a.length+a.length+a.length;
(a,b='meaningless')=>a[b]+a[b='length']+a[b]+a[b];
```
Variables are reassigned, or reused, to avoid creating new ones.  
  
Property | Min.
-------- | ----
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
`/\w\|-\|\./` | Search for certain characters, including dots | `/[\w.-]/`
`/[A-z\d]/` | Search for letters and numbers | `/[^\W_]/`
`!isFinite(c)?a():b()` | Execute `a` if not true, or `b` if true | `(isFinite(c)?b:a)()`
`a>=b?c():d()` | Execute `c` if `a` is greater than or equal to `b`, or `d` if not | `(a<b?d:c)()`
`undefined` | Return a nearly empty placeholder | `0[0]`, `0[statement]`, `function().$`, or `mangled_variable.$`
`false` and `true` | Return a basic value to use for logic | `!1` or `0`, and `!0` or `1`
`['a','b','c'][2]` | Return a character from a list | `'abc'[2]`
`(a/2\|0)-a/2` | Check for an even number | `a%2` or `a&1` (even after `2**31-1`)
`~~a-a` | Check for decimals | `a%1`
`Math.sqrt(a)` | Return the square root of `a` | `a**.5`
`Math.random()*2\|0` | Randomly return truthy or falsy values | `Math.random()<.5`

## Minor Adjustments
Logic, ternary operators, and arrow functions tend to use parentheses to run groups of statements. In these situations, code such as `(isFinite(c)?b:a)()` is preferable to `isFinite(c)?b():a()`, even though they have the same size. It creates a set of parentheses at no cost to size, and their singular return value can be abused to make more room.
```js
isFinite(c)||(console.log('c is infinite'),isNaN(c)?b():a());
isFinite(c)||(console.log('c is infinite'),isNaN(c)?b:a)();      //parentheses abused

(a=>a&&(delete array1,a[4]))(array1);
array1&&(array1[delete array1,4]);                               //bracket notation abused

a=b&&(console.log(b),b.replace(/123/,a));
a=b&&b.replace(/123/,a,console.log(b));                          //unused arguments abused

a?(s='123',s++,`abc${s}ghi`):s=0;
a?`abc${s='123',++s}ghi`:s=0;                                    //expression interpolation abused
```
Keep in mind that if this changes the order in which functions or statements execute, then the code may not run as intended. Beyond function returns, conditionals, and the rest of the demonstrations above, the method may prove useful in working with keywords, default parameters, passing arguments, and efficiently reassigning variables.  
  
### Synonymous Code
Unfortunately, some pieces of code have optimizations so situational that the challenge lies in recognition, rather than revision. While some of the more applicable methods are shared, not all are covered. Further revisions may involve identifying niche "synonyms."  
  
Consider some of the shorthand assignment operators. They can be applied to many scenarios, if approached creatively.  
```js
a=null,c=a=0,b=256;
a=null,c=a&=b=256;


a=(a+(b=c))%256;
a=(a+=b=c)%256;
a+=b=c,a%=256;
```
###### Replacing `.startsWith`
The `.startsWith` function is made completely useless by this usage of `.indexOf`. It's not only shorter; it's faster!
```js
'abc'.startsWith(d);
!'abc'.indexOf(d);
```
While functions such as `setTimeout` and `Array.prototype.push` return varying integers, they cannot return `0`, so any logic that happens to include them may be reformatted.
```js
Math.random()*2|0&setTimeout(console.log,5e3,'test');
Math.random()*2|!setTimeout(console.log,5e3,'test');
```
`**0` and all bitwise operators will parse `NaN` (or `undefined`, `'abc'`, etc.) as `0`. Consequently, there are differences between `a!=3`, `a-3`, and `a^3` as conditions. Numerical operations are commonplace in minification. As such, these operators may reduce bugs and overall code size when examined.  
  
Is a value inconsistent? Is it causing issues with logic? Arrays are always truthy. Use `!value`, `!!value`, `[value]`, or `![value]` when in a pinch!
```js
self.onkeydown=e=>(e.keyCode-43?alert`Press the correct key.`:onkeydown=console.log(e.key)?0:e=>e,!1);
self.onkeydown=e=>![e.keyCode-43?alert`Press the correct key.`:onkeydown=console.log(e.key)?0:e=>e];
```
# Major Redesigns
Specific tasks or abnormal cases may be more suited for unconventional methods. For instance, a self-invoked function may out-shrink a temporary `for` loop, `while` loop, `Array.forEach`, or `Array.map`. The `.split` function can even be used to split every 2,000 characters better than `for` loops can:
```js
for(i='long_example'.repeat(400),e=0,a=[];e<i.length;)a.push(i.slice(e,e+=2e3));a;

'long_example'.repeat(400).split(/(?=^(?:[^]{2000})+)/);
```
In RegExp, certain cases, such as when a variable or primitive must be parsed, should be handled with the `RegExp()` function itself.
```js
'long_example'.repeat(4e20).split(/(?=^(?:[^]{3000000000000000000})+)/);
'long_example'.repeat(4e20).split(RegExp(`(?=^(?:[^]{${3e18}})+)`));
```
Sometimes, upon discovering a better combination of methods, an entire system may need to be reworked. If redesigns are too time-consuming, then they should be done thoroughly on the first try.  
  
Grouping code that is repeated throughout a script may drastically reduce its length. Functions are the most flexible method to group statements. Where default parameters are useful for grouping repeated objects and strings, functions are able to group entire chunks that include `for` loops and more. They can be modified to call themselves, and even replace finite loops altogether, but execution speed and memory space may dip when called often.
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
If a script needs to run thousands of times per minute, then steer clear of RegExp, and avoid any function-group-minifying. `.split`, as a replacement for arrays of strings, should also be avoided under those conditions.  
  
`.indexOf`, when paired with bracket notation, is likely the most optimal searching method (between `.includes`, `.startsWith`, `.endsWith`, `.search`, etc.). `.includes` is only the fastest when used with arrays.  
  
Unnecessary parentheses create unneeded return values, which could eventually affect speeds under an incredibly high-stress environment. Turning arrays into strings, or from one type to another, also results in some lag. The lag from tagged templates in place of function calls is a different story; they do more than turn arrays into strings.  
  
Properties accessed through bracket notation leave minimal differences in performance, even if the string is replaced by a variable. However, large variable assignments will consume memory.  
