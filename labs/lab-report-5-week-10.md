# lab report 5 [week 10]

Due: 3/11/2022  
## **Testing many files; comparing programs**
(Instructions from [here](https://ucsd-cse15l-w22.github.io/week/week10/#lab-report-5).)

### Background

**Working with MarkdownParse**

My group's implementation: **markdown-parse-Connor**
> Thank you, Connor Huang!

Joe's implementation: **markdown-parse-Joe/markdown-parse**
> Cloned from his repository

**Tests:**

652 commonmark-spec files
> Cloned from Joe, who found it on CommonMark

**Running all these test files through MarkdownParse:**

File `script.sh`:
```
for file in test-files/*.md;
do
  echo $file
  java MarkdownParse $file
done
```
> for loop going through every file in test-files folder with .md extension


Command:
```
$ bash script.sh > results.txt
```
> saves the terminal results printed from running `bash script.sh` to `results.txt` file

----
## Finding 2 commonmark test files with different results
Using the test `results.txt` files in Connor's and Joe's folders...

Command to show differences between two files
```
$ diff [first file] [second file]
```
Command run:
```
$ diff markdown-parse-Connor/results.txt markdown-parse-Joe/markdown-parse/results.txt
```
### First file:
In the `diff` output:
```
862c862
< [/my uri]
---
> []
```
- Means on line 862 in both txt files...
    - Group's implementation returned `[/my uri]`
    - Joe's implementation returned `[]`

Line 816-862 in group's `results.txt`:
```
test-files/487.md
[/my uri]
```
Line 816-862 in Joe's `results.txt`:
```
test-files/487.md
[]
```

According to `script.sh`, the name of the test file is printed before its result through MarkdownParse.

So, the first file I've chosen is **`487.md`**:

```
[link](/my uri)
```

### Second file:
In the `diff` output:
```
1043c1040
< [not a link]
---
> []
```
- Means on line 1043 in the first file and 1040 in the second file...
    - Group's implementation returned `[not a link]`
    - Joe's implementation returned `[]`

Line 1043 in group's `results.txt`:
```
test-files/567.md
[not a link]
```
Line 1040 in Joe's `results.txt`:
```
test-files/567.md
[]
```

According to `script.sh`, the name of the test file is printed before its result through MarkdownParse. The file `567.md` caused different outputs.

So, the second file I've chosen is **`567.md`**:

```
[foo](not a link)

[foo]: /url1
```

## Choosing which output is the correct one

### File 1 (`487.md`):
```
[link](/my uri)
```
According to the VScode preview:
- There are **no links**.
    - All the text is shown normally on the page.

Therefore, Joe's implementation is correct, and my group's is not correct.
- Joe's returned no links.
- The group's returned `/my uri`.

### File 2 (`567.md`):
```
[foo](not a link)

[foo]: /url1
```
The name `not a link` seems to be telling.
According to the VScode preview:
- There is one link: **`/url1`**

Therefore, neither implementation is correct.
- The group's returned `not a url`.
- Joe's returned no links.

## Identifying the bugs

### File 1 (`487.md`):

The group's returned `/my uri`, which is not a valid link. To identify that this is not a valid link, a case could be added to the implementation that discards any links beginning with `/`.

In addition, the invalid link has a space in it, which is another case that could eliminate a possible link.
- This is notably how Joe's implementation seems to account for this:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab5/file1joe.PNG?raw=true)

This fix could take place here in the code by adding an if statement:
![Image](https://github.com/jeanniekim/cse15l-lab-reports/blob/main/assets/images/lab5/file1connor.PNG?raw=true)

### File 2 (`567.md`):

Neither implementation was correct. 

The group's implementation returns `not a link` which, once again, is invalid, presumably because it contains spaces. A similar fix as the one from File 1 could be done here, to eliminate this link.

Both implementations failed to include the link `/url1`. This link seems to be in a different format than the one we were accounting for in the past few weeks — it's in an entirely different format:
```
[foo]: /url1
```
Whereas the format we have been working with the past weeks is more like:
```
[foo](url1)
```

Therefore, it makes sense that both implementations would fail to account for this as a link.

To fix this, a lot of code (about the same length as the current code) would have to be written to account for a link occuring in this other format.

This could be written to sense a `:` directly after `]`, followed by a `/`, which is then followed by the link.

There isn't a specific place in either file where this code would be most effective; it would be an entirely new case, so a lot would have to be changed.