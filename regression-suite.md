# Regression Test Suite — SauceDemo

Purpose: a lightweight regression suite covering the core user flows that should be re-verified before any release — login, product browsing, cart, and checkout. Based on test cases from [test-cases-testrail.md](./test-cases-testrail.md).

## Scope

| Module | Test Case IDs | # of Cases |
|---|---|---|
| Login | TC-001 – TC-004 | 4 |
| Product listing & sort | TC-005 – TC-009 | 5 |
| Cart | TC-010, TC-012, TC-013 | 3 |
| Checkout | TC-010, TC-011 | 2 |

*(Product listing cases TC-005–TC-009 not shown in full here — kept out of this excerpt for brevity; same format as the login/checkout suite.)*

## Sample Regression Run — v1.0 build

| Test Case | Title | Result | Notes |
|---|---|---|---|
| TC-001 | Successful login with standard user | ✅ Pass | |
| TC-002 | Login blocked for locked-out user | ✅ Pass | |
| TC-003 | Login fails with incorrect password | ✅ Pass | |
| TC-004 | Login fails with empty fields | ✅ Pass | |
| TC-010 | Complete checkout with single item | ✅ Pass | |
| TC-011 | Checkout blocked with missing required field | ✅ Pass | |
| TC-012 | Cart total matches sum of item prices | ✅ Pass | |
| TC-013 | Remove item from cart before checkout | ✅ Pass | |

**Run summary:** 8/8 passed, 0 failed, 0 blocked. Executed manually against saucedemo.com, standard_user account, Chrome (latest).

## How I'd track this in TestRail

In a real project this suite would live as a Test Suite in TestRail with:
- A **Test Run** created per release/build
- Each case marked Pass/Fail/Blocked/Retest with timestamp and tester
- Failed cases auto-linked to a new JIRA bug via the TestRail–JIRA integration
- A milestone view to track regression pass rate release over release
