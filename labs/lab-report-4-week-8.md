# lab report 4 [week 8]

Due: 2/25/2022  
## **Testing on JUnit**
(Instructions from [here](https://ucsd-cse15l-w22.github.io/week/week8/#week-8-lab-report).)

Testing these three markdown snippets on `MarkdownParse.java`:

### Snippet 1
Contents:
```
`[a link`](url.com)

[another link](`google.com)`

[`cod[e`](google.com)

[`code]`](ucsd.edu)
```

What should `MarkdownParse.java` output for this file?

Using the VSCode preview:

    `[a link`](url.com)
    - not a link

    [another link](`google.com)`
    - "another link" that links to '`google.com'

    [`cod[e`](google.com)
    - "cod[e" that links to 'google.com'

    [`code]`](ucsd.edu)
    - "code]" that links to 'ucsd.edu'

Added test:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab4/1testcode.PNG?raw=true)

**My group's implementation**

Test output:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab4/1testresultConnor.PNG?raw=true)



**Other group's implementation**

Test output:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab4/1testresultOther.PNG?raw=true)

## Snippet 2
```
[a [nested link](a.com)](b.com)

[a nested parenthesized url](a.com(()))

[some escaped \[ brackets \]](example.com)
```
What should `MarkdownParse.java` output for this file?

Using the VSCode preview:

    [a [nested link](a.com)](b.com)
    - "nested link" that links to 'a.com'

    [a nested parenthesized url](a.com(()))
    - "a nested parenthesized url" that links to 'a.com(())'

    [some escaped \[ brackets \]](example.com)
    - "some escaped [ brackets ]" that links to 'example.com'

Added test:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab4/2testcode.PNG?raw=true)

**My group's implementation**

Test output:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab4/2testresultConnor.PNG?raw=true)



**Other group's implementation**

Test output:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab4/2testresultOther.PNG?raw=true)



## Snippet 3
What should `MarkdownParse.java` output for this file?

```
[this title text is really long and takes up more than 
one line

and has some line breaks](
    https://www.twitter.com
)

[this title text is really long and takes up more than 
one line](
    https://ucsd-cse15l-w22.github.io/
)


[this link doesn't have a closing parenthesis](github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



)

And then there's more text
```
What should `MarkdownParse.java` output for this file?

    "https://www.twitter.com" links to 'https://www.twitter.com'

    "this title text is really long and takes up more than one line" links to 'https://ucsd-cse15l-w22.github.io/'

    no link for github.com

    "https://cse.ucsd.edu/" links to 'https://cse.ucsd.edu/'


Added test:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab4/3testcode.PNG?raw=true)

**My group's implementation**

Test output:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab4/3testresultConnor.PNG?raw=true)



**Other group's implementation**

Test output:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab4/3testresultOther.PNG?raw=true)


## Possible fixes

**Snippet 1**

Is there a small code change (<10 lines) that would make our group's program work for Snippet 1 and all related cases w/ inline code (backticks)?
- I think the bug in Snippet 1 could be fixed by checking if there is only one backtick within the [], and not adding the link if that is the case.
- However, I can't say this would account for all cases with backticks; there are more tests that could be done with different numbers of backticks in and outside of []. To account for everything, we'd probably need a bigger code change.

**Snippet 2**

Is there a small code change (<10 lines) that would make our group's program work for all related cases that nest parentheses, brackets, and escaped brackets?
- I think the bug in Snippet 2 could be fixed by using a stack to match parentheses pairs and correctly identify the full link. This could take less than 10 lines. 
- However, more testing needs to be done to account for all related cases of nesting parentheses, brackets, and escaped brackets. There was a link that VScode's preview doesn't identify as a link that was identified as a link. This would need to be accounted for by using matching algorithms for [] and nested ()s and []s.
- I don't think this would take less than 10 lines.

**Snippet 3**

Is there a small code change (<10 lines) that would make our group's program work for Snippet 3 and  all related cases that have newlines in brackets and parentheses?
- There wasn't a single correctly parsed link for Snippet 3, which could point to a bigger change in the code. Some of these links just had too many newlines inside them, which could be solved easily by trimming them down or skipping newlines. However, the github.com link was also parsed in, which it shouldn't have been, due to the next parenthesis being located in a new link. There would need to be a check for a new [, ], (, and ) to disregard the link if they are all found.
- Interestingly, the VScode preview doesn't title these links with the links in the [], but rather just as the URLs themselves, which could be different from expected.
- I think this could be coded in less than 10 lines, but it's unlikely.