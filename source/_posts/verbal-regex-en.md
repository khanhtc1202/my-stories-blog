title: VerbalExpressions - RegularExpression made easy
tags: technical
thumbnail: 'https://i.imgur.com/nMveuD7.png'
date: 2018-08-12 08:00:19
---
When you start learning a new programming language, maybe you had been learning follow those steps: variable, assignment, string, operators... One major theme you need to focus is `string operations`. Fox example: get first name from fullname, find and censor all mobile numbers in message,...

Along the operation we usually need to process some common procedure. One repeated procedure is finding a substring and implement some operations over the substring. Maybe you had done like something like this in the very beginning of your learning path.

```C
int checkMatchStubPattern(char* string) {
    for(int i = 0; i < strlen(string); i++) {
        // logic for checking string pattern
        ...
    }
    return ...
}
```
<!-- more -->

Not a wrong way, but a time consuming. You must change the checking logic in every case. More code, more bug and of course hard for maintainance. Luckily, `Regular Expression - Regex` come as a hero to solve those kind of problems: find, input validation... As a confirmation for the usage of `Regex`, every programming language supports `Regex` for string operations.

>Regular expression is a sequence of characters that define a search pattern. This pattern is then used by string searching algorithms for "find" or "find and replace" operations on strings, or for input validation. (Source: Wikipedia)

`Regex` is an efficient tool to solve that problem; but it comes at a price. It is really hard to read and understand (but *not hard to learn*). First try to read the example below.

![Check whether a string is a valid url or not](https://i.imgur.com/nMveuD7.png)

Because of complicated syntax, it is very hard to read and understand `Regex`. Furthermore, you seems not to work with `Regex` too often. ROI (return-on-investment) is too low; almost common `Regex` you need to use can be found on the internet (password, url, IP address,...). Are you willing to spend some weeks for learning something that you only use 4 or 5 times a year? Or just skimming over some sites for the result in around 5 minutes? That way of thinking make developers tend to google some `Regex` and modify to fit into their needs. Sometime it can cost some hours to a whole day for the repetition process of searching - modifying...

![In every angel a demon hides...](https://i.imgur.com/j3G9xyP.png)

`Regex` solve the `string operation` problem; but how about the problem of `Regex`? Fortunately, it can be solved with [`Verbal Expressions`](http://verbalexpressions.github.io/). Try to look at this example.

```javascript
VerEx()
.startOfLine()
.then('http')
.maybe('s')
.then('://')
.anythingBut(' ')
.endOfLine();
``` 

I hope that you will not be frustrated after reading this example. The `Verbal Expressions` above is defined following this rule:
- The URL must start with either "http" or "https".
- The URL must then have "://".
- The URL can have anything following "://", as long as is not a space.

The generated `Regex` from the above code is: `/^(?:http)(?:s)?(?:\:\/\/)(?:[^ ]*)$/`. A bit diffenret but the functional is the same. You can find the implementation of `Verbal Expressions` in several languages [here](http://verbalexpressions.github.io/).

VerbalExpressions solve the biggest probelem of `Regex`. It is readable and easy-to-understand regular expressions. In my opinion, the transition from `Regex` to `VerbalExpressions` is great as the movement from `SQL` to `ORM`.  

Anyways, `VerbalExpressions` still have some drawbacks. You need to install a new library to your project, sometimes it is quite painful (e.g. you client, manager... don't think it's neccessary). In that case, you can go to [VerbalRegex](https://verbalregex.com/); write the code and it will generate the `Regex` for you.

![Online tool for generating Regex](https://i.imgur.com/w0uMzYE.png)

Try this tool by accessing [verbalregex.com](https://verbalregex.com/).

### Conclusion

`VerbalExpressions` is not a replacement of `Regex`; but an easy way to write readable `Regex`. It can ease the pain of `Regex`, and actually make writing expressions fun again. But keep in mind that `Regex` still seems to be the best choice in some complicated cases.