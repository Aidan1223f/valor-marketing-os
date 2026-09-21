# Analytics stack (v1 lock)

## Decision
- **Supabase** = system of record for product events (map taps, verify, link, first earn). Thin `analytics` mart views — not raw OLTP as the scorecard.
- **Hex** = skip for v1 (analysis UI later; not a warehouse).
- **Warehouse** (BQ/Snowflake) = defer until joins hurt or ad-level history exceeds API windows.
- Scorecard reads: **Supabase marts + live API pulls** (Meta, Mailchimp; Twilio/UGC custom when available).

## Sources
1. Supabase — product truth
2. Meta Ads — paid
3. Mailchimp — email (`utm_medium=email`, AppsFlyer `media_source=mailchimp`)
4. Twilio — SMS/phone gate
5. UGC custom reporting — creator_handle, content_id, merchant_on_creative, spend/delivery, join key to user

## Join contract
See `schema.md`. Kill signal for creatives/captions/UGC: first earn @ Better Buzz Point Loma in 7d by `utm_content` / `creator_handle`.
