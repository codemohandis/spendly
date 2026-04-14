# Spec: Login and Logout

## 1. Context & User Story (Human Layer)
**Goal:** Enable session-based authentication to secure the Spendly application.  
**User Story:** As a registered user, I want to securely log into my account and end my session when finished so that my financial data remains private.  
**Dependencies:** Step 01 (DB Setup), Step 02 (User Registration).

---

## 2. Technical Blueprint (Agent Layer)

### **Interface & Contract**
| Interface | Method | Path | Access | Action |
| :--- | :--- | :--- | :--- | :--- |
| **Route** | `GET` | `/login` | Guest only | Redirect to `/` if logged in; else render `login.html`. |
| **Route** | `POST` | `/login` | Guest only | Validate → Verify Hash → Set `session['user_id']`. |
| **Route** | `GET` | `/register` | Guest only | Redirect to `/` if logged in; else render `register.html`. |
| **Route** | `GET` | `/logout` | Public | `session.clear()` → Redirect to `/`. |
| **Template**| `base.html` | N/A | Global | Toggle nav links based on `session.get('user_id')`. |

### **Logic Scenarios (Gherkin-style)**
* **Scenario: Already logged in visits /login or /register**
    * **When:** `session.get('user_id')` is set.
    * **Then:** Redirect immediately to `url_for('landing')`.
* **Scenario: Invalid Credentials**
    * **When:** Email not found OR `check_password_hash` returns `False`.
    * **Then:** Re-render `login.html` with `error="Invalid email or password."`.
* **Scenario: Successful Login**
    * **When:** Credentials verified.
    * **Then:** Set `session['user_id']`, redirect to `url_for('landing')`.
* **Scenario: Logout**
    * **When:** `/logout` accessed.
    * **Then:** Clear session, redirect to `url_for('landing')`.

### **Filesystem & Changes**
* **`app.py`**:
    * Add imports: `session`, `flash`, `check_password_hash`.
    * Implement logic for `/login` and `/logout`.
* **`templates/base.html`**:
    * Logic: `{% if session.get('user_id') %}` show Logout else show Login/Register.
* **`templates/login.html`**:
    * Update `<form>` to use `url_for('login')` and `POST`.
    * Add `{{ error }}` display block.

### **Hard Constraints (The Guardrails)**
* **Security:** Never compare plaintext passwords; use `check_password_hash`.
* **Security:** Use generic error messages (don't reveal if the email exists).
* **Database:** Use `?` placeholders for SQL. No ORM.
* **UI:** Use existing CSS variables. All templates `extend "base.html"`.
* **Stability:** Use `session.get()` in templates to prevent `KeyError`.

---

## 3. Definition of Done (Verification)
- [ ] `GET /login` renders without errors when logged out.
- [ ] `GET /login` redirects to `/` when already logged in.
- [ ] `GET /register` redirects to `/` when already logged in.
- [ ] Valid credentials create a session and redirect to `/`.
- [ ] Invalid/Empty credentials fail with a generic error message.
- [ ] `/logout` clears the session and redirects to `/`.
- [ ] Nav bar dynamically switches between Login/Register and Logout links.
- [ ] All SQL is parameterized and no plaintext passwords are stored or compared.

---
