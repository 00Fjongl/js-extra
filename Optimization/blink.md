### Things to Avoid
###### Aside from reducing access to the HTML DOM and the number of statements within a loop, there are a number of notable mistakes to avoid. These may conflict with certain minification methods.  
  
Whenever a variable is assigned, some memory is siphoned off and allocated to it. The time required for memory allocation should roughly scale with the size of the object. Assigning multiple values to variables may reduce execution speed.  
  
Template literals used with grave accent marks, or backticks, are theoretically slower than the ordinary string. With template literals, the string is searched for `${}`, in addition to anything a regular string is searched for.  
While the difference is not extreme, they should be avoided if they do not save any space.
  
Tagged templates, on the other hand, have proven to be a significantly slower replacement for strings. For optimization, they should only be used if the `raw` property or interpolated expression arguments are needed.  
  
Regular expressions tend to be slower than several of the other search functions combined, as they are created as objects. They can replace large groups of `.indexOf` and other search methods, but should not see frequent usage.  
  
Avoid `x.startsWith` altogether, when `!x.indexOf` can be used instead. The latter is somehow faster, and renders `.startsWith` obsolete.  
  
Self-invoking functions lead to memory leaks, and will exceed the call stack limit in extreme cases. If they do not finish promptly, steer away from them. `for` loops, `while` loops, and `.forEach` loops have more reliable use cases. Self-invoking functions tend to be slowest, whereas a `while` loop tends to be fastest.  
  
Division and remainder operators are noticeably slower than other operators. Instead of using something like `a/b<b`, use `a<b*b`.  
  
When it comes to long loops, the standard `for` loop will perform better than looping with `TypedArray.from`, `Array.from`, `.forEach`, `.map`, `for...of`, `for...in`, and `.replace`. The standard `while` loop will perform equally as well as the standard `for` loop in most cases. When it comes to optimizing the condition, if there is a property of an object being called or retrieved for each time the condition is evaluated, it should instead be assigned to a variable outside of the loop beforehand. Assigning it to a variable prevents it from having to attempt to reupdate the value of the condition.  
  
### Quick Operations
When dealing with whole numbers, bitwise operators are generally faster than many of the presented alternatives, although addition and subtraction may be faster when performing all operations with `BigInt` values. The left shift operator is a quicker alternative to multiplying with two raised to another power. Similarly, the right shift operator is a quicker alternative to dividing by a power of two, although any decimals in either operand or the evaluated result will be truncated and ignored.  
  
Operations with ordinary numbers, whether floating point numbers or integers, are faster than operations with `BigInt` values. Keep in mind that prefixes for integer literals, which consist of the `0b`, `0o`, and `0x` prefixes for binary, octal, and hexadecimal representations, respectively, are faster than using `parseInt` with their corresponding radices/radixes. In order to use them with variables, a setup such as `Number('0b'+variable)`, `+('0b'+variable)`, or `BigInt('0b'+variable)` should serve as a significantly faster, albeit less flexible, alternative to the `parseInt` function. Keep in mind that there are no other simple ways to perform such conversions with `BigInt` values, as the `parseInt` function only accepts the ordinary integers.
##### More `BigInt` Magic
As a result of many `BigInt` operations turning out to be slower than operations with ordinary numbers, the speed difference between some specific operators becomes more significant, which may affect which ones are to be avoided. The increment and decrement operators should be avoided in most cases that rely on `BigInt`, as they are even slower than the `+=1n` and `-=1n` alternatives. Addition using large integers is generally faster than multiplication with smaller integers, as the multiplication essentially applies addition multiple times, instead of one single time. In other words, if there is a way to represent a number as a sum with only two operands, then that should be favored over a product with only two operands. These properties continue to hold true for extremely large values.  
  
For bitwise operations, applying `BigInt.asUintN` or `BigInt.asIntN` with a specified bit length will be faster than using the bitwise AND operator (`&`), although it will only work as a replacement if the other operand would not have a `0` in any of its bits for the specified length. Additionally, storing the function as a variable will actually increase the execution speed, if it is to be called multiple times.  
  
In cases where `BigInt` values are being used to index an array or a string via bracket notation, they should first be converted into ordinary integers. This can be done by calling the `Number` function with a `BigInt` value as the first argument (e.g. `'string'[Number(5n)]`). The reason why this is faster is because JavaScript attempts to stringify the `BigInt` value when indexing, which is slower than turning it into a regular integer. As no instance of JavaScript in the near future should be supporting array or string lengths which exceed `Number.MAX_SAFE_INTEGER`, there isn't much of a reason to not apply this.
