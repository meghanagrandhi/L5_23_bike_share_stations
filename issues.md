# Issues Found and Fixed — bike_share_stations.py

Before: Pylint 3.89/10, Bandit 3 issues, Flake8 10 issues.
After: Pylint 10.00/10, Bandit 0 issues, Flake8 0 issues.

| Issue | Type | Line(s) | Description | Fix Approach |
|-------|------|---------|-------------|--------------|
| SQL injection (Bandit B608, Medium) | Security | 9 | `addStation` builds the INSERT query by string concatenation, so a station name like `x'); DROP TABLE stations;--` could alter the query | Use a parameterized query with `?` placeholders and pass `(name, capacity, bikes)` as a tuple |
| Mutable default argument (Pylint W0102) | Bug | 8 | `log=[]` is created once and shared by every call, so names from earlier calls leak into later ones | Default to `None` and create a new list inside the function |
| Insecure random for PIN (Bandit B311, Low) | Security | 22 | `random.randint` is predictable and unsuitable for an unlock PIN | Use `secrets.randbelow(9000) + 1000` |
| Try/except/pass with broad exception (Bandit B110, Pylint W0718) | Bug / Security | 32–35 | `except Exception: pass` hides every error silently, including real database failures | Catch only `sqlite3.Error` and print a message instead of passing |
| Crash on missing station + `== None` (Pylint C0121, Flake8 E711) | Bug | 39–40 | `hasBikes` indexes `row[0]` without checking `row`, so an unknown id raises `TypeError`; also compares with `== None` | Check `row is None or row[0] is None` first |
| Inconsistent return (Pylint R1710) | Bug | 14–18 | `rentBike` returns `True` or falls through to `None` | Add explicit `return False`; update `main()` to test `is False` |
| Unused variable (Pylint W0612, Flake8 F841) | Code quality | 27 | `summary` is computed and never used | Removed the line |
| Non-snake_case names (Pylint C0103) | Style | 8, 14, 21, 25, 31, 38 | `addStation`, `rentBike`, etc. break PEP 8 naming | Renamed to `add_station`, `rent_bike`, `unlock_pin`, `full_stations`, `reset_station`, `has_bikes` |
| Missing docstrings (Pylint C0114/C0116) | Style | 1 and every function | No module or function docstrings | Added a one-line docstring to the module and each function |
| Module-level `main()` call | Code quality | 53 | Importing the module would run the demo | Wrapped in `if __name__ == "__main__":` |
| Long lines (Flake8 E501, Pylint C0301) | Style | 5, 9, 15, 17, 26, 33, 39 | Lines over 79 characters | Wrapped SQL strings and arguments across lines |
| Trailing whitespace (Flake8 W291, Pylint C0303) | Style | 27 | Spaces at end of line | Removed (line deleted with `summary`) |
