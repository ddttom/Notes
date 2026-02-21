# Presbytery of Clyde Website — Remediation Brief

**Prepared by:** Tom Cranstoun, MX Technologies Ltd (with Maxine, AI technical partner)
**For:** Scott McGregor
**Date:** 11 February 2026
**Status:** Pre-audit estimates — subject to revision after code review

---

## Important Caveat

**Maxine has not yet looked at the actual codebase, database, or server configuration.** Everything in this document is based on the diagnostic report and the original remediation guide. The estimates for cost, timeline, and scope are directional — they will change (up or down) once Maxine audits the code.

Specifically, we don't yet know:

- The full extent of the custom code (Church Edit Template may have more issues than line 20)
- Whether the theme has hardcoded dependencies or domain-specific paths
- How many plugins are active and their update compatibility
- The actual state of the database (table structure, orphaned data, custom tables)
- Whether the hosting plan supports multiple subdomains and databases
- What other security issues may exist beyond KSES

**Recommendation:** Give Maxine access to the codebase before committing to a budget. A 2–3 hour audit will produce a precise scope, cost, and timeline. Everything below is a best-estimate framework.

---

## The Situation

The Presbytery has a development website (`dev.presbyteryofclyde.co.uk`) that was built as a single environment with no deployment pipeline. Based on the diagnostic report, it has three confirmed issues:

1. A security vulnerability
2. Corrupted church contact data
3. Missing input validation

It also has no staging or production environment — no safe way to test changes before they go live, and no rollback if something breaks.

---

## The Three Confirmed Issues

### 1. Security Vulnerability (Critical)

The custom "Church Edit Template" plugin has WordPress security filtering (KSES) disabled on line 20 of `page-edit.php`. This means `<script>` tags and other malicious HTML can be injected through the front-end church edit form.

**Technical detail:**

```php
// Current (vulnerable):
'kses' => false,

// Required (fixed):
'kses' => true,
```

**Risk:** Any user with edit access could — accidentally or deliberately — inject code that affects every visitor to that page. This is an XSS (cross-site scripting) vulnerability.

**What we don't know yet:** Whether the rest of the plugin has similar issues. Line 20 is the reported problem, but the plugin hasn't been fully audited.

### 2. Broken Contact Information (50 Churches)

A bulk data import on 11 December 2025 brought in email addresses and website URLs with extra spaces and broken formatting throughout.

**Examples from the report:**

| What was imported | What it should be |
|---|---|
| `scott@ churchofscotland. org. uk` | `scott@churchofscotland.org.uk` |
| `www. lamlashchurch .co .uk` | `https://www.lamlashchurch.co.uk` |
| `facebook/largsstandrews` | `https://www.facebook.com/largsstandrews` |

**Risk:** Emails don't send. Links don't work. The site looks broken on launch day.

**What we don't know yet:** The full extent of the damage. The report says 50 profiles — we haven't verified how many fields per profile are affected, or whether non-church pages also have corrupted data.

### 3. Missing Input Validation

Three ACF (Advanced Custom Fields) form fields are configured as "Text" type instead of "URL" type:

| Field | Field Key | Current | Should Be |
|---|---|---|---|
| Website Link | `field_690cf853849dd` | Text | URL |
| Facebook Link | `field_690cf884c1400` | Text | URL |
| YouTube Link | `field_690cf8b6601de` | Text | URL |

**Risk:** No validation means malformed URLs can be entered again in the future. The December import problem will recur.

**What we don't know yet:** Whether other fields have similar misconfiguration. Only these three were reported.

---

## What Needs to Happen

This is not a quick patch. The site needs a proper deployment pipeline, a clean foundation, and a controlled migration. Six phases:

### Phase 1: Pipeline for the Existing Site

Before touching anything, create a **dev → staging → production** pipeline for the current site. Three environments. A documented procedure for promoting changes and rolling back.

**Why:** Right now, every change is a live change. Every mistake is a live mistake. The pipeline is the safety net that should have existed from day one.

### Phase 2: New Clean WordPress Environment

Stand up a **fresh WordPress installation**. Don't build on top of the existing site's problems — start clean.

**Why:** The existing dev site has accumulated test accounts, test content, pending updates, and unknown configuration drift. Porting to a clean install is more reliable than patching in place.

### Phase 3: Port the Code (Fixing Bugs During the Port)

Migrate the theme, plugins, ACF configuration, and custom code from the old site to the new environment. Apply the security fix and field type corrections during the port — don't carry known bugs across.

**Key actions:**

