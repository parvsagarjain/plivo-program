# Internship Handoff — Plivo Voice AI Program

**Last updated:** 2026-08-12
**Status:** 11 of 12 days done — the AI answers **+1 (617) 925-7056**, 24/7
**Provider:** Twilio (Plivo signup was rejected — see "Provider switch" below)
**Machine:** macOS (Darwin 25.5.0), Apple Silicon (arm64)
**Root folder:** `~/plivo`

| Repo | What it is |
|---|---|
| `twilio-health-checker` | Wk1 D2 — account report |
| `call-service` | Wk1 D3–5 — Flask + Redis + Postgres, the keypad IVR, on Vercel |
| `llm-service` | Wk2 D1 — LLM calls, streaming, function calling |
| `speech-service` | Wk2 D2 — Deepgram + ElevenLabs |
| `voice-agent` | Wk2 D3–7 — the AI receptionist, on Railway |
| `plivo-program` | **This file.** Program-level documentation only |

All six are private repos under `github.com/parvsagarjain`, all pushed.

**This document is version-controlled** (added 2026-08-12). `~/plivo` is a git
repo whose `.gitignore` excludes the five project folders — they have their own
remotes and nesting them would create submodule confusion for no benefit.

The program-brief screenshots (`WhatsApp Image *.jpeg`) are **ignored by
default**: they are personal photos not reviewed for whatever else is in frame.
Delete the `*.jpeg` line from `.gitignore` to include them and make this
document self-contained.

Checked for credentials before the first push. The only match was the Twilio
Account SID in the support-ticket draft, which is an identifier, not a secret.
**Do not paste auth tokens, API keys or connection strings into this file** —
it is now on GitHub.

---

## The program

2-week program, ~6–8 hrs/week. Source material is 5 screenshots in `~/plivo/`
(`WhatsApp Image 2026-08-04 at 21.05.17*.jpeg` etc.).

**Core philosophy:** the human directs, Claude Code builds. Never write Python or
JavaScript by hand. Describe requirements in plain English, let Claude generate
everything, verify by testing (calling the number, checking dashboards) — not by
reading code.

**Golden rules from the doc:**
1. Never write code manually
2. Copy-paste commands exactly, don't improvise
3. Verify in dashboards (Vercel, Railway, provider console), not by reading code
4. When stuck, paste the full error to Claude
5. Ultimate test: can you call the phone number and talk to your AI?

### Week 1: Foundation & Deployment (5 days)

| Day | Focus | Deliverable | Status |
|---|---|---|---|
| 1 | Environment + Git | Cursor IDE, GitHub, terminal, first Python project | ✅ Done (GitHub finished 2026-08-05) |
| 2 | Multi-file projects | Twilio Account Health Checker | ✅ **Done** — runs against the live account |
| 3 | Web servers + Databases | Flask, Redis sessions, PostgreSQL | ✅ **Done** — 13/13 verification passing |
| 4 | Voice IVR | "Press 1 for Sales, 2 for Support" | ✅ **Done** — real call answered, menu heard, keypress logged |
| 5 | Cloud Deployment | IVR on Vercel 24/7 | ✅ **Done** — live at call-service-bice.vercel.app |

**End of Week 1 target:** a production IVR anyone can call.

### Week 2: AI Voice Agents (7 days)

| Day | Focus | Deliverable | Status |
|---|---|---|---|
| 1 | LLM APIs | OpenAI calls, streaming, function calling | ✅ **Done** — 38/38 including live API calls |
| 2 | Speech AI | Deepgram transcription, ElevenLabs voice | ✅ **Done** — 50/50 including live calls to both services |
| 3 | Pipecat Framework | Local voice bot via microphone | ✅ **Done** — talked to it through the mic |
| 4 | Pipecat + telephony | AI agent answering real phone calls | ✅ **Done** — real calls, with working tools |
| 5 | LiveKit Alternative | Same agent via LiveKit + SIP (optional) | ⏭️ **Skipped deliberately** — optional, and rebuilds what already works |
| 6 | Railway Deployment | Voice AI running 24/7 in cloud | ✅ **Done** — agent live at voice-agent-production-1621.up.railway.app |
| 7 | Polish + Demo | Final features, demo video, external testing | 🟡 Demo script written (`voice-agent/DEMO.md`); **video + external tester outstanding** |

**End of Week 2 target:** production AI receptionist that answers calls, detects
intent, and logs everything.

---

## 🔀 Provider switch — Plivo → Twilio (2026-08-05)

### What happened

Plivo signup was rejected on every email tried. First a custom-domain Zoho
address (`"cannot use this email at this moment"` — diagnosed as new-domain
reputation, since a freshly registered domain with no site and no sending history
looks like a throwaway fraud domain to anti-fraud screening). The `bu.edu`
fallback did not get through either. Rather than lose days to support tickets on
a 2-week timeline, the program moved to **Twilio**, where signup succeeded.

### Does this matter? — ✅ No. Accepted 2026-08-11.

The switch was raised and **accepted**. Twilio is the provider for this
program; no need to port anything back to Plivo. This is settled — do not
re-open it in a later session.

Technically, almost nothing is lost. Both providers are the same shape —
XML-over-webhooks telephony (TwiML vs Plivo XML), near-identical REST SDKs, the
same IVR/`<Gather>`/media-stream concepts. Porting back later would be a
mechanical rewrite, not a redesign.

### Where Twilio is actually better for this program

| | Plivo | Twilio |
|---|---|---|
| Getting a number | ~$10 balance top-up first | ~~Free number on trial~~ — **wrong.** Needed a $20 upgrade + Trust Hub KYC, then $1.15/mo |
| US number KYC | none | **Trust Hub profile required** — see Day 4 |
| Pipecat support | exists, thinner | **more mature, better documented** |
| Trial call restriction | verified numbers only | verified numbers only (same) |

