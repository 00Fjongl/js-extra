### Standard JavaScript constraints for shortening property and object references:
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
`.1234567890+` | 2 | `.codePointAt`, `.charCodeAt`, etc.
`.123456(789)` | 3 | `.replace`, `.length`
`.12345` | 4 | `.split`, `.slice`, `.round`
`.1234` | 5 | `.join`, `.ceil`
`.123` | 9 | `.map`

Object | Min. | Examples
------ | ---- | --------
`123456+` | 2 | `String`, `Object`, `Math.ceil`, etc.
`1234(5)` | 3 | `Math`, `Array`, `self`
`123` | 4 | `URL`
`12` | 6

For shortening, destructuring should be used after combining at least **7** variables.
```js
a='1',b='2',c='3',d='4',e='5',f='6',g='7'
[a,b,c,d,e,f,g]='1,2,3,4,5,6,7'.split`,`
```
Every additional variable included will save 2 bytes each, regardless of string length.
## Via Variable Reassignment
```js
(a,b='meaningless')=>a[b]+a.length+a.length+a.length;
(a,b='meaningless')=>a[b]+a[b='length']+a[b]+a[b];
```
Property | Minimum Uses
-------- | ----
`.12345678+` | 2
`.12345(67)` | 3
`.1234` | 4
`.123` | 7

```js
(a,b='meaningless')=>a[b]+123456+123456;
(a,b='meaningless')=>a[b]+(b=123456)+b;
```
Reassignment for non-property-identifiers (e.g. `Object["identifier"]`) is the same as with the default parameters. This also applies to destructuring.
