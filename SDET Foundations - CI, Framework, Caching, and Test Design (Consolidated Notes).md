# SDET Foundations - CI, Framework, Caching, and Test Design (Consolidated Notes)

These notes capture **everything we studied across the last sessions**, structured the way an SDET is expected to think: from automation mechanics to test design intent. This is not tool-first documentation; it is **principle-first**, with tools supporting those principles.

---

## 1. Big Picture: What We Were Building

The objective was not simply to “set up CI” or “write tests.”

The real objective was to:

* understand how automated tests run in real systems
* design tests that are readable, reliable, and scalable
* integrate those tests into CI so failures give clear signals

By the end of this work, we moved from **scripts** to the beginnings of a **test framework**, enforced by CI.

---

## 2. Continuous Integration (CI)

### Definition 

Continuous Integration is a practice where every code change is automatically:

* integrated
* validated
* tested

This happens through automation triggered by events like pushing code.

CI is not a tool. It is a **process** supported by tools.

---

### What CI Guarantees

CI ensures:

* every change is tested
* failures are caught early
* broken code does not silently move forward
* results are reproducible

CI deliberately favors **correctness over speed**.

---

## 3. GitHub Actions (Our CI Tool)

GitHub Actions is GitHub’s native automation system.

Key components:

* **Workflow**: the configuration file that defines automation
* **Job**: a logical unit of work
* **Step**: an individual action or command
* **Runner**: the machine executing the job

Workflows live inside:

```
.github/workflows/
```

---

## 4. Pipeline vs Workflow vs Test Run

These terms are often confused, so clarity matters.

* **Test**: one test function
* **Test Run**: one execution of pytest (runs all tests)
* **Pipeline / CI Run**: one full workflow execution (fresh machine)

A pipeline contains jobs.
A job contains steps.
A test run happens inside a job.

---

## 5. Fresh Environment Model (Critical Concept)

Every CI run:

* starts with a brand-new virtual machine
* has no memory of previous runs
* installs everything from scratch

This prevents:

* hidden state
* machine-specific bugs
* “works on my machine” problems

---

## 6. Dependency Installation in CI

### What Happens

* dependencies are installed **once per CI run**
* pytest runs all tests in that same environment

### What Does NOT Happen

* dependencies are not installed per test
* dependencies are not installed per parameter

This distinction is essential to understanding performance and correctness.

---

## 7. Dependency Caching

### Why Caching Exists

Reinstalling dependencies every run can be slow in large projects.

Caching allows reuse of downloaded packages **without reusing environments**.

---

### What We Cached

* pip download cache only

We did NOT cache:

* test results
* virtual environments
* Python installations

---

### Cache Safety Principle

1. Fresh environment → correctness
2. Tests every run → reliability
3. Cache downloads → performance

Caching is an optimization, never a shortcut.

---

## 8. Test Framework Foundations

A framework is NOT:

* a library
* a tool
* a folder name

A framework IS:

* structure
* conventions
* reusable setup
* clear intent

Frameworks make tests easy to write, easy to read, and hard to misuse.

---

## 9. Project Structure

We adopted an industry-standard structure:

```
src/
  calculator.py

tests/
  conftest.py
  test_calculator.py

pytest.ini
```

### Why This Matters

* clear separation of code and tests
* scalable layout
* predictable imports

---

## 10. conftest.py

`conftest.py` is a special pytest file that:

* is auto-discovered
* holds shared setup logic
* does not require imports

It is a core building block of test frameworks.

---

## 11. Fixtures

Fixtures:

* provide reusable setup or data
* are injected by name into tests
* separate setup from assertions

Fixtures improve:

* readability
* reuse
* maintainability

---

## 12. Parametrized Tests (Data-Driven Testing)

Parametrization allows:

* one test
* many data sets
* clear coverage

This avoids:

* duplicated tests
* noisy files
* maintenance issues

Parametrization scales naturally to API and UI testing.

---

## 13. Import Failures and CI Feedback

When CI failed after restructuring, it exposed:

* implicit assumptions about import paths
* differences between local and CI environments

This was not a CI bug.
It was a **project structure issue**, correctly caught by CI.

---

## 14. Correct Import Configuration

We fixed imports by:

* making `src` a Python package
* configuring pytest with `pytest.ini`

Explicit configuration is part of framework design.

---

## 15. Test Naming and Intent (Core SDET Skill)

### The Principle

Test names should describe **behavior**, not implementation.

Bad:

```
test_add_parametrized
```

Good:

```
test_add_returns_expected_result_for_various_inputs
```

---

### Why Intent Matters

* tests are read more than written
* CI failures rely on names
* debugging time is reduced

A test name should tell you what broke without opening the test.

---

## 16. SDET Mindset Shift

This work represents a shift from:

* “does the code run?”

to

* “does the system behave correctly under clear conditions?”

SDETs design **signals**, not just scripts.

---

## 17. Commands Used (Conceptual Summary)

| Command              | Purpose                     |
| -------------------- | --------------------------- |
| git clone            | Get code locally            |
| git status           | Inspect changes             |
| git add / git add -u | Stage changes and deletions |
| git commit           | Record intent               |
| git push             | Trigger CI                  |
| git pull             | Sync remote changes         |
| del / ren            | Local file operations       |
| pytest               | Execute test suite          |

---

## 18. What We Learned (Final Takeaways)

By the end of this work, we learned:

* how CI really executes
* how tests are run and grouped
* how frameworks are structured
* how failures provide feedback
* how caching improves performance safely
* how naming improves long-term quality

Most importantly:

> **CI enforces discipline, but test design creates quality.**

This foundation is transferable to:

* API automation
* UI automation (Playwright, Selenium)
* performance testing
* production debugging

---

## End State

You now have:

* a clean CI pipeline
* a structured test framework
* reusable setup
* readable, intent-driven tests

This is a strong SDET foundation and a solid base to build advanced automation on.
