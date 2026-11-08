# Website Field Study: Lefkoşa Türk Belediyesi

**Site:** <https://...>  
**Organisation:** Lefkoşa Türk Belediyesi (Nicosia Turkish Municipality)  
**Student:** <your name> – 2403060010  
**Audit date:** <YYYY-MM-DD, HH:MM>  
**Browser:** <name and version>

> Write each item as: what you saw first, then what it means for this site. End each paragraph with its evidence ID, e.g. [A4]. Max 6 pages before the appendix.

## Part A: Under the hood

### A1 – HTTPS and redirects
<observation> <interpretation> [A1]

### A2 – URL structure

| URL | HTTPS | Short & readable | Lowercase | Hyphens not underscores | Few query params |
|---|---|---|---|---|---|
| https://lefkosabelediyesi.org/baskan | Pass | Pass | Pass | Pass (single word) | Pass |
| https://lefkosabelediyesi.org/sehir-fotograflari | Pass | Pass | Pass | Pass | Pass |
| https://lefkosabelediyesi.org/haberler/metaverse-nft-ve-yapay-zeka-sinemayla-bulusuyor | Pass | <Pass / Fail – your call: readable, but 6-word slug> | Pass | Pass | Pass |
| https://lefkosabelediyesi.org/etkinlik-takvimi | Pass | Pass | Pass | Pass | Pass |
| https://lefkosabelediyesi.org/sehrin-tarihi | Pass | Pass | Pass | Pass | Pass |

<interpretation> [A2]

### A3 – One request, closely
Method: GET · Status: 200 OK · Content-Type: text/html; charset=utf-8 · Requests: <re-check with Keep log off> · Total size: <...> · Fetch/XHR example: <...>

<interpretation> [A3]

### A4 – A page that does not exist
Status code: <confirm in DevTools – request row is red, so likely 404> · Visitor sees: large "404", "Maalesef aradığınız sayfayı bulamadık." ("Sorry, we couldn't find the page you were looking for."), buttons "Ana Sayfaya Dön" (back to home) and "Online İşlemlere Dön" (back to online services).  
<soft 404 or not, and what it means for visitors and search engines> [A4]

### A5 – Caching
Main document `Cache-Control`: `no-cache, private` – <one-sentence meaning>  
Image `Cache-Control` (/storage/3752/FOTO-1-%2810%29.jpeg): `max-age=14400` (Age: 4909, served from memory cache) – <one-sentence meaning>  
[A5]

### A6 – Where the page is built
<server-rendered / browser-rendered / mixed, with justification> [A6]

### A7 – SEO basics

| Item | Found? | Value / note |
|---|---|---|
| `<title>` | Yes | "Lefkoşa Türk Belediyesi" – organisation name only, no description of the page |
| Meta description | Yes (weak) | "Lefkoşa Türk Belediyesi Resmi Web Sitesi" ("official website") – generic, mentions no services |
| Canonical link | **No** | Missing; both `https://lefkosabelediyesi.org/` and `https://www.lefkosabelediyesi.org/` return the page [A3] |
| `lang` attribute | Yes | `<html lang="tr">` |
| Number of `<h1>` | **0** | No `<h1>` on the home page; the main heading "Nasıl yardımcı olabiliriz?" is an `<h2>` |
| `rel="sponsored"` / `rel="ugc"` | Not applicable | No adverts or user comments on the page |
| Other | – | `og:type` declared twice; `<meta name="keywords" content="">` is empty; Open Graph title, description and image are present |

<interpretation> [A7]

## Part B: The experience

### B1 – Loading state (Slow 3G)
Throttling: Slow 4G <note why not 3G, or retake on 3G>.  
Stage 1: completely blank white page – no logo, no spinner, no text [B1-loading-1].  
Stage 2: layout and all text visible on a plain dark background (no video yet); every icon shows as an empty square – search icon, language-menu arrow, carousel arrows, "Tüm haberler için" chevron – because the icon font (around-icons) is still downloading [B1-loading-2].  
Stage 3: background video and icon font loaded; page looks complete [B1-loading-3].  
<what this means for a visitor on a slow connection> [B1]

