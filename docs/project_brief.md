# Project Brief: Patchwork

## What we want
A command-line tool that fixes small bugs in a small Python project without calling any AI model. The user gives it the project folder and a failing pytest test that shows the bug. It finds the most suspicious lines, tries small rule-based fixes and saves the first one that passes every test as a diff file for a person to review.

## Users
A student or developer on a laptop with a small Python project that already has pytest tests. They run it when a test starts failing.

## Must do
1. Take two inputs: a Python project folder path and the name of a failing pytest test. An optional note of 300 characters or fewer is copied into the report.
2. Work on a copy of the project and never change the original folder.
3. Run the test suite once and stop with a clear message if the named test does not fail.
4. Record which lines each test runs and score every line with the Ochiai formula, so lines run by the failing test and by few passing tests rank highest.
5. Try six fix templates on the 20 highest-ranked lines, one change at a time: swap a comparison operator, swap an arithmetic operator, shift a number or range bound by 1, negate a condition, swap "and" with "or", and add a guard for an empty or None input.
6. Rerun the full suite after each change. A fix counts only when the named test passes and every test that passed before still passes. Otherwise report "not fixed" and the lines tried.
7. Save a unified diff file plus a short report: ranked lines with scores, the template used, candidates tried and seconds taken.
8. Stop after 200 candidates or 120 seconds and still save the report.
9. Include an evaluation script that runs the tool on a sample project with 10 seeded bugs and prints a table: bug ID, fixed or not, template, candidates, seconds.

## Should do
- Open a pull request on a new branch instead of only saving a diff.
- Grow the sample project from 10 seeded bugs to 15.

## Constraints
- Platform / technology: Python 3.12 .py files using only the standard library plus pytest. One terminal command runs it. No web server, no database.
- No AI at run time: the app never calls a model and needs no API key. The same input always gives the same output.
- Data: nothing leaves the user's computer. Results stay in a local output folder.
- Cost: $0 per use.
- Safety: the only command the tool runs is pytest, and it never writes outside the copy.
- Accessibility / privacy / legal: output is plain text, so screen readers work. MIT license, like se-agent.

## Out of scope
Languages other than Python, projects with no tests or more than 50 Python files, bugs that need more than one change, writing the failing test for the user, a web or chat interface, and more than one failing test per run.

## Quality expectations
Each step (copy, run tests, record lines, score, apply a template, check, report) is its own small function with one or two inputs and outputs, a docstring and a unit test of what the docstring promises. Each fix template is a separate function, so adding one never touches the others.

## Acceptance criteria
- On the sample project, the failing test for "average() crashes on an empty list" gives a ranked line list, a patch and a diff where all tests pass.
- After any run, the original project folder is unchanged.
- Running the same input twice gives the same diff.
- Two of the 10 seeded bugs are kinds no template covers. For those, the tool reports "not fixed" and still saves its report.
- The evaluation script prints a results table for all 10 seeded bugs.

## Open questions
- None for the instructor.
- Decided: the team writes the sample buggy project by hand, and a pull request stays a "Should do."
- For the team: how many of the 10 seeded bugs the six templates fix. The evaluation table answers it.
