# Business Logic Bypass in Delivery Partner Onboarding via Gmail Alias Abuse

## Summary

The application allows the creation of multiple delivery partner accounts using Gmail email aliasing (`+tag` syntax). The platform does not normalize email addresses before uniqueness validation, allowing a single user to create multiple accounts tied to the same inbox.

This behavior may enable abuse of onboarding workflows, referral systems, incentive programs, and account restriction mechanisms.

---

## Vulnerability Type

- Business Logic Flaw
- Improper Input Validation
- OWASP Top 10: A04 – Insecure Design

---

## Affected Functionality

- User Registration
- Delivery Partner Onboarding
- Incentive / Referral System

---

## Technical Description

The application treats Gmail aliases as completely unique email identities during registration.

For example:

```text
example@gmail.com
example+1@gmail.com
example+admin@gmail.com
```

All emails are delivered to the same Gmail inbox, but the application accepts them as separate accounts.

No canonicalization or normalization is performed before uniqueness validation.

---

## Steps to Reproduce

1. Register an account using:

```text
example@gmail.com
```

2. Verify the account.

3. Log out.

4. Register another account using:

```text
example+1@gmail.com
```

5. Verify the account.

6. Observe that a second account is successfully created.

7. Repeat using additional aliases:

```text
example+admin@gmail.com
example+test@gmail.com
```

---

## Proof of Concept

- Multiple accounts successfully created
- All verification emails received in the same inbox
- No duplicate detection triggered
- Delivery partner onboarding allowed for multiple accounts

---

## Impact

An attacker may be able to:

- Create multiple delivery partner accounts
- Abuse signup bonuses or referral rewards
- Bypass one-account-per-user restrictions
- Evade account bans or negative ratings
- Manipulate platform operations

This issue weakens identity assurance controls and may result in operational and financial abuse.

---

## Severity Assessment

Low to Medium

The issue becomes more severe when financial incentives, onboarding rewards, or operational trust mechanisms rely solely on email uniqueness.

---

## Recommended Remediation

- Normalize email addresses before uniqueness checks
- Strip Gmail alias components (`+tag`)
- Apply email canonicalization
- Enforce stronger identity verification
- Detect duplicate KYC, phone number, or device identifiers

---

## References

- OWASP Business Logic Vulnerabilities
- OWASP Top 10 – A04: Insecure Design

---

## Disclaimer

This report is shared strictly for educational and responsible disclosure purposes.
No malicious exploitation was performed.
