# Montana Block Email Studio

Standalone app for Montana Block Co. marketing email. Claude writes each email and draws its header graphic from the Brand Kit and the live Shopify catalog. Mitchell reviews and approves, and only then can it be sent through Omnisend.

Separate from the Shop Hub (own repo, own address, own install). It shares the hub's login system and server keys (Supabase project `montana-block-hub`), so the same email and password work in both.

## The approval rule (enforced in the database)

- Only the owner can approve (`mkt_approve`), and approval is tied to the exact content (hash).
- Any edit to subject, preview text, body or audience after approval cancels the approval.
- Status can't be set to approved/scheduled/sent any other way.
- `mkt-send` re-checks owner + approved + unchanged content + typed SEND before handing anything to Omnisend. Proofs go only to @mtblock.com inboxes.

## Pieces

| Piece | Where |
|---|---|
| App | This repo, GitHub Pages. Plain HTML/CSS/JS, installable. |
| Writing | Supabase Edge Function `mkt-generate` (Claude Sonnet, Brand Kit, live Shopify catalog). Usage in `mkt_ai_log`. |
| Sending | Supabase Edge Function `mkt-send` (Omnisend API; key stored server-side in `mkt_private`). |
| Data | `mkt_*` tables. Schema: `supabase/migrations/001_email_studio.sql`. Seed: `002_email_studio_seed.sql`. |
