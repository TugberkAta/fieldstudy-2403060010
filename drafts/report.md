# Website Field Study: Lefkoşa Türk Belediyesi

**Site:** https://lefkosabelediyesi.org/  
**Organisation:** Lefkoşa Türk Belediyesi (Nicosia Turkish Municipality)  
**Student:** Tuğberk Ata – 2403060010  
**Audit date:** 2026-11-08, from 17:13 EET (follow-up checks on 2026-11-09)  
**Browser:** Arc (Chromium-based) on macOS, with Chrome DevTools

## Part A: Under the hood

### A1 – HTTPS and redirects
Typing `http://` ended on `https://`; the 307 shown in DevTools came from the browser upgrading the connection itself. The certificate is valid (WE1, Google Trust Services, expires 28 Dec 2026), so visitors get an encrypted connection with no warnings – important for a site with payment and complaint forms [A1].

### A2 – URL structure

| URL | HTTPS | Short & readable | Lowercase | Hyphens not underscores | Few query params |
|---|---|---|---|---|---|
| https://lefkosabelediyesi.org/baskan | Pass | Pass | Pass | Pass (single word) | Pass |
| https://lefkosabelediyesi.org/sehir-fotograflari | Pass | Pass | Pass | Pass | Pass |
| https://lefkosabelediyesi.org/haberler/metaverse-nft-ve-yapay-zeka-sinemayla-bulusuyor | Pass | Pass (long but readable) | Pass | Pass | Pass |
| https://lefkosabelediyesi.org/etkinlik-takvimi | Pass | Pass | Pass | Pass | Pass |
| https://lefkosabelediyesi.org/sehrin-tarihi | Pass | Pass | Pass | Pass | Pass |

All five URLs are short, lowercase Turkish words joined by hyphens, with no IDs or parameters, and Turkish letters are converted (`fotograflari`), so addresses stay readable and easy to share. News slugs copy the whole headline and can get long [A2].

### A3 – One request, closely
Method: GET · Status: 200 OK · Content-Type: text/html; charset=utf-8 · Requests: <re-check with Keep log off> · Total size: <...> · Fetch/XHR example: `widget-settings` (fetch, started by the Tawk.to live-chat script, visible in B3-offline-navigation)

The HTML is small (about 13 kB), but the page loads a 3.2 MB background video – about two-thirds of the total – plus many third-party scripts (Google Analytics, Meta Pixel, Tawk.to chat). Cloudflare does not cache the HTML (`Cf-Cache-Status: DYNAMIC`), so every visit waits about 0.7 s for the municipality's server. The site also answers on both `lefkosabelediyesi.org` and `www.` without redirecting one to the other [A3].

### A4 – A page that does not exist
Status code: **404** (a second made-up address, `/532523`, also returned GET 404 for the document [A4-status-404]) · Visitor sees: large "404", "Maalesef aradığınız sayfayı bulamadık." ("Sorry, we couldn't find the page you were looking for."), buttons "Ana Sayfaya Dön" (back to home) and "Online İşlemlere Dön" (back to online services).  
The page is friendly and branded, with a plain-Turkish apology and two ways out. The server returns a real 404 status, not a normal 200, so it is a proper 404 rather than a soft 404 and search engines will not index dead links. It offers no search box or contact link [A4].

### A5 – Caching
Main document `Cache-Control`: `no-cache, private` – the browser must check with the server before reusing it, and Cloudflare may not store it.  
Image `Cache-Control` (/storage/3752/FOTO-1-%2810%29.jpeg): `max-age=14400` (Age: 4909, served from memory cache) – the photo can be reused for 4 hours without asking again.  
Fresh HTML and reusable images is a sensible split, but 4 hours is short for news photos that never change; Lighthouse flagged the same ("Use efficient cache lifetimes") [A5].

### A6 – Where the page is built
**Server-rendered, with browser enhancements.** The raw source already contains all real content – menu, search box, news cards, Online İşlemler cards and footer. JavaScript only adds extras: the accessibility widget, the chat iframes, carousel copies and the search results, which `search.js` fills in. Because the content is in the HTML, text appears before scripts finish (B1) and search engines can read it [A6].

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

The title and description only say "Lefkoşa Türk Belediyesi – official website", telling searchers nothing about services. There is no `<h1>` (the main heading is an `<h2>`) and no canonical link, although A3 showed the site answering on two addresses. Lighthouse still scored SEO 100, so a perfect automated score does not mean nothing is missing [A7].

## Part B: The experience

### B1 – Loading state (Slow 3G)
Throttling: Slow 4G <note why not 3G, or retake on 3G>.  
Stage 1: completely blank white page – no logo, no spinner, no text [B1-loading-1].  
Stage 2: layout and all text visible on a plain dark background (no video yet); every icon shows as an empty square – search icon, language-menu arrow, carousel arrows, "Tüm haberler için" chevron – because the icon font (around-icons) is still downloading [B1-loading-2].  
Stage 3: background video and icon font loaded; page looks complete [B1-loading-3].  
The first seconds are a blank white page, even though the site's code contains a loading spinner, so a resident on weak signal may think the site is broken. Text then arrives before decoration (good, see A6), but icons appear as empty boxes until their font loads. The finished look waits on a purely decorative 3.2 MB video [B1].

