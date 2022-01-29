# lab report 2 [week 4]

Due: 1/28/2022  
### **Debugging `markdown-parse`**
(Instructions from [here](https://ucsd-cse15l-w22.github.io/week/week4/).)

`markdown-parse`: parses a Markdown file for links; attempts to get and print all links from any Markdown file in a String array.
> Reminder: Markdown formats links as follows: \[Display text](Link.com)

## #1

Code change diff: 1DIFF

Failure-inducing input - the test file that prompted the change
- [test-file3.md](https://github.com/jeanniekim/markdown-parse/blob/4922bc11db4e619c447407af78687cc35670972d/test-file3.md)
    - contains `"[]"` with no `"()"` following it.

Symptom of failure-inducing input 1SYM
> ...an IndexOutOfBoundsError, specifically for the last index.

Relationship between bug, symptom, and failure-inducing input:
- The *bug* was in the basic algorithm, which tries to get the index of `"("` when it finds `"]"`
    > Assumes `"("` even exists in the file
- This caused the *symptom*, the `IndexOutOfBoundsError`, because it tried to get the indexOf `"("`
- ... which doesn't exist in the *failure-inducing input* `test-file3.md`.

The fix: We rewrote the algorithm from scratch using a group member's idea. This new code (shown in image above) starts with finding all the indicies for `"("`, so such an error does not exist. If an index for `"("` is not found, nothing happens.


## #2

Code change diff: 2DIFF

Failure-inducing input
- test-break.md
    - contains: `"](what"`

Symptom of failure-inducing input 2SYM

Relationship between bug, symptom, and failure-inducing input
- The *bug* was that the code, for every `"("` preceded by a `"["`, tries to get the indexOf `")"`
    > Assumes a `")"` exists if a `"]("` exists, which isn't always true
- The *symptom* was the `IndexOutOfBoundsError` with a `-1` as its end index (`")"` couldn't be found)
- Once again, `")"` was not present in the *failure-inducing input*

The fix: I modified the code to include a **try-catch** block that tries to get an index for `")"`, catches an IndexOutOfBoundsError, and stops the process to avoid the error.


## #3

Code change diff: 3DIFF

Failure-inducing input - the test file that prompted the change
- test-break2.md
    - contains `"](what [link](yes.com)"`
    - expected output: `[yes.com]`

Symptom of failure-inducing input

> Output link seems to be `"what [link](yes.com"` instead of `"yes.com"`.

Relationship between bug, symptom, and failure-inducing input
- The *bug* is that the code is parses everything from the first valid `"("` it sees to the next `")"` as a link, regardless of its contents
    
- The *symptom*, a clearly invalid link output, results from this bug
    - Note: the first `"("` isn't even part of a valid link (no `"["`), but it is valid in this code's definition; *it is preceded by `"]"`*.
    - It ignores the second valid `"("` and misses the valid link

- This symptom shows because the *failure-inducing input* contains an invalid link followed by a valid one, but the open parenthesis `"("` in the invalid link "pairs" with the `")"` in the valid link.

The fix: I added a link-checking function to getLinks() that would ensure that the links are "valid" to a specific standard. This includes not returning links containing unsafe characters (as described [here](https://tinyurl.com/339ncmvh)). 