### B2 – Empty state
Search term: "zzzzq" (home-page search box, filter "Genel") · Exact wording shown: "Maalesef hiç sonuç bulamadık." ("Unfortunately, we found no results.")  
Results appear instantly in a dropdown under the box, without a page reload; the five filter buttons (Genel, Haberler, Etkinlikler, Tarihi ve Turistik Yerler, Meclis Kararları) stay visible. No spelling suggestion, no link to Online İşlemler, Alo 185 or the contact page.  
<judgement> [B2]

### B3 – Error state (Offline)
Navigation while offline (clicked "Online İşlemler" → /online): the request failed and the browser replaced the site with its own error page (dinosaur, "Press space to play", ERR_INTERNET_DISCONNECTED). Nothing from the municipality is shown – no logo, no message, no retry button [B3-offline-navigation].  
Search while offline: <result of typing in the search box while offline> [B3-offline-search]  
<judgement> [B3]

### B4 – Form validation
Form tested: "Lefkoşalılar Söylüyor" contact form <confirm page URL, e.g. /iletisim> · Fields left empty: all five (Adınız, Soyadınız, Telefon Numaranız, E-posta Adresiniz, message) · "Gönder" pressed; the form was not sent.  
Errors appear inline on every field at once: red border plus a red ⊘ icon inside each box. **No error text** – nothing says what is wrong or what format is expected (e.g. for phone or e-mail). The Cloudflare check above the button shows "Success!". Required fields are marked with a red asterisk in advance. Is typed input kept: <retest with dummy text in some fields>.  
<judgement> [B4]

### B5 – Labels

| Field | Visible label? | Clicking label focuses field? | Placeholder only? |
|---|---|---|---|
| Adınız | Yes | **No** | No |
| Soyadınız | Yes | **No** | No |
| Telefon Numaranız | Yes | **No** | No |
| E-posta Adresiniz | Yes | **No** | No |
| Soru görüş ve önerilerinizi bekliyoruz. (message) | Yes | **No** | No |

Why clicking the label does nothing: each field is not inside its `<label>`, and the label has no `for` attribute pointing to the field (the message `<textarea name="questions_comments_suggestions">` has no `id` at all). The label text is only visually above the field, so the browser does not connect them [B5].

[B5]

### B6 – Keyboard only
Task: <...> · Keys used: Tab, Shift+Tab, Enter · Focus visible? **No** – Tab does move focus (Console `document.activeElement` returned the "İşyeri Veri Güncelleme Formu" card link), but that card shows no outline, border or colour change; it looks exactly like the cards around it. The only hint is the browser's own link preview at the bottom of the window, so it is very hard to tell where focus is [B6-keyboard] · Order logical? Hard to judge without a visible focus; <note order seen via activeElement, if checked> · Traps? <...> · Skip link? **No** – none appears on the first Tab press · Task completed? <yes/no>  
**Shift+Tab appeared not to work at all** <re-check with `document.activeElement`: since Tab turned out to move focus invisibly, Shift+Tab may be working invisibly too – keep this sentence only if focus really does not move back>  
<judgement> [B6]

### B7 – Automated audit (Lighthouse Accessibility)

