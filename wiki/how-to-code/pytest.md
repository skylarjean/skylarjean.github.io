---
title: Pytest
date: 2026-07-22
description: The magic words to get pytest to work
tags:
  - pytest
  - python
  - code-development
importance: 5
abstract: "Leave empty if you don't want to feature an abstract. "
---

[Pytest Docs](https://docs.pytest.org/en/stable/)
## Running Tests

`pytest -Werror` runs all pytests and shows the errors

`pytest --cov=. --cov-report=html` Runs all pytests and generates a coverage report. You can then open `htmlcov/index.html` to view the coverage report. You can click on the files to see what specifically is covered in each file.

`pytest tests/your_tests.py` runs just the specified file