### Kept, not deleted

`~/plivo/plivo-health-checker/` is untouched — built, error-paths tested, 7 files
staged, no commit. If Plivo access ever appears it needs only credentials pasted
into its `.env`. The root folder is still named `~/plivo` on purpose: it is the
program's name.

---

## Environment — current state

| Thing | Status | Notes |
|---|---|---|
| Homebrew | ✅ 6.0.12 | `/opt/homebrew/bin/brew` |
| Python 3.12.13 | ✅ Installed 2026-08-04 | `/opt/homebrew/bin/python3.12` |
| Python 3.9.6 (system) | ✅ Untouched | `/usr/bin/python3` — macOS needs it, do not modify |
| Redis 8.10.0 | ✅ Installed 2026-08-05 | `brew services start redis`. **Config was patched — see Day 3.** |
| PostgreSQL 16 | ✅ Installed 2026-08-05 | `brew services start postgresql@16`. DB `call_service` created. |
| git | ✅ 2.50.1 | Apple Git-155 |
| Plivo account | ❌ Abandoned | Signup rejected on every email tried |
| Twilio account | ✅ Upgraded 2026-08-11 | **Type: Full**, KYC approved, nothing locked. Owns **+1 (617) 925-7056**. Balance $18.84. |
| ngrok 3.39.10 | ✅ 2026-08-11 | Authtoken configured. Only needed for local webhook testing now. |
| twilio-cli 6.2.4 | ✅ 2026-08-11 | **npm, not brew** — brew builds from source and fails on outdated Xcode CLT. |
| vercel-cli 58.9.1 | ✅ 2026-08-11 | npm. Logged in as `parvjain-2478`. |
| OpenAI account | ✅ 2026-08-11 | $5 credit, key in `llm-service/.env`. **Key was pasted in chat — rotate it.** |
| Deepgram account | ✅ 2026-08-12 | Key in `speech-service/.env`, verified live (confidence 1.000, 2.2s). **Pasted in chat — rotate.** |
| ElevenLabs account | ✅ 2026-08-12 | Free tier. **Pasted in chat — rotate.** ⚠️ Free accounts cannot use *library* voices over the API (402); only the ~21 defaults. Default is "Sarah" `EXAVITQu4vr4xnSDxMaL`. |

**Why 3.12 was installed:** the machine only had Apple's system Python 3.9.
Pipecat (Week 2, Day 3) requires Python 3.10+. Fixed early rather than hitting a
wall mid-program. 3.12 chosen over 3.13 for maximum voice-AI library compatibility.

Verified working on 3.12: `venv`, `ssl`, `sqlite3`.

---

## Day 2 project — `~/plivo/twilio-health-checker/`

A terminal program that logs into Twilio via API and prints an account report:
credentials valid, balance, phone numbers, recent calls, TwiML applications.

**The real lesson is project structure**, not the health check itself. Multiple
files importing each other, `requirements.txt` for dependencies, `.env` for
secrets that never reach GitHub. Every later project reuses this skeleton.

### Files

| File | Purpose |
|---|---|
| `main.py` | Entry point. Run this. Prints the report. |
| `health_check.py` | One function per API question (account, numbers, calls, apps) |
| `config.py` | Loads `.env`, validates credentials are filled in |
| `.env` | **Currently holds placeholders.** Gitignored. |
| `.env.example` | Safe-to-commit template |
| `.gitignore` | Ignores `.env`, `venv/`, `__pycache__`, `.DS_Store` |
| `requirements.txt` | `twilio==9.10.9`, `python-dotenv==1.2.2` |
| `venv/` | Project-private Python 3.12. Gitignored. |
| `README.md` | Run instructions + verification checklist |

### How to run

```bash
cd ~/plivo/twilio-health-checker
source venv/bin/activate
python main.py
```

`(venv)` appears in the prompt when activation works. Needed once per new terminal.

### What has been tested

**Two login methods are supported** (2026-08-05): Account SID + **API key**
(`SK...` + secret), or Account SID + **Auth Token**. The API key wins if both are
present, and the report prints which one it used. API keys are the better habit —
revocable on their own, whereas leaking the Auth Token means rotating the
credential the whole account depends on.

All six credential paths tested, each giving a readable message rather than a
crash or a bare 401:

| Case | Behaviour |
|---|---|
| All placeholders | Names the missing Account SID and where to find it |
| `SK...` pasted into `TWILIO_ACCOUNT_SID` | Explains it belongs in `TWILIO_API_KEY_SID` |
| API key SID filled, secret missing | Flags the half-fill, notes the secret is shown only once |
| Account SID only, no method | Lists both methods to choose from |
| Complete API key, wrong values | `Twilio rejected these credentials (API key).` |
| Complete Auth Token, wrong values | `Twilio rejected these credentials (Auth Token).` |

✅ **Secret safety** — `git add -A` stages exactly 7 files. `.env` and `venv/`
confirmed ignored via `git check-ignore -v`.

✅ **Success path — verified 2026-08-05 against the live account.** Report prints
`Credentials: working (via Auth Token)`, account `My First Twilio Account`,
`Type: Trial`, `Status: active`. Attribute names in the account / numbers / calls
sections are now confirmed correct against real data, not guesses.

### Credential troubleshooting — what actually happened

Two dead ends, both worth remembering:

1. **The API key (`SK...`) was rejected** — Twilio error `20003 Authenticate`.
   Values were structurally perfect: right lengths, no stray whitespace, no
   TextEdit smart quotes. Cause never pinned down; either the secret was copied
   wrong (it is shown exactly once) or the key belongs to a different project
   than the Account SID. **The Auth Token worked immediately.** The dead key's
   values are commented out in `.env`, not deleted.
