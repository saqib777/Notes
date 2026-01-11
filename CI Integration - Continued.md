# CI Integration – Day 2

## Focus of the Day

Today’s session was about **moving from “CI works” to “tests are structured like a real framework.”**

We intentionally did not add new CI tools or complexity. Instead, we focused on understanding **how test frameworks are built, why they break, and how CI exposes structural mistakes early**.

This day was about *design*, not speed.

---

## 1. Where We Started Today

At the start of the day:

* CI pipeline was already working
* Tests were passing
* Repository had a clean base structure
* GitHub Actions was stable

This meant we were ready to move beyond basics and focus on **test architecture**.

---

## 2. What “Framework” Means (Clarified)

A framework is **not**:

* a library
* a tool
* a folder name

A framework **is**:

* agreed structure
* separation of concerns
* reusable setup
* predictable behavior

The goal is:

> Make tests easy to write, easy to read, and hard to misuse.

---

## 3. Introducing `conftest.py`

We introduced a new file:

```
tests/conftest.py
```

### What `conftest.py` Is

* A special pytest file
* Automatically discovered by pytest
* Holds shared setup logic
* Does not need to be imported in tests

### Why It Exists

Without `conftest.py`:

* Setup logic is duplicated
* Tests become noisy
* Maintenance becomes painful

With `conftest.py`:

* Setup is centralized
* Tests focus only on assertions
* Reuse becomes natural

---

## 4. Pytest Fixtures (First Real Framework Concept)

We added a fixture:

```python
import pytest

@pytest.fixture
def numbers():
    return 2, 3
```

### What a Fixture Does

* Provides reusable test data
* Manages setup automatically
* Can be injected into tests by name

Fixtures solve the problem of **repeated setup logic**.

---

## 5. Refactoring Tests to Use Fixtures

Before:

```python
assert add(2, 3) == 5
```

After:

```python
def test_add(numbers):
    a, b = numbers
    assert add(a, b) == 5
```

### Why This Matters

* Test intent becomes clearer
* Setup is separated from behavior
* Tests scale without duplication

This is the **first real step from scripts to framework**.

---

## 6. Introducing `src/` and `tests/` Structure

We formalized the project layout:

```
src/
  calculator.py

tests/
  test_calculator.py
  conftest.py
```

### Why This Structure Is Used

* Separates application code from tests
* Mirrors real-world projects
* Works cleanly with CI and pytest

---

## 7. The Failure That Happened (Important)

After restructuring, CI started failing ❌.

This was intentional and valuable.

### What Failed

* Tests could not import application code
* CI turned red immediately

### Key Insight

> CI environments are clean. They do not assume anything about your local machine.

What worked locally failed in CI.

That is exactly what CI is supposed to catch.

---

## 8. Root Cause Explained Clearly

Python did not know where `src` was.

Even though the folder existed, Python did not treat it as a package.

This is **not a CI issue**.
This is a **Python project structure issue**.

---

## 9. Correct Fix (Industry Standard)

### Step 1: Make `src` a Package

We added:

```
src/__init__.py
```

This tells Python:

> “This folder is a package.”

---

### Step 2: Configure Pytest Import Path

We added a root-level file:

```
pytest.ini
```

With content:

```ini
[pytest]
pythonpath = .
```

This tells pytest:

> “Treat the project root as part of Python’s path.”

This is clean, explicit, and scalable.

---

## 10. Why CI Failed Before and Passed After

Before:

* Python could not resolve imports
* Tests failed
* CI blocked the change

After:

* Import paths were explicit
* Project structure was correct
* CI turned green again

This shows CI acting as a **safety gate**, not an obstacle.

---

## 11. Commands Used Today

| Command        | Purpose                         |
| -------------- | ------------------------------- |
| `del filename` | Delete files locally on Windows |
| `git add -u`   | Stage deletions                 |
| `git status`   | Inspect tracked changes         |
| `git commit`   | Record changes                  |
| `git push`     | Trigger CI pipeline             |

---

## 12. What We Learned Today (Key Takeaways)

By the end of today, you learned:

* What a test framework really is
* Why fixtures matter
* How `conftest.py` works
* Why CI catches structural problems
* How Python import paths affect CI
* Why configuration is part of framework design

Most importantly:

> **CI does not just test code. It tests assumptions.**

Anything that is unclear, implicit, or environment-dependent will eventually fail in CI.

That is a feature, not a flaw.

---

## End State

You now have:

* A structured Python project
* Reusable test setup
* Explicit configuration
* A CI pipeline that enforces correctness

This is the real foundation of professional test automation.
