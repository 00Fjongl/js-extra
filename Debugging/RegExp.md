## Inconsistencies
The regular expression implementation in JavaScript may differ slightly from other languages, such as Python.  
An expression written in JavaScript is more likely to cause errors when transported to Python's implementation.  
When transporting outside of JavaScript:
* Look-behind assertions must be fixed-width, meaning that the number of characters cannot vary.
  * This does not limit the use of assertions for different terms with the same length, or the use of look-ahead assertions.
* `[^]` may throw an error.
* Word breaks signified by `\b` may not function as intended.
* Regular expression flags are set in different ways.

To remedy some compatibility issues, the following can be replaced.
JavaScript | All
---------- | ---
`(?<!abb?)TEST` | `(?<!ab)(?<!abb)TEST`
`(?<=a[bc]{1,2})TEST` | `(?:(?<=a[bc])\|(?<=a[bc]{2}))TEST`
`(?<!ab{1,5}cdefghijklmnop)TEST` | `(?<!(?:(?<=ab)\|(?<=abb)\|(?<=abbb)\|(?<=ab{4})\|(?<=ab{5}))cdefghijklmnop)TEST`
`[^]` | `[\s\S]`, `[\w\W]`, `[\d\D]`, etc.
`\bTEST\b` | `(?<!\w)TEST(?!\w)`
`\BTEST\B` | `(?<=\w)TEST(?=\w)`
###### Inclusion of `?:` is optional.  
  
For transporting a JavaScript regular expression like `(?<!abc+)TEST`, one may be forced to take the first captured group from a replacement such as this:  
`(?!(?<=ab)c+TEST)[\s\S]*(TEST)`  
  
This is the consequence of using a look-behind assertion that can vary indefinitely.
#### Captured Groups
Content captured within parentheses will be updated whenever called via back references. For example, `/(... )+\1/` cannot match `123 aaa 123 `, but will match the entirety of `123 aaa 123 123 `. `(...)+` acts normally until `\1` is reached. The capturing group initially picks up `123 `, but is updated after `aaa `, meaning that `\1` searches for that instead of `123 `.