2. **TextEdit silently reverted an edit.** The file had been open in TextEdit
   from before a Claude-side edit; saving overwrote it, re-enabling the broken
   API key and making it look like the Auth Token was being ignored. **Close
   TextEdit before editing `.env` from the terminal, and vice versa.**

⚠️ The API key secret passed through a chat transcript during debugging. **Delete
that key in the console** rather than reusing it.

### 🔓 The compliance hold is gone — it is now a plain trial limit (2026-08-11)

**Re-checked 2026-08-11. The error text changed on every affected endpoint.**

| | 2026-08-05 | 2026-08-11 |
|---|---|---|
| Message | `401 Policy evaluation failed` | `401 This feature is not available on a Trial account. Please upgrade your account to gain access.` |
| Remedy | Twilio Support only | **Upgrade the account** |

Probed one by one — AvailablePhoneNumbers, OutgoingCallerIds, Keys, Usage
records, Applications, Balance. **Not one still returns `Policy evaluation
failed`.** Messages/Calls/Account/Numbers all answer normally.

So the account is no longer flagged; what remains is the ordinary trial ceiling,
and Twilio names its own fix. **Day 4 is now gated on money, not on Support** —
which is a much better place to be, since Day 4 needed a paid US number anyway.

### ✅ Upgraded — everything unlocked (2026-08-11)

Account upgraded, and the health report came back **completely clean**:

```
Type:    Full
Balance: 20.0 USD
Day 4 readiness: Number search works. You can buy a number on Day 4.
Report complete. Nothing locked.
```

Balance, TwiML applications and number search all answer now. **The residual
doubt is closed** — it was a genuine trial ceiling, not the old compliance hold
re-worded. No support ticket was ever needed. Day 4 is unblocked.

**The 08-05 API-key diagnosis still stands corrected.** The `SK...` key never had
a chance — the whole Keys resource was (and still is) blocked. Don't recreate it.

⚠️ **Pattern still worth noting:** Plivo rejected signup on both a custom domain
*and* a `bu.edu` address, and Twilio then flagged this account. Two independent
anti-fraud systems agreeing points at something environmental — network/IP (VPN
or proxy) or region — rather than the email. **Keep any VPN off when paying**;
payment is a second fraud checkpoint.

### ⚠️ `TWILIO-SUPPORT-TICKET.md` is now out of date

