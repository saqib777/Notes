# Page Object Model (POM)

## What POM actually is

Page Object Model (POM) is a **design pattern** used in UI automation to organize code in a way that is readable, maintainable, and scalable. It is not a Selenium feature, not a framework, and not tied to any language. It is simply a way of deciding **where code should live**.

At its core, POM answers one question:

> Who should know how the page works – the test or the page itself?

POM says: **the page should know how it works**.

---

## The problem POM solves

Without POM, automation projects usually suffer from:

* Selenium code duplicated across many tests
* Locators scattered everywhere
* One UI change breaking multiple tests
* Tests that are hard to read and harder to debug

Typical non-POM code mixes everything together:

* Browser actions
* Element locators
* User flow logic
* Assertions

This creates fragile and unmaintainable tests.

---

## The core idea of POM

POM separates responsibilities into two layers:

### 1. Page layer (Page Objects)

* Knows **how the page works**
* Contains locators
* Contains Selenium interactions
* Encapsulates page behavior

### 2. Test layer

* Knows **what the user wants to do**
* Calls page methods
* Contains assertions
* Describes scenarios

This separation keeps tests clean and pages reusable.

---

## What a Page Object is

A Page Object is a **class that represents a page or part of a page**.

It typically contains:

* Element locators
* Methods that interact with the page
* Page-specific behavior

It does NOT contain:

* Assertions
* Test logic
* Test data combinations

Example responsibilities of a page object:

* Open the page
* Enter text into fields
* Click buttons or links
* Return information from the page

---

## What a test should look like with POM

A test written using POM should:

* Read like a user story
* Avoid Selenium calls directly
* Focus on behavior and validation

Good POM tests feel boring – and that is a good thing.

---

## Folder structure in a POM-based project

A clean, professional structure usually looks like this:

* src/

  * pages/            → Page Objects live here
  * utils/            → Helpers, config, utilities

* tests/

  * ui/               → UI tests
  * conftest.py       → Pytest fixtures

* pytest.ini

* requirements.txt

This separation makes it obvious where new code should go.

---

## Why locators belong in Page Objects

Locators are part of **page knowledge**, not test knowledge.

If a locator changes:

* You update one page file
* All tests continue to work

If locators are inside tests:

* Multiple tests break
* Fixing becomes slow and error-prone

POM centralizes change.

---

## Why assertions do NOT belong in Page Objects

Assertions answer the question:

> Did the test succeed or fail?

That is test responsibility, not page responsibility.

Putting assertions inside Page Objects:

* Couples pages to tests
* Reduces reusability
* Makes debugging harder

Pages **act**. Tests **verify**.

---

## Page methods design guidelines

Good page methods:

* Represent meaningful user actions
* Hide Selenium details
* Have clear names

Examples of good page methods:

* search(text)
* login(username, password)
* add_item_to_cart()

Examples of bad page methods:

* click_search_box()
* send_keys_to_input()
* wait_for_element()

Pages should expose **intent**, not mechanics.

---

## BasePage (when and why)

A BasePage is an optional abstraction used when:

* Multiple pages share common behavior
* Common waits, clicks, or utilities are duplicated

BasePage should be introduced only **after repetition is visible**.

Common BasePage responsibilities:

* Generic wait helpers
* Common click / send_keys wrappers
* Utility methods

BasePage should NOT become a dumping ground.

---

## Common POM mistakes (important)

* Creating one Page Object per test
* Putting assertions inside page classes
* Over-abstracting too early
* Creating huge page classes with unrelated actions
* Making Page Objects depend on tests

These mistakes make automation harder, not easier.

---

## Why POM matters in real projects

POM gives you:

* Maintainability
* Readable tests
* Faster fixes when UI changes
* Reusable automation components
* Cleaner collaboration in teams

This is why POM (or a variation of it) is used in nearly every serious UI automation project.

---

## One-line interview-ready definition

> Page Object Model is a design pattern where each page is represented by a class that encapsulates locators and interactions, allowing tests to focus on behavior and assertions.

---

## Final takeaway

POM is not about structure for the sake of structure.

It is about **protecting tests from UI change** and **keeping automation readable as it grows**.

If tests describe *what* the user does, and pages know *how* it is done, POM is working correctly.
