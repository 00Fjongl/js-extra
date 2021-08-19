# Mixing with HTML
JavaScript can be loaded via script tags or event listeners. Using an image element with the `onload` (or `onerror`) attribute is the most reliable alternative.  
Mixing via attributes introduces the incentive of designing the code in a way that validates without requiring quotes or apostrophes for attribute assignments.  
## Common Alternatives
Standard | Substitute
------ | ----------
`document.documentElement` | `document.all[0]`
