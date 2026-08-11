# Getting a live link you can share

## Fastest option — 2 minutes, no account, free

1. Go to **https://app.netlify.com/drop**
2. Drag **`little-miracles-site.zip`** (in this folder) onto the page
3. Wait about 20 seconds

You'll get a live URL like `https://spontaneous-marzipan-4f8c21.netlify.app` that you can text to anyone. It works on phones, it's HTTPS, and it's free.

The link stays up for a while on its own. To keep it permanently and get a nicer address, click **"Claim your site"** — that's when it asks you to make an account. You can rename it to something like `little-miracles.netlify.app`, or point your own domain at it later.

**To update the site after a change:** drag the new zip onto the same Netlify page (or the site's Deploys tab if you claimed it). It replaces the old version.

---

## What's in this folder

| | |
|---|---|
| `little-miracles-site.zip` | Drag this to Netlify. Everything the site needs, 5.5 MB. |
| `site-to-publish/` | The same thing unzipped, if you'd rather drag a folder. |
| `website/` | The working copy. **Edit here**, then re-zip when you want to publish. |
| `website/index.html` | The entire site — one file. Open it in a browser to preview. |
| `website/Download Sift Photos.command` | Re-downloads the product photos from the old Sift form. |
| `instagram-…/` | Your Instagram export. Not published — source material only. |

---

## Turning on real customer accounts (about 10 minutes)

Accounts need a database. A plain HTML file has nowhere to store a customer or check a password, so this part is not optional if you want people to actually sign in.

**Step 1. Create the database**

- Go to **supabase.com**, sign up, create a project
- Pick a region close to San Diego (West US)
- Choose a strong database password and save it somewhere. You will rarely need it.

**Step 2. Create the tables**

- In your project, open **SQL Editor** in the left sidebar, click **New query**
- Open `website/supabase-setup.sql`, copy the whole file, paste it in, press **Run**
- You should see profiles, orders, order_items, favorites and restock_alerts under **Table Editor**

**Step 3. Connect the site**

- Go to **Project Settings → API**
- Copy the **Project URL** and the **anon public** key
- Open `website/index.html`, find the CONFIG block near the top of the script, paste both in
- Re-zip and re-upload

That's it. Signup, sign-in, order history, saved addresses, loyalty stamps and favourites all go live.

**A note on the key you just pasted.** The anon key is meant to be public and it is safe sitting in the page. What actually protects your customers is Row Level Security, which `supabase-setup.sql` switches on. It makes the database itself refuse to hand one customer another customer's orders, no matter what is asked of it. **Never paste the `service_role` key into the site.** That one bypasses every protection, and anyone viewing your page source would have full access to all your data.

**Managing orders.** Customers can create orders but deliberately cannot change them. To move an order from received to making to ready, open **Table Editor → orders** in Supabase and edit the `status` column. Valid values are `received`, `paid`, `making`, `ready`, `completed`, `cancelled`. The customer's tracker updates the next time they look.

**Loyalty stamps** are counted from completed orders, currently one stamp per order and ten for a reward. Tell me the real rule you use on Instagram and I will match it.

---

## Before you share it widely

**1. Turn on email notifications** (5 minutes)

Right now the contact form and order button open your email app instead of sending on their own. To fix that:

- Go to **https://web3forms.com**
- Enter `Stephaniemcornejo@gmail.com`, click **Create Access Key**
- Check that inbox for the key
- Open `website/index.html` in TextEdit, find the **CONFIG** block near the top of the script section, and paste the key between the quotes on the `WEB3FORMS_KEY` line
- Re-zip and re-upload

**2. Know what this is and isn't**

This is a working prototype — real product data, real photos, a real cart with the 3-for-$19 discount applied automatically, and a real contact form. It is **not** a real store yet:

- Payments aren't processed. "Place order" emails you the order; it doesn't take money.
- Accounts don't actually save. Sign-in is a demo view.
- Order history is sample data.

That's what the move to Square Online is for. Share this link to get reactions to the design and the flow, not to take live orders.

---

## Still needed

**Three product photos** — square, shot the same way as your others:

- `body-tallow-cream.jpg` — Tallow Face + Body Cream (your hero product, and the only gap that really matters)
- `soap-sweet-vanilla.jpg` — Sweet Vanilla soap bar
- A cleaner `soap-honey-oat.jpg` — the current one has a customer's "Baby Shower — Liv Rascon" labels on it

Drop them into `website/images/products/` with exactly those filenames and they appear automatically. No code change needed.

**Real reviews.** The site had three placeholder testimonials on it. I removed them — publishing invented customer quotes breaks FTC rules and one of them made a medical claim. That section now says only things that are factually true about the business. When you're ready, send me real quotes from your Reviews highlight (first name and city is enough) and I'll put them back properly.

**Better source video, if you have it.** Instagram's export downgraded two of your three reels to 360p. If the originals are still on Stephanie's phone, send them and I'll re-encode at full quality — the shop clip came through at 1080p and looks noticeably sharper than the other two.

---

## Things I changed for legal safety

Worth knowing about, since they affect how you describe products anywhere — not just on this site.

**Removed disease claims.** The tallow cream was described as "our most-loved for eczema-prone skin," and one testimonial said it "cleared up my son's eczema." In the US, saying a cosmetic treats or heals a condition legally reclassifies it as an unapproved drug. It now reads "rich, simple, and made for very dry skin," which says the same thing to a customer without the exposure.

**Removed the SPF / sunscreen claim.** Sunscreen is regulated as an over-the-counter drug, and selling something as SPF requires specific testing and labeling. The product is now "Mineral Body Stick," described by what's in it — mango and shea butter, zinc oxide, vitamin E. **If you're currently listing it anywhere as sunscreen or SPF, that's worth fixing there too.**

**Softened "a nurse formulated these"** to "made by a nurse." The first version implies clinical authority behind the formulation, which invites a level of scrutiny you don't want.

**Removed "we've emailed your receipt"** from the order confirmation, since at the prototype stage that isn't true yet.

Separately: one of your Instagram captions says "FDA approved ingredients." The FDA doesn't approve cosmetic ingredients, and that phrasing is a common enforcement trigger. Worth editing that post.

---

## The assistant

The chat bubble in the bottom-right answers questions about turnaround, pickup locations, delivery, shipping, payment methods, the soap bundle, ingredients, sensitive skin, dog products, gift sets, custom orders, pricing, and your story. It also handles direct product questions like "how much is the paw balm."

When it doesn't know something, it says so and drops the customer's question straight into the contact form rather than guessing.

**To add an answer:** open `website/index.html`, find the `KB` list in the script section, copy any block, and edit it:

```js
{ id:'refunds',
  k:['refund','return','exchange','send it back'],
  a:`Your answer here. <b>Bold</b> works, and <ul><li>lists</li></ul> too.`,
  acts:[['Button label',"go('contact')"]] },
```

`k` is the list of words a customer might type. Put in every phrasing you can think of — more is better.
