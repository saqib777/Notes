Skipping tests in Pytest (Why tests still run & how to fix it)

Problem:
Even after adding @pytest.mark.skip, Pytest still runs multiple tests.

Why this happens:
Pytest ONLY skips a test if the skip decorator is applied correctly.
If done wrong, Pytest will still collect and execute the test.

Correct way to skip a test:

1. You MUST import pytest
2. The skip decorator MUST be directly above the test function
3. No blank lines between decorator and function
4. The test name must start with test_

Correct example:

import pytest
from pages.google_home_page import GoogleHomePage

@pytest.mark.skip(reason="Google UI is unstable – kept for learning")
def test_google_image_search(driver):
    ...

Common mistakes:
- Forgetting: import pytest
- Putting @pytest.mark.skip below the function
- Skipping the file name instead of the function
- Running cached tests

Important command to avoid cache issues:
pytest -v --cache-clear

How to verify what Pytest is running:
pytest --collect-only

This shows ALL tests Pytest has discovered.

Key concept (industry practice):
- We do NOT delete flaky tests
- We classify them (skip / flaky / regression)
- We control execution using markers, not deletion

Result after correct skip:
- Stable tests → PASSED
- Flaky tests → SKIPPED
- Framework remains clean and scalable
