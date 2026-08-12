# Twilio support ticket — draft

**Where:** https://help.twilio.com → Get Help → open a ticket
(or email `help@twilio.com` from the address the account is registered to)

**Category:** Account / Billing → Account restricted or suspended

---

## Subject

```
Account ACce9de77d21bbb0705cb823d636ef0824 — "Policy evaluation failed" on number search and caller ID verification
```

## Body

```
Hello,

My trial account is returning HTTP 401 with code 20003, "Policy evaluation
failed", on a specific set of API endpoints. I would like the restriction
reviewed and lifted.

Account SID: ACce9de77d21bbb0705cb823d636ef0824
Account status: active
Account type: Trial
Created: 2026-08-05

I have completed the verification steps in the console, and the restriction
did not change.

What works:
  - Fetching account details
  - Listing incoming phone numbers (0)
  - Listing calls (0) and messages (0)

What returns 401 "Policy evaluation failed":
  - AvailablePhoneNumbers (searching for a number to buy)
  - OutgoingCallerIds (verifying my own phone number)
  - Balance
  - Keys (API keys)
  - Usage records
  - Applications

The pattern is that read-only access to existing data works, while anything
involving provisioning, billing or credentials is blocked.

Context: I am a student working through a two-week learning project on voice
applications. I want to buy one US phone number and point it at a simple IVR
("press 1 for sales, 2 for support") that I am hosting myself. This is a
personal learning exercise — no marketing, no bulk messaging, and the only
number I intend to call is my own verified mobile.

Could you review the restriction on this account, or tell me what additional
verification you need from me?

Thank you,
Parv Jain
```

---

## Before you send

- **Check whether a VPN or proxy was active when you signed up.** Plivo rejected
  this signup on two different email addresses, including a `bu.edu` one, and
  Twilio has now flagged the account. Two independent anti-fraud systems
  reaching the same conclusion points at the network, not the email. If a VPN
  was on, say so in the ticket — it's a common and easily-cleared false positive.
- Send from the email the Twilio account is registered to.

## After they reply

Re-run the health report — it is the pass/fail test:

```bash
cd ~/plivo/twilio-health-checker
source venv/bin/activate
python main.py
```

The **"Day 4 readiness"** section tells you whether the hold is gone:

- `Number search works. You can buy a number on Day 4.` → unblocked, Day 4 can start
- `Cannot search for numbers to buy` → still held, reply to the ticket
