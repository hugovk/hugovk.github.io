---
title: "Help test Python 3.15!"
date: "2026-09-01T14:33:38Z"
tags: ["Python", "python3.15", "testing", "ci", "github-actions"]
---

Calling all Python library maintainers! 🐍

The _second and final_ Python 3.15 release candidate is out! 🎉

It's your last chance to join the cool testers' club and say that YOU helped test 3.15
before the big day on 1st October! 😎

[PEP 790](https://peps.python.org/pep-0790/#release-schedule) defines the release
schedule for Python 3.15.0:

- The first release candidate came out on 4th August 2026
- The second release candidate came out on 1st September 2026
- And the full release is set for 1st October 2026

In his
[announcement](https://discuss.python.org/t/python-3-15-0-candidate-2-is-here/108841/1?u=hugovk),
Hugo van Kemenade (👋 hi, that's me!), release manager for Python 3.14 and 3.15, gave a
call to action:

> We _**strongly encourage**_ maintainers of third-party Python projects to prepare
> their projects for 3.15 during this phase, and publish Python 3.15 wheels on PyPI to
> be ready for the final release of 3.15.0, and to help other projects do their own
> testing. Any binary wheels built against Python 3.15.0 release candidates _**will
> work**_ with future versions of Python 3.15. As always, report any issues to
> [the Python bug tracker](https://github.com/python/cpython/issues).

## Test with 3.15

It's now time for us library maintainers to start testing our projects with 3.15.
There's two big benefits:

1. There have been
   [removals and changes](https://docs.python.org/3.15/whatsnew/3.15.html#removed) in
   Python 3.15. Testing now helps us make our code compatible and avoid any big
   surprises (for us and our users) at the big launch in October.

2. We might find bugs in Python itself! Reporting those will help get them fixed and
   help everyone.

## How

To test the latest alpha, beta or release candidate on GitHub Actions with
[actions/setup-python](https://github.com/actions/setup-python#supported-version-syntax),
add `3.15` and `allow-prereleases: true` to your workflow matrix.

For example:

```yml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        python-version: ["3.11", "3.12", "3.13", "3.14", "3.15"]

    steps:
      - uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1

      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@5fda3b95a4ea91299a34e894583c3862153e4b97 # v7.0.0
        with:
          python-version: ${{ matrix.python-version }}
          allow-prereleases: true
```

(We can instead use `3.15-dev` and omit `allow-prereleases: true`, but I find the above
a bit neater, and when 3.15.0 final is released in October, it will continue testing
with full release versions.)

## When to support 3.15?

Now is also a good time to declare support and add the
`Programming Language :: Python :: 3.15`
[Trove classifier](https://pypi.org/classifiers/). Many
[projects already have](https://pyreadiness.org/3.15/)!

Especially if you have extension modules and other projects depend on yours, a release
with binary wheels will help them test and prepare.

### ABI breaks?

From the announcement:

> There will be _**no ABI changes**_ from this point forward in the 3.15 series, and the
> goal is that there will be as few code changes as possible.

Let's start testing 3.15 now! 🚀

## See also

- [What’s New In Python 3.15](https://docs.python.org/3.15/whatsnew/3.15.html)

---

<small>Header photo: A woodcut of a crowned snake in the Museo di Palazzo Poggi,
Bologna, used for Ulisse Aldrovandi's
[_Serpentum, et draconum historiæ libri duo_ (1640)](https://archive.org/details/bub_gb_Ul-fTdRAMIwC/page/n371/mode/1up).</small>
