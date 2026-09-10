---
title: "Soft-deprecating re.match()"
date: "2026-09-10T08:08:01Z"
tags: ["Python", "re", "regex"]
---

Quick, without looking it up, what does `re.match()` do? Which of these return a match?

```python
import re
re.match("pi", "pi")
re.match("pi", "pie")
re.match("pi", "api")
re.match("pi", "magpie")
```

How does it compare to `re.search()` and `re.fullmatch()`?

Whilst you're (quickly) thinking about it, let's introduce _soft deprecation_.

## Soft deprecation

Python's backwards compatibility policy (PEP 387) introduced
[soft deprecation](https://peps.python.org/pep-0387/#soft-deprecation) in 2023:

> A soft deprecation can be used when using an API which should no longer be used to
> write new code, but it remains safe to continue using it in existing code. The API
> remains documented and tested, but will not be developed further (no enhancement).

A soft deprecation does not imply future removal of the API, nor does it issue a
warning. It's a docs-only recommendation to not use an API, ideally with a suggested
replacement.

It's a completely separate decision whether, if ever, to turn a soft deprecation into a
regular "hard" deprecation (where removal may follow); soft deprecations don't
"graduate" into regular deprecations or removals.

## `re.match()`

Now the answer:

```python
>>> import re
>>> re.match("pi", "pi")      # ✅ Matches
<re.Match object; span=(0, 2), match='pi'>
>>> re.match("pi", "pie")     # ✅ Matches
<re.Match object; span=(0, 2), match='pi'>
>>> re.match("pi", "api")     # ❌ No match
>>> re.match("pi", "magpie")  # ❌ No match
>>>
```

So [`re.match()`](https://docs.python.org/3/library/re.html#re.match) only matches at
the _beginning_ of a string! This can be surprising: why is the start of the string
special?

## `re.search()`

If you don't want to anchor at the start, and want to match anywhere in the string, use
[`re.search()`](https://docs.python.org/3/library/re.html#re.search):

```python
>>> import re
>>> re.search("pi", "pi")      # ✅ Matches
<re.Match object; span=(0, 2), match='pi'>
>>> re.search("pi", "pie")     # ✅ Matches
<re.Match object; span=(0, 2), match='pi'>
>>> re.search("pi", "api")     # ✅ Matches
<re.Match object; span=(1, 3), match='pi'>
>>> re.search("pi", "magpie")  # ✅ Matches
<re.Match object; span=(3, 5), match='pi'>
>>>
```

## `re.fullmatch()`

If you want to anchor both the start and the end, and check the entire string matches,
use [`re.fullmatch()`](https://docs.python.org/3/library/re.html#re.fullmatch):

```python
>>> import re
>>> re.fullmatch("pi", "pi")      # ✅ Matches
<re.Match object; span=(0, 2), match='pi'>
>>> re.fullmatch("pi", "api")     # ❌ No match
>>> re.fullmatch("pi", "pie")     # ❌ No match
>>> re.fullmatch("pi", "magpie")  # ❌ No match
>>>
```

## Introducing `re.prefixmatch()`

Because of the surprising half-anchored behaviour,
[we've introduced a new alias for `re.match()` in Python 3.15](https://docs.python.org/3.15/library/re.html#prefixmatch-vs-match),
named [`re.prefixmatch()`](https://docs.python.org/3.15/library/re.html#re.prefixmatch):

> Quoting from the Zen Of Python (`python3 -m this`): _“Explicit is better than
> implicit”._ Anyone reading the name `prefixmatch()` is likely to understand the
> intended semantics. When reading `match()` there remains a seed of doubt about the
> intended behavior to anyone not already familiar with this old Python gotcha.

## Soft-deprecating `re.match()`

And with a more explicit replacement, we've
[soft-deprecated `re.match()` in Python 3.15](https://docs.python.org/3.15/library/re.html#prefixmatch-vs-match):

> We do not plan to remove the older `match()` name, as it has been used in code for
> over 30 years. It has been
> [soft deprecated](https://docs.python.org/3/glossary.html#term-soft-deprecated): code
> supporting older versions of Python should continue to use `match()`, while new code
> should prefer `prefixmatch()`.

Use `re.prefixmatch()` if you only really meant to use the half-anchor; otherwise use
`re.search()` or `re.fullmatch()`.

## Comparison

| Function           | Start anchor | End anchor | Added in | With [special characters](https://docs.python.org/3/library/re.html#regular-expression-syntax) |
| ------------------ | ------------ | ---------- | -------- | ---------------------------------------------------------------------------------------------- |
| `re.search()`      | ❌           | ❌         | 1.5      | `re.search("pi", string)`                                                                      |
| `re.match()`       | ✅           | ❌         | 1.5      | `re.search("^pi", string)`<br>`re.search(r"\Api", string)`                                     |
| `re.prefixmatch()` | ✅           | ❌         | 3.15     | `re.search("^pi", string)`<br>`re.search(r"\Api", string)`                                     |
| `re.fullmatch()`   | ✅           | ✅         | 3.4      | `re.search("^pi$", string)`<br>`re.search(r"\Api\z", string)`                                  |

The functions without special characters are generally a bit faster.

## See also

- Seth Larson: [Use “\A...\z”, not
  “^...$” with Python regular expressions](https://sethmlarson.dev/use-backslash-A-and-z-not-%5E-and-$-with-python-regular-expressions)

---

<small>Header photo: Double-exposure bike and pedestrian stencils
(<a target="_blank" rel="noopener noreferrer" href="https://creativecommons.org/licenses/by-nc-sa/2.0/">CC
BY-NC-SA 2.0</a>
[Hugo van Kemenade](https://www.flickr.com/photos/hugovk/26955055864/)).</small>
