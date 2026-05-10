# Spec: Bastide Saint-Pierre Noyer — Gîte Website (MVP)

## 1. Goal

A bilingual (French default, English) single-page static website to advertise a vacation rental gîte in Provence, France. The primary goal is to persuade visitors to contact the owners to book a weekly reservation.

**Target users:**
- French-speaking families/couples looking for a Provençal rental
- English-speaking tourists visiting the South of France

**Problem solved:** Potential guests need clear information about the property, its location, and a way to contact the owners without exposing the email to bots.

---

## 2. Scope

### In Scope (MVP)
- Single-page Hugo site with smooth scroll navigation
- Bilingual support: French (`fr`) as default, English (`en`) available
- Language auto-detection + manual toggle (Hugo i18n standard behavior)
- Sections:
  - **Hero / Header**: Property name, tagline, CTA button
  - **About**: Description of the property and hosts (Bernard & Anne)
  - **Gallery / Property**: Photos of rooms, garden, kitchen, etc.
  - **Amenities**: Key features (Wi-Fi, parking, beds, etc.)
  - **Location**: Area description, proximity to Marseille/Sainte-Baume
  - **Contact**: Bot-protected email with a pre-filled message template
- Email obfuscation to prevent scraping by bots
- Responsive design (mobile-first)
- GitHub Pages deployment via git push
- Hugo's built-in i18n (`i18n/fr.yaml`, `i18n/en.yaml`)

### Out of Scope (for MVP)
- Online booking system or availability calendar
- Payment processing
- Multi-page structure (blog, separate gallery page, etc.)
- Dynamic content or CMS backend
- Contact form with server-side processing (no backend budget)
- Guest reviews / testimonials
- Live chat or messaging widget
- SEO beyond basic meta tags
- Cookie consent banner (no tracking planned)

---

## 3. Inputs / Outputs

### Inputs
- `hugo.toml` / `config.toml` — site config with languages, menus, params
- `i18n/fr.yaml` — French translations for all UI strings
- `i18n/en.yaml` — English translations for all UI strings
- `data/homepage.yaml` — section content (texts, image paths)
- `content/` — minimal or empty (single-page site driven by data)
- `static/assets/img/` — property photos and logo
- Theme: `graysx` (existing, may be modified for i18n)

### Outputs
- Static HTML/CSS/JS site
- Deployed to `bastidesaintpierrenoyer.fr` via GitHub Pages
- Contact section renders email in a human-readable but bot-resistant way

---

## 4. Edge Cases

| Scenario | Behavior |
|----------|----------|
| **Visitor has no email client** | Display email in obfuscated form; allow manual copy-paste |
| **JavaScript disabled** | Mailto link still works; email obfuscation uses HTML/CSS technique (not JS-only) |
| **Bot scrapes the page** | Email is not present in plain text; uses entity encoding, image fallback, or CSS-reversed text |
| **Translation missing for a string** | Hugo falls back to default language (French) |
| **Visitor's browser is set to German/Spanish** | Falls back to French default; manual toggle available |
| **Image fails to load** | Alt text in current language displayed |
| **Mobile with small screen** | Single-column layout, hamburger menu, touch-friendly targets |

---

## 5. Constraints

- **Platform:** Hugo static site generator (already in use)
- **Hugo Version:** Must be kept up-to-date (latest stable); build should not rely on deprecated features
- **Hosting:** GitHub Pages (free, already configured with CNAME)
- **Cost:** $0 for infrastructure; no paid services for MVP
- **No backend:** No server-side code, no database, no form handler
- **Performance:** Target < 2s first contentful paint on 3G
- **Accessibility:** WCAG 2.1 AA where feasible (alt text, color contrast, keyboard nav)
- **Security:** No secrets in repo; email must not be commit-able in plain text if possible (or obfuscated at build time)
- **Maintenance:** Single person (non-technical) should be able to update text by editing YAML files

---

## 6. Success Criteria

- [ ] Site loads at `bastidesaintpierrenoyer.fr` with HTTPS
- [ ] French is the default language; English is accessible via toggle
- [ ] All UI text is translatable via `i18n/` files
- [ ] Contact section contains a working `mailto:` link to `bastidesaintpierrenoyer@gmail.com`
- [ ] Email is not harvestable by simple regex scrapers (obfuscated in DOM/source)
- [ ] Pre-filled email template includes: guest name, desired dates, number of guests, message
- [ ] Site is responsive and usable on mobile (iPhone SE size and up)
- [ ] All placeholder text ("Graysx", lorem ipsum) is replaced with real content
- [ ] `baseURL` in config points to `https://bastidesaintpierrenoyer.fr`
- [ ] Build passes (`hugo`) without errors
- [ ] Deploy succeeds via git push to origin

---

## OpenSpec Delta

### ADDED
- `i18n/fr.yaml` — French UI translations
- `i18n/en.yaml` — English UI translations
- `data/homepage.yaml` — Real content replacing placeholders
- Email obfuscation logic in contact partial (CSS/HTML technique)
- Language switcher partial (Hugo multilingual menu or custom toggle)
- `hugo.toml` multilingual configuration (`languages`, `defaultContentLanguage`)
- Real property images in `static/assets/img/`

### MODIFIED
- `config.toml` / `hugo.toml` — Add `[languages]` block, set `defaultContentLanguage = 'fr'`, update `baseURL`
- `themes/graysx/layouts/index.html` — Wire i18n strings (`i18n "key"`), add language switcher
- `themes/graysx/layouts/partials/header.html` — Add language toggle to navbar
- `themes/graysx/layouts/partials/footer.html` — Update copyright and links
- Contact section in `index.html` — Replace generic form with obfuscated email + mailto template
- `README.md` — Replace with project description

### REMOVED
- `content/posts/my-first-post.md` — Draft blog post (not needed for single-page MVP)
- Placeholder text in `data/homepage.yaml` ("Graysx", lorem ipsum)
- Unused `layouts/shortcodes/rawhtml.html` (if no raw HTML needed)
- `REPO_DESCRIPTION.md` (or merge into README)

---

## Task Checklist

- [ ] Update `hugo.toml` / `config.toml` with multilingual setup and correct baseURL
- [ ] Create `i18n/fr.yaml` with all French UI strings
- [ ] Create `i18n/en.yaml` with all English UI strings
- [ ] Write real `data/homepage.yaml` content (hero, about, gallery, amenities, location, contact)
- [ ] Add language switcher to navbar partial
- [ ] Wire all template strings to `i18n` keys
- [ ] Implement email obfuscation in contact section
- [ ] Build pre-filled mailto link with template subject/body
- [ ] Replace placeholder images with real property photos
- [ ] Test responsive layout on mobile viewport
- [ ] Build locally with `hugo` and fix errors
- [ ] Deploy to GitHub Pages
- [ ] Verify site loads correctly in French and English
- [ ] Verify contact email is obfuscated in page source
- [ ] Verify mailto link opens email client with correct template
