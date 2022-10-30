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

### Bracket Expressions

### Greedy and Lazy Match

### Boundaries

### Back-references

### Look-ahead and Look-behind

## Author

If you liked this gist you can fork it or star it on it's github repository. You can also find me on [github](https://github.com/AbdalehHersi)