It is written entirely around `401 Policy evaluation failed`, which no longer
reproduces. **Do not send it as-is** — the reply would be "you are on a trial,
please upgrade". If a ticket is still wanted, it should be rewritten as a short
pre-purchase question ("is this account under any restriction beyond standard
trial limits?") rather than a restriction appeal.

`main.py` asks for each section independently, so a locked endpoint is reported
rather than crashing the run, and the footer lists what is locked. Re-running the
report after verification is the way to confirm the gate has lifted — that is
what the "Day 4 readiness" section exists for.

**Note:** the earlier assumption that a Twilio trial hands you a free number
turned out to be wrong for this account — `Phone numbers` is empty and number
search is locked. Budget for Day 4 accordingly.

### Notes carried over from the Plivo build

Two bugs found there do **not** apply to Twilio (different SDK), but the shape of
the lesson does: SDK attribute names are guesses until a real call proves them,
and a client constructor can raise before any network request. Both are why the
Twilio `connect()` builds the client *inside* the try block.

### Git state — ✅ committed and pushed

- Commit `6fb9646` "Day 2: Twilio account health checker", 7 files, 429 insertions
- Remote: **https://github.com/parvsagarjain/twilio-health-checker** (private)
- `.env` verified absent from the remote — no credentials ever left the machine

GitHub was set up on 2026-08-05 with the `gh` CLI, not GitHub Desktop. Future
repos follow the same two commands:

```bash
git commit -m "message"
gh repo create <name> --private --source=. --push
```

---

## ▶️ Resume here — paste credentials

```bash
open -a TextEdit ~/plivo/twilio-health-checker/.env
```

Replace the text after each `=`. No quotes, no spaces around `=`.

The **Account SID** (`AC...`) is always required — Account Info panel on the
[console.twilio.com](https://console.twilio.com) home page. Then complete **one**
of the two blocks in the file: the API key (`SK...` + secret), or the Auth Token
(same panel, behind a "show" toggle).

Then:

```bash
cd ~/plivo/twilio-health-checker
source venv/bin/activate
python main.py
```

### Day 2 verification checklist

- [ ] `python main.py` runs without a crash
- [ ] Prints `Credentials: working (via ...)`
- [ ] Account name matches console.twilio.com
- [ ] Balance matches the dashboard
- [ ] `git status` does **not** list `.env`

Expect `Type: Trial` until the upgrade lands. **No free number was issued** on
this account, contrary to the original assumption — "Phone numbers" is correctly
empty. Empty "Recent calls" is also correct. "TwiML applications" reads as
*locked* rather than empty while on trial; an application gets created on Day 4.

---

## Day 3 project — `~/plivo/call-service/`

A Flask server with Redis sessions and PostgreSQL storage, per the spec
("Flask server, Redis sessions, PostgreSQL storage"). Deliberately shaped like
the Day 4 IVR so the phone number can be pointed straight at it.

**The lesson:** a phone call is several web requests, not one, and the two kinds
of memory it needs are different. Redis holds the call *while it happens*
(minutes, auto-expiring). PostgreSQL holds it *after* (forever). Using Postgres
for live state means a disk write per keypress for data worthless in ten
minutes; using Redis for the log means losing everything on restart.

### Files

| File | Purpose |
|---|---|
| `app.py` | The web server. Run this. |
| `sessions.py` | Redis — live call state with a self-cleaning TTL |
| `database.py` | PostgreSQL — permanent call log + table schema |
| `config.py` | Loads `.env`, working local defaults |
| `verify.py` | Walks a fake call end-to-end, 13 checks |

### How to run

Two terminals — server in one, test in the other:

```bash
cd ~/plivo/call-service && source venv/bin/activate && python app.py
cd ~/plivo/call-service && source venv/bin/activate && python verify.py
```

### What has been tested

✅ **13/13 passing**, 2026-08-05 — Flask up, both databases connected, a call
opened a Redis session, two keypresses recorded, session readable mid-call,
call ended, session gone from Redis, row present in PostgreSQL with the right
menu choice, status and duration.

✅ **Verified outside the app too** (Rule 3): row confirmed via
`psql -d call_service -c "SELECT * FROM calls;"`, and `redis-cli keys 'call:*'`
confirmed empty after the call ended.

### Two environment bugs found and fixed

1. **Port 5000 is taken by macOS AirPlay Receiver** (`ControlCenter`), which
   answers with a `403` and an `AirTunes/950.7.1` header — so it looks like a
   broken app, not a port clash. **The project uses port 5001.**
2. **Redis 8.10.0 will not start as shipped.** The Homebrew formula's
   `redis.conf` loads four modules (redisbloom, redisearch, redisjson,
   redistimeseries) by *relative* path, and the `.so` files are not installed at
   all — `brew services start redis` aborted with `Can't load module … server
   aborting`. Fixed by commenting out the four `loadmodule` lines in
   `/opt/homebrew/etc/redis.conf`. Original saved as
   `/opt/homebrew/etc/redis.conf.backup-2026-08-05`.

### Git state — ✅ committed and pushed

- Commit `75c449f` "Day 3: Flask server with Redis sessions and PostgreSQL storage"
- Remote: **https://github.com/parvsagarjain/call-service** (private)
- Working tree clean

---

## Day 4 project — the IVR, inside `~/plivo/call-service/`

Built **into** the Day 3 project rather than as a new one, because Day 3 already
had the Redis sessions and Postgres log the IVR needs, and its own header said
Day 4 would point a number at it. Copying `sessions.py`/`database.py` into a
separate folder would have meant fixing every future bug twice.

### What was added

| File | Change |
|---|---|
| `twiml.py` | **New.** Every word the caller hears, and the XML that makes Twilio say it. The menu is one dict — adding "3 for Billing" is one line. |
| `app.py` | Three endpoints Twilio calls: `/voice` (phone rings), `/voice/menu` (keypress), `/voice/status` (call over) |
| `sessions.py` | Added `set_choice()` — the keypress and the hangup are separate requests, so the choice has to survive between them |
| `config.py` | IVR timeouts, retry cap, and optional Twilio signature checking |
| `verify.py` | Day 4 checks appended — **39 passing total** |
| `requirements.txt` | `twilio==9.10.9` |

### The idea that unblocked it

**A Twilio IVR is a web server with an accent.** Twilio POSTs form-encoded
fields and expects XML back; the phone number only decides who sends the POSTs.
So `verify.py` imitates Twilio directly — form-encoded POSTs, TwiML parsed back
out — and the entire menu was built and proven **before any number existed**.

### What has been tested

✅ **39/39 on 2026-08-11.** Phone rings → menu offered; wrong key → told it
isn't an option and re-asked; press 1 → routed to Sales, call ended, no second
menu; choice held in Redis across requests; hangup → row in PostgreSQL with the
right choice, status and caller; silent caller → gives up after 3 attempts
instead of looping forever, still logged as `no-choice`.

✅ **Verified outside the app** (Rule 3): rows confirmed via `psql`, and
`redis-cli keys 'call:*'` empty once calls ended.

### Two things worth knowing

1. **The greeting used to lie.** The first draft said "this call may be recorded
   for training" because that is what real IVRs say — but nothing records
   anything, and telling real callers that is false in a way that carries legal
   weight in two-party-consent states. Removed.
2. **Twilio signature validation is built but off** (`VALIDATE_TWILIO_SIGNATURE`).
   Once the server is public, anyone who learns the URL can POST fake calls at
   it. Turn it on for ngrok/Vercel, and set `PUBLIC_BASE_URL` too — Twilio signs
   the *public* URL, not the one Flask thinks it received, and getting that
   wrong fails every request for reasons that look nothing like the cause.

### ✅ Number bought — +1 (617) 925-7056 (2026-08-11)

KYC approved (`twilio-approved`, profile `BU2a5e45…`) and the number is owned.

| | |
|---|---|
| Number | **+1 (617) 925-7056** — Brighton, MA |
| SID | `PN33e3182a15cabd660c7ee17bdc563743` |
| Cost | $1.15/month; balance still reads $20.00 (bills next cycle) |

**Not the number originally picked.** +1 617 398 4224 was taken by someone
else during the KYC wait — Twilio's pool is shared and first-come, and the
retry came back `400 not available`. The next available 617 was bought
immediately rather than losing that one too.

### 🚧 Buying a number needed Trust Hub KYC — cleared 2026-08-11

Number search works, and the purchase call is correct, but Twilio rejects it:

```
HTTP 401 — Unable to create record: Primary compliance profile is not
approved. Please complete the KYC process in Trust Hub to gain access.
```

**Nothing was charged.** The purchase was refused outright, not half-completed.

Trust Hub is **empty** — 0 customer profiles, 0 end users, 0 supporting
documents. It has never been started.

⚠️ **This is not the old compliance hold returning.** That one was specific to
this account and is genuinely gone. This is a standard regulatory gate Twilio
applies to everyone before number purchase. Different problem, different fix,
and no reason to think the account is flagged again.

**This corrects the provider comparison table above**, which claimed US numbers
need no KYC on Twilio. That was true once; it is not true now.

**To clear it** (user must do this — it needs personal identity details):
console.twilio.com → **Trust Hub** → **Primary Customer Profile** → choose
**Individual**, not Business (asks for less, clears faster) → legal name,
address, contact details, possibly a photo ID → submit. Individual profiles
typically approve within minutes to a day.

Confirmed pricing, fetched from Twilio's pricing API rather than assumed:
**US local $1.15/month**, **inbound voice $0.0085/min**. The $20 balance covers
roughly 17 months of number rent.

Number picked and still available when approval lands: **+1 617 398 4224**
(Charlestown MA, voice + SMS + MMS) — the only 617 on offer.

### ✅ Day 4 proven on a real call (2026-08-11)

```
call_id     : CA65c8408562d3820154ac3e702ad1e77c
from        : +1 617 925 7056     (the Twilio number)
to          : +91 7073982082      (the user's phone)
menu_choice : support             (pressed 2)
status      : completed
duration    : 25 seconds
```

Redis session opened on answer, held the keypress mid-call, and was gone
afterwards. PostgreSQL kept the row. Cost ~2¢ (India is $0.0496/min).

### ⚠️ US calls were silently filtered — the one real gotcha

Two calls to the user's US T-Mobile mobile came back `no-answer` after
17 and 20 seconds of ringing, **with no Twilio error and no charge**. The
same code then worked first try to an Indian number.

**Cause: carrier/handset spam filtering, not a bug.** A brand-new Twilio
number has zero calling reputation, which is exactly what T-Mobile Scam
Shield and iPhone's "Silence Unknown Callers" are built to suppress. The
call reaches the network and never reaches the handset.

**Before demoing to anyone with a US mobile: have them save the number to
contacts first.** Contacts bypass both filters. This is worth knowing for
the Week 2 demo, where a supervisor not hearing the phone ring would look
like a broken project.

Note this only affects **outbound** calls to mobiles. Inbound — someone
dialling +1 (617) 925-7056 — is unaffected.

### Tooling added on Day 4

| Tool | Notes |
|---|---|
| ngrok 3.39.10 | `brew install ngrok`. Free tunnel needs an authtoken (signup). |
| twilio-cli 6.2.4 | **Installed via `npm i -g twilio-cli`, not brew.** The brew formula builds from source and fails on this machine — Xcode Command Line Tools are outdated (wants 26.3). npm took 6 seconds. |
| dev-phone plugin | Browser softphone. **Warning: starting it silently repoints the number's `voice_url` at its own handler.** Re-run `wire_number.py` after using it. Not needed in the end. |

### Still to do for Day 4 — ✅ all done

1. ~~Trust Hub KYC~~ ✅ approved
2. ~~Buy the number~~ ✅ +1 (617) 925-7056
3. **ngrok authtoken** ← the only blocker. ngrok is installed
   (`brew install ngrok`, 3.39.10) but a free tunnel needs an account:
   sign up at [ngrok.com](https://dashboard.ngrok.com/signup), copy the
   authtoken, then `ngrok config add-authtoken <token>`.
4. `ngrok http 5001`, then wire the number to whatever URL it prints:
   ```bash
   cd ~/plivo/call-service && source venv/bin/activate
   python wire_number.py https://<whatever>.ngrok-free.app
   ```
5. **Call +1 (617) 925-7056 from your mobile** — the program's ultimate test

**`wire_number.py` exists because the free ngrok URL changes on every
restart**, and Twilio keeps POSTing the dead one, so calls fail with nothing
obvious to look at. Re-run it after every ngrok restart.

**No-signup alternative to ngrok:** `brew install cloudflared` then
`cloudflared tunnel --url http://localhost:5001` gives a public https URL with
no account at all. The program doc specifies ngrok, so that is the default
here, but cloudflared is a legitimate substitute if another signup is
unwelcome — `wire_number.py` does not care which tunnel produced the URL.

---

## Week 2 Days 3–4 — `~/plivo/voice-agent/`

The AI receptionist. `bot.py` talks through the laptop microphone,
`phone.py` answers the real number. Same pipeline; only the transport differs.

```bash
cd ~/plivo/voice-agent && source venv/bin/activate
python bot.py --selftest      # builds pipeline, checks the microphone
python bot.py                 # talk to it locally
python phone.py               # serve real calls (needs ngrok + wire_phone.py)
```

### ⚠️ The number can only point at one thing

While `phone.py` is wired up, **Week 1's deployed IVR is not answering**, and
the agent only lives as long as the laptop and its ngrok tunnel. Put it back
with:

```bash
python wire_phone.py --restore      # number -> deployed Vercel IVR, 24/7
```

### What was tuned, and why — all from real calls

| Symptom | Cause | Fix |
|---|---|---|
| ~3s before it replied | Pipecat's default turn detector is a neural model that deliberates | Plain 0.45s timer (`USER_SPEECH_TIMEOUT`). **Measured median dropped to 0.77s** |
| Repeated itself 3× | Treated the first sound as an interruption, so noise cut it off | Requires 2 words (`MIN_WORDS_TO_INTERRUPT`) |
| "Parv" → "Faz", "opening hours" → "opening us" | 8kHz mu-law loses proper nouns first | Deepgram `keyterm` list (`KEYTERMS`) |
| Sounded robotic | High `stability` is steady but flat; prompt said "How can I assist you" and repeated the name every turn | stability 0.35, style, 1.05× speed; prompt rewritten for contractions and name-at-most-once |

**Voice is "Matilda"** (`XrExE9yKIg1WjnnlVkGX`), chosen by listening to all 21
voices the account can use, then comparing finalists on a *hesitation* line —
hedging is most of what this receptionist says.

### 🚨 The bug worth remembering: it invented facts

Asked for opening hours, the agent said *"we're open nine to five, Monday
through Friday"* with total confidence. **Nothing in the system knew any
hours.** It also offered to connect callers to departments it made up, and
promised transfers it could not perform.

Prompting a model not to invent things helps and does not fix it. `tools.py`
fixes it properly:

| Tool | Behaviour |
|---|---|
| `get_opening_hours` | Real data — edit `tools.py` and answers change |
| `list_departments` | Real list, so it cannot invent "marketing" |
| `transfer_call` | **Refuses honestly** — no `FORWARD_TO` configured. Set it in `.env` to enable |
| `take_message` | Writes to the **same Neon database** as Week 1's call log |

Verified: "what's your address" now returns *"I don't have that info on hand.
I can take a message"* rather than an invented address.

`take_message` never loses a message silently — if the database is unreachable
it logs locally and still confirms to the caller. A real bug was found here:
`tools.py` read `DATABASE_URL` without loading `.env`, so it degraded to
log-only in some paths while still telling callers it had saved.

### Pipecat 1.7.0 API notes

The API moves fast and the shipped examples lag it. Verified by reading the
installed package:

- `PipelineTask` is deprecated, removed in 2.0 — use `PipelineWorker`
- Services take a `Settings` object, not keyword arguments
- VAD is a pipeline processor (`VADProcessor`), not a transport parameter
- Turn strategies live under `pipecat.turns.*`, not with the aggregators

### Tooling

`portaudio` (brew) for the microphone; `fastapi`/`uvicorn` for the call server.
macOS prompts the **terminal app** for microphone access, not Python —
`bot.py --selftest` opens the mic deliberately to force that prompt early,
because without permission the pipeline runs happily and simply never hears
anything.

---

## Week 2 Day 6 — Railway deployment ✅ (2026-08-12)

**The agent runs without the laptop.** `+1 (617) 925-7056` now reaches the AI,
not the keypad IVR.

| | |
|---|---|
| Agent | https://voice-agent-production-1621.up.railway.app |
| Project | `voice-agent` on Railway, service `voice-agent` |
| Cost | Trial credit, then **$5/month** — the only recurring charge besides the number |

```bash
cd ~/plivo/voice-agent
railway logs --service voice-agent      # live logs, including tool calls
railway status
railway up --service voice-agent --detach   # redeploy after changes
```

### Why Railway and not Vercel

A call needs **one websocket held open for its whole duration**. Serverless
functions are torn down between requests, so Vercel can host the Week 1 IVR
(request/response) but can never host the agent. This was flagged back on
Day 5 and is the reason the program schedules Railway.

### Three things that broke, and the fixes

1. **The build would have died on `pyaudio`.** The `[local]` pipecat extra
   needs portaudio system headers no container has. Split into
   `requirements.txt` (deployment) and `requirements-local.txt` (adds the
   extra for `bot.py`'s microphone). Also added `fastapi`, `uvicorn`,
   `psycopg2-binary`, `twilio` — installed in the venv but never recorded, so
   it ran locally and would have crashed on boot in the cloud.
2. **Deployed "Online" but returned 404.** Railway assigned the domain with no
   target port and never injected `PORT`, so the app sat on 8080 with nothing
   routed to it. Fixed with
   `railway domain update <domain> --port 8080 --service voice-agent`.
   **"Online" is not "reachable" — always curl before wiring the number.**
3. **No `PUBLIC_HOST` chicken-and-egg this time.** `phone.py` falls back to the
   incoming Host header, which Railway sets correctly, so the TwiML was right
   on the first deploy. Unlike Vercel's `PUBLIC_BASE_URL`, which needed two.

### Turn-taking, tuned against real calls

| Setting | Value | Why |
|---|---|---|
| `USER_SPEECH_TIMEOUT` | 0.30s | Was 0.45. Median reply ~0.73s |
| `MIN_WORDS_TO_INTERRUPT` | **1** | Was 2 — and that **killed two calls**. See below |

⚠️ **`MIN_WORDS_TO_INTERRUPT=2` was a real regression.** It was added to stop
line noise cutting the agent off, but Pipecat only applies the threshold
*while the bot is speaking* — exactly when someone interjects a short word.
Two calls ended in 4s and 8s because a one-word answer was discarded and the
caller got no reply. "Yes", "hello", "sales" and "billing" are what people
actually say. Noise rejection now rests on Deepgram having to transcribe a
real word, plus the keyterm list.

**Latency floor:** dropping the timeout further will not help. The remaining
~0.7s is Deepgram finalising, the LLM's first token, and ElevenLabs' first
audio — three network round trips. Cutting the timer only makes it interrupt
people. Cold start on the *first* reply is ~3.5s.

### The prompt had overcorrected

Asked to spell a caller's name back, the agent refused — the
anti-hallucination instructions had generalised from "don't invent facts" to
"don't do things". The prompt now separates the two: it may always spell,
repeat, speak slowly, summarise and do arithmetic; the restrictions are about
*facts it doesn't have* and *actions it can't perform*. Verified: spells
"P-A-R-V", still refuses to invent a street address.

### Proven end to end on a live call (2026-08-12)

```
tool list_departments({})                    -> real list
tool transfer_call({'department': 'sales'})  -> refused honestly
tool take_message({'caller_name': 'Sagarjan',
                   'message': 'sales test'}) -> row #2 in Neon
```

A caller spoke to an AI on a real phone number and the message landed in the
PostgreSQL table built in Week 1 Day 3. That is the whole program in one call.

### Day 7 so far

- **`voice-agent/call.py`** — place a call from the terminal:
  `python call.py +917073982082`. Same TwiML path as an inbound call; only the
  dialler differs. `--status SID` looks up any past call, `--local URL` uses a
  tunnel.
- **`voice-agent/DEMO.md`** — timed two-minute script, what to say when
  something misfires, and an honest list of what the system cannot do.

---

## Known upcoming risks

| When | Risk | Mitigation |
|---|---|---|
| ~~Now~~ | ~~Number search locked by the trial ceiling~~ | ✅ Resolved 2026-08-11 — account upgraded, everything unlocked |
| **Now** | **Trust Hub KYC not started — number purchase refused** | Console → Trust Hub → Primary Customer Profile → Individual. Only the user can do this. |
| Day 4 | No free number was issued; buying one needs credit | Buy a **US local** number ($1.15/mo), not toll-free ($2.15/mo + extra verification you don't need) |
| Day 4 | ~~Indian numbers require KYC~~ | Still true; buy a **US** number |
| Day 4 | Trial accounts can only call **verified** numbers | Verify personal phone in the Twilio console, do not skip |
| Day 4 | Trial calls play a Twilio trial message before connecting | Expected, not a bug. Goes away on upgrade. |
| Day 4/5 | Webhooks need a public URL | `ngrok` for local dev, Vercel once deployed |
| Wk2 Day 3 | Pipecat needs Python 3.10+ | ✅ Already handled — 3.12.13 installed |
| Wk2 | OpenAI / Deepgram / ElevenLabs all need paid API keys | Budget and sign up before Week 2 |
| Any | Twilio also screens accounts for fraud post-signup | Don't test with random numbers; keep usage to your own verified phone |

---

## Tools the program uses

| Tool | Purpose | Set up? |
|---|---|---|
| Claude Code | Engineering partner — writes all code | ✅ |
| Cursor IDE | Where Claude Code runs | ✅ (Day 1) |
| `gh` CLI 2.97.0 | Version control — GitHub from the terminal | ✅ Installed 2026-08-05, authed as `parvsagarjain` |
| GitHub Desktop | — | ❌ Never installed. The Day 1 note claiming otherwise was wrong. |
| Twilio | Phone numbers and calls | ✅ Trial account created |
| ngrok 3.39.10 | Local tunnel for webhook testing | ✅ authtoken configured. Only needed for local testing now |
| Vercel CLI 58.9.1 | Serverless hosting — the Week 1 IVR | ✅ npm, logged in as `parvjain-2478` |
| Railway CLI 5.37.7 | Container hosting — the voice agent | ✅ npm, logged in as `parvjain@bu.edu` |
| Neon | Cloud PostgreSQL | ✅ `us-east-1`, PG 16.14, pooled endpoint |
| Upstash | Cloud Redis | ✅ `us-east-1`, Redis 8.2.0, `rediss://` |
| OpenAI / Deepgram / ElevenLabs | The AI stack | ✅ all three keyed and verified live |
| Pipecat 1.7.0 | Voice agent framework | ✅ + portaudio for the local mic |
| Plivo | Phone numbers and calls | ❌ Signup rejected — abandoned |

---

## Day 5 — ✅ deployed and proven (2026-08-11)

**Live URL: https://call-service-bice.vercel.app** — the number points here,
not at ngrok. Verified by a real call served with the laptop uninvolved:

```
CAb29bf4556b30853d110389a7e82b0326
+1 617 925 7056 -> +91 7073982082   choice=sales   completed   16s
```

| Piece | Where it lives now |
|---|---|
| Flask app | Vercel, function region **iad1** |
| PostgreSQL | **Neon** `us-east-1`, PostgreSQL 16.14 (pooled endpoint) |
| Redis | **Upstash** `us-east-1`, Redis 8.2.0 |

All three in us-east-1/iad1 deliberately — every keypress does a database
round trip, and cross-region latency is dead air the caller hears.

Signature validation is **on in production** and verified there: unsigned
request → 403, correctly signed → 200.

### Four things that went wrong, and why

1. **`vercel.json` 404'd everything** despite a successful build. Legacy
   `builds` only honours `routes`, never `rewrites` — the two cannot be
   mixed. Fix: drop `builds` entirely and let Vercel auto-detect
   `api/index.py`. Keep it that way.
2. **Vercel Authentication blocked Twilio.** New projects default to
   `ssoProtection: all_except_custom_domains`, which 302s every request to
   an SSO login. Twilio cannot log in, so every call would have failed.
   Disabled via the API. **A webhook endpoint must be publicly reachable —
   the signature check is what protects it, not deployment protection.**
3. **The per-deploy URL is not the one to use.** `vercel --prod` prints
   `call-service-<hash>-...vercel.app`, which changes every deploy. The
   stable alias is `call-service-bice.vercel.app`, found under
   `targets.production.alias` in the projects API.
4. **`PUBLIC_BASE_URL` is a chicken-and-egg problem.** It cannot be set
   until the domain exists, but signature validation fails without it. Deploy
   first, set it, redeploy.

### Vercel environment variables (all set, Sensitive)

`DATABASE_URL` (Neon pooled), `REDIS_URL` (Upstash rediss://), `SECRET_KEY`
(freshly generated — *not* the dev placeholder), `TWILIO_ACCOUNT_SID`,
`TWILIO_AUTH_TOKEN`, `VALIDATE_TWILIO_SIGNATURE=true`, `PUBLIC_BASE_URL`.

### 🔑 Credentials to rotate — four now

**Highest priority: the OpenAI key** (`sk-proj-...`, created 2026-08-11). It
was pasted into chat and it can *spend money* — unlike the others, misuse has
a direct bill attached. Rotate at
[platform.openai.com/api-keys](https://platform.openai.com/api-keys): create a
new key, paste it into `llm-service/.env`, then delete the old one. Setting a
low monthly usage limit under Billing is worth doing at the same time.



All three passed through a chat transcript during setup:

- **ngrok authtoken** — dashboard.ngrok.com → Reset
- **Neon password** — Neon → Project → Settings → Reset password (then update
  `DATABASE_URL` on Vercel and redeploy)
- **Upstash Redis password** — Upstash console → reset (same follow-up)
- Plus the long-dead **Twilio API key** from Day 2, still worth deleting

Nothing is known to be compromised; this is hygiene, and it is easier to do
now than to remember later.

---

## Day 5 — original plan (kept for reference)

Right now the IVR only works while a laptop is open, a Flask process is
running, and an ngrok tunnel is alive. Day 5 fixes all three. Three things
need a home in the cloud, not one:

| Piece | Local now | Needs to become |
|---|---|---|
| Flask app | `python app.py` on port 5001 | A deployed service with a fixed URL |
| PostgreSQL | Homebrew, localhost | Hosted Postgres (Neon / Vercel Postgres) |
| Redis | Homebrew, localhost | Hosted Redis (Upstash) |

**The honest catch with Vercel:** it is serverless, so the app is torn down
between requests. Flask works there, but every webhook pays a cold start and
a fresh database connection, which on a phone call is dead air the caller
hears. It is fine for Day 5's IVR. It is the **wrong** shape for Week 2's
Pipecat voice agent, which holds an open media stream for the length of the
call — that is what Railway is for, and the program already schedules Railway
on Week 2 Day 6.

So Day 5 on Vercel is worth doing as specified (it teaches serverless and
managed databases), with the expectation that Week 2 moves to Railway rather
than extending it.

**Do first, before any deploy:** turn on `VALIDATE_TWILIO_SIGNATURE=true` and
set `PUBLIC_BASE_URL`. A permanent public URL with no signature checking is a
standing invitation to have fake calls POSTed at it — tolerable for a
ten-minute ngrok test, not for something left running.

---

## Where things stand (2026-08-11)

**Days 1, 2 and 3 are done and pushed.** The compliance hold that paused things
on 08-05 is gone; what is left is the trial ceiling, and the user is upgrading
the account to clear it.

The pass/fail test remains the **"Day 4 readiness"** section of the Day 2 health
report. Nothing else needs checking.

**Day 4 is less blocked than it looks.** A Twilio IVR is just a web server
answering HTTP POSTs with TwiML — the phone number only decides who sends those
POSTs. The whole menu (greeting, `<Gather>`, "press 1 for Sales, 2 for Support",
routing, Redis session, Postgres log) can be built and tested by firing
simulated Twilio webhook payloads at it, exactly the way `call-service/verify.py`
already tests Day 3. **The number is needed only for the final "call it from my
phone" check.**

---

## Immediate next action (2026-08-12)

**11 of 12 days done.** All five repos committed and pushed, working trees
clean. Both deployments live and answering.

| What | Where |
|---|---|
| 📞 The number | **+1 (617) 925-7056** → the AI agent |
| Agent | voice-agent-production-1621.up.railway.app (Railway) |
| Week 1 IVR | call-service-bice.vercel.app (Vercel) — not on the number now |
| Databases | Neon Postgres + Upstash Redis, both `us-east-1` |

### Left to do

1. **Record the demo video** — script is ready at `voice-agent/DEMO.md`.
2. **Have someone else call it.** Everything so far has been tested by people
   who know what to say; an outsider will find things this session could not.
3. **Rotate six credentials** — OpenAI, Deepgram, ElevenLabs, Neon, Upstash,
   ngrok. All passed through a chat transcript. **OpenAI first**: it is the
   only one that spends money directly. Also still outstanding: delete the
   dead Twilio API key from Day 2.

   Rotating Neon or Upstash means updating the value in **three** places —
   `voice-agent/.env`, Railway (`railway variables --set`), and Vercel
   (`vercel env add … --force`) — then redeploying both. Easy to do two and
   wonder why something broke.
4. **Decide on commit attribution** (see below).
5. Optional: **rate-limit the agent.** See the risk below.

### Commit attribution — an open decision

Every commit across all six repos ends with:

```
Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
```

**This grants nobody access.** Checked on 2026-08-12: `parvsagarjain` is the
only collaborator on all six repos. Claude Code runs `git` and `gh` as the
user, with the user's credentials; there is no Anthropic account involved and
`noreply@anthropic.com` is not a real one. GitHub simply renders the trailer as
a second avatar, which makes it look like more than it is.

**Undecided:** whether to keep it. Three options —

1. Leave it. Accurate, and the program's whole premise is that Claude wrote the
   code.
2. Stop adding it to future commits. Costs nothing.
3. Strip it from history too — `filter-branch`/`rebase` plus a force-push
   across six repos. Every hash changes and the old history on GitHub is
   destroyed. Only worth it if the attribution genuinely misrepresents the
   work.

Worth deciding before a supervisor reads the repos.

### ⚠️ The number is public, unauthenticated and attached to paid APIs

Nothing limits call length or concurrency. Anyone who dials it spends your
ElevenLabs allowance and OpenAI credit. Fine while only you have the number;
worth capping before it goes in a video or gets shared.

**ElevenLabs' free tier is the binding constraint — 10,000 characters a
month, roughly 15 minutes of the agent talking.** Not Twilio, not Deepgram
($200 credit, barely touched), not OpenAI. A handful of demo calls will use
it up, and the agent goes mute when it runs out.

### Putting the keypad IVR back

The number can only point at one thing. To return it to the Week 1 IVR:

```bash
cd ~/plivo/voice-agent && source venv/bin/activate && python wire_phone.py --restore
```

Both are deployed, so either choice survives the laptop closing. This is now
a question of *which* you want answering, not whether anything is running.

### Old list (Week 1) — all done, kept for the record

1. ~~Commit Day 2~~ ✅ done — committed and pushed 2026-08-05.
2. ~~Start Day 3~~ ✅ done — 13/13, committed and pushed (`75c449f`).
3. ~~Open a Twilio support ticket~~ ❌ **dropped 2026-08-11** — the error it was
   written about no longer occurs. See the 08-11 section.
4. ~~Upgrade the Twilio account~~ ✅ done — Full, $20 credit.
5. **Delete the dead Twilio API key** in the console — its secret passed
   through a chat transcript on Day 2. Still outstanding.
6. ~~Build Day 4's IVR~~ ✅ done, deployed to Vercel.
7. ~~Send the supervisor a note about Plivo → Twilio~~ ✅ accepted 2026-08-11.
