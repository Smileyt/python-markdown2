# Note: This project has been moved to [trentm/python-markdown2 on Github](https://github.com/trentm/python-markdown2). #

This extra **disables** the use of leading and trailing and (most importantly) **intra-word** emphasis and strong using underscores. These can easily get in the way when writing docs about source code with `variables_like_this` or (common in Python) like the following: `_my_internal_var`, `_version_`, `__init__`, `__repr__`, etc.

Module usage:

```
>>> markdown2.markdown("use 'self.this_long_attr' for the *real* mccoy")
"<p>use 'self.this<em>long</em>attr' for the <em>real</em> mccoy</p>\n"
>>> markdown2.markdown("use 'self.this_long_attr' for the *real* mccoy",
...                    extras=["code-friendly"])
"<p>use 'self.this_long_attr' for the <em>real</em> mccoy</p>\n"
```

Command-line usage:

```
$ cat foo.txt
use 'self.this_long_attr' for the *real* mccoy
$ markdown2 -x code-friendly
<p>use 'self.this<em>long</em>attr' for the <em>real</em> mccoy</p>
```

## Discussion ##

It is quite common for other Markdown implementations to disable intra-word emphasis either by default or with an extra. Examples are Stack Overflow, Github Flavored Markdown and PHP Markdown Extra -- though their details differ slightly (see [issue 38](https://code.google.com/p/python-markdown2/issues/detail?id=38) for some quick details). At the time of this writing python-markdown2 doesn't have a facility that disables **just** intra-word emphasis like some of the above. I may add one as part of [issue 38](https://code.google.com/p/python-markdown2/issues/detail?id=38).

(Return to [Extras](Extras.md) page.)