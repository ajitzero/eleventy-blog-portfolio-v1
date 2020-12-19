---
title: Replacing substrings with JavaScript
description: Dealing with a minor, unintuitive feature
date: 2020-06-01
isHighlight: true
tags:
- notes
- javascript
---

If you've ever had to quickly look up how to replace all occurences of a word or pattern within a string, you've probably come across [`String.prototype.replace()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace "MDN Link"). It's easy to assume that it'll work as advertised, except that we've all skipped reading the docs and just copied the syntax.

The catch: this method only changes the *first* occurence of the pattern.

The method expects two parameters: the substring to be replaced (say, `target`), and the substring to replace it with (say, `replacement`). What often passes unnoticed during development or during code reviews is the `target`.

Here, `target` can either be a string or a regular expression.

If we want to replace all occurences of the pattern, we need to pass a *regular expression* with the global flag enabled. This flag matches with all occurences of that pattern. All standard regex rules still apply, so always [cross-check](https://regex101.com/) [your](https://regextester.com) [pattern](https://regexr.com/).

``` js/8/3
// Example:
let phrase = "some cool words and some not so cool ones";

let newPhrase = phrase.replace("cool", "nice");
console.log(newPhrase);
// -> 'some nice words and some not so cool ones'

let regexp = /cool/g;
let newPhrase = phrase.replace(regexp, "nice");
console.log(newPhrase);
// -> 'some nice words and some not so nice ones'
```

But this is still prone to mistakes.

With a cursory glance, it is not immediately clear whether we are replacing all occurences or not. A simple change (or lack there of) from `target` to `/target/g` is not immediately apparent to a team member unfamiliar with the exact specification or those who jump in between cross-functional teams & various languages.

The better alternative with modern JavaScript is to use [`String.prototype.replaceAll()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll "MDN Link"). While `replace()` has been supported from the beginning, `replaceAll()` is a [recent addition](https://v8.dev/features/string-replaceall).

It has the exact same API as `replace()`, with some minor changes like requiring the RegExp (if used) to have the global flag enabled.

``` js/7
// Example:
let phrase = "some cool words and some not so cool ones";

let newPhrase = phrase.replace(/cool/g, "nice");
console.log(newPhrase);
// -> 'some nice words and some not so nice ones'

let newPhrase = phrase.replaceAll(/cool/g, "nice");
console.log(newPhrase);
// -> 'some nice words and some not so nice ones'
```

Readability improves with a simple addition of three characters.

No support for IE11 though. Naturally.
