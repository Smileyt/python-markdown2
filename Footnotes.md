# Note: This project has been moved to [trentm/python-markdown2 on Github](https://github.com/trentm/python-markdown2). #

(Note: This extra is still a little experimental. The final result will be
almost exactly as now, but I want to make sure the generated output is as close
to other Markdown implementations as reasonable and possible.)

Support for footnotes with the following syntax:

```
This is a paragraph with a footnote. [^note-id]

[^note-id]: This is the text of the note. 
```

Which produces HTML like this:

```
<p>This is a paragraph with a footnote. 
<sup class="footnote-ref" id="fnref-note-id">
<a href="#fn-note-id">1</a></sup> 
</p>

...
<div class="footnotes">
<hr />
<ol>
<li id="fn-note-id">
<p>This is the text of the note.&nbsp;
<a href="#fnref-note-id" class="footnoteBackLink" title="Jump back to footnote 1 in the text.">&#8617;</a></p>
</li>

<li>...for subsequent footnotes
</li>
</ol>
</div>
```


This is as close as I can tell is the favoured output from the following sources:

  * http://daringfireball.net/2005/07/footnotes
  * http://six.pairlist.net/pipermail/markdown-discuss/2005-August/001442.html
  * http://six.pairlist.net/pipermail/markdown-discuss/2005-August/001480.html

Notes:

  * I couldn't tell from the markdown-discuss discussion on footnotes whether John Gruber meant that the current date was added as a suffix to the anchors in his private Markdown.pl or whether he was literally putting in the "YYYY-MM-DD" strings in his markdown text.
  * Daringfireball.net (John Gruber's) site uses the "footnoteBackLink" class on the backref `<a>`. Also, he does **not** add the `class="footnote-ref"` on `<sup>`. Neither of these jive with [what was described here](http://six.pairlist.net/pipermail/markdown-discuss/2005-August/001480.html).
  * Add hooks/variables on the `Markdown` class to allow easy tweaking of this output above.

[Patch](http://code.google.com/p/python-markdown2/issues/detail?id=1) partly
from Adam Gomaa (thanks Adam!).

(Return to [Extras](Extras.md) page.)