| Page | Score | Top 3 issues |
|---|---|---|
| Home (https://lefkosabelediyesi.org/, 8:53:17 PM) | **86** (Performance 81, Best Practices 96, SEO 100) | <1> <2> <3> – scroll to the Accessibility section of the report |
| <inner page> | | |

[B7]

### B8 – Link and button text

| Current text | Where | Better text |
|---|---|---|
| Image only, `alt="Logo"` (19 links) | Home page, partner/project logo strip (orchestra, theatre, Paylaşım Mutfağı, marathon, etc.) | Name each project in the alt text, e.g. "Lefkoşa Belediye Orkestrası" / "Paylaşım Mutfağı" |
| "Previous slide" / "Next slide" (icon-only buttons) | Home page, news carousel arrows | Turkish labels matching the page language: "Önceki haber" / "Sonraki haber" |
| Image only, `alt="ai-logo"` | Footer, designer credit link | "Ai Web Solutions" |
| "Tüm haberler için ›" | Home page, under the news carousel | "Tüm haberleri gör" (a complete action, not a sentence fragment) |
| "Soru ve Cevaplar →" / "Kent Rehberi →" | Home page, FAQ and city-guide cards | Acceptable; could be more specific, e.g. "Sıkça sorulan soruları oku" / "Kent rehberi haritasını aç" |

[B8]

## Part C: Recommendations

### What works well
1. <strength> [ID]
2. <strength> [ID]

### Top three problems
Why this order: <one sentence>

| # | Problem (one sentence) | Evidence | Who it affects | Fix in plain words | Effort | Impact |
|---|---|---|---|---|---|---|
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |

## Appendix A: Evidence log

| ID | File | What it shows | Time and date | Page address |
|---|---|---|---|---|
| A1 | evidence/A1-https-address.png | Address bar ending on https:// | <time and date> | https://www.lefkosabelediyesi.org/ |
| A1 | evidence/A1-certificate.png | Certificate for lefkosabelediyesi.org, issued by WE1 (root GTS Root R4), expires 28 Dec 2026, shown as valid | <time and date> | https://www.lefkosabelediyesi.org/ |
| A2 | | | | |
| A3 | evidence/A3-network.png | Main document request: GET, 200 OK, Content-Type text/html; charset=utf-8, Server cloudflare, Cache-Control no-cache, private | Sun 8 Nov 2026, 20:28 EET (server Date header 18:28:30 GMT) | https://lefkosabelediyesi.org/ |
| A4 | evidence/A4-missing-page.png | Made-up page /no-such-page-123: branded 404 page with two navigation buttons; request highlighted red in Network panel | <time and date> | https://lefkosabelediyesi.org/no-such-page-123 |
| A5 | evidence/A5-document-cache-control.png | Main document Cache-Control: no-cache, private | <time and date> | https://lefkosabelediyesi.org/ |
| A5 | evidence/A5-asset-cache-control.png | Image Cache-Control: max-age=14400, Age 4909, 200 OK (from memory cache) | <time and date> | https://lefkosabelediyesi.org/storage/3752/FOTO-1-%2810%29.jpeg |
| A6 | evidence/A6-source.png, evidence/A6-elements.png | View-source vs Elements panel for the home page (Elements shows JS-added accessibility wrapper, chat widget, Swiper slide copies) | <time and date> | https://lefkosabelediyesi.org/ |
| A7 | evidence/A7-head.png, evidence/A7-h1-search.png | `<head>` meta tags (title, description, no canonical) and Elements search for `<h1>` returning 0 results | <time and date> | https://lefkosabelediyesi.org/ |
| B1 | evidence/B1-loading-1.png, evidence/B1-loading-2.png, evidence/B1-loading-3.png | Home page loading on Slow 4G: blank → text with missing icons → complete with video | <time and date, seconds apart> | https://lefkosabelediyesi.org/ |
| B2 | evidence/B2-empty-search.png | Home-page search for "zzzzq": message "Maalesef hiç sonuç bulamadık." with filter buttons, no next step offered | <time and date> | https://lefkosabelediyesi.org/ |
| B3 | evidence/B3-offline-navigation.png | Offline navigation to /online: browser's own ERR_INTERNET_DISCONNECTED page, request "online" failed in Network panel | <time and date> | https://lefkosabelediyesi.org/online |
| B3 | evidence/B3-offline-search.png | <search while offline> | <time and date> | https://lefkosabelediyesi.org/ |
| B4 | evidence/B4-form-errors.png | Contact form "Lefkoşalılar Söylüyor" submitted empty: red borders and ⊘ icons on all five fields, no error text | <time and date> | <page URL> |
| B5 | evidence/B5-labels.png | Contact form with visible labels; Elements panel shows the message `<textarea>` has a name but no `id`, next to a separate `<label>` | <time and date> | <page URL> |
| B6 | evidence/B6-keyboard.png | Keyboard focus on the "İşyeri Veri Güncelleme Formu" card (confirmed by `document.activeElement` in Console) with no visible focus indicator on the page | <time and date> | https://lefkosabelediyesi.org/ |
| B7 | evidence/B7-lighthouse-home.png | Lighthouse on home page: Performance 81, Accessibility 86, Best Practices 96, SEO 100 | Sun 8 Nov 2026, 20:53 | https://lefkosabelediyesi.org/ |
| B7 | evidence/B7-lighthouse-home-issues.png, evidence/B7-lighthouse-inner.png | <home accessibility issues; inner page score and issues> | <time and date> | <inner page URL> |
| B8 | evidence/B8-logo-links.png, evidence/B8-carousel-buttons.png | Partner-logo links with alt="Logo"; carousel buttons with English aria-labels | <time and date> | https://lefkosabelediyesi.org/ |
