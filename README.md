# Royal Mail Barcode Validation Test - React.js

This test is designed to assess your ability to build a small but complete React application against specific functional and non-functional requirements. 
It is intentionally scoped so you don't need to complete every requirement. Clean core logic and clear reasoning matter more than completeness.

You should aim to take no more than **2 hours** on this test. If you're short on time, prioritise what you judge to be the most important parts.

## 📋 Overview

Build a single-page web application that validates Royal Mail barcodes. Users should be able to enter a barcode string and get immediate feedback on whether it's valid or invalid, with clear explanations of any failures.

Royal Mail barcodes use a specific format with a weighted checksum algorithm. Your app needs to implement these rules accurately.

## 🎯 The Task

Build a single-page **React.js** application that:

- Accepts barcode input from users
- Performs immediate client-side validation
- Simulates an async API validation call for valid barcodes
- Maintains a persistent history of all validation attempts during the session
- Shows clear success or error messages
- Has a clean, intuitive, and accessible interface
- Uses some Royal Mail branding (colours, fonts) for styling

## ✅ Functional Requirements

### 🏷️ Barcode Format

Valid barcodes are **exactly 13 characters** with this structure:

| Position(s) | Type | Meaning |
|---|---|---|
| 1–2 | Uppercase letters (`A–Z`) | Prefix |
| 3–10 | Digits (`0–9`) | Serial number (**8 digits**) |
| 11 | Digit (`0–9`) | Check digit (calculated) |
| 12–13 | Exactly `GB` | Country code |

**Valid examples for testing**:

- `AB473124829GB`
- `XH545554533GB`
- `ZZ999999990GB`
- `AA000000005GB`

### 🔢 Check Digit Algorithm

The check digit uses a weighted sum:

1. Take the 8-digit serial number (characters **3–10**)
2. Multiply each digit by its corresponding weight: `[8, 6, 4, 2, 3, 5, 9, 7]`
3. Sum all the results
4. Calculate: `11 - (sum % 11)` where `%` is the modulo operator
5. Apply these special cases:
   - If result is `10` → use `0`
   - If result is `11` → use `5`
   - Otherwise → use the calculated result

**Example for** `AB47312482?GB`:

- Serial: `47312482`
- Weighted sum:

  ```text
  (4×8) + (7×6) + (3×4) + (1×2) + (2×3) + (4×5) + (8×9) + (2×7) = 200
  ```

- Check digit:

  ```text
  11 - (200 % 11) = 11 - 2 = 9
  ```

- Valid barcode: `AB473124829GB`

## 🔄 Validation Flow

### 1️⃣ Phase 1: Pre-validation (client-side, immediate)

When a user submits a barcode, validate:

- Length is exactly **13 characters**
- Prefix (chars **1–2**) are letters `A–Z`
- Serial number (chars **3–10**) are all digits
- Check digit (char **11**) matches the calculation
- Country code (chars **12–13**) is `GB`

If validation fails:

- Show a **specific error message**
- Keep the barcode in the input field

If validation passes:

- Clear the input field
- Show a brief success notification
- Move to **Phase 2**

### 2️⃣ Phase 2: API validation (async, simulated)

For barcodes that pass pre-validation, build a mock API function that simulates a backend validation service.

Your mock function should:

- Return a `Promise`
- Randomly succeed or fail (e.g., `Math.random() >= 0.5`)
- Resolve or reject after a random delay between **1–30 seconds**
- Be called when a valid barcode is submitted

Why random failure? This is intentional so we can see how you handle async operations, loading states, and error scenarios (e.g., network issues, timeouts, server errors).

## 📜 Validation History

**Critical requirement:** Maintain a persistent list of all validation attempts that grows throughout the user's session.

When a barcode passes pre-validation:

- Immediately add it to the validation history list with status **"Validating..."**
- Show a loading/spinner indicator for that entry
- The list should never clear or reset—each new validation adds to the existing list
- When the async validation completes, update that specific entry's status to **"Valid barcode"** or **"Invalid barcode"**
- Multiple validations can be running concurrently

## 🎨 User Experience

### ⌨️ Input

- Convert input to **uppercase** automatically
- Show a hint about the expected format
- Include a placeholder example like: `EG. XH545554533GB`

### 📊 Validation History Display

Each entry in the history should show:

- The barcode string
- Current status text (`"Validating..."`, `"Valid barcode"`, or `"Invalid barcode"`)
- Visual indicator (spinner icon, checkmark, or error icon)
- Entries should remain visible and never be removed

### ⚠️ Error Messages

Be specific about what failed. Examples:

- "Validation failed - Barcode is not the correct length"
- "Validation failed - Prefix is not in the range AA to ZZ"
- "Validation failed - Serial number is not in the range 00 000 000 to 99 999 999"
- "Validation failed - Check digit is not correct"
- "Validation failed - Country code is not GB"

Feel free to adjust the wording — just make sure users understand what went wrong.

## ♿ Accessibility

Consider basic accessibility best practices:

- Proper semantic HTML
- Keyboard navigation (tab order, form submission with Enter key)
- ARIA labels for icons and status indicators
- Screen reader announcements for validation results
- Sufficient color contrast
- Focus indicators

## 🧪 Edge Cases

Handle these properly:

- Empty input or only whitespace
- Lowercase letters (convert to uppercase)
- Special characters
- Wrong length
- Multiple validations running at once
- Updating the correct item in the list when async calls complete out of order

## 🏗️ Non-Functional Requirements

- Write clean, readable code with sensible naming
- Keep validation logic separate from UI components so it's easy to test
- Handle edge cases without breaking the UX
- Include unit tests for the validation logic, especially the check digit algorithm
- Manage async operations cleanly (no race conditions when updating the list)
- Handle concurrent async operations correctly
- Implement basic accessibility features

## 🛠️ Technical Constraints

- Must use **React.js** (any recent version)
- Must use **TypeScript**
- State management is your choice (local state, Context, Redux, Zustand, etc.)
- Styling is your choice (CSS, styled-components, Material UI, Tailwind, etc.)
- Build tools are your choice (CRA, Vite, Next.js, custom webpack, etc.)

Make whatever architectural decisions you think make sense.

## 🔍 What We're Looking For

- Correct implementation of the check digit algorithm
- Clean separation between validation logic and UI
- Proper handling of async operations and loading states
- Managing a list with concurrent async updates (avoiding race conditions)
- Immutable state updates
- Logical code organisation and component structure
- Unit tests for complex logic
- Good UX with thoughtful error handling
- Basic accessibility implementation
- Appropriate abstractions (not over-engineered, not under-engineered)
- Comments or docs explaining key decisions

We'd rather see a well-structured core solution than a rushed feature-complete one.

## ⏱️ Time Expectation

If you're short on time, prioritise in this order:

1. Correct validation logic (especially check digit)
2. Clean code structure with separation of concerns
3. Basic validation history with async handling
4. Unit tests for validation
5. Accessibility and UI polish (lower priority)

## 📦 Submission

Either: create a PR on this repo, or send us a zip file containing your source code (place all code inside a /src folder off the repo root)

Include a `README` with:
  - Setup and run instructions
  - Technical decisions and why
  - Structure overview
  - Known limitations / trade-offs
  - What you'd improve with more time, what you prioritised, and why
  - A working app runnable locally (ideally `npm install && npm start`)
  - Tests with instructions for running them