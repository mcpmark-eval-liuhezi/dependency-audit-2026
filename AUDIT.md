# Dependency Security Audit

Source: GitHub global security advisories, npm ecosystem, severity filter `critical`. The first two advisories returned by the listing were researched and recorded below. All field values are reported exactly as the advisory lookups returned them.

## GHSA-673q-mjm6-r5xr

- **GHSA identifier:** GHSA-673q-mjm6-r5xr
- **Affected package:** `next` (npm)
- **Severity:** critical
- **CVSS score:** not returned by the advisory lookup (the record's `cvss` field came back empty)
- **Affected version range:** `>= 16.0.0, <= 16.4.0` (first patched version: `16.4.1`)
- **Summary:** Next.js: Unauthenticated Remote Code Execution in `use cache` functions returning typed arrays (CWE-502, Deserialization of Untrusted Data)

## GHSA-cg2g-xh64-v4q7

- **GHSA identifier:** GHSA-cg2g-xh64-v4q7
- **Affected package:** `dompurify` (npm)
- **Severity:** critical
- **CVSS score:** not returned by the advisory lookup (the record's `cvss` field came back empty)
- **Affected version range:** not returned by the advisory lookups (the record's `vulnerabilities` field was truncated in every response, including the raw advisory-database record)
- **Summary:** DOMPurify SAFE_FOR_TEMPLATES bypass: split template expressions survive in attached shadow root content on DOM output modes (CWE-79, Cross-site Scripting)

## Higher-scoring advisory

Of the two advisories, **GHSA-673q-mjm6-r5xr (`next`)** scored higher. Both advisory records returned an empty `cvss` field, so the higher-scoring determination was made by the first-listed tie-break rule, which selects GHSA-673q-mjm6-r5xr.
