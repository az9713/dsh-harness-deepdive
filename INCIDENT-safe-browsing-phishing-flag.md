# Incident: Safe Browsing flagged this site as phishing

**Date:** 2026-08-13
**Status:** Resolved by renaming the repository.
**Impact:** The site was unreachable in Chrome for about 20 minutes. No data was
at risk at any point.

## Summary

Twenty minutes after this site went live, Chrome blocked it with a full-page red
**"Dangerous site"** interstitial. Google Safe Browsing had classified it as
**phishing**. The pages contain no scripts, no forms, and no external requests,
so no page could have done what the warning describes.

The trigger was the address, not the content. The original URL was:

```
https://az9713.github.io/deepseek-harness-deepdive/
```

A vendor's brand name (`deepseek`) inside a path on a personal `github.io` host
is the exact shape of a fake-vendor page. An automated impersonation classifier
matched it. Renaming the repository removed the brand name from the address and
cleared the block.

## Timeline

| Time (local) | Event |
|---|---|
| 16:42 | `419f440` — 8 HTML pages published to a new public repository. |
| 16:44 | `fc8d9c4` — GitHub Pages enabled; site verified live, HTTP 200, correct content. |
| ~17:00 | Chrome shows "Dangerous site" on `az9713.github.io/deepseek-harness-deepdive/index.html`. |
| ~17:01 | A second site on the same host, `az9713.github.io/field-medal-lecture-and-AI/...`, loads normally. |
| ~17:03 | The interstitial's **Details** panel names the threat type: phishing. |
| 17:04 | `d6757d7` — repository renamed to `dsh-harness-deepdive`; links retargeted; disclaimer added. |
| after | New URL loads with no warning. |

## Diagnosis, and one wrong turn

**First theory, wrong: the whole host was flagged.** `az9713.github.io` serves 92
GitHub Pages sites out of 464 public repositories, and Safe Browsing does
sometimes block a whole `username.github.io` host when one page on it trips a
rule. That theory died as soon as a second site on the same host loaded
normally. Recording the wrong turn matters: the host-wide theory would have sent
us to a Search Console appeal, which fixes nothing when the flag is per-URL.

**Second theory, also wrong: the security vocabulary.** These pages describe an
agent sandbox, so they say "sandbox" 68 times, plus "credentials" 8,
"authentication" 7, "danger-full-access" 3, `curl | sh` 2, "bypass" 2,
"exfiltrate" 1, "poisoning" 1. That reads like attack documentation to a
classifier. But content of that kind trips the **malware** or **unwanted
software** list, not the phishing list.

**Actual cause: brand impersonation, from the URL.** The Details panel settled
it. Chrome's own words:

> Google Safe Browsing … recently found phishing on the site you tried visiting.
> Phishing sites pretend to be other sites to trick you.

"Pretend to be other sites" is impersonation. The only thing on this site that
imitated anything was the vendor name in the path. Compare the two URLs on the
same host, one blocked and one not:

| URL path | Vendor name in path | Result |
|---|---|---|
| `deepseek-harness-deepdive` | yes | blocked as phishing |
| `field-medal-lecture-and-AI` | no | loads normally |

## What the pages actually contain

Verified by grep across all 8 published files, before and after the rename:

| Checked for | Count |
|---|---|
| `<script>` tags | 0 |
| External URLs (`http://`, `https://`) | 0 |
| `<form>`, `<input>`, `<iframe>` | 0 |
| `onclick`, `eval(` | 0 |
| The word `password` | 0 |

Every page is text, inline CSS, and inline SVG. Nothing loads from the network,
which is also why the pages work offline from a local folder. There is no
mechanism by which they could install software or capture a credential — the two
harms the warning names.

## The fix

1. **Renamed the repository**, `deepseek-harness-deepdive` → `dsh-harness-deepdive`.
   The site moved to `https://az9713.github.io/dsh-harness-deepdive/`.
2. **Retargeted every link** in `README.md`: the read-it link, the how-to-view
   link, and the clone command.
3. **Added an unaffiliated-third-party notice** to the top of `README.md`. It
   states that the site is not operated or endorsed by DeepSeek, does not imitate
   any DeepSeek service, asks for no credentials, hosts no downloads, and runs no
   scripts. This is the evidence a human reviewer reads during an appeal.
4. **Pointed the local clone's `origin`** at the new name.

Verification after the rename: the new URL returned HTTP 200 with the correct
page title, and the old URL returned **404 rather than a redirect**. That detail
mattered — a redirect from a flagged URL can carry the flag onto its target.

Not used, because the rename was enough: a Search Console review request, and the
"Let us know" report link on the interstitial. Both remain the correct route if a
flag ever survives a rename.

## Lessons

1. **Do not put a vendor's name in a path on a personal `github.io` host.** For a
   review, a commentary, or a teardown, use a neutral or abbreviated name in the
   URL and put the vendor's full name in the page text, where it belongs. Prose
   about a company is not impersonation; an address that looks like the company
   is.
2. **Read the Details panel before theorising.** The threat type — phishing,
   malware, or unwanted software — points at a different cause in each case, and
   it takes one click. Two wrong theories were argued before anyone looked.
3. **A neighbouring URL on the same host is the cheapest test.** One page that
   loads separates a host-wide block from a per-URL block in seconds.
4. **A clean bill of health from grep is not a defence, but it is the starting
   point.** Confirming zero scripts, zero forms, and zero external requests ruled
   out any real danger immediately, and turned the question from "is this
   compromised?" into "which classifier matched, and on what?".
