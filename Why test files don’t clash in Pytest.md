Why test files don’t clash in Pytest

Pytest only executes:
- files starting with: test_
- functions starting with: test_

Everything else (pages, utils, helpers) is NOT executed directly.

Example:
- src/pages/google_home_page.py   → never run by pytest
- src/pages/dynamic_loading_page.py → never run by pytest
- tests/ui/test_google_images.py  → runs ONLY if pytest collects it
- tests/ui/test_dynamic_loading.py → runs ONLY if pytest collects it

Conclusion:
- Multiple page files can coexist safely
- Multiple test files can coexist safely
- No clash happens unless:
  1. Two test files have the same name
  2. Imports are wrong
  3. You explicitly run a specific test file

Industry practice:
- Keep unstable tests (Google) for learning
- Keep stable tests (Dynamic Loading) for CI
- Control execution using markers, not deletion
