# ADR-002: Authentication System Architecture

**Date:** 2026-11-22  
**Status:** Approved  
**Issue:** #11 - Analysis - Authentication Impact  
**Milestone:** Milestone 2 - Authentication System

---

## Decision

Implement authentication using:
1. **Session Storage:** PostgreSQL database-backed sessions (repository pattern)
2. **CSRF Protection:** Server-side tokens in sessions.csrf_token field
3. **Middleware:** Context-based chain (Throttle → Auth → CSRF → Handler)
4. **Password Hashing:** bcrypt cost 12
5. **Schema Migration:** v2 → v3 (add users + sessions tables)

---

## Rationale

- **Consistency:** Follows existing patterns (repository, middleware, Goose migrations)
- **Simplicity:** Single database, no new external services
- **Performance:** ~5ms overhead, 100x headroom for 10 users
- **Security:** OWASP-compliant, timing attack protection, bcrypt

---

## Consequences

✅ Architectural consistency, simple implementation, excellent performance
⚠️ Breaking change (all routes require auth), mitigated with CLI user creation
⚠️ Template updates (add CSRF token fields), systematic review planned

---

## Implementation

**New Code:** internal/auth/ package (~500-800 lines)
**Modified:** web.go (~50 lines), main.go (~30 lines), templates (CSRF fields)
**Migration:** 00003_add_authentication.sql (users + sessions tables)

**See:** Technical repository docs/analysis/ for detailed analysis documents

---

**Status:** Approved for Design phase (Issue #12)
