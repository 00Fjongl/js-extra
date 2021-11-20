# Scripts with HTML
JavaScript can be loaded via script tags or event listeners. Using an image element with the `onload` (or `onerror`) attribute is the most reliable alternative to ordinary scripts.  
Mixing via attributes introduces the incentive of designing the code in a way that validates, without requiring quotes or apostrophes for attribute assignments. 
## Elements
In certain cases, elements such as `<span>` or `<div>` can be replaced with `<a>`, or even elements such as `<b>`, `<i>`, `<s>`, and `<u>`, when the styling (e.g. `display:block` or `font:1em Arial`) is properly readjusted. If the descendants of those elements cannot be placed in `<a>`, then the other alternatives may be valid.  
While the primary purpose of this is to reduce the size of HTML code, it can also shorten methods such as `.getElementsByTagName` or comparisons with `.tagName`.
## Common Alternatives
Standard | Substitute
------ | ----------
`document.documentElement` | `document.all[0]` or `document.lastChild`
`<img src=data:image/png;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAAA onload=alert()>` | `<img src hidden onerror=alert()>`
# Filenames
Files requested through the Node.js filesystem, client-side scripts, or HTML code can be optimized if renamed.
```js
//basic querystring system
switch(file){case'a':console.log('first.html');break;case'b':console.log('second.html');break;default:console.log('third.html')}
//regular minification
console.log(['third','first','second']['ab'.indexOf(file)+1]+'.html')
//specific and for many files
console.log((['first','second'][file.charCodeAt()-97]||'third')+'.html')

//rename files to A.html, B.html, and C.html
console.log('CAB'['ab'.indexOf(file)+1]+'.html')
//also rename queries to A, B, and C
console.log((file||'C')+'.html')
```
