# Analytics schema (seed lock)

Source of truth for scorecard joins. growth analytics reads this; bots do not invent columns.

## Attribution grain (one row ≈ one session/journey touch)

| column | meaning |
| --- | --- |
| `angle_tag` | named-merchant / densify-military-proof / borrowed-authority / invisible-offer / awkward-ask-auto / hood-density |
| `merchant_on_creative` | merchant featured on ad or UGC (e.g. better_buzz_pl) |
| `map_merchant_tapped` | merchant pin tapped on Member Loop map |
| `channel` | paid_social \| organic_social \| email \| ugc |
| `creator_handle` | TT/IG handle when channel=ugc; else null |
| `creative_id` | Meta creative id or organic asset id |
| `gate` | email/phone captured at map gate (bool + timestamp) |
| `install` | AppsFlyer app install attributed to this journey |
| `first_earn_7d` | first earn at named merchant within 7d of gate/install |

## UTM contract (day one)

- `utm_medium` = paid_social | organic_social | email | ugc
- `utm_content` = `{creative_id}` or `{creator_handle}`
- `utm_campaign` = carries e2e pin (e.g. bb_pl_e2e)

## Boards

Keep **member BB e2e** and **merchant grader** boards separate. No blended CAC.

## Build order (black boxes)

1. merchant_on_creative ↔ map_merchant_tapped
2. gate
3. install (AppsFlyer; mailchimp → `media_source=mailchimp`)
4. first_earn_7d
5. drip joins for optimization (UTMs stamped from day one)

Aidan still needs to confirm where `first_earn` and `map_merchant_tapped` live today (Supabase / AppsFlyer / nowhere).
