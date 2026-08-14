# Bug Reports — SauceDemo

Found through exploratory testing using SauceDemo's seeded test accounts (`problem_user`, `performance_glitch_user`, `visual_user`), which intentionally contain UI/behavior bugs for QA practice. Format mirrors how these would be logged in JIRA.

---

## BUG-001: Product images do not match product names

**Environment:** saucedemo.com, `problem_user` account, Chrome (latest), Desktop

**Severity:** Medium
**Priority:** High

**Steps to Reproduce:**
1. Go to saucedemo.com
2. Log in with username `problem_user`, password `secret_sauce`
3. Observe the product listing (inventory) page

**Expected Result:**
Each product should display its own correct image (e.g., backpack shows a backpack, t-shirt shows a t-shirt).

**Actual Result:**
Multiple products display the same incorrect image, making it impossible for a user to visually distinguish items on the listing page.

**Notes:**
Purely visual/data-mapping issue — underlying add-to-cart and pricing functionality is unaffected. Recommend checking the image-to-SKU mapping in the product catalog data.

---

## BUG-002: Inventory page load time is significantly delayed

**Environment:** saucedemo.com, `performance_glitch_user` account, Chrome (latest), Desktop

**Severity:** Medium
**Priority:** Medium

**Steps to Reproduce:**
1. Go to saucedemo.com
2. Log in with username `performance_glitch_user`, password `secret_sauce`
3. Note the time from clicking "Login" to the product listing page becoming interactive

**Expected Result:**
Page should load and become interactive within a couple of seconds, consistent with the `standard_user` experience.

**Actual Result:**
Noticeably longer load time before the inventory page renders and becomes usable, compared to `standard_user` under the same network conditions.

**Notes:**
Worth flagging to the dev team for a performance profile — could point to an inefficient query or blocking call on that user's session/data path.

---

## BUG-003: Visual/layout inconsistencies on inventory and cart pages

**Environment:** saucedemo.com, `visual_user` account, Chrome (latest), Desktop

**Severity:** Low
**Priority:** Low

**Steps to Reproduce:**
1. Go to saucedemo.com
2. Log in with username `visual_user`, password `secret_sauce`
3. Browse the inventory page and open the cart

**Expected Result:**
Layout, spacing, and alignment of product cards and cart items should match the `standard_user` experience.

**Actual Result:**
Visible layout inconsistencies (element positioning/spacing) compared to the standard layout — cosmetic only, no functional impact on add-to-cart or checkout.

**Notes:**
Low priority since it doesn't block any user flow, but worth a design/CSS review pass since it affects perceived polish.
