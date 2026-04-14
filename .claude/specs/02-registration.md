# Spec: Registration

## Overview
This step wires up the registration form so new users can create an account. The `GET /register` route and `register.html` template already exist; this step adds the `POST /register` handler that validates input, checks for duplicate emails, hashes the password, and inserts the new user into the database. On success the user is shown with a success message and then redirected to the login page. On failure the form is re-rendered with an inline error message. This is the first step that writes user data and is a prerequisite for all authenticated features.

## Depends on
- Step 01 — Database setup (`users` table, `get_db()`)

## Routes
- `POST /register` — process registration form, create user, redirect to `/login` — public

## Database changes
No database changes. The `users` table already exists with the correct schema (`id`, `name`, `email`, `password_hash`, `created_at`).

Two new helper functions must be added to `database/db.py`:
- `create_user(name, email, password_hash)` — inserts a new row into `users`, returns the new user's `id`
- `get_user_by_email(email)` — returns the `users` row matching `email`, or `None`

## Templates
- **Modify:** `templates/register.html`
  - Change `action="/register"` to `action="{{ url_for('register') }}"` (fix hardcoded URL)

## Files to change
- `app.py` — add `POST` method to the existing `register` route; add form handling logic; import `request`, `redirect`, `url_for`, `flash`, `session` from flask; import `generate_password_hash` from werkzeug; import new DB helpers
- `database/db.py` — add `create_user()` and `get_user_by_email()`
- `templates/register.html` — replace hardcoded `action` URL with `url_for()`

## Files to create
None.

## New dependencies
No new dependencies. `werkzeug.security` is already available.

## Rules for implementation
- No SQLAlchemy or ORMs
- Parameterised queries only — never use f-strings or string concatenation in SQL
- Hash passwords with `werkzeug.security.generate_password_hash` — never store plaintext
- Use CSS variables — never hardcode hex values
- All templates extend `base.html`
- Set `app.secret_key` in `app.py` (required for `flash()`); use a hard-coded dev string for now, e.g. `app.secret_key = "dev-secret-change-in-prod"`
- The `register` route function must handle both `GET` and `POST` via `methods=["GET", "POST"]`
- Validate that `name`, `email`, and `password` are all non-empty before touching the DB
- Validate that `password` is at least 8 characters
- If email already exists, re-render the form with `error="An account with that email already exists."`
- On success, redirect to `url_for('login')` — do not auto-login the user (that is Step 3)
- Use `abort(400)` for malformed requests only; use inline `error` variable for user-facing validation failures

## Definition of done
- [ ] `GET /register` still renders the form with no errors
- [ ] Submitting the form with all valid fields creates a new row in the `users` table with a hashed (not plaintext) password
- [ ] After successful registration, the browser redirects to `/login`
- [ ] Submitting with a duplicate email re-renders the form with an error message visible on the page
- [ ] Submitting with an empty name, email, or password re-renders the form with an error message
- [ ] Submitting with a password shorter than 8 characters re-renders the form with an error message
- [ ] No raw SQL strings use f-strings or `.format()` — all values go through `?` placeholders
- [ ] The form `action` attribute uses `url_for('register')`, not a hardcoded string
