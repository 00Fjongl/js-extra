# Scripts with HTML
JavaScript can be loaded via script tags or event listeners. Using an image element with the `onload` (or `onerror`) attribute is the most reliable alternative to ordinary scripts.  
Mixing via attributes introduces the incentive of designing the code in a way that validates, without requiring quotes or apostrophes for attribute assignments.  
## Common Alternatives
Standard | Substitute
------ | ----------
`document.documentElement` | `document.all[0]`
`<img src=data:image/png;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAAA style=display:none onload=alert()>` | `<img src style=display:none onerror=alert()>`
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
