## Inconsistencies
The regular expression implementation in JavaScript may differ slightly from other languages, such as Python.  
An expression written in JavaScript is more likely to cause errors when transported to Python's implementation.  
When transporting outside of JavaScript:
* Look-behind assertions must be fixed-width, meaning that the number of characters cannot vary.
* `[^]` may throw an error.
* Word breaks signified by `\b` may not function as intended.
* Regular expression flags are set in different ways.  
To remedy some compatibility issues, the following can be replaced.
JavaScript | All
---------- | ---
`(?<!abb?)TEST` | `(?<!ab)(?<!abb)TEST`
`(?<=a[bc]{1,2})TEST` | `((?<=a[bc])|(?<=a[bc]{2}))TEST` or `(?:(?<=a[bc])|(?<=a[bc]{2}))TEST`
`[^]` | `[\s\S]`, `[\w\W]`, `[\d\D]`, etc.
`\bTEST\b` | `(?<!\w)TEST(?!\w)`
`\BTEST\B` | `(?<=\w)TEST(?=\w)`
For a JavaScript regular expression like `(?<!abc+)TEST`, one may need to take the first captured group from a replacement such as `(?!(?<=ab)c+TEST)[\s\S]*(TEST)`.  
This is the consequence of using a look-behind assertion that can vary indefinitely.
