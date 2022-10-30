# Breaking Down and Understanding Regex

When you first encounter regex, it can seem like random gibberish with an awkward syntax. But with time and some logical thinking, you would be suprised by how useful and easy it is to understand (at least at a base level).

## Summary

We'll go through an example and break it down so you can see how all the components come together and function as a whole. The regex we'll use is for matching a valid email: 

```
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### Anchors

Anchors have a special meaning in regular expressions. They don't match any character but instead match a position before or after characters. The two anchors used in the above example are `^` and `$`. You can see how they're used in the beginning and end respectively.

```
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```
The caret anchor `^` is to match the beginning of a string. In this example it's used to make sure only the email is submitted with no whitespaces at the beginning. Similarly the dollar sign anchor `$` matches the end of a string. Here it is used to make sure no whitespaces trail the email. An example of this in action looks like this:

``` js
let string = " Regex example test";

console.log(/^R/.test(string));

// Output: false
```
The output would be false as there is a space before `Regex example test`. However if we were to remove the space we would pass the test.

``` js
let string = "Regex example test";

console.log(/^R/.test(string));

// Output: true
```

You can also use the dollar anchor `$` to match the end of a string. Below you can see how it is used to match lowercase `t` at the end of the text.

```js
let string = "Regex example test";

console.log(/t$/.test(string));

// Output: true
```
Below you can see how this applies to our example.

```js
var email = "testemail@email.com";

console.log(/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/.test(email));

// Output: true

var email2 = " testemail@email.com "; 

console.log(/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/.test(email2));

// Output: false
```

### Quantifiers

Quantifiers specify how many instances of a character, group or a character class must be present for a match to be found. The quantifier used in our example is the `+` quantifier. It matches the preceding element one or more times. For example `/a+/` would match the `a` in `apple` and all `a`'s in `aaaaaaaaaaaaapple`. Here it's used in front of the character classes `[a-z0-9_\.-]` and `[\da-z\.-]` to match for them one or multiple times. 

### OR Operator

Alternation (more commonly known as the OR operator) is denoted with the vertical line charcter `|`. Alternation matches anything as long as it meets one of it's expressions. It's straight forward to apply and easy to grasp. 

```js
const greyText = "grey";

const grayText = "gray";

console.log(/gr(e|a)y/.test(greyText));

// Output: true

console.log(/gr(e|a)y/.test(grayText));

// Output: true
```

Above you can see it matches both strings as it accepts either `grey` or `gray`. Be careful as the brackets affect how it functions. `gre|ay` matches either `gre` or `ay`.

### Character Classes

A character class matches any charcters/elements enclosed in the `[ ]`. Character classes also let you specify a range of characters using a hypen except when you use a hyphen as the first or last character in the character class. In that case it is interpreted as a literal hyphen. `[0-9]` matches for `0`, `1`, `2`, `3`, ... `8`, `9`. Similarly `[a-z]` matches for `a` through `z`. 

However the hyphen behaves a little differently in the example below.

```js
const textExample = "American Red Cross is one of the most popular non-profit organisations."

const regexMatch = /\w+[-]\w+/; 

console.log(text.match(regexMatch)); 

// Output: Array ["non-profit"]
```

You can see how it matches for a literal hyphen in this case.

### Flags

Flags are options that allow for further functionality when matching regex expressions like `global searching` and `case insensitive searching`. Flags can be used seperately or together in any order and are included as part of the regex. For example `/e/g` matches for the letter `e` across the whole string due to the `g` (global) flag.

```js
const text = "The quick brown fox jumped over the lazy dog."

console.log(text.match(/e/)); 

// Output: Array [ "e" ]

console.log(text.match(/e/g)); 

// Output: Array(4) ["e", "e", "e", "e" ]
```

### Grouping and Capturing

Groups group multiple patterns as a whole, and capturing groups provide extra submatch information when using regex to match against a string. Capturing groups match `[x]` and remembers the regex match. The syntax used for capturing groups is `()`. If you match for `/(foo)/`, it not only matches but remembers both `foo` in `foobar` and `foo` in `food`.

```js
const imageDescription = 'This image has a resolution of 1440×900 pixels.';

const regexpSize = /([0-9]+)×([0-9]+)/;

const match = imageDescription.match(regexpSize);

console.log(`Width: ${match[1]} / Height: ${match[2]}.`);

// expected output: "Width: 1440 / Height: 900."
```

Above you can see how it remembers both matches and allows you to reference them separately via match[`1`] and match[`2`]. Let's take a look at how this applies to our example: 
```js
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

There are three capturing groups in our regex with their own self-contained matches.

```js
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
      // [1]         // [2]         // [3]
```

In the example below take note of how there are three different submatches: 

```js
const emailMatch = 'testemail@email.com';

const regexpEmail = /^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/;

const match = emailMatch.match(regexpEmail);

console.log(`Capturing group 1: ${match[1]} / Capturing group 2: ${match[2]} / Capturing group 3: ${match[3]}`); 

// Output: Capturing group 1: testemail / Capturing group 2: email / Capturing group 3: com
```
### Bracket Expressions

There are three types of brackets used in regular expressions. `[]`, `()` and `{}`. Square brackets `[]` represent [charcter classes](#character-classes) while parenthesis represent [grouping and capturing](#grouping-and-capturing). We've already taken a look at both in the sections above. The last one, curly brackets `{}`, is used to specify the amount of times to match for.

```js
"panama".match(/na{2}/) // no matches

"banana".match(/na{2}/) // matches "nana"
```

Depending on what you encase in `{}` you can define a minimum number of matches and a maximum. Let's take a snippet from our example:

```js
/([a-z\.]{2,6})/
```

Here the curly braces is used to match for whatever is in the character classes a minimum of `2` times and a maximum of `6` times.

### Greedy and Lazy Match

By default quantifiers like `*` and `+` are "greedy", meaning they try to match as much of the string as possible. The `?` character after the quantifier makes it "non-greedy" or "lazy" meaning it will match as little as possible.

```js
const text = "some <foo> <bar> new </bar> </foo> thing"; 

const greedyMatch = /<.*>/;

const lazyMatch = /<.*?>/;

console.log(text.match(greedyMatch));

// Output: Array [ "<foo> <bar> new </bar> </foo>" ]

console.log(text.match(lazyMatch));

// Output: Array [ "<foo>" ]
```

The second regex is more inline with what was intended. Instead of matching anything between the first `<` and the last `>` regardless of the content in between, it matches the first two `< >` signs.

### Boundaries

Boundaries indicate the beginning and endings of lines of words. We've already taken a look at two boundary-type assertions, the [^](#anchors) sign and the [$](#anchors) sign, but there are two more options to match boundaries. `\b` matches a word boundary (where a word character is not followed or preceded by another word-character like between a letter, a new space or a new line).

```js
/\bm/  // matches "m" in " moon"
```

To `not` match a word boundary, you could use `\B`. It matches any position that is not a a word boundary.

```js
/\Bm/ // does not match "m" in moon
```

### Back-references

### Look-ahead and Look-behind

## Author

If you liked this gist you can fork it or star it on it's github repository. You can also find me on [github](https://github.com/AbdalehHersi)