### B2 – Empty state
Search term: "zzzzq" (home-page search box, filter "Genel") · Exact wording shown: "Maalesef hiç sonuç bulamadık." ("Unfortunately, we found no results.")  
Results appear instantly in a dropdown under the box, without a page reload; the five filter buttons (Genel, Haberler, Etkinlikler, Tarihi ve Turistik Yerler, Meclis Kararları) stay visible. No spelling suggestion, no link to Online İşlemler, Alo 185 or the contact page.  
The message is clear, polite and instant, matching the tone of the 404 page. But it offers no next step – no link to Alo 185, Online İşlemler or İletişim, which the site already has [B2].

### B3 – Error state (Offline)
Navigation while offline (clicked "Online İşlemler" → /online): the request failed and the browser replaced the site with its own error page (dinosaur, "Press space to play", ERR_INTERNET_DISCONNECTED). Nothing from the municipality is shown – no logo, no message, no retry button [B3-offline-navigation].  
Search while offline: <result of typing in the search box while offline> [B3-offline-search]  
Offline, clicking a link replaces the site with the browser's own error page, with no message or retry from the municipality. That is normal for most sites, so it is a missed opportunity rather than a serious fault <add the offline search result in one sentence> [B3].

### B4 – Form validation
Form tested: "Lefkoşalılar Söylüyor" contact form <confirm page URL, e.g. /iletisim> · Fields left empty: all five (Adınız, Soyadınız, Telefon Numaranız, E-posta Adresiniz, message) · "Gönder" pressed; the form was not sent.  
Errors appear inline on every field at once: red border plus a red ⊘ icon inside each box. **No error text** – nothing says what is wrong or what format is expected (e.g. for phone or e-mail). The Cloudflare check above the button shows "Success!". Required fields are marked with a red asterisk in advance. Is typed input kept: <retest with dummy text in some fields>.  
The form blocks incomplete submissions and marks required fields in advance, but errors are shown only by colour and an icon, with no text explaining what is wrong. Colour-blind and screen-reader users get no usable feedback [B4].

### B5 – Labels

| Field | Visible label? | Clicking label focuses field? | Placeholder only? |
|---|---|---|---|
| Adınız | Yes | **No** | No |
| Soyadınız | Yes | **No** | No |
| Telefon Numaranız | Yes | **No** | No |
| E-posta Adresiniz | Yes | **No** | No |
| Soru görüş ve önerilerinizi bekliyoruz. (message) | Yes | **No** | No |

Why clicking the label does nothing: each field is not inside its `<label>`, and the label has no `for` attribute pointing to the field (the message `<textarea name="questions_comments_suggestions">` has no `id` at all). The label text is only visually above the field, so the browser does not connect them [B5].

Sighted users will not notice, but a screen reader announces each field only as "edit text", so blind users cannot tell the fields apart [B5].

### B6 – Keyboard only
Task: move from the top of the home page to the Online İşlemler cards and open one <confirm task and whether it was completed> · Keys used: Tab, Shift+Tab, Enter · Focus visible? **No** – Tab does move focus (Console `document.activeElement` returned the "İşyeri Veri Güncelleme Formu" card link), but that card shows no outline, border or colour change; it looks exactly like the cards around it. The only hint is the browser's own link preview at the bottom of the window, so it is very hard to tell where focus is [B6-keyboard] · Order logical? Hard to judge without a visible focus; <note order seen via activeElement, if checked> · Traps? <...> · Skip link? **No** – none appears on the first Tab press · Task completed? <yes/no>  
**Shift+Tab appeared not to work at all** <re-check with `document.activeElement`: since Tab turned out to move focus invisibly, Shift+Tab may be working invisibly too – keep this sentence only if focus really does not move back>  
Keyboard users must guess where they are, and there is no skip link. This contradicts the site's own accessibility widget, which claims WCAG 2.1 compliance; one CSS rule for `:focus-visible` would fix it [B6].

### B7 – Automated audit (Lighthouse Accessibility)

