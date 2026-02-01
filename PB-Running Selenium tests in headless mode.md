## Running Selenium tests in headless mode (OS-specific)

### The problem

Running this command failed on Windows:

`HEADLESS=true pytest -v`

Reason: This syntax works on Linux/macOS shells, **not on Windows PowerShell**. PowerShell treats `HEADLESS=true` as a command, not an environment variable.

---

### Correct way on Windows (PowerShell)

Set the environment variable first, then run pytest:

```
$env:HEADLESS="true"; pytest -v
```

This runs all tests **without opening the browser**.

To return to normal (browser visible):

```
Remove-Item Env:HEADLESS
pytest -v
```

---

### Correct way on Linux / macOS

```
HEADLESS=true pytest -v
```

---

### Why this works in our framework

In `conftest.py` we read the environment variable:

```
if os.getenv("HEADLESS") == "true":
    options.add_argument("--headless=new")
```

So:

* `HEADLESS=true` → headless mode ON
* Not set → normal browser mode

---

### Why this matters

* CI pipelines always run headless
* Local debugging can run with UI
* Same codebase, different execution modes

This is a **CI-ready design**, not a workaround.
