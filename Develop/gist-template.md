# Understanding Regex: By a Beginner, For Beginners

Are you diving into the world of web development like I am? If so, you've probably stumbled upon this thing called regex, or regular expression. Your first thought might be a confused '...Huh?...What?' Honestly, mood.

In this tutorial, I'll guide you through the rage-inducing world of regex, breaking down its components into simple, digestible chunks. I'll also throw in a bonus because I'm so generous: deciphering a regex used for matching emails. 

Let's get started!

## Summary

A regex, or regular expression, is a sequence of characters that forms a search pattern. It's used mainly for pattern matching within strings or large bodies of text. It allows you to specify conditions or patterns that are then used to find, extract, or manipulate text you've targeted.

For this specific tutorial, we'll be deciphering a regex that is used to validate whether a string of text is a email or not:

`/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`

Before we start though, it's important to understand the common components used in creating a regex.

## Table of Contents
- [Regex Components](#regex-components)
     - [Anchors](#anchors)
     - [Quantifiers](#quantifiers)
     - [Grouping Constructs](#grouping-constructs)
     - [Bracket Expressions](#bracket-expressions)
     - [Character Classes](#character-classes)
     - [The OR Operator](#the-or-operator)
     - [Flags](#flags)
     - [Character Escapes](#character-escapes)
- [Deciphering Time](#deciphering-time)
- [Closing](#closing)
- [Resources](#resources)

## Regex Components

### Anchors

Anchors are special characters that act as constraints, telling the regex where to perform its matching function within a string. It's important to note that they don't match to any actual characters, but instead define the boundaries or positions for the regex to do its matching.

The four most commonly used anchors are:

- **Caret** `^`: Tells the regex to match at the beginning of the string.
     
     Regex: `/^\w/g`
     
     Translation: "Matches any word character at the beginning of the string."

     Example: apples grow from trees

     Result: **a**pples grow from trees

- **Dollar sign** `$`: Tells the regex to match at the end of the string.
     
     Regex: `/\w$/g`
     
     Translation: "Matches any word character at the end of the string."

     Example: apples grow from trees

     Result: apples grow from tree**s**

- **Word boundary** `\b`: Tells the regex to match a position where the word is not surrounded by other characters.
     
     Regex: `/cat\b/g`
     
     Translation: "Matches all occurrences of the word 'cat' that are standalone words."

     Example: cat cats catwoman

     Result: **cat** cats catwoman 

- **Not word boundary** `\B`: Tells the regex to match a position where the word is surrounded by other characters.
     
     Regex: `/cat\B/g`
     
     Translation: "Matches all occurrences of the word 'cat' that are not standalone words."

     Example: cat cats catwoman

     Result: cat **cat**s **cat**woman 


### Quantifiers

Quantifiers tells the regex how many times a particular character or group of characters should appear in the text for a match to occur. They help the regex know if these characters or group of characters should be option, required, or repeated a certain number of times.

- **Asterisk** `*`: Tells the regex to match zero or more occurrences of the preceding character or character group.

     Regex: `/a*/g`
     
     Translation: Match any sequence of zero or more 'a' characters.

     Example: a aa aaa aaa aaaa

     Result: **a** **aa** **aaa** **aaa** **aaaa**

- **Plus** `+`: Tells the regex to match one or more occurrences of the preceding character or character group.

     Regex: `/a+/g`
     
     Translation: Match any sequence of one or more 'a' characters.

     Example: a aa aaa aaa aaaa

     Result: **a** **aa** **aaa** **aaa** **aaaa**

     - You might look at the results of the previous two quantifiers and think "Wait! Don't they both end up looking the same? What's the difference?" The difference lies in the requirement for the preceding character or character group. Specifically, `*` allows **zero** or more occurrences, while `+` requires **one** or more occurrences. Because `*` matches zero or more occurrences, it actually counts any empty string (where the character 'a' has occurred zero times) as a match as well, resulting in double the match. On the other hand, `+` only results in a match if the preceding character or character group is an 'a' as well. An easier way of understanding the difference is that `+` is more strict that `*` when it comes to producing matches.

- **Question mark** `?`: Tells the regex that its preceding character can occur 0 or 1 times, essentially making that character optional. This I find is useful if you want to match words that have alternative spelling.
     
     Regex: `/colou?r/g`
     
     Translation: Match strings that contain the word 'color' followed by zero or one occurrences of the letters 'u'.

     Example: color colour

     Result: **color** **colour**

     - There are words with alternative spelling that will require a more robust regex configuration (e.g. center and centre). But for now the above example should help you in understanding the use of '?'

- **Curly braces** `{}`: Tells the regex to match a specific range or exact number of occurrences of the preceding element, depending on how you configure the quantifier.
     
     Regex: `/a{3}/g`
     
     Translation: Match strings that contain the character 'a' exactly 3 times.

     Example: a aa aaa aaaa aaaaa aaaaaa

     Result: a aa **aaa** **aaa**a **aaa**aa **aaaaaa**

     - Note: Because the last group of 'a's has a length of 6, the matching will occur twice there, resulting in it looking like the whole character group was matched even though we specified the regex to only match 3 times. This is because quantifiers are 'greedy' by default, meaning the regex will attempt to match the maximum number of occurrences.
          
     Regex: `/b{3,}/g`
     
     Translation: Match strings that contain the character 'b' 3 or more times.

     Example: b bb bbb bbbb bbbbb bbbbbb

     Result: b bb **bbb** **bbbb** **bbbbb** **bbbbbb**
          
     Regex: `/c{3,4}/g`
     
     Translation: Match strings that contain the character 'c' with a minimum of 3 times and a maximum of 4 times.

     Example: c cc ccc cccc ccccc cccccc

     Result: c cc **ccc** **cccc** **cccc**c **cccc**cc


### Grouping Constructs

### Bracket Expressions

Bracket expressions refers to any expression enclosed within square brackets that tells the regex what to match. They allow you to manually configure what type of characters to match or not match, and are more customizable than character classes, which we will discuss later.

### Character Classes

Character classes are similar to bracket expressions, but they are predefined patterns so you don't need to wrap them around a '[]'.

Commonly used character classes are:

- **Word** `\w`: Equivalent to `[a-zA-Z0-9_]`. Tells the regex to match any alphanumeric characters and underscores. Note that \w will not match any accented characters or non-roman characters.
     
     Example: abc 123 áéí !@#

     Result: **abc** **123** áéí !@#

- **Not word** `\W`: Equivalent to `[^a-zA-z0-9_]`. Tells the regex to match any characters that are not alphanumeric or underscores.
     
     Example: abc 123 áéí !@#

     Result: abc 123 **áéí** **!@#**
          
     - Note: It will also match the white spaces between the character groups. In the example above, `\W` will result in 9 matches.

- **Digit** `\d`: Equivalent to `[0-9]`. Tells the regex to match any digits.
     
     Example: abc 123 áéí !@#

     Result: abc **123** áéí !@#

- **Not digit** `\D`: Equivalent to `[^0-9]`. Tells the regex to any characters that are not digits.
     
     Example: abc 123 áéí !@#

     Result: **abc** 123 **áéí** **!@#**
          
     - Note: Again, it will also match the white spaces between the character groups. In the example above, `\D` will result in 12 matches.

- **Whitespace** `\s`: Equivalent to `[\t\n\r\f\v]`. Tells the regex to match any whitespace characters (i.e. tab, newline, carriage return, form feed, and vertical tab characters). It's generally preferred to just use \s.
     
     Example: abc 123 áéí !@#

     Result: abc 123 áéí !@#
          
     - Note: You can't see any difference between the example and the result because I can't bold whitespace, but in the example above, `\s` will result in 3 matches.

- **Not whitespace** `\S`: Equivalent to `[^\t\n\r\f\v]`. Tells the regex to match any characters that are not whitespace characters (i.e. tab, newline, carriage return, form feed, and vertical tab characters). It's also generally preferred to just use \S.

     Example: abc 123 áéí !@#

     Result: **abc** **123** **áéí** **!@#**

You can wrap character classes inside a bracket expression to for further customization. For example, if you wrap /s and /S inside a bracket, the regex will match EVERYTHING!!! (i.e. `/[\s\S]/g`)

### The OR Operator

The OR operator, denoted by the `|` symbol, give a lits of choices for the regex to match.

For example, if you want write a regex that matches either "cat" or "dog" in a string, you would use `/cat|dog|/g`.

Which would result in:

**cat** bat **dog** cog **dog** log sat **cat**

Interestingly, you're not limited to only two choices. If you want to add in more alternative to the regex, simply add another `|` and then the next alternative e.g. `/cat|dog|log/g`.

Although our email matching regex does not use the OR operator, it's still good to understand what it does in case you ever want to use it in creating future regex.

### Flags

Flags are like instruction you put at the end of the regex to tell it how to behave when matching. There are more than the five I'm going to show you here, but these are the more commonly used ones.

- **Global** `g`: This flag will make the regex find all matches rather than stopping after finding the first match.

- **Insensitive** `i`: This flag allows the regex to find matches regardless of letter case.

- **Multiline** `m`: This flag allows the `^` and `$` anchor to match the start and end of every line.

- **Sticky** `s`: This flag allows the `.` metacharacter to match any character, including newline characters.

- **Unicode** `u`: This flag enables support for Unicode character and properties.

You are not limited to just using one flag when creating you regex. You can stick them all in your regex and it'll still work (e.g. `/a*/gimsu`). But do make sure the flags serves the purpose of your regex or you'll just be wasting you precious key presses.


### Character Escapes

## Deciphering Time

## Closing

## Resources

## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)
