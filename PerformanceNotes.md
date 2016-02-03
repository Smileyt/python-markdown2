# Note: This project has been moved to [trentm/python-markdown2 on Github](https://github.com/trentm/python-markdown2). #

The markdown2 sources have a `perf` sub-directory with some performance tools
for profiling markdown2 and comparing its speed against markdown.py and
Markdown.pl.


# The performance "suites" #

There are two sets of "suites" (sets of .text files to convert) that I've been
using for testing. One is a copy of all the test cases from the `test` dir (see
[TestingNotes](TestingNotes.md)). The second is a .text file for every description and comment
in the [Python Cookbook](http://code.activestate.com/langs/python/). To generate the performance suites run:

```
cd perf
python gen_perf_cases.py
```

I'd be very interested in other real-world performance suites. Please
[log an issue](http://code.google.com/p/python-markdown2/issues/entry?summary=idea%20for%20new%20perf%20suite) if you have a suggestion for another perf suite that I should
check.


# Timing comparisons #

WARNING: `markdown.py` has gone through some significant work since I ran these numbers. I haven't followed the recent `markdown.py` work and haven't re-run the perf suite yet.

Currently I compare the processing time against
[Markdown.pl](http://daringfireball.net/projects/markdown/) and
[markdown.py](http://www.freewisdom.org/projects/python-markdown/Features). I'd
like to compare against [markdown.php](http://www.michelf.com/projects/php-markdown/) but I'd appreciate someone's help on doing for `markdown.php`
what [perf/Markdown.pm](http://python-markdown2.googlecode.com/svn/trunk/perf/Markdown.pm) and
[perf/time\_markdown\_pl.pl](http://python-markdown2.googlecode.com/svn/trunk/perf/time_markdown_pl.pl) are for `Markdown.pl` (basically tweaking it to be
runnable from the command line).

Of course, as with all performance comparisons, only the relative numbers are
important. These timings were done on a MacBook (1.82 Ghz Intel Core 2 Duo)
running Mac OS X 10.4.10 with Python 2.5.1:

```
$ python perf.py -r 10 tmp-test-cases 
Time conversion of tmp-test-cases/*.text (plat=darwin):
  Markdown.pl: best of 10: 0.372s
  markdown.py: best of 10: 0.295s
  markdown2.py: best of 10: 0.221s

$ python perf.py tmp-aspn-cases
Time conversion of tmp-aspn-cases/*.text (plat=darwin):
  Markdown.pl: best of 3: 5.842s
We've got a problem header!
We've got a problem header!
We've got a problem header!
We've got a problem header!
We've got a problem header!
We've got a problem header!
  markdown.py: best of 3: 18.920s
  markdown2.py: best of 3: 4.956s
```

The "We've got a problem header!" are warnings spit out by `markdown.py` for
syntax that it cannot handle in some of the ASPN test cases.

# markdown2.py profile #

Here is profiling information for `markdown2.py` to show where the best
possible performance gains could be found. However, I'm content with the
current performance -- at least with the current perf suites. The profiling is
done with [Python 2.5's hotshot module](http://docs.python.org/lib/module-hotshot.html).

```
$ python perf.py --hotshot -i markdown2.py --repeat 5 tmp-test-cases
Profile conversion of tmp-test-cases/*.text (plat=darwin):
  markdown2.py: best of 5: 0.233s

         79855 function calls (77872 primitive calls) in 1.246 CPU seconds

   Ordered by: internal time, call count
   List reduced from 167 to 20 due to restriction <20>

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
      950    0.257    0.000    0.258    0.000 markdown2.py:159(_detab)
      745    0.189    0.000    0.207    0.000 markdown2.py:212(_hash_html_blocks)
     4270    0.078    0.000    0.078    0.000 markdown2.py:880(_encode_backslash_escapes)
     2700    0.058    0.000    0.079    0.000 markdown2.py:800(_do_italics_and_bold)
      465    0.045    0.000    0.065    0.000 markdown2.py:567(_do_headers)
  860/325    0.044    0.000    0.150    0.000 markdown2.py:596(_do_lists)
  465/420    0.040    0.000    0.055    0.000 markdown2.py:839(_do_block_quotes)
  465/280    0.039    0.000    0.779    0.003 markdown2.py:292(_run_block_gamut)
      290    0.034    0.000    0.034    0.000 markdown2.py:930(_unescape_special_chars)
     2700    0.033    0.000    0.344    0.000 markdown2.py:319(_run_span_gamut)
     2700    0.032    0.000    0.033    0.000 markdown2.py:904(_do_auto_links)
     2700    0.029    0.000    0.029    0.000 markdown2.py:415(_do_links)
     2700    0.025    0.000    0.103    0.000 markdown2.py:362(_escape_special_chars)
      465    0.023    0.000    0.049    0.000 markdown2.py:716(_do_code_blocks)
     3040    0.021    0.000    0.029    0.000 re.py:136(sub)
     5139    0.020    0.000    0.076    0.000 re.py:219(_compile)
     2700    0.018    0.000    0.025    0.000 markdown2.py:746(_do_code_spans)
        1    0.018    0.018    1.246    1.246 perf.py:59(time_markdown2_py)
     2835    0.018    0.000    0.018    0.000 markdown2.py:871(_encode_amps_and_angles)
      465    0.016    0.000    0.303    0.001 markdown2.py:845(_form_paragraphs)

$ python perf.py --hotshot -i markdown2.py tmp-aspn-cases
Profile conversion of tmp-aspn-cases/*.text (plat=darwin):
  markdown2.py: best of 1: 5.317s

         359188 function calls (357235 primitive calls) in 5.377 CPU seconds

   Ordered by: internal time, call count
   List reduced from 166 to 20 due to restriction <20>

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
     6161    0.446    0.000    0.464    0.000 markdown2.py:567(_do_headers)
6200/6020    0.372    0.000    0.486    0.000 markdown2.py:596(_do_lists)
        1    0.365    0.365    5.377    5.377 perf.py:59(time_markdown2_py)
    11373    0.358    0.000    0.474    0.000 markdown2.py:800(_do_italics_and_bold)
6161/5756    0.355    0.000    3.960    0.001 markdown2.py:292(_run_block_gamut)
    11635    0.302    0.000    0.302    0.000 markdown2.py:880(_encode_backslash_escapes)
     5757    0.287    0.000    0.287    0.000 markdown2.py:930(_unescape_special_chars)
6161/5897    0.246    0.000    0.311    0.000 markdown2.py:839(_do_block_quotes)
     6161    0.212    0.000    0.377    0.000 markdown2.py:716(_do_code_blocks)
     7852    0.211    0.000    0.213    0.000 markdown2.py:159(_detab)
    11373    0.203    0.000    0.203    0.000 markdown2.py:904(_do_auto_links)
    11917    0.185    0.000    0.220    0.000 markdown2.py:212(_hash_html_blocks)
    41701    0.182    0.000    0.239    0.000 re.py:219(_compile)
     5756    0.156    0.000    4.899    0.001 markdown2.py:114(convert)
    17394    0.153    0.000    0.199    0.000 re.py:136(sub)
     5756    0.144    0.000    0.198    0.000 markdown2.py:252(_strip_link_definitions)
    11373    0.140    0.000    1.668    0.000 markdown2.py:319(_run_span_gamut)
    11373    0.127    0.000    0.128    0.000 markdown2.py:746(_do_code_spans)
    11373    0.107    0.000    0.107    0.000 markdown2.py:871(_encode_amps_and_angles)
     6161    0.102    0.000    0.117    0.000 re.py:154(split)
```


# markdown.py profile #

Here is some profiling info for markdown.py, in case that might be useful for
that implementor:

```
$ python perf.py --hotshot -i markdown.py --repeat 5 tmp-test-cases
Profile conversion of tmp-test-cases/*.text (plat=darwin):
  markdown.py: best of 5: 0.350s

         327998 function calls (302997 primitive calls) in 1.821 CPU seconds

   Ordered by: internal time, call count
   List reduced from 167 to 20 due to restriction <20>

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
88325/77920    0.632    0.000    0.719    0.000 markdown.py:1530(_applyPattern)
3615/3070    0.278    0.000    0.983    0.000 markdown.py:1480(_handleInlineWrapper)
    11100    0.129    0.000    0.129    0.000 markdown.py:451(_isLine)
 6870/485    0.114    0.000    1.225    0.003 markdown.py:1213(_processSection)
 6140/270    0.080    0.000    0.204    0.001 markdown.py:281(toxml)
      280    0.067    0.000    1.739    0.006 markdown.py:1599(convert)
     7925    0.051    0.000    0.051    0.000 markdown.py:183(normalizeEntities)
     7165    0.040    0.000    0.100    0.000 markdown.py:357(toxml)
    88325    0.029    0.000    0.029    0.000 markdown.py:689(getCompiledRegExp)
      270    0.027    0.000    1.465    0.005 markdown.py:1161(_transform)
      690    0.025    0.000    0.033    0.000 markdown.py:915(_findHead)
     7730    0.021    0.000    0.025    0.000 markdown.py:165(createTextNode)
     6140    0.021    0.000    0.037    0.000 markdown.py:158(createElement)
        1    0.020    0.020    1.821    1.821 perf.py:34(time_markdown_py)
      270    0.020    0.000    0.020    0.000 markdown.py:414(run)
      270    0.020    0.000    0.020    0.000 markdown.py:581(run)
     4380    0.019    0.000    0.027    0.000 markdown.py:1410(_linesUntil)
     7185    0.018    0.000    0.018    0.000 markdown.py:354(handleAttributes)
     6140    0.016    0.000    0.016    0.000 markdown.py:219(__init__)
      270    0.015    0.000    0.019    0.000 markdown.py:508(run)

$ python perf.py --hotshot -i markdown.py tmp-aspn-cases
Profile conversion of tmp-aspn-cases/*.text (plat=darwin):
We've got a problem header!
We've got a problem header!
  markdown.py: best of 1: 21.303s

         1464504 function calls (1323926 primitive calls) in 21.359 CPU seconds

   Ordered by: internal time, call count
   List reduced from 164 to 20 due to restriction <20>

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
350801/263536   15.007    0.000   16.187    0.000 markdown.py:1530(_applyPattern)
15884/11305    1.084    0.000   17.072    0.002 markdown.py:1480(_handleInlineWrapper)
    61479    0.896    0.000    0.896    0.000 markdown.py:451(_isLine)
   350801    0.825    0.000    0.825    0.000 markdown.py:689(getCompiledRegExp)
32038/5880    0.449    0.000   17.946    0.003 markdown.py:1213(_processSection)
        1    0.387    0.387   21.359   21.359 perf.py:34(time_markdown_py)
26431/5737    0.357    0.000    1.022    0.000 markdown.py:281(toxml)
    43395    0.293    0.000    0.293    0.000 markdown.py:183(normalizeEntities)
     5737    0.281    0.000   19.691    0.003 markdown.py:1161(_transform)
    37647    0.192    0.000    0.525    0.000 markdown.py:357(toxml)
     5737    0.118    0.000    0.118    0.000 markdown.py:581(run)
     5737    0.114    0.000    0.114    0.000 markdown.py:414(run)
     5737    0.105    0.000    0.107    0.000 markdown.py:508(run)
    37647    0.101    0.000    0.101    0.000 markdown.py:354(handleAttributes)
    42226    0.100    0.000    0.120    0.000 markdown.py:165(createTextNode)
     2075    0.086    0.000    0.143    0.000 markdown.py:915(_findHead)
     5737    0.084    0.000    0.980    0.000 markdown.py:445(run)
    22822    0.075    0.000    0.107    0.000 markdown.py:1410(_linesUntil)
    26431    0.075    0.000    0.075    0.000 markdown.py:219(__init__)
     5756    0.073    0.000   20.827    0.004 markdown.py:1599(convert)
```



