# CI Integration From Scratch - Detailed Notes

## 1. What We Were Trying to Achieve

The goal of this exercise was to understand **Continuous Integration (CI)** from the ground up, without shortcuts, templates, or prebuilt frameworks. Instead of starting with a large project, we intentionally created a very small project and slowly layered CI on top of it. This approach helped make every moving part visible and understandable.

By the end of the session, the objective was not just to "have CI working," but to **understand why it works, how it reacts to changes, and how it protects code quality**.

---

## 2. Key Concepts and Definitions

### Continuous Integration (CI)

Continuous Integration is a practice where code changes are automatically built and tested every time they are pushed to a shared repository. The idea is simple: integrate code frequently and validate it automatically so problems are caught early.

In practical terms, CI means:

* Every push triggers an automated process
* Tests are run without manual effort
* Failures are visible immediately
* Broken code is stopped early

CI is not a tool. It is a **process** supported by tools.

---

### GitHub Actions

GitHub Actions is GitHub’s built-in CI/CD system. It allows you to define automation rules using YAML files stored inside your repository.

GitHub Actions provides:

* Runners (virtual machines that execute jobs)
* Prebuilt actions (like checking out code or installing Python)
* Event-based triggers (push, pull request, etc.)

---

### Workflow

A workflow is a configuration file that defines **when** and **how** automation should run. In GitHub Actions, workflows live inside:

```
.github/workflows/

```

A workflow answers these questions:

* What event should trigger it?
* What jobs should run?
* In what environment?
* What steps should be executed?

Our workflow file was named `ci.yml`.

---

### Pipeline

A pipeline is the full sequence of automated steps executed by CI. While GitHub uses the term "workflow," the industry often calls this entire flow a **pipeline**.

In our case, the pipeline was:

* Checkout code
* Set up Python
* Install dependencies
* Run tests

---

### Job

A job is a logical unit inside a workflow. Each job runs on its own machine.

Our workflow had one job:

* `test`

---

### Step

Steps are individual actions inside a job. They execute in order.

Examples from our pipeline:

* Checkout code
* Install Python
* Install pytest
* Run pytest

---

### Runner

A runner is the machine that executes the job. We used:

* `ubuntu-latest`

This means GitHub created a fresh Linux machine every time the workflow ran.

---

### Framework (in context)

A framework is **not a single tool**. It is a structured way of organizing code, tests, and execution rules.

In this session, we did not use a heavy framework. Instead, we built a **minimal structure**:

```
src/
  calculator.py
tests/
  test_calculator.py
```

This is the foundation of a testing framework.

---

### pytest

pytest is a Python testing tool that automatically discovers and runs tests.

Key features we relied on:

* Auto-discovery of test files
* Simple assertions
* Clear failure output

---

## 3. Project Structure Explained

Final structure:

```
CI-integration-Project/
├── .github/
│   └── workflows/
│       └── ci.yml
├── src/
│   └── calculator.py
├── tests/
│   └── test_calculator.py
└── README.md
```

Why this works well:

* Clear separation of code and tests
* CI configuration isolated
* Scales naturally for larger projects

---

## 4. GitHub Actions Workflow Explained

Key parts of `ci.yml`:

* `name`: Friendly name of the pipeline
* `on: push`: Triggers workflow on every push to main
* `jobs`: Defines what runs
* `runs-on`: Specifies environment
* `steps`: Commands and actions executed

The workflow is declarative, meaning we describe **what should happen**, not how to provision machines manually.

---

## 5. Commands Used – Detailed Table

| Command              | What It Does and Why It Matters                              |
| -------------------- | ------------------------------------------------------------ |
| `git clone`          | Downloads the remote repository to the local machine         |
| `git status`         | Shows the current state of files (modified, deleted, staged) |
| `git add .`          | Stages new and modified files                                |
| `git add -u`         | Stages deleted files                                         |
| `git commit`         | Records changes into Git history                             |
| `git push`           | Sends commits to GitHub                                      |
| `git pull`           | Fetches and merges remote changes                            |
| `ren github .github` | Renames folder in Windows CMD                                |
| `del filename`       | Deletes a file locally in Windows CMD                        |
| `pytest`             | Executes all tests automatically                             |

---

## 6. Timeline of What We Did (With Context)

**Start of Session**

* Created a new GitHub repository
* Started with only a README

**Initial Code Setup**

* Added a simple Python function
* Added a basic test

**CI Setup**

* Created GitHub Actions workflow
* Initially placed workflow in wrong folder
* Learned that `.github` is mandatory

**Problem Encountered**

* GitHub Actions did not trigger
* Discovered folder naming issue

**Fix Applied**

* Renamed `github` to `.github`
* Committed fix
* CI triggered successfully

**Validation Phase**

* Confirmed green pipeline
* Understood job and step execution

**Failure Simulation**

* Intentionally broke a test
* Observed red pipeline
* Read CI logs

**Recovery**

* Fixed test
* CI returned to green

**Refactoring**

* Introduced `src/` and `tests/` structure
* Deleted old files
* Learned about staging deletions

**Final State**

* Clean repository
* Structured code
* Fully working CI pipeline

---

## 7. Common Issues Faced and What They Taught Us

* Wrong folder name: CI is extremely strict
* Push rejected: Git protects remote history
* Files not deleted on GitHub: Deletions must be committed
* CI failures: Failures are feedback, not errors

---

## 8. What We Learned (Summary)

By the end of this session, we learned:

* How CI works internally
* How GitHub Actions detects workflows
* How pipelines are triggered
* How failures protect code quality
* How structure affects scalability
* How Git tracks deletions

Most importantly, we learned that **CI is not magic**. It is a predictable, rule-based system that becomes powerful once understood.

This project now serves as a clean, reusable reference for CI fundamentals and a strong base for future framework expansion.
