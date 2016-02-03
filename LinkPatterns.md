# Note: This project has been moved to [trentm/python-markdown2 on Github](https://github.com/trentm/python-markdown2). #

The "link-patterns" extra enables mapping of given regex patterns in text to
links. The `link_patterns` list on the `Markdown` class provides
the mapping.

For example, the following with map "recipe NNNN" and "komodo bug NNNN" to
appropriate links:

```
>>> import markdown2, re
>>> link_patterns = [
...    (re.compile("recipe\s+(\d+)", re.I), r"http://code.activestate.com/recipes/\1/"),
...    (re.compile("(?:komodo\s+)?bug\s+(\d+)", re.I), r"http://bugs.activestate.com/show_bug.cgi?id=\1"),
... ]
>>> markdown2.markdown('Recipe 123 and Komodo bug 234 are related.'
...     extras=["link-patterns"], link_patterns=link_patterns)
'<p><a href="http://code.activestate.com/recipes/123/">Recipe 123</a> and \
<a href="http://bugs.activestate.com/show_bug.cgi?id=234">Komodo bug 234</a> are related.</p>'
```

Here is a script that uses link-patterns to auto-link WikiWords (for Wiki syntaxes that do that):

http://code.google.com/p/python-markdown2/source/browse/trunk/sandbox/wiki.py


## link patterns file ##

For the command-line interface, the `--link-patterns-file` option has been
added.  A "link patterns file" has one link pattern per line of the form:

```
<regex-pattern> <href-pattern>
```

For example:

```
/recipe\s+(\d+)/i               http://code.activestate.com/recipes/\1/
/(?:komodo\s+)?bug\s+(\d+)/i    http://bugs.activestate.com/show_bug.cgi?id=\1
```

Spaces are not allowed in the `<href-pattern>` (to simplify parsing). Lines
beginning with a hash (#) are comments.

```
$ python markdown2.py -x link-patterns --link-patterns-file patterns foo.text
```


(Return to [Extras](Extras.md) page.)