# Lab 5: Static Code Analysis — Insurance Premium Calculator

**Your file:** `premium_calculator.py`

Follow the lab handout for the full instructions.

## About the program

An insurance premium calculator. It quotes health, motor and life premiums, works out an applicant's age, saves quotes to a log, imports policies from partner XML, downloads rate tables, extracts customer claim archives, splits premiums into instalments, and computes renewal dates.

The code works, but only just. It has quality, style and security problems of the kind Pylint, Flake8 and Bandit are built to find.

## What to do

1. **Run it first** and read the output carefully:
   ```bash
   python premium_calculator.py
   ```
   Check whether every number and date it prints makes sense, then try changing the inputs in `main()` to see what happens. Not every problem in this file is something a tool will flag, so pay attention to output that looks wrong.

2. **Install the tools:**
   ```bash
   python -m pip install pylint bandit flake8 defusedxml
   ```

3. **Run the analysis** and save the reports:
   ```bash
   python -m pylint premium_calculator.py > pylint_report.txt
   python -m bandit -r premium_calculator.py > bandit_report.txt
   python -m flake8 premium_calculator.py > flake8_report.txt
   ```

4. **Document the issues** in `issues.md` using this table (at least 4 rows, high and medium severity first):

  | Issue | Type | Line(s) | Description | Fix Approach |
|---|---|---|---|---|
| C0114: missing module docstring | Convention (pylint) | 1 | The file had no docstring at the top, so pylint counted one message against the score. | Added a one-line module docstring: `"""Premium calculator and quote book."""`. |
| C0103: invalid function name (11 functions) | Convention (pylint) | each def line | camelCase names such as `calculatePremium` and `saveQuote` break snake_case. | Renamed all 11, for example `calculatePremium` to `calculate_premium`, and updated the callers in `main()`. |
| C0116: missing function docstring (12 functions) | Convention (pylint) | each def line | None of the functions had a docstring. | Added a one-line docstring to each. |
| R1705: unnecessary else after return in monthlyInstalment | Refactor (pylint) | 69-72 | The `else` branch followed a `return`, so the code was nested one level deeper than needed. | Removed the `else:` and de-indented its body. Behaviour is identical. |
| R1705 and R1703: isAuthorised used an if/else to return True or False | Refactor (pylint) | 74-75 | An `if` that returns a `bool` in both branches can be written as the comparison itself. This cleared 2 messages. | Replaced the 4-line `if`/`else` with `return token == UNDERWRITER_TOKEN`. |
| C0200: iterating with range(len(...)) in printQuotes | Convention (pylint) | 85-87 | The loop built a key list and indexed into it, which is the pattern `enumerate` replaces. | Now `for i, (number, quote) in enumerate(quotes.items(), start=1)`. Output is the same. |
| Crash on 29 Feb start dates in renewalDue | Bug fix (logic) | 77-83 | `date.replace(year=...)` raises `ValueError` when the start is 29 Feb and the target year isn't a leap year.  | Wrapped the `replace` in `try/except ValueError` and fell back to 28 Feb. Use `month=3, day=1` if your insurer's rule is 1 March. |
| E722 / W0702 / B112: bare except in importPolicies | Warning (pylint), Bug risk | 55 | `except:` swallows every exception, including `KeyboardInterrupt` and `SystemExit`. | Changed to `except (TypeError, ValueError):`. Malformed age or sum attributes are still skipped, and real errors now surface. |

6. **Fix the code.** Fix at least 4 of the issues you documented, then rerun all three tools to confirm they are gone. Repeat until you are satisfied. The program should still do its job when you are done.

7. **Reflect.** Answer the four reflection questions from the handout in `reflection.md`.

## Deliverables

1. `premium_calculator.py`: your cleaned version, with at least 4 issues fixed.
2. `issues.md`: the completed issue table.
3. `reflection.md`: your answers to the reflection questions.

**Target:** aim for a Pylint score above 9.5 on your final file. A perfect 10.00/10 is very good.

Be ready to explain the bugs you found and how you fixed them. You may be called on to present.
