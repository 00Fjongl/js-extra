### Things to Avoid
Aside from reducing access to the HTML DOM and the number of statements within a loop, there are a number of notable mistakes to avoid. These may conflict with certain minification methods.  
  
Whenever a variable is assigned, some memory is siphoned off and allocated to it. The time required for memory allocation should roughly scale with the size of the object. Assigning multiple values to variables may reduce execution speed.  
  
Template literals used with grave accent marks, or backticks, are theoretically slower than the ordinary string. With template literals, the string is searched for `${}`, in addition to anything a regular string is searched for.  
While the difference is not extreme, they should be avoided if they do not save any space.
  
Tagged templates, on the other hand, have proven to be a significantly slower replacement for strings. For optimization, they should only be used if the `raw` property or interpolated expression arguments are needed.  
  
Regular expressions tend to be slower than several of the other search functions combined, as they are created as objects. They can replace large groups of `.indexOf` and other search methods, but should not see frequent usage.