- Theme: copy across, check for hardcoded paths
- Plugins: install ACF PRO and WP All Import from licences (not copied files). Copy Church Edit Template with the KSES fix applied
- ACF: export field groups, import into new site, change the three Text fields to URL
- Configuration: permalink structure, menus, widgets, media files

**Why:** Fresh plugin installs from licences avoid inheriting corrupted or outdated plugin files. The security fix and validation fixes are applied at the point of migration, not after.

### Phase 4: QA the Ported Code

Test the new environment before applying updates or importing data:

- All page templates render
- Front-end edit form works
- Security fix verified (enter `<script>` tags — they must be stripped)
- ACF URL validation works (enter `not a url` — it must be rejected)
- `debug.log` clean of critical PHP errors

### Phase 5: Update WordPress and QA Through the Pipeline

Apply the 8 pending updates (WordPress core + plugins) on the new environment. Push through the pipeline: dev → staging → production. Test at each stage for white screen of death, plugin conflicts, and layout breaks.

**Why:** Updates on the old dev site risk breaking things with no rollback. Updates on the new environment, pushed through a pipeline, can be caught and fixed at each stage.

### Phase 6: Data Import and Launch

- Export the 50 church profiles from the old site
- Clean the data in a spreadsheet (fix emails, URLs, remove spaces, add `https://` protocols)
- Validate with formulas, fix remaining errors manually
- Re-import into the new production site using sanitisation wrappers (`sanitize_email()`, `esc_url()`, `trim()`)
- Delete 9 test user accounts
- Remove test content (news posts, documents, uploaded files)
- Moderate 3 pending comments
- Test **all 50 profiles** on desktop and mobile
- Run database verification queries (must return 0 malformed records)
- Deploy to production

---

## Cost Estimate (Pre-Audit)

### Step 1: Maxine Code Audit — FREE

