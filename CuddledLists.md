# Note: This project has been moved to [trentm/python-markdown2 on Github](https://github.com/trentm/python-markdown2). #

This extra allows a list to be cuddled to a preceding paragraph. (Normally a blank line is necessary between a leading paragraph and a bulleted list.) For example, with this extra, this text:

```
I did these things:
* bullet1
* bullet2
* bullet3
```

becomes this:

```
<p>I did these things:</p>

<ul>
<li>bullet1</li>
<li>bullet2</li>
<li>bullet3</li>
</ul>
```

instead of the normal:

```
<p>I did these things:
* bullet1
* bullet2
* bullet3</p>
```

Example module usage:

```
>>> markdown2.markdown(text, extras=["cuddled-lists"])
```

Example command-line usage:

```
$ python markdown2.py -x cuddled-lists FOO.txt
```

(Return to [Extras](Extras.md) page.)