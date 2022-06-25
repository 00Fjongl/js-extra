# js-extra
A nonstandard guide to reformatting JavaScript code.
Currently a work-in-progress.  
  
Contents:  
[Regular expressions](Debugging/RegExp.md)  
[Numerical rounding](Debugging/num.md)  
[Basic minification](Minification/basic.md)  
[Further minification](Minification/mini.md)  
[Site minification](Minification/page.md)  
[Optimization](Optimization/blink.md)  
[Speed comparisons](Optimization/speed.md)  
  
Note: Minification and optimization styles are specifically structured to align with strict mode.  
Strict mode can be enabled by executing `"use strict";` or `'use strict';`, but not ````use strict`;``` at the beginning of a script, an HTML event listener attribute, a bookmarklet, or a new function scope.  
Enabling strict mode will allow JavaScript interpreters to use more speed optimizations that will improve performance.
