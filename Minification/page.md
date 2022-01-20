# Scripts with HTML
JavaScript can be loaded via script tags or event listeners. Using an image element with the `onload` (or `onerror`) attribute is the most reliable alternative to ordinary scripts.  
Using attributes to execute JavaScript introduces the incentive of designing the code in a way that validates, without requiring quotes or apostrophes for attribute assignments.
## Elements
In certain cases, elements such as `<span>` or `<div>` can be replaced with `<a>`, or even elements such as `<b>`, `<i>`, `<s>`, and `<u>`, when the styling (e.g. `display:block` or `font:1em Arial`) is properly readjusted. If the descendants of those elements cannot be placed in `<a>`, then the other alternatives may be valid.  
While the primary purpose of this is to reduce the size of HTML code, it can also shorten methods such as `.getElementsByTagName` or comparisons with `.tagName`.  
  
Certain workarounds can be used when an event listener requires spaces or quotes in the attribute value:  
* Hypothetically, for strings that could be represented as primitives, quotes could be avoided by adding `[]` to a primitive value.
  * This would be unlikely, as it would not help in cases where tagged templates or equality comparisons are used.  
  
* Many keywords are separated from variables by spaces or unary operators. Some spaces can be negated by wrapping the variable in `()`.
  * In some instances, `[]`, unary operators, or other symbols may be placed next to a keyword instead of `()`.
  
Similarly, SVG `<path>` elements can have quotes removed from the `d` attribute when loaded alongside HTML. Replace all spaces with commas, but preserve the space that precedes the `/` at the end of a self-closing tag.
## Events
Event listeners can be handled in several different ways. Writing one directly into the HTML code as an attribute will create a new scope, where `this` refers to the element, instead of `window`. If `this` is not needed as a reference to the element, then the event listener may be assigned via JavaScript, so that default parameters can be used to condense the code.  
If `this` is needed, then the scopes will be separated, and generally harder to minify. One remedy could be assigning global variables through `document.body.onload`. In strict mode, these may be defined as properties of `self`.  
In a few obscure cases where only one variable is needed in an event, the pre-defined variable `event` can be reassigned, as an alternative to executing a function.
## Common Alternatives
Standard | Substitute
------ | ----------
`document.documentElement` | `document.all[0]` or `document.lastChild`
`<img src=data:image/png;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAAA onload=alert()>` | `<img src hidden onerror=alert()>`
`<input onkeyup="delete self.onkeyup">` | `<input onkeyup=delete(self.onkeyup)>`
# Filenames
Files requested through the Node.js filesystem, client-side scripts, or HTML code can be optimized if renamed.
```js
//basic querystring system
switch(file){case'a':console.log('first.html');break;case'b':console.log('second.html');break;default:console.log('third.html')}
//regular minification
console.log(['third','first','second']['ab'.indexOf(file)+1]+'.html');
//specific and for many files
console.log((['first','second'][file.charCodeAt()-97]||'third')+'.html');

//rename files to A.html, B.html, and C.html
console.log('CAB'['ab'.indexOf(file)+1]+'.html');
//also rename queries to A, B, and C
console.log((file||'C')+'.html');
```
