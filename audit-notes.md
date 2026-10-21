# Dependency Audit Notes — 2026 (lodash & axios)

Quarterly security review: dependency-exposure section. Scope: two npm packages our team relies on heavily.

## 1. lodash

- **Upstream repository:** https://github.com/lodash/lodash
- **Most recent release:** `4.18.1` (tag `4.18.1`, published 2026-04-01)
- **CI workflows (10):** CI Browsers, CI Bun, CI Dist Sync, CI Docs, CI Node.js, CodeQL, NPMscan, Renovate, Scorecard supply-chain security, Copilot code review
- **Most severe published advisory:** GHSA-jf85-cpcp-j695 (CVE-2019-10744)
  - **Severity:** Critical (CVSS 3.1: 9.1)
  - **Affected versions:** lodash `< 4.17.12` (patched in 4.17.12)
  - **Issue:** Prototype Pollution in lodash via `defaultsDeep` using `{constructor: {prototype: {...}}}`, which modifies the prototype of `Object`.
- **Also relevant:** GHSA-r5fr-rjxr-66jc (CVE-2026-4800) — High — code injection via `_.template` `options.imports` key names — affects `>= 4.0.0, <= 4.17.23`, fixed in 4.18.0.

**Bottom line:** any deployment of lodash older than 4.17.12 carries a Critical prototype-pollution exposure; the recent 2026 code-injection advisory requires `_.template` with untrusted `options.imports` keys and is fixed in 4.18.0+.

## 2. axios

- **Upstream repository:** https://github.com/axios/axios
- **Most recent release:** `v1.20.0` (published 2026-08-24)
- **CI workflows (20):** CI, Dependency Review, CodeQL, Publish package to NPM, Dependabot Updates, Copilot cloud agent, Copilot code review, Continuous integration, Create release branch, CodeQL (github-code-scanning), Continuous integration (v0.x), AI Moderator, GitHub Actions Security Analysis with zizmor, .github/workflows/auths-verify-commits.yml, CodeQL (codeql.yml), Lockfile lint, Verify build reproducibility, Bundle Size, Copilot, CodeQL - Code Quality
- **Most severe published advisory:** GHSA-x97p-jq2g-jp4f (CVE-2026-101909)
  - **Severity:** High (CVSS 4.0: 8.3)
  - **Affected versions:** `>= 1.15.1, < 1.20.0` (1.x line) and `>= 0.28.0, < 0.34.0` (0.x line); fixed in 1.20.0 / 0.34.0
  - **Issue:** Prototype Pollution Gadget in axios `toFormData` Options — six serialization option properties (`visitor`, `maxDepth`, `dots`, `indexes`, `metaTokens`, `Blob`) are read from a plain object inheriting from `Object.prototype` without own-property guards.
- **Other high-severity advisories fixed in 1.20.0:**
  - GHSA-542g-h47m-68v8 (CVE-2026-101901) — High (8.2) — DoS via unhandled 'error' event in HTTP/2 ClientHttp2Session init — `>= 1.13.0, < 1.20.0`
  - GHSA-m8m8-qj5v-23w3 (CVE-2026-101905) — High (7.6) — request socket hijack via inherited `createConnection` — `>= 1.15.2, < 1.20.0`
  - GHSA-3g43-6gmg-66jw (CVE-2026-44495) — High (7.0) — credential theft / response hijacking via `transformResponse` prototype-pollution gadget in config merge
- **Medium advisories (recent):** GHSA-j8rh-479h-cp32 (CVE-2026-101904) — Medium — header injection via inherited headers after minimal interceptor — `>= 1.0.0, < 1.20.0`; GHSA-fvcv-3m26-pcqx (CVE-2026-40175) — Medium — cloud metadata exfiltration via CRLF header injection chain — `>= 1.0.0, < 1.15.0`.

**Bottom line:** all of axios's 2026 high-severity advisories are fixed in 1.20.0. Any 1.x version below 1.20.0 is exposed to multiple high-severity prototype-pollution gadget chains and an HTTP/2 DoS. Upgrade to 1.20.0 or later.

## Recommended actions

1. Confirm the lodash version in our lockfile is `>= 4.17.12` (ideally 4.18.1) and axios is `>= 1.20.0`.
2. For axios, note most 2026 issues are "prototype-pollution gadgets": they require a separate prototype-pollution primitive in the same process, so remediating known PP sources (e.g., old lodash, qs, minimist) reduces risk on both fronts.
3. Avoid passing untrusted input to `_.template` `options.imports` key names.
4. Track remediation in the audit repository issues.