| Page | Score | Top 3 issues |
|---|---|---|
| Home (https://lefkosabelediyesi.org/, 8:53:17 PM) | **86** (Performance 81, Best Practices 96, SEO 100) | 1. Form elements do not have associated labels · 2. Links do not have a discernible name · 3. <third issue – check the Contrast section below; "Image elements do not have [alt] attributes that are redundant text" is only informational (grey circle)> <screenshot shows 87 – confirm which run/page; home run above was 86> |
| İletişim (https://lefkosabelediyesi.org/iletisim, 3:28:11 PM) | **83** (Performance 89, Best Practices 73, SEO 100) | <1> <2> <3> – open the Accessibility tab of this report; screenshot shows only Performance insights |

Also seen while inspecting: the large footer logo (`ltb-tum-renkler.png`, "Lefkoşa Nicosia – tüm renklerin kardeşçe yaşadığı şehir") has `alt=""`, so its slogan is hidden from screen-reader users [B7-footer-logo-alt]. Icons render as garbled characters (e.g. `î¤Œ`) while the icon font loads [B7-lighthouse-inner].

Scores in the mid-80s look fine, but Lighthouse's two failures are the same problems found by hand in B5 and B8, and it cannot detect the invisible keyboard focus from B6 at all [B7].

### B8 – Link and button text

| Current text | Where | Better text |
|---|---|---|
| Image only, `alt="Logo"` (19 links) | Home page, partner/project logo strip (orchestra, theatre, Paylaşım Mutfağı, marathon, etc.) | Name each project in the alt text, e.g. "Lefkoşa Belediye Orkestrası" / "Paylaşım Mutfağı" |
| "Previous slide" / "Next slide" (icon-only buttons) | Home page, news carousel arrows | Turkish labels matching the page language: "Önceki haber" / "Sonraki haber" |
| Image only, `alt="ai-logo"` | Footer, designer credit link | "Ai Web Solutions" |
| "Tüm haberler için ›" | Home page, under the news carousel | "Tüm haberleri gör" (a complete action, not a sentence fragment) |
| "Soru ve Cevaplar →" / "Kent Rehberi →" | Home page, FAQ and city-guide cards | Acceptable; could be more specific, e.g. "Sıkça sorulan soruları oku" / "Kent rehberi haritasını aç" |

Visible link text is mostly clear; the problem is image- and icon-only links, which screen readers announce as "Logo" nineteen times [B8].

## Part C: Recommendations

### What works well
1. **Clean, readable Turkish URLs.** Addresses like `/sehrin-tarihi` are lowercase Turkish words with hyphens and no codes, so residents can read and share them [A2].
2. **A friendly, branded 404 page.** Broken links show a plain-Turkish apology with two ways out, and the server returns a real 404 status, so search engines are not misled [A4].

### Top three problems
**Why this order:** ranked by how badly each problem stops a resident from getting something done – problem 1 can block contact entirely, problem 2 slows everyone down, problem 3 mainly affects search and screen readers.

| # | Problem | Evidence | Who it affects | Fix in plain words | Effort | Impact |
|---|---|---|---|---|---|---|
| 1 | The contact form cannot be used properly by keyboard or screen reader: labels are not linked, errors are colour-only, keyboard focus is invisible. | B4, B5, B6, B7 | Blind and partially sighted residents, people who cannot use a mouse, colour-blind users | Link each label to its field, add short error text under wrong fields, show a visible frame around the selected element | Low | High |
| 2 | The home page is slow and data-heavy: blank at first, icons missing, then a 3.2 MB decorative video. | A3, B1, B7 | Every visitor, especially on mobile data or weak signal | Replace the video with a compressed photo, show the existing loading spinner, cache images longer | Medium | Medium |
| 3 | The page structure is incomplete: no `<h1>`, no canonical link while two addresses work, logo links named only "Logo". | A3, A7, B7, B8 | Screen-reader users and residents searching on Google | Make the main question an `<h1>`, redirect to one address and declare it canonical, name each logo | Low | Medium |

## Appendix A: Evidence log

| ID | File | What it shows | Time and date | Page address |
|---|---|---|---|---|
| A1 | evidence/A1-https-address.png | Address bar ending on https:// | <time and date> | https://www.lefkosabelediyesi.org/ |
| A1 | evidence/A1-certificate.png | Certificate for lefkosabelediyesi.org, issued by WE1 (root GTS Root R4), expires 28 Dec 2026, shown as valid | <time and date> | https://www.lefkosabelediyesi.org/ |
| A2 | | | | |
| A3 | evidence/A3-network.png | Main document request: GET, 200 OK, Content-Type text/html; charset=utf-8, Server cloudflare, Cache-Control no-cache, private | Sun 8 Nov 2026, 20:28 EET (server Date header 18:28:30 GMT) | https://lefkosabelediyesi.org/ |
| A4 | evidence/A4-status-404.png | Network panel: made-up address /532523 returns GET 404 (document) | <time and date> | https://lefkosabelediyesi.org/532523 |
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
| B7 | evidence/B7-lighthouse-home-issues.png | Accessibility 87: failed audits "Form elements do not have associated labels" and "Links do not have a discernible name" | <time and date> | https://lefkosabelediyesi.org/ <confirm> |
| B7 | evidence/B7-lighthouse-inner.png | Lighthouse on İletişim page: Performance 89, Accessibility 83, Best Practices 73, SEO 100 (Performance insights shown) | Mon 9 Nov 2026, 15:28 | https://lefkosabelediyesi.org/iletisim |
| B7 | evidence/B7-footer-logo-alt.png | Footer logo `ltb-tum-renkler.png` with empty `alt` in Elements | <time and date> | https://lefkosabelediyesi.org/ |
| B8 | evidence/B8-logo-links.png <still to take>, evidence/B8-carousel-buttons.png | Partner-logo links with alt="Logo"; carousel buttons with English aria-labels | <time and date> | https://lefkosabelediyesi.org/ |
