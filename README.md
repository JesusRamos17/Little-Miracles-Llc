# Little Miracles Co. — website

A single-file storefront for a San Diego handmade soap and body care business. Shop, cart with automatic bundle pricing, customer accounts, order tracking, a built-in FAQ assistant, and a contact form — all in one `index.html` with no build step and no dependencies.

## Run it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploy

Drag this folder onto [app.netlify.com/drop](https://app.netlify.com/drop) for a live HTTPS URL. No build command, no publish directory to configure. Every later push redeploys automatically if you connect the repo in Netlify instead.

## What's here

| Path | What it is |
|---|---|
| `index.html` | The entire site — markup, styles, logic, product data, and the assistant |
| `images/` | Product, category, and brand photography |
| `video/` | Behind-the-scenes clips with poster frames |
| `supabase-setup.sql` | Database schema and Row Level Security policies for customer accounts |
| `tests/` | 321 checks across 7 suites, no dependencies |

## Tests

```bash
node tests/run-all.js
```

Covers cart and bundle pricing, account signup and recovery, the coming-soon section, the config diagnostics panel, the assistant's answers, and asset/markup integrity — including a check that one customer can never see another customer's orders, and that no medical or SPF claims creep back into the copy. Run it before any deploy; it exits non-zero on failure.

Two product photos are skipped on purpose (`body-tallow-cream`, `soap-sweet-vanilla`) until they've been shot. Remove them from the skip list in `tests/07-assets-and-markup.test.js` once the files exist.

## Configuration

Everything configurable lives in one `CONFIG` block near the top of the script section in `index.html`:

- **`WEB3FORMS_KEY`** — turns on real email delivery for the contact and order forms. Without it, forms fall back to opening the visitor's mail app with the message pre-filled.
- **`SUPABASE_URL` / `SUPABASE_ANON_KEY`** — enable real customer accounts. Run `supabase-setup.sql` in the Supabase SQL editor first. Leave blank and account pages run in demo mode.
- **`FREE_DELIVERY_ZIPS` / `FREE_DELIVERY_MINIMUM`** — the free local delivery zone, expressed as postcodes so no home address appears in the source.

The Supabase **anon** key is designed to be public and is safe in this file; Row Level Security is what protects customer data. The `service_role` key must never go here — the built-in diagnostics panel will flag it if it does.

## Status

A working prototype, not a live store. Product data, photos, pricing, and the cart are real; payments are not processed and order history is sample data until Supabase is connected.

## License

Code is MIT. Photography, video, product names, and brand assets are © Little Miracles LLC — see [LICENSE](LICENSE).
