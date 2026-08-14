# Test Cases — SauceDemo (saucedemo.com)

Application under test: [SauceDemo](https://www.saucedemo.com/) — a public demo e-commerce site built by Sauce Labs for QA practice, with seeded test accounts and intentional bugs.

Format below mirrors how these would be entered in TestRail (Title / Preconditions / Steps / Expected Result / Priority).

---

## Suite: Login

### TC-001 — Successful login with standard user
- **Preconditions:** Browser open at saucedemo.com login page
- **Priority:** High
- **Steps:**
  1. Enter username `standard_user`
  2. Enter password `secret_sauce`
  3. Click "Login"
- **Expected Result:** User is redirected to the Products (inventory) page; page title displays "Products"

### TC-002 — Login blocked for locked-out user
- **Preconditions:** Browser open at saucedemo.com login page
- **Priority:** High
- **Steps:**
  1. Enter username `locked_out_user`
  2. Enter password `secret_sauce`
  3. Click "Login"
- **Expected Result:** Login is rejected; error message is displayed indicating the user has been locked out

### TC-003 — Login fails with incorrect password
- **Preconditions:** Browser open at saucedemo.com login page
- **Priority:** Medium
- **Steps:**
  1. Enter username `standard_user`
  2. Enter password `wrong_password`
  3. Click "Login"
- **Expected Result:** Login is rejected; generic "username and password do not match" error is shown (no indication of which field is wrong, for security)

### TC-004 — Login fails with empty fields
- **Preconditions:** Browser open at saucedemo.com login page
- **Priority:** Medium
- **Steps:**
  1. Leave username and password blank
  2. Click "Login"
- **Expected Result:** Error message indicates username is required

---

## Suite: Checkout

### TC-010 — Complete checkout with single item
- **Preconditions:** Logged in as `standard_user`
- **Priority:** High
- **Steps:**
  1. Add "Sauce Labs Backpack" to cart
  2. Open cart, click "Checkout"
  3. Enter First Name, Last Name, Zip/Postal Code
  4. Click "Continue"
  5. Review order summary, click "Finish"
- **Expected Result:** "Thank you for your order" confirmation page is displayed; cart is emptied

### TC-011 — Checkout blocked with missing required field
- **Preconditions:** Logged in as `standard_user`, item in cart, on checkout info page
- **Priority:** High
- **Steps:**
  1. Leave "Zip/Postal Code" blank
  2. Fill in First Name and Last Name
  3. Click "Continue"
- **Expected Result:** Error message: "Error: Postal Code is required"; user remains on the same page

### TC-012 — Cart total matches sum of item prices
- **Preconditions:** Logged in as `standard_user`
- **Priority:** Medium
- **Steps:**
  1. Add two items with known prices to cart
  2. Proceed to checkout, complete info step
  3. View order overview (item total, tax, total)
- **Expected Result:** Item total equals the sum of the two item prices; total equals item total + tax, both correctly calculated

### TC-013 — Remove item from cart before checkout
- **Preconditions:** Logged in as `standard_user`, 2 items in cart
- **Priority:** Medium
- **Steps:**
  1. Open cart page
  2. Click "Remove" on one item
- **Expected Result:** Item is removed from cart; cart badge count decreases by 1; remaining item still present
