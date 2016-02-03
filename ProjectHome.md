# Note: This project has been moved to [trentm/python-markdown2 on Github](https://github.com/trentm/python-markdown2). #

Markdown is a light text markup format and a processor to convert that to HTML. The originator describes it as follows:

> Markdown is a text-to-HTML conversion tool for web writers.
> Markdown allows you to write using an easy-to-read,
> easy-to-write plain text format, then convert it to
> structurally valid XHTML (or HTML).

> -- http://daringfireball.net/projects/markdown/

This project provides a converter written in Python that closely matches the behaviour of the original Perl-implemented Markdown.pl. There is another Python markdown.py (see link at right), but markdown2.py is faster (see [PerformanceNotes](PerformanceNotes.md)) and, to my knowledge, more correct (see [TestingNotes](TestingNotes.md)).


# Basic Python Module Usage #

```
>>> import markdown2
>>> markdown2.markdown("*boo!*")  # or use `html = markdown_path(PATH)`
u'<p><em>boo!</em></p>\n'

>>> markdowner = Markdown()
>>> markdowner.convert("*boo!*")
u'<p><em>boo!</em></p>\n'
>>> markdowner.convert("**boom!**")
u'<p><strong>boom!</strong></p>\n'
```


# Basic Command-Line Usage #

```
$ cat foo.txt
*boo!*
$ python markdown2.py foo.txt
<p><em>boo!</em></p>
$ python markdown2.py -h
...detailed command-line usage...
```


Please see the [Change Log](http://code.google.com/p/python-markdown2/source/browse/trunk/CHANGES.txt) for recent work.