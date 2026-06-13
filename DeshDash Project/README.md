# DeshDash — Masterclass Data Backend

Upload these 4 files to a **public GitHub repo**. The notebook reads them via raw URLs — no auth.

| File | Role | Read via |
|---|---|---|
| `deshdash.db` | OrderDB — orders table — 12 months, ~125k rows (Jun 2025 - May 2026), incl. dirt | `sqlite3` + `pd.read_sql` |
| `restaurants.json` | Partner API response (450 restaurants) | `requests.get(RAW_URL).json()` |
| `campaigns.csv` | Marketing campaigns log (3 rows) | `pd.read_csv` |
| `zones.xlsx` | Ops Zone Tracker, header on row 4 → `skiprows=3` | `pd.read_excel` |

Raw URL pattern: `https://raw.githubusercontent.com/<user>/<repo>/main/<file>`

## orders table columns
order_id, customer_id, restaurant_id, zone, order_time, order_amount,
delivery_fee, discount_pct, promo_code, delivery_minutes, status

## Planted dirt (Silver/cleaning step)
- 390 duplicate rows
- zone text: lowercase, trailing spaces, "Mohammedpur" misspelling
- 220 missing zones
- order_time stored as text
- cancelled/refunded rows to filter
- merge creates a zone_x/zone_y collision (teachable)

## INSTRUCTOR ONLY — the 6-step chain (verified)
NOTE: DB holds 12 months. Months Jun2025-Apr2026 look healthy (per-order commission ~110-117 Tk).
      The analyst filters to the window named in the email (Apr vs May 2026) where it breaks.

0. orders +9%, commission -18%  ->  money per order must have fallen
1. commission/order 111.5 -> 83.2 Tk  ->  order mix changed (rates are fixed)
2. premium orders -42%, budget orders +28%  ->  where?
3. premium collapse = Gulshan/Banani; budget surge = Mirpur/Uttara  ->  why budget surge?
4. 85% of May Mirpur/Uttara budget orders = promo MIRP-MAY25; campaigns.csv shows
   "Mirpur-Uttara Budget Blast", 1 May, those zones, 25% off  ->  surge explained.
   But why did Gulshan premium die?
5. Gulshan delivery 33->55 min, Banani 33->53; Excel riders Gulshan 32->19, Banani 28->17,
   Mirpur 54->72, Uttara 46->61. Promo pulled riders from premium zones -> slow delivery
   -> premium customers left.
ROOT CAUSE: our own Mirpur/Uttara budget promo drove low-margin volume AND forced a
rider reallocation that wrecked delivery times in high-margin premium zones. We grew
order count into a margin loss. (Commission is charged on discounted amount, compounding it.)
