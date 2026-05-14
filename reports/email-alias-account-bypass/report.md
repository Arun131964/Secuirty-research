# 🔐 Vulnerability Report #001

| Field | Details |
|---|---|
| **Title** | Improper Email Validation Allows Multiple Accounts via Email Alias Bypass |
| **Type** | Business Logic Flaw / Improper Input Validation |


---

## 📌 Summary

The application does not normalize email addresses during registration.
Email aliases using the `+` character (e.g., `user+alias@gmail.com`) are treated
as unique identities — even though all emails are delivered to the same inbox.

This allows a single person to create **multiple delivery partner accounts**,
bypassing uniqueness restrictions and abusing incentive/referral systems.

---

## 🎯 Affected Functionality

- User Registration
- Delivery Partner Onboarding
- Incentive / Referral / Bonus System

---

## 🪜 Steps to Reproduce

1. Register with `victim@gmail.com`
   → ✅ Verification email received. Account activated.

2. Log out.

3. Register with `victim+admin@gmail.com`
   → ✅ Verification email received. Account activated.

4. Log out.

5. Register with `victim+1@gmail.com`
   → ✅ Verification email received. Account activated.

> All 3 verification emails were delivered to the **same inbox**.
> The system treated each alias as a **separate unique account**.

---

## 💥 Impact

An attacker exploiting this can:

- Create unlimited delivery partner accounts from one inbox
- Claim signup bonuses, referral rewards, and incentives multiple times
- Bypass one-account-per-person restrictions
- Evade account bans or poor ratings
- Manipulate order flows and platform operations
- Cause **direct financial loss** to the platform

---

## ✅ Proof of Concept

- [x] 3 accounts successfully created using aliases
- [x] All verification emails received in one inbox
- [x] No duplicate detection triggered
- [x] Bypass confirmed on Registration & Delivery Partner Onboarding

> 📎 See `/assets/` folder for screenshots

---

## 🛠️ Recommended Fix

Strip the `+alias` portion before uniqueness checks:

```python
def normalize_email(email: str) -> str:
    local, domain = email.split('@')
    local = local.split('+')[0]
    return f"{local}@{domain}".lower()
```

Apply this at:
- ✅ Registration
- ✅ Login
- ✅ Any account lookup or duplicate check

---

## 🔗 References

- [OWASP A04 – Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/)
- [Gmail Address Aliasing](https://support.google.com/mail/answer/22370)

---

*Report by Bharathaarunkumar | Responsible Disclosure Practiced*