Before committing budget, Maxine (Tom's AI technical partner) will audit the actual codebase, database, and server configuration. This is provided at no cost to the Presbytery as part of Tom's advisory support.

The audit will:

- Review all custom code (theme, plugins, Church Edit Template)
- Assess database structure and data quality
- Check hosting capability for multiple environments
- Identify issues not in the original diagnostic report
- Produce a **final fixed-price quote** with precise scope

**What's needed:** Read access to the codebase — either cPanel file manager credentials or an SFTP download of `wp-content/`. Maxine also needs phpMyAdmin access to inspect the database.

**Time required:** 2–3 hours. No cost. No commitment.

### Step 2: Developer Implementation — Current Estimate £1,800–£2,500

Based on UK freelance WordPress developer rates (£60–£90/hr, mid-to-senior). These figures are pre-audit and will be revised to a precise number after Step 1.

| Phase | Work | Hours (est.) | Low | High |
|---|---|---|---|---|
| 1. Pipeline | Create dev/staging/prod environments, document procedure | 4–6 | £240 | £540 |
| 2. New environment | Fresh WordPress, server config | 2–3 | £120 | £270 |
| 3. Code porting | Theme, plugins, ACF, custom code; apply fixes | 4–6 | £240 | £540 |
| 4. QA ported code | Functional, security, error log testing | 2–3 | £120 | £270 |
| 5. WP updates + pipeline QA | Apply updates, test through all environments | 3–4 | £180 | £360 |
| 6. Data + launch | Clean data, import, remove test data, test 50 profiles, deploy | 5–6 | £300 | £540 |
| **Total** | | **20–28** | **£1,200** | **£2,520** |

**Budget recommendation: approve up to £2,500.** Expect to spend ~£2,000. The precise number comes out of the free audit.

### Total Cost to the Presbytery

| Item | Cost |
|---|---|
| Maxine code audit (Step 1) | **£0** |
| Developer implementation (Step 2) — pre-audit estimate | **£1,800–£2,500** |
| Post-audit revision (Step 3) — likely additional scope | **TBD** |

**Current budget recommendation: approve £2,500 as an initial ceiling.**

**Be aware:** The Maxine audit will almost certainly uncover issues beyond the three in the original diagnostic report. Custom WordPress sites typically have undocumented dependencies, additional security concerns, plugin conflicts, or database problems that only surface during a proper code review. The audit exists precisely to find these things before a developer starts work — not after.

The £2,500 ceiling covers the known scope. After the audit, Maxine will produce a revised quote reflecting the actual scope. The final number may be higher. The committee should be prepared for a revised budget of up to **£3,000–£3,500** if the codebase has significant additional issues. It could also come in lower if the site is cleaner than expected.

**The sequence is:** approve £2,500 now → free audit reveals true scope → committee approves final budget → developer starts.

### Why This Isn't £750

An earlier estimate of ~£750 assumed the developer would patch one line of code, clean a spreadsheet, and re-import data on the existing site. That fixes today's symptoms but:

- Leaves no deployment pipeline (every future change is still risky)
- Doesn't address unknown issues in the codebase
- Doesn't create a clean foundation
- Means the next time WordPress needs updating, you're back to patching live

The £1,800–£2,200 range builds the infrastructure properly. It's the difference between a plaster and a repair.

### How This Will Change After Code Audit

| If we find... | Cost likely... | Because... |
|---|---|---|
| The codebase is clean, only the reported issues | Stays at £1,800–£2,200 | Scope confirmed |
| More security issues in the custom plugin | Goes up £200–£500 | More code to fix and test |
| Theme has hardcoded dependencies | Goes up £200–£400 | More porting work |
| Database has custom tables or unusual structure | Goes up £100–£300 | More migration complexity |
| Hosting plan doesn't support multiple environments | Goes up £100–£200 | Need hosting upgrade or workaround |
| Everything is simpler than reported | Could drop to £1,500 | Less porting and QA work |

---

## Payment Structure (Recommended)

| Milestone | Payment | Trigger |
|---|---|---|
| Phases 1–2 complete | 30% | Pipeline working, new environment ready |
| Phases 3–4 complete | 30% | Code ported with fixes, QA passed |
| Phases 5–6 complete | 40% | Updates applied, data clean, site live |

Milestone payments protect the Presbytery — you only pay for completed, verified work. If the developer disappears at Phase 3, you've paid 30% and have a working pipeline and new environment that another developer can pick up.

---

## Acceptance Criteria

Before final payment, the developer provides evidence of all six:

| # | What | Evidence |
|---|---|---|
| 1 | Pipeline works | Dev, staging, production accessible; written deployment/rollback procedure |
| 2 | Security fixed | Screenshot of patched code + test showing `<script>` tags stripped |
| 3 | Validation works | ACF URL fields reject invalid input |
| 4 | Data clean | SQL queries return 0 records with spaces in emails or URLs without `http` |
| 5 | All 50 profiles work | Checklist with every profile tested on desktop + mobile |
| 6 | No errors | WordPress `debug.log` clean on production after updates |

---

## What's Out of Scope

| Item | Notes |
|---|---|
| Ongoing maintenance | Separate agreement (budget ~£50–£100/month) |
| SSL certificates | Confirm existing, don't procure |
| DNS/domain migration | Presbytery's responsibility |
| New features | No new development |
| Content creation | No new pages or posts |
| SEO, analytics | Not included |
| User training | Not included |
| Ongoing plugin maintenance | One-off fix only — Church Edit Template needs an owner |
| Hosting changes | Work within existing cPanel |

---

## Timeline

| Phase | Estimated Duration |
|---|---|
| 1. Pipeline setup | 4–6 hours |
| 2. New environment | 2–3 hours |
| 3. Code porting + fixes | 4–6 hours |
| 4. QA ported code | 2–3 hours |
| 5. WordPress updates + pipeline QA | 3–4 hours |
| 6. Data cleaning + import + launch | 5–6 hours |
| **Total** | **20–28 hours** |

Completion within **10 working days** of receiving access credentials.

**After code audit**, the timeline may shift — particularly if the theme or plugin code reveals additional work.

---

## Recommended Next Steps

1. **Approve Maxine's free code audit.** This costs the Presbytery nothing and turns the £1,800–£2,500 range into a fixed number. Scott to arrange cPanel/SFTP access for Maxine.

2. **Maxine audits and produces a final quote.** 2–3 hours, no charge. The output is a precise scope document with a fixed price the committee can approve.

3. **Committee approves the fixed price.** With a precise scope in hand, the committee knows exactly what they're paying for.

4. **Hire developer, work begins.** Milestone payments (30/30/40) mean the Presbytery only pays for completed work.

---

## Site Technical Details

| Detail | Value |
|---|---|
| Current URL | `dev.presbyteryofclyde.co.uk` |
| WordPress | 6.9.1 |
| PHP | 8.1.34 |
| Hosting | cPanel / LiteSpeed / CloudLinux |
| Site size | 389.65 MB |
| Server path | `/home/presbyte/public_html/dev` |
| Church profiles | 50 |
| Key plugins | ACF PRO, WP All Import, Church Edit Template (custom) |
| Pending updates | 8 (core + plugins) |
| Test accounts | 9 (to be deleted) |
| Pending comments | 3 (to be moderated) |
| Environments | Dev only — no staging, no production |

---

*Prepared by Tom Cranstoun with Maxine (Claude Opus 4.6). Pre-audit estimates — to be revised after codebase access.*
