# Secure Notes — Flask Web Application

A secure Flask web app with user authentication and personal notes CRUD.

## Quick Start

```bash
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # macOS/Linux
pip install -r requirements.txt
cp .env.example .env         # edit SECRET_KEY to a random string
python run.py
```

Open http://127.0.0.1:5000/login

## Security Controls → OWASP Top 10 Mapping

| # | Control | Implementation | OWASP Category |
|---|---------|---------------|----------------|
| 1 | Password hashing | `flask-bcrypt` with automatic salt | **A02 — Cryptographic Failures** |
| 2 | JWT session tokens | `PyJWT` stored in HttpOnly, SameSite cookies | **A02 — Cryptographic Failures**, **A07 — XSS** |
| 3 | Authorization middleware | `@login_required` decorator validates JWT on every protected route | **A01 — Broken Access Control** |
| 4 | SQL injection prevention | SQLAlchemy ORM only — no raw SQL queries anywhere | **A03 — Injection** |
| 5 | CSRF protection | `flask-wtf` CSRF tokens on all POST forms | **A01 — Broken Access Control** |
| 6 | Input validation | `bleach.clean()` strips HTML + length limits enforced server-side | **A03 — Injection**, **A07 — XSS** |
| 7 | Secure headers | `flask-talisman` adds CSP, X-Frame-Options DENY, X-Content-Type-Options nosniff | **A05 — Security Misconfiguration** |

## Project Structure

```
app/
  __init__.py          — App factory, extensions, Talisman config
  models.py            — User and Note SQLAlchemy models
  routes/
    auth.py            — Register, login, logout, JWT helpers, login_required decorator
    notes.py           — Dashboard, create/delete notes
  templates/           — Jinja2 templates with CSRF tokens
  static/              — Static assets
run.py                 — Entry point
.env.example           — Environment variable template
requirements.txt       — Python dependencies
```
