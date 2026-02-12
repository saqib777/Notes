# CI Dependency Installation & Test Execution - Clear Notes

## 1. Why This Topic Matters

Understanding *when* and *how often* dependencies are installed is critical to understanding CI correctly. Many beginners assume that dependencies are installed for every test or every test case. That assumption leads to confusion about performance, failures, and optimization.

This note clarifies exactly what happens in our current setup and why it behaves that way.

---

## 2. Key Terms (Clarified First)

### Test

A test is a single test function, take an  example:

```python
def test_add_parametized():
    ...
```

A test validates one behavior.

---

### Test Run

A test run is **one execution of the test runner** (pytest).

Example:

```
pytest
```

During a single test run:

* all test files are collected
* all test functions are executed
* parametrized tests run multiple times

---

### CI Run (Pipeline Run)

A CI run is **one full execution of the CI pipeline**, triggered by an event such as:

* pushing code to a branch
* opening a pull request

A CI run includes:

* setting up a machine
* installing dependencies
* running tests

---

## 3. What Happens in Our CI Pipeline

Every time code is pushed to the repository, GitHub Actions performs the following steps:

1. Creates a **brand-new virtual machine**
2. The machine starts completely empty
3. Checks out the repository code
4. Installs Python
5. Installs dependencies (`pytest` in our case)
6. Executes `pytest`
7. Reports success or failure

After the run finishes, the machine is destroyed.

---

## 4. Do Dependencies Get Installed Every Time?

### Answer: Yes, per CI run

Because each CI run uses a fresh machine:

* dependencies are downloaded again
* nothing is reused from previous runs

This is expected and intentional behavior.

---

## 5. What Does NOT Happen (Common Misconceptions)

Dependencies are **not** installed:

* for every test
* for every test file
* for every parametrized case

Inside one CI run:

* dependencies are installed once
* pytest executes all tests using the same environment

Parametrized tests do **not** trigger reinstallations.

---

## 6. Why CI Uses Fresh Environments

Fresh environments ensure:

* no leftover state
* no hidden dependencies
* reproducible results
* elimination of "works on my machine" issues

This design prioritizes **correctness and reliability over speed**.

---

## 7. Fixtures vs Dependency Installation

Fixtures:

* prepare test data
* manage setup and teardown
* run inside pytest

Fixtures do **not**:

* install dependencies
* modify the environment globally

Fixtures operate entirely *after* dependencies are installed.

---

## 8. Performance Considerations

For small projects:

* reinstalling dependencies is fast
* simplicity is preferred

For large projects:

* dependency installation can slow CI
* teams introduce caching strategies

Caching is an optimization step, not a requirement.

---

## 9. Professional Rule of Thumb

1. Make CI correct
2. Make CI reliable
3. Make CI fast

Optimizing too early often hides problems.

---

## 10. Final Takeaway

In our current setup:

* dependencies are installed once per CI run
* tests run in a single environment
* parametrization increases coverage, not setup cost

This behavior is **exactly what we want** at this learning stage and reflects real-world CI systems.

Understanding this distinction prevents confusion and builds correct mental models for scaling CI later.
