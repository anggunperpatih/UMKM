# Kopi Nusantara Raya — Premium UMKM Landing Page

A premium, animated, fully responsive single-page company profile built for an Indonesian coffee UMKM ("Usaha Mikro, Kecil, dan Menengah"), demonstrating modern landing-page craft on a lightweight, framework-free stack.

## 🗂️ File Structure

```
kopi-nusantara/
├── index.html      # Markup, SEO meta, JSON-LD, all 13 sections
├── style.css        # Custom styles layered on top of Tailwind CDN utilities
├── script.js         # Vanilla JS: nav, counters, Swiper, FAQ, lightbox, video, form
├── assets/            # Reserved folder for local assets (currently empty — all
│                       media is pulled from Unsplash/YouTube CDNs, see below)
└── README.md
```

## 🧱 Tech Stack

| Purpose            | Library / Tool                         | Load method |
|---------------------|-----------------------------------------|-------------|
| Styling             | Tailwind CSS                            | CDN (`cdn.tailwindcss.com`) + `style.css` |
| Typography          | Google Fonts — Poppins (body/UI) & Playfair Display (headline accent) | `<link>` |
| Icons               | Font Awesome 6                          | CDN |
| Scroll animation    | AOS (Animate on Scroll)                 | CDN |
| Carousel            | Swiper.js 11                            | CDN |
| Interactivity       | Vanilla JavaScript (no framework)       | `script.js` |

No build step, no bundler — open `index.html` in any browser or deploy as static files.

## 🎨 Design Tokens

- **Primary:** `#16a34a` — **Secondary:** `#22c55e` — **Accent:** `#f59e0b` — **Background:** `#ffffff`
- **Ink / dark text:** `#3f2a1d` (warm "roast" brown, used instead of pure black for a coffee-appropriate feel)
- **Type system:** Poppins for UI/body copy, Playfair Display (italic accents) for headlines — a warm, editorial pairing that reads "artisan roastery" rather than generic SaaS.
- **Motifs:** rounded-2xl/3xl cards, soft green-tinted shadows, glassmorphism navbar & hero stat bar, warm gradient CTAs.

## 🖼️ Media Sources (royalty-free)

All photography is served directly from Unsplash's CDN (`images.unsplash.com`), free to use under the [Unsplash License](https://unsplash.com/license) — no attribution required. Testimonial avatars use [randomuser.me](https://randomuser.me), a free placeholder-portrait API. The company-profile video embed uses a real, publicly available YouTube video about Indonesian coffee UMKM (`https://www.youtube.com/watch?v=m3w7KqaOzO0`).

> ℹ️ For a real client deployment, swap these stock URLs for the client's own product photography, team photos, and an original company-profile video for full brand authenticity and legal safety.

## ✨ Sections Implemented

1. Sticky glassmorphism navbar with WhatsApp CTA & mobile menu
2. Full-screen hero with background image, badge, headline, dual CTAs, floating glass stat bar, scroll indicator
3. About: story, vision/mission, values, animated timeline (1998–2026)
4. Why Choose Us — 6 hover-animated cards
5. Featured Products — 4 cards with images, price (IDR), description, WhatsApp CTA
6. Video Profile — click-to-load responsive YouTube embed (lazy, for performance)
7. Gallery — CSS-columns masonry layout + custom lightbox
8. Our Process — 5-step animated timeline
9. Testimonials — Swiper slider with star ratings & customer photos
10. Statistics — scroll-triggered animated counters (also duplicated in hero)
11. FAQ — accessible accordion
12. Contact — Google Maps embed, WhatsApp/email/phone cards, validated contact form
13. Footer — social links, quick links, dynamic copyright year

Plus: floating WhatsApp button, back-to-top button, preloader.

## ⚡ Performance & SEO

- All non-critical images use `loading="lazy"`; hero uses `fetchpriority="high"` + `srcset` for responsive delivery.
- Video iframe is only injected into the DOM on click (no third-party JS/network cost until requested).
- Semantic HTML5 landmarks (`header`, `main`, `section`, `footer`), ARIA labels/expanded states on nav, accordion, and modal.
- Visible focus states and a skip-to-content link; `prefers-reduced-motion` respected globally.
- Meta title/description, Open Graph, Twitter Card, and JSON-LD `LocalBusiness` structured data included in `<head>`.

## 🔧 Customization Notes

- Update WhatsApp number by find-and-replace on `6281234567890`.
- Update `JSON-LD` block in `index.html` with the real business address/hours before going live.
- Replace the Google Maps `iframe` query string with the client's exact address for pinpoint accuracy (or swap in an API-key-based embed for a custom pin).
- The contact form currently shows a client-side success message only — connect `#contactForm`'s submit handler in `script.js` to your backend, form service (e.g. Formspree), or serverless function to actually deliver messages.

## 🚀 Local Preview

Just open `index.html` in a browser, or serve the folder with any static server, e.g.:

```bash
npx serve .
```
