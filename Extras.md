# Note: This project has been moved to [trentm/python-markdown2 on Github](https://github.com/trentm/python-markdown2). #

By default `markdown2.py`'s processing attempts to produce output exactly as
defined by http://daringfireball.net/projects/markdown/syntax -- the "Markdown
core." However, a few optional extras are also provided.


# Implemented Extras #

  * [CodeFriendly](CodeFriendly.md): Disable `_` and `__` for `em` and `strong`.
  * [Footnotes](Footnotes.md): support footnotes as in use on daringfireball.net and implemented in other Markdown processors (tho not in Markdown.pl v1.0.1).
  * [CodeColor](CodeColor.md): Pygments-based syntax coloring of `<code>` sections.
  * [LinkPatterns](LinkPatterns.md): Auto-link given regex patterns in text (e.g. bug number references, revision number references).
  * [CuddledLists](CuddledLists.md): Allow lists to be cuddled to the preceding paragraph.
  * pyshell: Treats unindented Python interactive shell sessions as `<code>` blocks. (TODO: wiki page for this)
  * xml: Passes one-liner processing instructions and namespaced XML tags. (TODO: wiki page for this)


# How to turn on extras #

Extras are all **off** by default and turned on as follows on the command line:

```
python markdown2.py --extras name1,name2 ...
```

and via the module interface:

```
>>> import markdown2
>>> html = markdown2.markdown_path(path, ..., extras=["name1", "name2"])
>>> html = markdown2.markdown("some markdown", ..., extras=["name1", "name2"])

>>> markdowner = Markdown(..., extras=["name1", "name2"])
>>> markdowner.convert("*boo!*")
<em>boo!</em>
```

(**New in v1.0.1.2**) You can also now specify extras via the "markdown-extras"
[emacs-style local variable](http://www.gnu.org/software/emacs/manual/html_node/emacs/Specifying-File-Variables.html) in the markdown text:

```
<!-- markdown-extras: code-friendly, footnotes -->
This markdown text will be converted with the "code-friendly" and "footnotes"
extras enabled.
```

or:

```
This markdown text will be converted with the "code-friendly" and "footnotes"
extras enabled.

<!--
  Local Variables:
  markdown-extras: code-friendly, footnotes
  End:
  -->
```



# extras TODO #

  * smartypants (http://daringfireball.net/projects/smartypants/)
  * table of contents
  * see a lot more in [TODO.txt](http://python-markdown2.googlecode.com/svn/trunk/TODO.txt)