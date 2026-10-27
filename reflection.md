# Reflection

**1. Which issues were the easiest to fix, and which were the hardest?**
The easiest were the style issues: trailing whitespace, long lines, the unused `summary` variable and the missing docstrings. They are mechanical and don't change behaviour. The hardest was the SQL injection in `addStation`, not because the fix is long but because it needed understanding why string concatenation is dangerous and how parameterized queries separate data from SQL. The inconsistent return in `rentBike` also needed care, since changing `None` to `False` meant updating the check in `main()` so the program still behaved the same.

**2. Did the static analysis tools report any false positives? If so, describe one example.**
Not strictly, but some findings are context-dependent. Bandit's B311 warning on `random.randint` would be harmless in, say, a game or a simulation; it only matters here because the number is used as an unlock PIN. Also, Pylint's line-length limit is 100 while Flake8's is 79, so a line can pass one tool and fail the other, which shows the limits are conventions rather than errors.

**3. How would you integrate static analysis tools into your actual software development workflow?**
Run Flake8 and Pylint automatically as a pre-commit hook so style issues never reach the repository, and add Pylint, Flake8 and Bandit as a step in the CI pipeline (for example a GitHub Actions workflow on every push and pull request), failing the build on any Bandit issue of medium severity or higher, or on a Pylint score below a set threshold. Editor plugins can also show the warnings while typing.

**4. What tangible improvements did you observe in the code quality, readability, or potential robustness after applying the fixes?**
The Pylint score went from 3.89/10 to 10.00/10, and Bandit and Flake8 both report zero issues. Concretely: the code is no longer open to SQL injection, PINs are generated securely, `has_bikes` no longer crashes for a station that doesn't exist, `add_station` no longer leaks names between calls, and database errors are reported instead of silently swallowed. The snake_case names and docstrings make each function's purpose clear at a glance, and the `__main__` guard means the module can be imported without running the demo.
