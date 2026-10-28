# Backend review — student experience redesign

## Verified from source
- Supabase integration, schema migrations, seed content and submission RPC paths exist.
- Production requests are guarded against silent demo-score fallback.
- Production health previously called `auth.getSession()` only; a locally cached session does not establish database connectivity. Health now also checks a database read and no longer claims RLS or scoring are validated by that check.
- `rpc_ensure_student_session` returns the authoritative `studentId`; the client previously ignored RPC errors and constructed an ID containing UUID hyphens, while the migration removes them. The client now uses the returned ID and fails explicitly on RPC failure.

## Remaining production validation (not claimed complete)
The checkout contains environment templates, not the Vercel runtime secrets or access to the owner's Supabase project. No production records were changed.

- Confirm Vercel's configured operating mode and public Supabase values.
- Verify migrations and the approved content version exist in that project.
- Verify anonymous sign-in/captcha and rate limits with a real test session.
- Verify persisted journey/run IDs are loaded from server state rather than inferred local IDs before scored submissions.
- The current production app still uses the local persistence adapter as its UI cache; cross-device/server rehydration needs end-to-end verification.
- Verify correctness/hint/explanation returned by the server are used as authoritative UI feedback; the existing app still has client-side answer-vault imports for demo/practice.
- Verify cross-user RLS denial, administrator authorization, idempotency and leaderboard reads against the deployed project.

No claim is made that hiding the demo banner changes the backend's actual mode. Mode configuration is retained; technical diagnostics remain in the administrator area.
