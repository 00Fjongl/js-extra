Code | Function | Approximate Ranking by Fastest
---- | -------- | ------------------
`'abc123'.includes('abc')` | Check if `abc123` includes `abc` | 5
`'abc123'.indexOf('abc')+1` | Check if `abc123` includes `abc` | 1
`a=d;b=e;c=f` | Reassign three variables | 2
`[a,b,c]=[d,e,f]` | Reassign three variables | 3
`'test'[Number(1n)]` | Take the value at the index of 1 | 4
`'test'[1n]` | Take the value at the index of 1 | 6

Very much a work-in-progress...

Results are averaged between https://jsbench.me/ and https://jsbench.github.io/.
