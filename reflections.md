1.Which issues were the easiest to fix, and which were the hardest? Why?

the easiest was reducing code size by removing else after a return ,adding docstrings, replacing range(len(...)) with enumerate,etc.


Renaming the camelCase functions is a one-word fix per function, but it breaks every caller, which was on of the harder fixes

2.Did the static analysis tools report any false positives? If so, describe one example.

Yes. The clearest example is B104 (HOST = "0.0.0.0").

Bandit flags any string literal of "0.0.0.0" as a possible bind to all interfaces, which would expose a service on every network interface

even though no socket is every created


3.How would you integrate static analysis tools into your actual software development workflow?

Run pylint  and flake8 inside the IDE. Style issues and obvious bugs, like the bare except or the unused import, show up as you type

4.What tangible improvements did you observe in the code quality, readability, or potential robustness after applying the fixes?
Readability:
Every function has a docstring and a snake_case name, so the module reads as one consistent style and a newcomer can see what each function does without reading its body.

Robustness:
renewal_due no longer crashes on a 29 Feb start date. It falls back to 28 Feb.
