# Ulrike Bassenge — Website Implementierung

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Vollständiges Redesign der Website ulrike-bassenge.de als 10-seitige statische HTML-Website — mobil-first, SEO-optimiert, mit Altrosa-Design-System.

**Architecture:** Statische HTML-Seiten mit geteiltem `styles.css` (Design-Tokens, Custom Properties), Tailwind CDN für Utilities, Google Fonts CDN. Keine Build-Tools, kein Framework. Navigation und Footer werden in jede Seite als HTML-Snippet kopiert. Vanilla JS nur für Mobile-Nav und Dropdown (< 40 Zeilen).

**Tech Stack:** HTML5, CSS3 Custom Properties, Tailwind CSS (CDN), Google Fonts (Cormorant Garamond + DM Sans), Formspree für Kontaktformular, `node serve.mjs` für lokale Entwicklung.

**Spec:** `docs/superpowers/specs/2026-05-21-ulrike-bassenge-website-redesign.md`
**Mockup-Referenz:** `.superpowers/brainstorm/40333-1779287265/content/homepage-v2.html`

---

## Datei-Übersicht

| Datei | Zweck |
|---|---|
| `serve.mjs` | Lokaler Dev-Server auf Port 3000 |
| `styles.css` | Design-System: Custom Properties, shared Klassen |
| `index.html` | Startseite (10 Sektionen) |
| `hebammenhilfe.html` | Gesetzlicher Anspruch, Kassenleistungen |
| `schwangerschaft.html` | Vorsorge, Naturheilkunde, Kurse |
| `geburtshilfe.html` | Alle 3 Geburtsorte |
| `wochenbett.html` | Postpartum-Betreuung |
| `stillen.html` | Stillberatung |
| `ueber-mich.html` | Bio, Ausbildung, Philosophie |
| `netzwerk.html` | Partner-Cards (ehem. Links-Seite) |
| `kontakt.html` | Formular (Formspree) + Kontaktdaten |
| `impressum.html` | Rechtliche Pflichtangaben |
| `datenschutz.html` | DSGVO-Datenschutzerklärung |

---

## Phase 1: Design-System & Startseite

### Task 1: Dev-Server + Design-System aufsetzen

**Files:**
- Create: `serve.mjs`
- Create: `styles.css`

- [ ] **Schritt 1: `serve.mjs` erstellen**

```javascript
import http from 'http';
import fs from 'fs';
import path from 'path';
import { fileURLToPath } from 'url';

const __dirname = path.dirname(fileURLToPath(import.meta.url));
const PORT = 3000;
const MIME = {
  '.html': 'text/html', '.css': 'text/css',
  '.js': 'application/javascript', '.png': 'image/png',
  '.jpg': 'image/jpeg', '.webp': 'image/webp', '.svg': 'image/svg+xml',
};

http.createServer((req, res) => {
  const urlPath = req.url === '/' ? '/index.html' : req.url;
  const filePath = path.join(__dirname, urlPath);
  const ext = path.extname(filePath);
  fs.readFile(filePath, (err, data) => {
    if (err) { res.writeHead(404); res.end('Not found'); return; }
    res.writeHead(200, { 'Content-Type': MIME[ext] || 'text/plain' });
    res.end(data);
  });
}).listen(PORT, () => console.log(`http://localhost:${PORT}`));
```

- [ ] **Schritt 2: `styles.css` mit Design-System erstellen**

```css
/* === CUSTOM PROPERTIES === */
:root {
  --rose:       #c4867a;
  --rose-light: #e8c5bb;
  --rose-pale:  #f5e8e4;
  --rose-dark:  #6b3f36;
  --cream:      #fdf8f6;
  --cream-mid:  #f7ede9;
  --sand:       #edddd7;
  --muted:      #8a6960;
  --text:       #2a1a16;

  --font-serif: 'Cormorant Garamond', Georgia, serif;
  --font-sans:  'DM Sans', system-ui, sans-serif;

  --section-pad-desktop: 70px 48px;
  --section-pad-mobile:  40px 20px;
  --card-radius: 20px;
  --btn-radius:  40px;
}

/* === RESET === */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body { font-family: var(--font-sans); background: var(--cream); color: var(--text); line-height: 1.6; }
a { text-decoration: none; color: inherit; }
img { max-width: 100%; display: block; }

/* === TYPOGRAPHY === */
.font-serif   { font-family: var(--font-serif); }
.heading-xl   { font-family: var(--font-serif); font-size: clamp(36px, 5vw, 54px); line-height: 1.1; font-weight: 700; color: var(--rose-dark); }
.heading-lg   { font-family: var(--font-serif); font-size: clamp(26px, 3.5vw, 36px); line-height: 1.2; font-weight: 600; color: var(--rose-dark); }
.heading-md   { font-family: var(--font-serif); font-size: clamp(18px, 2.5vw, 24px); font-weight: 600; color: var(--rose-dark); }
.eyebrow      { font-size: 11px; letter-spacing: 0.2em; text-transform: uppercase; color: var(--rose); font-weight: 600; display: flex; align-items: center; gap: 8px; }
.eyebrow::before { content: ''; display: inline-block; width: 20px; height: 1px; background: var(--rose); }
.body-text    { font-size: 14px; color: var(--muted); line-height: 1.8; }
.quote-text   { font-family: var(--font-serif); font-style: italic; font-size: 17px; color: var(--muted); line-height: 1.65; }

/* === BUTTONS === */
.btn-primary {
  display: inline-flex; align-items: center; gap: 6px;
  background: var(--rose); color: #fff;
  padding: 13px 28px; border-radius: var(--btn-radius);
  font-size: 13px; font-weight: 600; font-family: var(--font-sans);
  transition: background .2s, transform .15s; cursor: pointer; border: none;
}
.btn-primary:hover { background: var(--rose-dark); transform: translateY(-1px); }

.btn-ghost {
  display: inline-flex; align-items: center; gap: 4px;
  color: var(--rose-dark); font-size: 13px; font-weight: 500;
  border-bottom: 1px solid var(--rose-light); padding-bottom: 2px;
  transition: color .2s, border-color .2s;
}
.btn-ghost:hover { color: var(--rose); border-color: var(--rose); }

.btn-outline {
  display: inline-flex; align-items: center; gap: 6px;
  border: 2px solid rgba(255,255,255,0.4); color: #fff;
  padding: 13px 28px; border-radius: var(--btn-radius);
  font-size: 13px; font-weight: 600; font-family: var(--font-sans);
  transition: border-color .2s;
}
.btn-outline:hover { border-color: rgba(255,255,255,0.9); }

/* === SECTION LABELS === */
.section-label {
  font-size: 10px; letter-spacing: 0.22em; text-transform: uppercase;
  color: var(--rose); font-weight: 600; margin-bottom: 10px;
  display: flex; align-items: center; gap: 8px;
}
.section-label::before { content: ''; display: inline-block; width: 20px; height: 1px; background: var(--rose); }

/* === CARDS === */
.card {
  background: #fff; border-radius: var(--card-radius); border: 1px solid var(--sand);
  transition: transform .25s, box-shadow .25s, border-color .25s;
}
.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 12px 32px rgba(196,134,122,0.18);
  border-color: var(--rose-light);
}

/* === NAVIGATION === */
.site-nav {
  background: var(--cream); border-bottom: 1px solid var(--sand);
  padding: 18px 48px; display: flex; align-items: center; justify-content: space-between;
  position: sticky; top: 0; z-index: 100;
}
.nav-logo-name { font-family: var(--font-serif); font-size: 20px; color: var(--rose-dark); }
.nav-logo-sub  { font-size: 10px; letter-spacing: 0.18em; text-transform: uppercase; color: var(--rose); font-weight: 500; }
.nav-links { display: flex; gap: 28px; list-style: none; align-items: center; }
.nav-links a  { font-size: 12px; color: var(--muted); letter-spacing: 0.06em; text-transform: uppercase; font-weight: 500; transition: color .2s; }
.nav-links a:hover { color: var(--rose); }
.nav-dropdown { position: relative; }
.nav-dropdown-menu {
  display: none; position: absolute; top: calc(100% + 12px); left: 50%;
  transform: translateX(-50%); background: #fff; border: 1px solid var(--sand);
  border-radius: 12px; padding: 8px 0; min-width: 180px;
  box-shadow: 0 8px 24px rgba(107,63,54,0.12); z-index: 200;
}
.nav-dropdown:hover .nav-dropdown-menu { display: block; }
.nav-dropdown-menu a {
  display: block; padding: 9px 18px; font-size: 13px; color: var(--text);
  text-transform: none; letter-spacing: 0; font-weight: 400;
  transition: background .15s, color .15s;
}
.nav-dropdown-menu a:hover { background: var(--cream-mid); color: var(--rose); }
.hamburger { display: none; flex-direction: column; gap: 5px; cursor: pointer; background: none; border: none; padding: 4px; }
.hamburger span { display: block; width: 24px; height: 2px; background: var(--rose-dark); border-radius: 2px; transition: all .3s; }

/* === FOOTER === */
.site-footer {
  background: var(--cream); border-top: 1px solid var(--sand);
  padding: 22px 48px; display: flex; justify-content: space-between; align-items: center;
  font-size: 11px; color: var(--muted);
}
.site-footer a { color: var(--muted); transition: color .2s; }
.site-footer a:hover { color: var(--rose); }

/* === MOBILE === */
@media (max-width: 768px) {
  .site-nav { padding: 16px 20px; }
  .nav-links { display: none; flex-direction: column; gap: 0; position: absolute; top: 100%; left: 0; right: 0; background: var(--cream); border-bottom: 1px solid var(--sand); padding: 8px 0; }
  .nav-links.open { display: flex; }
  .nav-links a { padding: 12px 20px; text-transform: none; font-size: 14px; }
  .nav-dropdown-menu { display: none !important; }
  .hamburger { display: flex; }
  .site-footer { flex-direction: column; gap: 8px; text-align: center; padding: 20px; }
}
```

- [ ] **Schritt 3: Server starten und prüfen**

```bash
node serve.mjs
```

Erwartetes Ergebnis: `http://localhost:3000` im Terminal. Öffne `http://localhost:3000` im Browser — zeigt 404 (noch kein index.html, das ist OK).

- [ ] **Schritt 4: Commit**

```bash
git add serve.mjs styles.css
git commit -m "feat: project setup — dev server + design system CSS"
```

---

### Task 2: Startseite — Navigation + Hero

**Files:**
- Create: `index.html`

- [ ] **Schritt 1: `index.html` Grundstruktur + Navigation erstellen**

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hebamme Potsdam — Ulrike Bassenge | Schwangerschaft, Geburt & Wochenbett</title>
  <meta name="description" content="Ulrike Bassenge – Ihre erfahrene Hebamme in Potsdam und Berlin. Beleghebamme, Hausgeburten & Geburtshaus. Schwangerschaft, Geburt, Wochenbett & Stillberatung.">
  <link rel="canonical" href="https://www.ulrike-bassenge.de/">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;0,700;1,400;1,600&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="styles.css">
  <style>
    /* Seiten-spezifische Styles kommen hier */
  </style>
</head>
<body>

<!-- NAVIGATION -->
<nav class="site-nav" id="main-nav">
  <a href="/" class="flex flex-col">
    <span class="nav-logo-name">Ulrike Bassenge</span>
    <span class="nav-logo-sub">Hebamme · Potsdam &amp; Berlin</span>
  </a>
  <button class="hamburger" id="hamburger" aria-label="Menü öffnen">
    <span></span><span></span><span></span>
  </button>
  <ul class="nav-links" id="nav-links">
    <li><a href="hebammenhilfe.html">Hebammenhilfe</a></li>
    <li class="nav-dropdown">
      <a href="#">Leistungen ▾</a>
      <div class="nav-dropdown-menu">
        <a href="schwangerschaft.html">Schwangerschaft</a>
        <a href="geburtshilfe.html">Geburtshilfe</a>
        <a href="wochenbett.html">Wochenbett</a>
        <a href="stillen.html">Stillberatung</a>
      </div>
    </li>
    <li><a href="ueber-mich.html">Über mich</a></li>
    <li><a href="netzwerk.html">Netzwerk</a></li>
    <li><a href="kontakt.html">Kontakt</a></li>
  </ul>
  <a href="kontakt.html" class="btn-primary" style="white-space:nowrap;">Jetzt anfragen</a>
</nav>

<!-- HERO -->
<section style="display:grid;grid-template-columns:1fr 1fr;min-height:600px;">
  <!-- LINKS -->
  <div style="background:var(--cream-mid);padding:80px 52px;display:flex;flex-direction:column;justify-content:center;">
    <div style="margin-bottom:18px;">
      <div style="font-family:var(--font-serif);font-size:32px;font-weight:700;color:var(--rose-dark);line-height:1.1;">Ulrike Bassenge</div>
      <p class="eyebrow" style="margin-top:6px;">Hebamme in Potsdam &amp; Berlin</p>
    </div>
    <h1 class="heading-xl" style="margin-bottom:24px;">
      Mit Herz<br>und Erfahrung<br><em style="color:var(--rose);font-style:italic;">begleiten.</em>
    </h1>
    <blockquote class="quote-text" style="border-left:3px solid var(--rose-light);padding-left:18px;margin-bottom:36px;">
      „Wo wahre Liebe hinführt,<br>beginnt die Arbeit der Hebamme."
    </blockquote>
    <div style="display:flex;gap:14px;align-items:center;flex-wrap:wrap;">
      <a href="kontakt.html" class="btn-primary">☎ Jetzt Kontakt aufnehmen</a>
      <a href="#leistungen" class="btn-ghost">Leistungen entdecken</a>
    </div>
  </div>
  <!-- RECHTS: Foto + Badges -->
  <div style="background:linear-gradient(145deg,var(--rose-pale) 0%,var(--cream-mid) 60%,var(--sand) 100%);position:relative;overflow:hidden;display:flex;align-items:center;justify-content:center;">
    <!-- Deko-Blobs -->
    <div style="position:absolute;width:340px;height:340px;background:var(--rose-light);opacity:.35;border-radius:60% 40% 55% 45%/50% 60% 40% 50%;top:-60px;right:-80px;"></div>
    <div style="position:absolute;width:220px;height:220px;background:var(--rose);opacity:.12;border-radius:40% 60% 45% 55%/55% 45% 60% 40%;bottom:-40px;left:-30px;"></div>
    <div style="position:absolute;width:100px;height:100px;background:var(--rose-dark);opacity:.08;border-radius:50%;bottom:80px;right:40px;"></div>
    <!-- Badge: Erfahrung -->
    <div style="position:absolute;top:60px;left:30px;z-index:3;background:#fff;border-radius:12px;padding:10px 14px;box-shadow:0 4px 20px rgba(107,63,54,.12);display:flex;align-items:center;gap:8px;">
      <span style="font-size:18px;">🌿</span>
      <div>
        <span style="display:block;font-size:9px;color:var(--muted);text-transform:uppercase;letter-spacing:.1em;">Erfahrung seit</span>
        <span style="font-family:var(--font-serif);font-size:14px;font-weight:700;color:var(--rose-dark);">2005</span>
      </div>
    </div>
    <!-- Badge: Krankenkasse -->
    <div style="position:absolute;bottom:90px;left:20px;z-index:3;background:#fff;border-radius:12px;padding:10px 14px;box-shadow:0 4px 20px rgba(107,63,54,.12);display:flex;align-items:center;gap:8px;">
      <span style="font-size:18px;">💚</span>
      <div>
        <span style="display:block;font-size:9px;color:var(--muted);text-transform:uppercase;letter-spacing:.1em;">Kosten</span>
        <span style="font-family:var(--font-serif);font-size:14px;font-weight:700;color:var(--rose-dark);">Krankenkasse</span>
      </div>
    </div>
    <!-- Badge: Ort -->
    <div style="position:absolute;top:100px;right:20px;z-index:3;background:#fff;border-radius:12px;padding:10px 14px;box-shadow:0 4px 20px rgba(107,63,54,.12);display:flex;align-items:center;gap:8px;">
      <span style="font-size:18px;">📍</span>
      <div>
        <span style="display:block;font-size:9px;color:var(--muted);text-transform:uppercase;letter-spacing:.1em;">Tätig in</span>
        <span style="font-family:var(--font-serif);font-size:14px;font-weight:700;color:var(--rose-dark);">Potsdam &amp; Berlin</span>
      </div>
    </div>
    <!-- Foto-Rahmen -->
    <div style="position:relative;z-index:2;width:220px;height:290px;background:linear-gradient(160deg,var(--sand) 0%,var(--rose-light) 100%);border-radius:120px 120px 80px 80px;display:flex;align-items:center;justify-content:center;box-shadow:0 20px 60px rgba(107,63,54,.18),0 4px 12px rgba(107,63,54,.1);">
      <img src="https://placehold.co/220x290/e8c5bb/6b3f36?text=Ulrike+Bassenge" alt="Hebamme Ulrike Bassenge, Potsdam" style="border-radius:120px 120px 80px 80px;width:100%;height:100%;object-fit:cover;">
    </div>
  </div>
</section>

<!-- Alle weiteren Sektionen kommen in späteren Tasks -->

<!-- FOOTER -->
<footer class="site-footer">
  <span>© 2026 Ulrike Bassenge · Hebamme Potsdam</span>
  <span>
    <a href="impressum.html">Impressum</a> &nbsp;·&nbsp;
    <a href="datenschutz.html">Datenschutz</a>
  </span>
</footer>

<!-- NAVIGATION JS -->
<script>
  const hamburger = document.getElementById('hamburger');
  const navLinks  = document.getElementById('nav-links');
  hamburger.addEventListener('click', () => navLinks.classList.toggle('open'));
</script>
</body>
</html>
```

- [ ] **Schritt 2: Im Browser prüfen**

Server läuft schon (`node serve.mjs`). Öffne `http://localhost:3000`:
- Navigation sticky und sichtbar ✓
- „Ulrike Bassenge" prominent über der Headline ✓
- Hero 2-spaltig auf Desktop ✓
- 3 Trust-Badges schweben über dem Foto-Bereich ✓
- Auf Mobile (DevTools → Toggle Device): Nav kollabiert zu Hamburger ✓

- [ ] **Schritt 3: Commit**

```bash
git add index.html
git commit -m "feat: startseite — navigation + hero section"
```

---

### Task 3: Startseite — Info-Strip + Hebammenhilfe-Kurzversion

**Files:**
- Modify: `index.html` — nach dem Hero, vor Footer einfügen

- [ ] **Schritt 1: Info-Strip + Anspruchs-Sektion nach dem Hero einfügen**

Füge folgenden Block in `index.html` direkt nach dem schließenden `</section>` des Heros ein (vor dem Footer):

```html
<!-- INFO-STRIP -->
<div style="background:var(--rose-dark);padding:20px 48px;display:flex;align-items:center;justify-content:center;gap:0;flex-wrap:wrap;">
  <div style="color:rgba(255,255,255,.9);font-size:12px;padding:8px 24px;display:flex;align-items:center;gap:8px;border-right:1px solid rgba(255,255,255,.2);">⚖️ Gesetzlicher Anspruch auf Hebammenhilfe</div>
  <div style="color:rgba(255,255,255,.9);font-size:12px;padding:8px 24px;display:flex;align-items:center;gap:8px;border-right:1px solid rgba(255,255,255,.2);">💶 Krankenkasse übernimmt alle Kosten</div>
  <div style="color:rgba(255,255,255,.9);font-size:12px;padding:8px 24px;display:flex;align-items:center;gap:8px;border-right:1px solid rgba(255,255,255,.2);">🏥 Beleghebamme Krankenhaus Waldfriede</div>
  <div style="color:rgba(255,255,255,.9);font-size:12px;padding:8px 24px;display:flex;align-items:center;gap:8px;">🏡 Hausgeburten &amp; Geburtshaus möglich</div>
</div>

<!-- HEBAMMENHILFE KURZVERSION -->
<section style="background:var(--rose-pale);padding:44px 48px;display:grid;grid-template-columns:1fr 2fr;gap:48px;align-items:center;border-top:1px solid var(--sand);border-bottom:1px solid var(--sand);">
  <div>
    <h2 class="heading-lg">Ihr gesetzliches Recht auf meine Hilfe</h2>
    <p style="font-size:12px;color:var(--muted);margin-top:8px;">Alle Leistungen werden von der Krankenkasse übernommen.</p>
  </div>
  <div>
    <p class="body-text" style="margin-bottom:12px;">Jede Frau hat <strong>mit Beginn der Schwangerschaft bis 8 Wochen nach der Geburt</strong> gesetzlichen Anspruch auf die Begleitung durch eine Hebamme in Potsdam — vollständig abgedeckt durch die gesetzliche Krankenversicherung.</p>
    <p class="body-text" style="margin-bottom:16px;">Ich biete Ihnen neben medizinischen Vorsorgeuntersuchungen vor allem Zeit: für Ihre Fragen, Ihre Ängste und Ihre Stärken. Melden Sie sich <em>möglichst schon in der ersten Schwangerschaftshälfte</em> bei mir, damit ich gut für Sie planen kann.</p>
    <div style="display:flex;align-items:center;gap:16px;flex-wrap:wrap;">
      <span style="display:inline-flex;align-items:center;gap:6px;background:var(--rose);color:#fff;font-size:11px;font-weight:600;padding:6px 14px;border-radius:30px;">✓ Privatversicherte bitte Kasse anfragen</span>
      <a href="hebammenhilfe.html" class="btn-ghost">Mehr erfahren →</a>
    </div>
  </div>
</section>
```

- [ ] **Schritt 2: Prüfen**

`http://localhost:3000` — nach dem Hero erscheinen:
- Dunkler Strip mit 4 Icons ✓
- Rose-pale Sektion mit 2-spaltigem Layout ✓
- „Mehr erfahren →"-Link zu hebammenhilfe.html ✓

- [ ] **Schritt 3: Commit**

```bash
git add index.html
git commit -m "feat: startseite — info-strip + hebammenhilfe kurzversion"
```

---

### Task 4: Startseite — Leistungskacheln

**Files:**
- Modify: `index.html`

- [ ] **Schritt 1: Leistungs-Sektion nach der Anspruchs-Sektion einfügen**

```html
<!-- LEISTUNGEN -->
<section id="leistungen" style="padding:70px 48px;">
  <p class="section-label">Meine Leistungen</p>
  <h2 class="heading-lg" style="margin-bottom:8px;">Was ich für Sie tue</h2>
  <p class="body-text" style="max-width:520px;margin-bottom:40px;">Von der ersten Vorsorge bis zum letzten Stillbesuch — ich begleite Sie in Potsdam und Berlin mit naturheilkundlichem Ansatz und echter Fürsorge.</p>

  <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:20px;">

    <a href="schwangerschaft.html" class="card" style="padding:32px 22px;display:flex;flex-direction:column;">
      <div style="font-size:30px;margin-bottom:16px;">🤰</div>
      <h3 class="heading-md" style="margin-bottom:10px;">Schwangerschaft</h3>
      <p class="body-text" style="flex:1;">Vorsorgeuntersuchungen, Ernährungsberatung, Akupunktur, Homöopathie und Geburtsvorbereitungskurse in Potsdam.</p>
      <span style="display:inline-flex;align-items:center;gap:4px;margin-top:18px;font-size:12px;color:var(--rose);font-weight:600;border-bottom:1px solid var(--rose-light);padding-bottom:1px;width:fit-content;">Mehr erfahren →</span>
    </a>

    <a href="geburtshilfe.html" class="card" style="padding:32px 22px;display:flex;flex-direction:column;">
      <div style="font-size:30px;margin-bottom:16px;">🌿</div>
      <h3 class="heading-md" style="margin-bottom:10px;">Geburtshilfe</h3>
      <p class="body-text" style="flex:1;">Hausgeburt, Geburtshaus am Neuen Garten (Potsdam) oder Beleghebamme im Krankenhaus Waldfriede, Berlin-Zehlendorf.</p>
      <span style="display:inline-flex;align-items:center;gap:4px;margin-top:18px;font-size:12px;color:var(--rose);font-weight:600;border-bottom:1px solid var(--rose-light);padding-bottom:1px;width:fit-content;">Mehr erfahren →</span>
    </a>

    <a href="wochenbett.html" class="card" style="padding:32px 22px;display:flex;flex-direction:column;">
      <div style="font-size:30px;margin-bottom:16px;">👶</div>
      <h3 class="heading-md" style="margin-bottom:10px;">Wochenbett</h3>
      <p class="body-text" style="flex:1;">Tägliche Hausbesuche in den ersten Tagen, Neugeborenenbetreuung, Rückbildung und liebevolle Begleitung der ganzen Familie.</p>
      <span style="display:inline-flex;align-items:center;gap:4px;margin-top:18px;font-size:12px;color:var(--rose);font-weight:600;border-bottom:1px solid var(--rose-light);padding-bottom:1px;width:fit-content;">Mehr erfahren →</span>
    </a>

    <a href="stillen.html" class="card" style="padding:32px 22px;display:flex;flex-direction:column;">
      <div style="font-size:30px;margin-bottom:16px;">🤱</div>
      <h3 class="heading-md" style="margin-bottom:10px;">Stillberatung</h3>
      <p class="body-text" style="flex:1;">Individuelle Stillbegleitung in Potsdam — von der richtigen Position bis zur Problemlösung. Auch nach den 8 Wochen möglich.</p>
      <span style="display:inline-flex;align-items:center;gap:4px;margin-top:18px;font-size:12px;color:var(--rose);font-weight:600;border-bottom:1px solid var(--rose-light);padding-bottom:1px;width:fit-content;">Mehr erfahren →</span>
    </a>

  </div>
</section>
```

- [ ] **Schritt 2: Mobile Grid-Anpassung in `styles.css` hinzufügen**

```css
/* Am Ende von styles.css anfügen */
@media (max-width: 1024px) {
  [style*="grid-template-columns:repeat(4,1fr)"] { grid-template-columns: repeat(2, 1fr) !important; }
}
@media (max-width: 640px) {
  [style*="grid-template-columns:repeat(4,1fr)"] { grid-template-columns: 1fr !important; }
  [style*="grid-template-columns:1fr 2fr"]       { grid-template-columns: 1fr !important; }
  [style*="grid-template-columns:1fr 1fr"]       { grid-template-columns: 1fr !important; }
  [style*="min-height:600px"]                    { grid-template-columns: 1fr !important; }
  section[style*="padding:70px 48px"]            { padding: 40px 20px !important; }
  section[style*="padding:44px 48px"]            { padding: 32px 20px !important; }
}
```

- [ ] **Schritt 3: Prüfen**

`http://localhost:3000` — 4 Cards nebeneinander auf Desktop, 2×2 auf Tablet, 1 Spalte auf Mobile ✓

- [ ] **Schritt 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: startseite — leistungskacheln (4 cards, responsive grid)"
```

---

### Task 5: Startseite — Über mich + Netzwerk

**Files:**
- Modify: `index.html`

- [ ] **Schritt 1: Über-mich-Sektion einfügen**

Nach der Leistungen-Sektion:

```html
<!-- ÜBER MICH -->
<section style="display:grid;grid-template-columns:1fr 1.3fr;">
  <!-- FOTO-BEREICH -->
  <div style="background:linear-gradient(160deg,var(--sand) 0%,var(--rose-light) 100%);min-height:480px;display:flex;align-items:flex-end;justify-content:flex-start;padding:32px;position:relative;overflow:hidden;">
    <div style="position:absolute;top:40px;right:-60px;width:200px;height:200px;background:var(--rose);opacity:.12;border-radius:50%;"></div>
    <div style="background:rgba(255,255,255,.85);backdrop-filter:blur(8px);border-radius:12px;padding:14px 18px;max-width:200px;line-height:1.5;">
      <p class="quote-text" style="font-size:14px;color:var(--rose-dark);">„Hebammen sind Unikate — und Schwangere auch."</p>
    </div>
  </div>
  <!-- TEXT -->
  <div style="background:var(--cream-mid);padding:70px 52px;display:flex;flex-direction:column;justify-content:center;">
    <p class="section-label">Über mich</p>
    <h2 class="heading-lg" style="margin-bottom:20px;">Ulrike Bassenge —<br>Ihre Hebamme<br>in Potsdam.</h2>
    <p class="body-text" style="margin-bottom:14px;">Ich bin seit 2005 als freiberufliche Hebamme tätig und begleite Familien in Potsdam und Berlin mit Fachkenntnis und ganzem Herzen. Jede Geburt ist einzigartig — genau wie jede Frau.</p>
    <p class="body-text" style="margin-bottom:28px;">Als Beleghebamme im <strong>Krankenhaus Waldfriede Berlin-Zehlendorf</strong> sowie für Hausgeburten und das <strong>Geburtshaus am Neuen Garten</strong> biete ich Ihnen eine persönliche, kontinuierliche Betreuung.</p>
    <div style="display:flex;gap:28px;margin-bottom:28px;">
      <div style="text-align:center;">
        <strong style="font-family:var(--font-serif);font-size:32px;color:var(--rose);display:block;">20+</strong>
        <span style="font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.08em;">Jahre Erfahrung</span>
      </div>
      <div style="text-align:center;">
        <strong style="font-family:var(--font-serif);font-size:32px;color:var(--rose);display:block;">3</strong>
        <span style="font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.08em;">Geburtsorte</span>
      </div>
      <div style="text-align:center;">
        <strong style="font-family:var(--font-serif);font-size:32px;color:var(--rose);display:block;">♥</strong>
        <span style="font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.08em;">Persönlich & individuell</span>
      </div>
    </div>
    <a href="ueber-mich.html" class="btn-primary" style="width:fit-content;">Lass uns kennenlernen</a>
  </div>
</section>

<!-- NETZWERK -->
<section style="padding:70px 48px;background:var(--cream);">
  <p class="section-label">Mein Netzwerk</p>
  <h2 class="heading-lg" style="margin-bottom:8px;">Vertrauensvolle Partner</h2>
  <p class="body-text" style="max-width:520px;margin-bottom:40px;">Ich arbeite eng mit erfahrenen Kolleginnen und Einrichtungen zusammen — für Ihre lückenlose Betreuung, auch in meiner Abwesenheit.</p>

  <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:20px;">

    <div class="card" style="padding:24px 22px;display:flex;flex-direction:column;gap:8px;">
      <span style="font-size:9px;letter-spacing:.14em;text-transform:uppercase;color:var(--rose);font-weight:600;">Geburtsklinik</span>
      <h3 class="heading-md">Krankenhaus Waldfriede</h3>
      <p class="body-text" style="flex:1;">Hier bin ich als Beleghebamme tätig. Eine familienfreundliche Klinik in Berlin-Zehlendorf mit persönlichem Ambiente.</p>
      <a href="https://www.krankenhaus-waldfriede.de" target="_blank" rel="noopener" style="font-size:11px;color:var(--rose);font-weight:600;display:inline-flex;align-items:center;gap:4px;margin-top:4px;">Zur Klinik →</a>
    </div>

    <div class="card" style="padding:24px 22px;display:flex;flex-direction:column;gap:8px;">
      <span style="font-size:9px;letter-spacing:.14em;text-transform:uppercase;color:var(--rose);font-weight:600;">Geburtshaus Potsdam</span>
      <h3 class="heading-md">Geburtshaus am Neuen Garten</h3>
      <p class="body-text" style="flex:1;">Eine wunderschöne Alternative zur Klinik — natürlich gebären in Potsdam, in vertrauter Atmosphäre.</p>
      <a href="https://www.geburtshaus-am-neuen-garten.de" target="_blank" rel="noopener" style="font-size:11px;color:var(--rose);font-weight:600;display:inline-flex;align-items:center;gap:4px;margin-top:4px;">Mehr erfahren →</a>
    </div>

    <div class="card" style="padding:24px 22px;display:flex;flex-direction:column;gap:8px;">
      <span style="font-size:9px;letter-spacing:.14em;text-transform:uppercase;color:var(--rose);font-weight:600;">Meine Vertretung</span>
      <h3 class="heading-md">Peggy Jahnel &amp; Martina Schulze</h3>
      <p class="body-text" style="flex:1;">Zwei erfahrene Kolleginnen, die Sie in meiner Abwesenheit genauso fürsorglich begleiten.</p>
      <div style="display:flex;gap:12px;margin-top:4px;">
        <a href="https://www.pots-hebamme.de" target="_blank" rel="noopener" style="font-size:11px;color:var(--rose);font-weight:600;">Peggy Jahnel →</a>
        <a href="https://www.hebamme-martina-schulze.de" target="_blank" rel="noopener" style="font-size:11px;color:var(--rose);font-weight:600;">Martina Schulze →</a>
      </div>
    </div>

    <div class="card" style="padding:24px 22px;display:flex;flex-direction:column;gap:8px;">
      <span style="font-size:9px;letter-spacing:.14em;text-transform:uppercase;color:var(--rose);font-weight:600;">Berufsverband</span>
      <h3 class="heading-md">Hebammenverband Brandenburg</h3>
      <p class="body-text" style="flex:1;">Informationen zu Hebammenleistungen in Brandenburg und eine Liste weiterer Hebammen in der Region.</p>
      <a href="https://www.hebammen-brandenburg.de" target="_blank" rel="noopener" style="font-size:11px;color:var(--rose);font-weight:600;display:inline-flex;align-items:center;gap:4px;margin-top:4px;">Zum Verband →</a>
    </div>

    <div class="card" style="padding:24px 22px;display:flex;flex-direction:column;gap:8px;">
      <span style="font-size:9px;letter-spacing:.14em;text-transform:uppercase;color:var(--rose);font-weight:600;">Fotografie</span>
      <h3 class="heading-md">fotoart13</h3>
      <p class="body-text" style="flex:1;">Professionelle Schwangerschafts- und Babyfotografie mit Leidenschaft in Potsdam.</p>
      <a href="https://www.fotoart13.de" target="_blank" rel="noopener" style="font-size:11px;color:var(--rose);font-weight:600;display:inline-flex;align-items:center;gap:4px;margin-top:4px;">Zur Website →</a>
    </div>

    <div class="card" style="padding:24px 22px;display:flex;flex-direction:column;gap:8px;background:var(--rose-pale);border-color:var(--rose-light);">
      <span style="font-size:9px;letter-spacing:.14em;text-transform:uppercase;color:var(--rose-dark);font-weight:600;">Wissen</span>
      <h3 class="heading-md">Ihr gesetzlicher Anspruch</h3>
      <p class="body-text" style="flex:1;">Informieren Sie sich über Ihren Anspruch auf Hebammenhilfe und die vollständige Kostenübernahme durch die Krankenkasse.</p>
      <a href="hebammenhilfe.html" style="font-size:11px;color:var(--rose);font-weight:600;display:inline-flex;align-items:center;gap:4px;margin-top:4px;">Mehr erfahren →</a>
    </div>

  </div>
</section>
```

- [ ] **Schritt 2: Mobile-Grid für 3-spaltig in `styles.css` ergänzen**

```css
@media (max-width: 900px) {
  [style*="grid-template-columns:repeat(3,1fr)"] { grid-template-columns: repeat(2, 1fr) !important; }
}
@media (max-width: 640px) {
  [style*="grid-template-columns:repeat(3,1fr)"] { grid-template-columns: 1fr !important; }
  [style*="grid-template-columns:1fr 1.3fr"]     { grid-template-columns: 1fr !important; }
}
```

- [ ] **Schritt 3: Prüfen** — Über-mich 2-spaltig ✓, Netzwerk 3×2 Grid ✓, alle Links klickbar ✓

- [ ] **Schritt 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: startseite — über-mich + netzwerk sektionen"
```

---

### Task 6: Startseite — Kontakt-CTA + SEO

**Files:**
- Modify: `index.html`

- [ ] **Schritt 1: Kontakt-CTA-Sektion vor dem Footer einfügen**

```html
<!-- KONTAKT CTA -->
<section style="background:var(--rose-dark);padding:80px 48px;">
  <div style="max-width:720px;margin:0 auto;text-align:center;">
    <h2 style="font-family:var(--font-serif);font-size:clamp(30px,4vw,42px);color:#fff;line-height:1.2;margin-bottom:14px;">
      Sie sind nicht<br><em style="color:var(--rose-light);font-style:italic;">allein damit.</em>
    </h2>
    <p style="color:rgba(255,255,255,.75);font-size:15px;line-height:1.7;max-width:480px;margin:0 auto 36px;">
      Ich höre Ihnen zu — ganz gleich ob Sie Fragen zur Schwangerschaft haben, eine Begleitung für die Geburt in Potsdam suchen, oder einfach unsicher sind. Melden Sie sich, ich antworte persönlich.
    </p>
    <div style="display:flex;justify-content:center;gap:14px;margin-bottom:36px;flex-wrap:wrap;">
      <a href="tel:015231810104" class="btn-primary" style="background:var(--rose);">📞 0152 3181 0104 · Anruf oder SMS</a>
      <a href="mailto:info@ulrike-bassenge.de" class="btn-outline">✉ E-Mail schreiben</a>
    </div>
    <div style="background:rgba(255,255,255,.08);border:1px solid rgba(255,255,255,.15);border-radius:16px;padding:24px 32px;color:rgba(255,255,255,.85);font-size:13px;line-height:1.7;max-width:480px;margin:0 auto;display:flex;align-items:center;gap:14px;text-align:left;">
      <span style="font-size:28px;flex-shrink:0;">📋</span>
      <div>
        <strong style="color:#fff;display:block;margin-bottom:4px;">Lieber ein Formular?</strong>
        Kein Problem — füllen Sie das Kontaktformular aus und ich melde mich schnellstmöglich zurück.
        <br><a href="kontakt.html" style="color:var(--rose-light);font-size:12px;margin-top:6px;display:inline-block;">Zum Kontaktformular →</a>
      </div>
    </div>
    <p style="margin-top:28px;color:rgba(255,255,255,.4);font-size:12px;">📍 Haeckelstraße 1, 14471 Potsdam</p>
  </div>
</section>
```

- [ ] **Schritt 2: JSON-LD strukturierte Daten im `<head>` von index.html ergänzen**

Füge direkt vor `</head>` ein:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "MedicalBusiness",
  "name": "Ulrike Bassenge – Hebamme Potsdam",
  "description": "Freiberufliche Hebamme und Beleghebamme in Potsdam und Berlin. Schwangerschaft, Geburtshilfe, Wochenbett, Stillberatung. Hausgeburten und Geburtshaus.",
  "url": "https://www.ulrike-bassenge.de",
  "telephone": "+4915231810104",
  "email": "info@ulrike-bassenge.de",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Haeckelstraße 1",
    "addressLocality": "Potsdam",
    "postalCode": "14471",
    "addressCountry": "DE"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 52.3945,
    "longitude": 13.0820
  },
  "medicalSpecialty": "Midwifery",
  "areaServed": ["Potsdam", "Berlin", "Brandenburg"]
}
</script>
```

- [ ] **Schritt 3: Vollständige Startseite prüfen**

`http://localhost:3000` — scrolle von oben nach unten und prüfe alle 9 Sektionen:
1. Nav ✓  2. Hero ✓  3. Info-Strip ✓  4. Hebammenhilfe-Kurzversion ✓
5. Leistungen ✓  6. Über mich ✓  7. Netzwerk ✓  8. Kontakt-CTA ✓  9. Footer ✓

Auf Mobile prüfen (DevTools 375px): alle Sektionen 1-spaltig ✓, Buttons full-width ✓

- [ ] **Schritt 4: Commit**

```bash
git add index.html
git commit -m "feat: startseite — kontakt-cta + JSON-LD strukturierte daten"
```

---

## Phase 2: Unterseiten

### Task 7: Shared Template für Unterseiten

**Files:**
- Create: `_template.html` (Referenz-Template — nicht ausgeliefert)

- [ ] **Schritt 1: `_template.html` als Kopiervorlage erstellen**

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SEITENTITEL | Ulrike Bassenge – Hebamme Potsdam</title>
  <meta name="description" content="BESCHREIBUNG (150–160 Zeichen)">
  <link rel="canonical" href="https://www.ulrike-bassenge.de/DATEINAME.html">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;0,700;1,400;1,600&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="styles.css">
</head>
<body>

<!-- NAVIGATION (identisch auf allen Seiten) -->
<nav class="site-nav" id="main-nav">
  <a href="/" class="flex flex-col">
    <span class="nav-logo-name">Ulrike Bassenge</span>
    <span class="nav-logo-sub">Hebamme · Potsdam &amp; Berlin</span>
  </a>
  <button class="hamburger" id="hamburger" aria-label="Menü öffnen">
    <span></span><span></span><span></span>
  </button>
  <ul class="nav-links" id="nav-links">
    <li><a href="hebammenhilfe.html">Hebammenhilfe</a></li>
    <li class="nav-dropdown">
      <a href="#">Leistungen ▾</a>
      <div class="nav-dropdown-menu">
        <a href="schwangerschaft.html">Schwangerschaft</a>
        <a href="geburtshilfe.html">Geburtshilfe</a>
        <a href="wochenbett.html">Wochenbett</a>
        <a href="stillen.html">Stillberatung</a>
      </div>
    </li>
    <li><a href="ueber-mich.html">Über mich</a></li>
    <li><a href="netzwerk.html">Netzwerk</a></li>
    <li><a href="kontakt.html">Kontakt</a></li>
  </ul>
  <a href="kontakt.html" class="btn-primary" style="white-space:nowrap;">Jetzt anfragen</a>
</nav>

<!-- PAGE HERO -->
<section style="background:var(--cream-mid);padding:60px 48px 50px;border-bottom:1px solid var(--sand);">
  <p class="section-label" style="margin-bottom:12px;">EYEBROW</p>
  <h1 class="heading-xl" style="max-width:640px;margin-bottom:16px;">SEITENTITEL</h1>
  <p class="body-text" style="max-width:600px;">EINLEITUNGSTEXT</p>
</section>

<!-- HAUPTINHALT -->
<main style="padding:60px 48px;max-width:900px;margin:0 auto;">
  <!-- Inhalt hier -->
</main>

<!-- KONTAKT CTA (mini — auf allen Unterseiten) -->
<section style="background:var(--rose-pale);padding:44px 48px;text-align:center;border-top:1px solid var(--sand);">
  <h2 class="heading-lg" style="margin-bottom:12px;">Fragen? Ich bin für Sie da.</h2>
  <p class="body-text" style="margin-bottom:24px;max-width:480px;margin-left:auto;margin-right:auto;">Melden Sie sich gerne — per Telefon, SMS oder E-Mail.</p>
  <div style="display:flex;justify-content:center;gap:12px;flex-wrap:wrap;">
    <a href="tel:015231810104" class="btn-primary">📞 0152 3181 0104</a>
    <a href="kontakt.html" class="btn-ghost">Zum Kontaktformular →</a>
  </div>
</section>

<!-- FOOTER -->
<footer class="site-footer">
  <span>© 2026 Ulrike Bassenge · Hebamme Potsdam</span>
  <span>
    <a href="impressum.html">Impressum</a> &nbsp;·&nbsp;
    <a href="datenschutz.html">Datenschutz</a>
  </span>
</footer>

<script>
  const hamburger = document.getElementById('hamburger');
  const navLinks  = document.getElementById('nav-links');
  hamburger.addEventListener('click', () => navLinks.classList.toggle('open'));
</script>
</body>
</html>
```

- [ ] **Schritt 2: Commit**

```bash
git add _template.html
git commit -m "feat: unterseiten-template mit nav, page-hero, mini-cta, footer"
```

---

### Task 8: `hebammenhilfe.html`

**Files:**
- Create: `hebammenhilfe.html`

- [ ] **Schritt 1: `hebammenhilfe.html` aus Template erstellen und füllen**

Kopiere `_template.html` → `hebammenhilfe.html`. Passe an:

```
<title>Hebammenhilfe Potsdam — gesetzlicher Anspruch | Ulrike Bassenge</title>
<meta name="description" content="Jede Frau hat Anspruch auf eine Hebamme in Potsdam. Ulrike Bassenge informiert über gesetzliche Leistungen, Kassenübernahme und wann Sie sich melden sollten.">
```

Page-Hero:
```html
<p class="section-label">Ihr Recht</p>
<h1 class="heading-xl">Hebammenhilfe — was Ihnen zusteht</h1>
<p class="body-text">Als werdende Mutter in Potsdam haben Sie gesetzlichen Anspruch auf umfassende Hebammenbegleitung — vollständig von der Krankenkasse finanziert.</p>
```

Hauptinhalt (`<main>`):
```html
<div style="display:grid;grid-template-columns:2fr 1fr;gap:48px;">
  <div>
    <h2 class="heading-md" style="margin-bottom:16px;">Der gesetzliche Anspruch</h2>
    <p class="body-text" style="margin-bottom:16px;">Jede Frau hat <strong>mit Beginn ihrer Schwangerschaft bis einschließlich 8 Wochen nach der Entbindung</strong> gesetzlichen Anspruch auf die Hilfe einer Hebamme. Dieser Anspruch umfasst Beratung, Betreuung und Vorsorge in der Schwangerschaft, bei der Geburt, im Neugeborenenalter und Wochenbett sowie in der Stillzeit.</p>
    <p class="body-text" style="margin-bottom:24px;">Neben medizinischen Untersuchungen nehme ich mir Zeit für Ihre persönlichen Fragen zu Ernährung, Lebensweise, körperliche Veränderungen, Geburtsplanung und Ihre Stärken als werdende Mutter.</p>

    <h2 class="heading-md" style="margin-bottom:16px;">Wann sollten Sie sich melden?</h2>
    <p class="body-text" style="margin-bottom:24px;">Für Hebammenhilfe außerhalb der Klinik setzen Sie sich <strong>möglichst schon in der ersten Hälfte der Schwangerschaft</strong> mit mir in Verbindung. Gute Hebammen sind gefragt — je früher, desto besser kann ich für Sie planen.</p>

    <h2 class="heading-md" style="margin-bottom:16px;">Kostenübernahme</h2>
    <p class="body-text" style="margin-bottom:12px;">Gesetzliche Krankenkassen übernehmen die Kosten für alle Hebammenleistungen gemäß der Hebammen-Gebührenverordnung vollständig.</p>
    <p class="body-text">Privatversicherte wenden sich bitte an ihre Krankenversicherung, um den genauen Leistungsumfang zu erfragen.</p>
  </div>
  <div>
    <div style="background:var(--rose-pale);border-radius:16px;padding:28px;border:1px solid var(--rose-light);">
      <h3 class="heading-md" style="margin-bottom:16px;">Leistungen im Überblick</h3>
      <ul style="list-style:none;display:flex;flex-direction:column;gap:10px;">
        <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;">✓</span> Schwangerschaftsvorsorge</li>
        <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;">✓</span> Geburtsbegleitung</li>
        <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;">✓</span> Wochenbettbesuche (täglich, 10 Tage)</li>
        <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;">✓</span> Besuche bis 12 Wochen nach Geburt</li>
        <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;">✓</span> Stillberatung (2 Zusatzbesuche)</li>
        <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;">✓</span> Familienplanungsberatung</li>
      </ul>
    </div>
  </div>
</div>
```

- [ ] **Schritt 2: Prüfen** — `http://localhost:3000/hebammenhilfe.html` — Inhalt vollständig ✓, Nav aktiv ✓, CTA unten ✓

- [ ] **Schritt 3: Commit**

```bash
git add hebammenhilfe.html
git commit -m "feat: hebammenhilfe.html — gesetzlicher anspruch + leistungsübersicht"
```

---

### Task 9: `schwangerschaft.html`, `wochenbett.html`, `stillen.html`

**Files:**
- Create: `schwangerschaft.html`, `wochenbett.html`, `stillen.html`

- [ ] **Schritt 1: `schwangerschaft.html`**

Kopiere Template. Title/Description:
```
<title>Schwangerschaft Potsdam — Hebamme Ulrike Bassenge | Vorsorge & Beratung</title>
<meta name="description" content="Schwangerschaftsbegleitung in Potsdam mit Hebamme Ulrike Bassenge. Vorsorge, Akupunktur, Homöopathie, Bachblüten und Geburtsvorbereitungskurse. Krankenkasse zahlt.">
```

Page-Hero:
```html
<p class="section-label">Leistung</p>
<h1 class="heading-xl">Begleitung in der Schwangerschaft</h1>
<p class="body-text">Eine Schwangerschaft ist ein besonderer Abschnitt im Leben — mit körperlichen, psychischen und sozialen Veränderungen. Als Ihre Hebamme in Potsdam bin ich Ihre wichtigste Ansprechpartnerin.</p>
```

Hauptinhalt:
```html
<h2 class="heading-md" style="margin-bottom:16px;">Schwangerenvorsorge</h2>
<p class="body-text" style="margin-bottom:24px;">Ich biete alle Routineuntersuchungen an: Gewichtskontrolle, Blutdruckmessung, Herztonüberwachung und vaginale Untersuchungen. Ergeben sich Auffälligkeiten, überweise ich Sie an Ihre Frauenärztin.</p>

<h2 class="heading-md" style="margin-bottom:16px;">Naturheilkundliche Unterstützung</h2>
<p class="body-text" style="margin-bottom:12px;">Bei Schwangerschaftsbeschwerden wie Übelkeit, Rückenschmerzen oder Ängsten bevorzuge ich naturheilkundliche Ansätze vor Medikamenten:</p>
<div style="display:grid;grid-template-columns:repeat(3,1fr);gap:16px;margin-bottom:24px;">
  <div class="card" style="padding:20px;text-align:center;"><div style="font-size:24px;margin-bottom:8px;">🌸</div><p class="body-text"><strong>Homöopathie</strong></p></div>
  <div class="card" style="padding:20px;text-align:center;"><div style="font-size:24px;margin-bottom:8px;">🌿</div><p class="body-text"><strong>Akupunktur</strong></p></div>
  <div class="card" style="padding:20px;text-align:center;"><div style="font-size:24px;margin-bottom:8px;">🌼</div><p class="body-text"><strong>Bachblüten</strong></p></div>
</div>

<h2 class="heading-md" style="margin-bottom:16px;">Geburtsvorbereitung</h2>
<p class="body-text" style="margin-bottom:24px;">Geburtsvorbereitungskurse mit Körperarbeit, Atemübungen und Gruppengesprächen. Väter und Partner sind herzlich eingeladen. Die Krankenkasse übernimmt die Kurskosten für Schwangere.</p>

<h2 class="heading-md" style="margin-bottom:16px;">Beratungsthemen</h2>
<ul style="list-style:none;display:grid;grid-template-columns:1fr 1fr;gap:8px;">
  <li class="body-text" style="display:flex;gap:8px;"><span style="color:var(--rose);">→</span> Ernährung in der Schwangerschaft</li>
  <li class="body-text" style="display:flex;gap:8px;"><span style="color:var(--rose);">→</span> Lebensweise &amp; körperliche Veränderungen</li>
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);">→</span> Wahl des Geburtsortes</li>
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);">→</span> Finanzielle Hilfen &amp; Elterngeld</li>
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);">→</span> Vorbereitung auf das Wochenbett</li>
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);">→</span> Stillen &amp; Säuglingspflege</li>
</ul>
```

- [ ] **Schritt 2: `wochenbett.html`**

Title: `Wochenbett Potsdam — Hebamme Ulrike Bassenge | Hausbesuche & Betreuung`
Description: `Wochenbettbetreuung in Potsdam mit Hebamme Ulrike Bassenge. Tägliche Hausbesuche, Neugeborenenbetreuung, Rückbildung und Stillunterstützung. Krankenkasse zahlt.`

H1: `Begleitung im Wochenbett`
Einleitung: `Mit dem Baby zuhause beginnt ein neuer Lebensabschnitt: spannend, aufregend und ganz anders als bisher. Ich begleite Sie in den ersten Wochen mit täglichen Hausbesuchen in Potsdam.`

Hauptinhalt (Leistungsliste + Erklärungstext aus der alten Website):
```html
<p class="body-text" style="margin-bottom:16px;">Die Wochenbettzeit umfasst die ersten acht Wochen nach der Geburt. Nach der Hebammen-Gebührenverordnung besteht in den ersten zehn Tagen Anspruch auf mindestens einen täglichen Besuch.</p>
<ul style="list-style:none;display:flex;flex-direction:column;gap:12px;margin-bottom:24px;">
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;min-width:20px;">✓</span> Überwachung des Neugeborenen: Nabelabheilung, Gelbsucht, Gewichtsentwicklung</li>
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;min-width:20px;">✓</span> Blutentnahme (U2) am 3. Lebenstag</li>
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;min-width:20px;">✓</span> Psychische Begleitung der Mutter — Baby Blues &amp; Erschöpfung</li>
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;min-width:20px;">✓</span> Kontrolle der körperlichen Rückbildung</li>
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;min-width:20px;">✓</span> Stillunterstützung und Stillberatung</li>
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;min-width:20px;">✓</span> Anleitung in Babypflege und Säuglingsbehandlung</li>
  <li class="body-text" style="display:flex;gap:10px;"><span style="color:var(--rose);font-weight:700;min-width:20px;">✓</span> Informationen zu Ernährung, Impfungen und Vorsorgeuntersuchungen</li>
</ul>
```

- [ ] **Schritt 3: `stillen.html`**

Title: `Stillberatung Potsdam — Hebamme Ulrike Bassenge`
Description: `Individuelle Stillberatung in Potsdam. Hebamme Ulrike Bassenge unterstützt Sie von Anfang an — bei Stillproblemen, richtiger Position und Stilldauer. Krankenkasse zahlt.`

H1: `Stillberatung`
Einleitung: `Mit dem Stillen geben Sie Ihrem Kind den besten Start ins Leben. Ich begleite Sie dabei individuell und ohne Druck — in Potsdam und Umgebung.`

Hauptinhalt:
```html
<div style="display:grid;grid-template-columns:1fr 1fr;gap:40px;">
  <div>
    <h2 class="heading-md" style="margin-bottom:16px;">Meine Unterstützung</h2>
    <p class="body-text" style="margin-bottom:16px;">Ich berate Sie während Schwangerschaft, Geburt und Wochenbett zur richtigen Stilltechnik und -position. Auch nach den regulären 8 Wochen Wochenbettbetreuung kann ich noch zweimal bei Stillproblemen helfen.</p>
    <p class="body-text">Die WHO empfiehlt, sechs Monate voll zu stillen. Muttermilch enthält neben Flüssigkeit und Nährstoffen auch Antikörper — optimal für Neugeborene. Ich respektiere aber auch bewusste Entscheidungen gegen das Stillen.</p>
  </div>
  <div>
    <h2 class="heading-md" style="margin-bottom:16px;">Vorteile</h2>
    <h3 class="body-text" style="font-weight:600;margin-bottom:8px;">Für Ihr Kind:</h3>
    <ul style="list-style:none;display:flex;flex-direction:column;gap:6px;margin-bottom:16px;">
      <li class="body-text" style="display:flex;gap:8px;"><span style="color:var(--rose);">✓</span> Ideale Nährstoffzusammensetzung</li>
      <li class="body-text" style="display:flex;gap:8px;"><span style="color:var(--rose);">✓</span> Schutz vor Allergien und Infektionen</li>
      <li class="body-text" style="display:flex;gap:8px;"><span style="color:var(--rose);">✓</span> Bessere Kieferentwicklung</li>
    </ul>
    <h3 class="body-text" style="font-weight:600;margin-bottom:8px;">Für Sie:</h3>
    <ul style="list-style:none;display:flex;flex-direction:column;gap:6px;">
      <li class="body-text" style="display:flex;gap:8px;"><span style="color:var(--rose);">✓</span> Schnellere Rückbildung</li>
      <li class="body-text" style="display:flex;gap:8px;"><span style="color:var(--rose);">✓</span> Geringeres Brustkrebsrisiko</li>
      <li class="body-text" style="display:flex;gap:8px;"><span style="color:var(--rose);">✓</span> Emotionale Verbundenheit</li>
    </ul>
  </div>
</div>
```

- [ ] **Schritt 4: Alle drei Seiten prüfen**

```
http://localhost:3000/schwangerschaft.html
http://localhost:3000/wochenbett.html
http://localhost:3000/stillen.html
```
Alle haben: Nav ✓, Page-Hero mit H1 ✓, Hauptinhalt ✓, Kontakt-CTA ✓, Footer ✓

- [ ] **Schritt 5: Commit**

```bash
git add schwangerschaft.html wochenbett.html stillen.html
git commit -m "feat: unterseiten schwangerschaft, wochenbett, stillen"
```

---

### Task 10: `geburtshilfe.html`

**Files:**
- Create: `geburtshilfe.html`

- [ ] **Schritt 1: `geburtshilfe.html` erstellen**

Title: `Geburtshilfe Potsdam — Hausgeburt, Geburtshaus & Beleghebamme | Ulrike Bassenge`
Description: `Geburtshilfe in Potsdam mit Hebamme Ulrike Bassenge. Hausgeburt, Geburtshaus am Neuen Garten oder Beleghebamme im Krankenhaus Waldfriede Berlin-Zehlendorf.`

H1: `Geburtshilfe — Ihr Ort, Ihre Wahl`
Einleitung: `Jede Geburt ist einzigartig — und Sie brauchen ein Umfeld, in dem Sie sich sicher und verstanden fühlen. Ich begleite Sie in Potsdam und Berlin, wo auch immer Sie gebären möchten.`

Hauptinhalt:
```html
<p class="body-text" style="margin-bottom:32px;font-family:var(--font-serif);font-style:italic;font-size:17px;border-left:3px solid var(--rose-light);padding-left:18px;">„Hebammen sind Unikate und Schwangere auch." — Ulrike Bassenge</p>

<div style="display:grid;grid-template-columns:repeat(3,1fr);gap:20px;margin-bottom:40px;">

  <div class="card" style="padding:28px;">
    <div style="font-size:32px;margin-bottom:12px;">🏡</div>
    <h2 class="heading-md" style="margin-bottom:12px;">Hausgeburt</h2>
    <p class="body-text">Die vertrauteste Umgebung für eine Geburt: Ihr Zuhause in Potsdam und Umgebung. Ich begleite Sie von den ersten Wehen bis nach der Geburt — persönlich, ruhig und kontinuierlich.</p>
  </div>

  <div class="card" style="padding:28px;">
    <div style="font-size:32px;margin-bottom:12px;">🌿</div>
    <h2 class="heading-md" style="margin-bottom:12px;">Geburtshaus am Neuen Garten</h2>
    <p class="body-text">Eine wunderschöne Alternative in Potsdam — natürlich und familiär, mit dem nötigen medizinischen Umfeld. Ich begleite Sie hier als Ihre persönliche Hebamme.</p>
    <a href="https://www.geburtshaus-am-neuen-garten.de" target="_blank" rel="noopener" style="display:inline-block;margin-top:12px;font-size:12px;color:var(--rose);font-weight:600;">Zum Geburtshaus →</a>
  </div>

  <div class="card" style="padding:28px;">
    <div style="font-size:32px;margin-bottom:12px;">🏥</div>
    <h2 class="heading-md" style="margin-bottom:12px;">Krankenhaus Waldfriede</h2>
    <p class="body-text">Als Beleghebamme im Krankenhaus Waldfriede in Berlin-Zehlendorf können Sie die Sicherheit einer Klinik mit der persönlichen Betreuung durch mich verbinden.</p>
    <a href="https://www.krankenhaus-waldfriede.de" target="_blank" rel="noopener" style="display:inline-block;margin-top:12px;font-size:12px;color:var(--rose);font-weight:600;">Zur Klinik →</a>
  </div>

</div>

<div style="background:var(--rose-pale);border-radius:16px;padding:28px;border:1px solid var(--rose-light);">
  <h2 class="heading-md" style="margin-bottom:12px;">Was ist eine Beleghebamme?</h2>
  <p class="body-text">Als Beleghebamme bin ich Ihre persönliche Hebamme, die Sie auch bei einer Klinikgeburt von Anfang bis Ende begleitet — nicht irgendeine Klinik-Hebamme, die gerade Dienst hat. Sie wählen mich, ich bin für Sie da.</p>
</div>
```

- [ ] **Schritt 2: Prüfen** — `http://localhost:3000/geburtshilfe.html` — alle 3 Geburtsort-Karten ✓

- [ ] **Schritt 3: Commit**

```bash
git add geburtshilfe.html
git commit -m "feat: geburtshilfe.html — hausgeburt, geburtshaus, beleghebamme waldfriede"
```

---

### Task 11: `ueber-mich.html`

**Files:**
- Create: `ueber-mich.html`

- [ ] **Schritt 1: `ueber-mich.html` erstellen**

Title: `Über mich — Hebamme Ulrike Bassenge, Potsdam`
Description: `Ulrike Bassenge, Hebamme in Potsdam seit 2005. Ausgebildet in Berlin, Beleghebamme im Krankenhaus Waldfriede. Erfahren in Hausgeburten, Geburtshaus und Klinikgeburten.`

H1: `Über mich`
Einleitung: `Ich bin Ulrike Bassenge, freiberufliche Hebamme in Potsdam seit 2005 — und ich begleite Sie mit Herz, Fachkenntnis und echtem Interesse an Ihrer Geschichte.`

Hauptinhalt:
```html
<div style="display:grid;grid-template-columns:1fr 1.5fr;gap:48px;align-items:start;">
  <div>
    <img src="https://placehold.co/400x500/e8c5bb/6b3f36?text=Ulrike+Bassenge" alt="Hebamme Ulrike Bassenge, Potsdam" style="border-radius:120px 120px 60px 60px;width:100%;">
    <div style="background:var(--rose-pale);border-radius:16px;padding:24px;margin-top:24px;border:1px solid var(--rose-light);">
      <h3 class="heading-md" style="margin-bottom:16px;">Kontakt</h3>
      <p class="body-text" style="margin-bottom:8px;"><strong>Telefon/SMS:</strong> <a href="tel:015231810104" style="color:var(--rose);">0152 3181 0104</a></p>
      <p class="body-text"><strong>E-Mail:</strong> <a href="mailto:info@ulrike-bassenge.de" style="color:var(--rose);">info@ulrike-bassenge.de</a></p>
      <p class="body-text" style="margin-top:8px;"><strong>Adresse:</strong> Haeckelstraße 1, 14471 Potsdam</p>
    </div>
  </div>
  <div>
    <h2 class="heading-md" style="margin-bottom:16px;">Mein Weg zur Hebamme</h2>
    <p class="body-text" style="margin-bottom:16px;">Ich wurde 1978 in Potsdam geboren. Nach einem Sportwissenschaftsstudium an der Universität Potsdam absolvierte ich meine Hebammenausbildung in Berlin-Neukölln, die ich im September 2005 erfolgreich abschloss.</p>
    <p class="body-text" style="margin-bottom:16px;">Nach Jahren als angestellte Hebamme und Beleghebamme — zuletzt am Klinikum Ernst von Bergmann Potsdam — begleite ich seit Januar 2016 Schwangere als Beleghebamme im <strong>Krankenhaus Waldfriede in Berlin-Zehlendorf</strong>.</p>
    <p class="body-text" style="margin-bottom:32px;">In Potsdam und Umgebung bin ich zudem für <strong>Hausgeburten</strong> und Geburten im <strong>Geburtshaus am Neuen Garten</strong> tätig.</p>

    <h2 class="heading-md" style="margin-bottom:16px;">Meine Philosophie</h2>
    <p class="body-text" style="margin-bottom:16px;">Ich glaube daran, dass jede Frau die Kraft hat, ihr Kind zu gebären und zu versorgen. Meine Aufgabe ist es, diese Kraft zu stärken — nicht zu ersetzen. Ich begleite Sie individuell, naturheilkundlich und mit echter Fürsorge.</p>
    <blockquote class="quote-text" style="border-left:3px solid var(--rose-light);padding-left:18px;margin-bottom:32px;">„Wo wahre Liebe hinführt, beginnt die Arbeit der Hebamme."</blockquote>

    <a href="kontakt.html" class="btn-primary">Lass uns kennenlernen</a>
  </div>
</div>
```

- [ ] **Schritt 2: Prüfen** — `http://localhost:3000/ueber-mich.html` ✓

- [ ] **Schritt 3: Commit**

```bash
git add ueber-mich.html
git commit -m "feat: ueber-mich.html — bio, ausbildung, philosophie, kontaktdaten"
```

---

### Task 12: `netzwerk.html` + `kontakt.html`

**Files:**
- Create: `netzwerk.html`, `kontakt.html`

- [ ] **Schritt 1: `netzwerk.html`**

Title: `Netzwerk & Partner — Ulrike Bassenge, Hebamme Potsdam`
Description: `Partner und Kolleginnen von Hebamme Ulrike Bassenge in Potsdam und Berlin: Krankenhaus Waldfriede, Geburtshaus am Neuen Garten, Vertretungs-Hebammen und mehr.`

H1: `Mein Netzwerk`
Einleitung: `Ich arbeite eng mit erfahrenen Kolleginnen und Einrichtungen zusammen — für Ihre lückenlose Betreuung in Potsdam und Berlin, auch wenn ich mal nicht verfügbar bin.`

Hauptinhalt: Identisches Grid wie auf der Startseite (6 Karten aus Task 5), aber mit je mehr Beschreibungstext pro Karte. Kopiere die Karten-HTML aus index.html und ergänze jeden `<p class="body-text">` um 1–2 Sätze.

- [ ] **Schritt 2: `kontakt.html`**

Title: `Kontakt — Hebamme Ulrike Bassenge, Potsdam`
Description: `Kontaktieren Sie Hebamme Ulrike Bassenge in Potsdam. Telefon, SMS oder E-Mail — ich antworte persönlich. Kontaktformular online.`

H1: `Kontakt aufnehmen`
Einleitung: `Ich freue mich auf Ihre Nachricht — melden Sie sich gerne früh in der Schwangerschaft.`

Hauptinhalt:
```html
<div style="display:grid;grid-template-columns:1fr 1.2fr;gap:48px;">
  <!-- KONTAKTDATEN -->
  <div>
    <h2 class="heading-md" style="margin-bottom:24px;">So erreichen Sie mich</h2>
    <div style="display:flex;flex-direction:column;gap:20px;">
      <div style="display:flex;gap:16px;align-items:flex-start;">
        <span style="font-size:24px;">📞</span>
        <div>
          <p style="font-weight:600;color:var(--rose-dark);margin-bottom:4px;">Telefon &amp; SMS</p>
          <a href="tel:015231810104" style="font-size:20px;font-family:var(--font-serif);color:var(--rose);">0152 3181 0104</a>
          <p class="body-text" style="margin-top:4px;">Anruf oder SMS — ich melde mich schnellstmöglich.</p>
        </div>
      </div>
      <div style="display:flex;gap:16px;align-items:flex-start;">
        <span style="font-size:24px;">✉</span>
        <div>
          <p style="font-weight:600;color:var(--rose-dark);margin-bottom:4px;">E-Mail</p>
          <a href="mailto:info@ulrike-bassenge.de" style="font-size:16px;color:var(--rose);">info@ulrike-bassenge.de</a>
        </div>
      </div>
      <div style="display:flex;gap:16px;align-items:flex-start;">
        <span style="font-size:24px;">📍</span>
        <div>
          <p style="font-weight:600;color:var(--rose-dark);margin-bottom:4px;">Adresse</p>
          <p class="body-text">Haeckelstraße 1<br>14471 Potsdam</p>
        </div>
      </div>
    </div>
  </div>
  <!-- FORMULAR -->
  <div>
    <h2 class="heading-md" style="margin-bottom:24px;">Kontaktformular</h2>
    <form action="https://formspree.io/f/FORMSPREE_ID" method="POST" style="display:flex;flex-direction:column;gap:16px;">
      <div>
        <label style="display:block;font-size:12px;font-weight:600;color:var(--rose-dark);margin-bottom:6px;text-transform:uppercase;letter-spacing:.06em;">Name *</label>
        <input type="text" name="name" required placeholder="Ihr vollständiger Name" style="width:100%;padding:12px 16px;border:1px solid var(--sand);border-radius:10px;font-family:var(--font-sans);font-size:14px;background:var(--cream);color:var(--text);outline:none;">
      </div>
      <div>
        <label style="display:block;font-size:12px;font-weight:600;color:var(--rose-dark);margin-bottom:6px;text-transform:uppercase;letter-spacing:.06em;">E-Mail *</label>
        <input type="email" name="email" required placeholder="ihre@email.de" style="width:100%;padding:12px 16px;border:1px solid var(--sand);border-radius:10px;font-family:var(--font-sans);font-size:14px;background:var(--cream);color:var(--text);outline:none;">
      </div>
      <div>
        <label style="display:block;font-size:12px;font-weight:600;color:var(--rose-dark);margin-bottom:6px;text-transform:uppercase;letter-spacing:.06em;">Telefon (optional)</label>
        <input type="tel" name="phone" placeholder="0152 …" style="width:100%;padding:12px 16px;border:1px solid var(--sand);border-radius:10px;font-family:var(--font-sans);font-size:14px;background:var(--cream);color:var(--text);outline:none;">
      </div>
      <div>
        <label style="display:block;font-size:12px;font-weight:600;color:var(--rose-dark);margin-bottom:6px;text-transform:uppercase;letter-spacing:.06em;">Nachricht *</label>
        <textarea name="message" required rows="5" placeholder="Wie kann ich Ihnen helfen?" style="width:100%;padding:12px 16px;border:1px solid var(--sand);border-radius:10px;font-family:var(--font-sans);font-size:14px;background:var(--cream);color:var(--text);outline:none;resize:vertical;"></textarea>
      </div>
      <div style="display:flex;align-items:flex-start;gap:10px;">
        <input type="checkbox" name="dsgvo" required id="dsgvo" style="margin-top:3px;accent-color:var(--rose);">
        <label for="dsgvo" class="body-text">Ich habe die <a href="datenschutz.html" style="color:var(--rose);">Datenschutzerklärung</a> gelesen und stimme der Verarbeitung meiner Daten zu. *</label>
      </div>
      <button type="submit" class="btn-primary" style="width:100%;justify-content:center;">Nachricht senden →</button>
    </form>
    <p class="body-text" style="margin-top:12px;font-size:11px;">* Pflichtfelder. Formularversand via Formspree — bitte <a href="https://formspree.io" target="_blank" style="color:var(--rose);">Formspree-Account erstellen</a> und FORMSPREE_ID ersetzen.</p>
  </div>
</div>
```

- [ ] **Schritt 3: Prüfen**

```
http://localhost:3000/netzwerk.html
http://localhost:3000/kontakt.html
```
Formular-Felder sichtbar ✓, DSGVO-Checkbox ✓, Telefon-Link klickbar ✓

- [ ] **Schritt 4: Commit**

```bash
git add netzwerk.html kontakt.html
git commit -m "feat: netzwerk.html + kontakt.html mit formspree formular"
```

---

### Task 13: `impressum.html` + `datenschutz.html`

**Files:**
- Create: `impressum.html`, `datenschutz.html`

- [ ] **Schritt 1: `impressum.html`**

Title: `Impressum — Ulrike Bassenge, Hebamme Potsdam`

Hauptinhalt:
```html
<h2 class="heading-md" style="margin-bottom:16px;">Angaben gemäß § 5 TMG</h2>
<p class="body-text" style="margin-bottom:24px;">
  Ulrike Bassenge<br>
  Hebamme (freiberuflich)<br>
  Haeckelstraße 1<br>
  14471 Potsdam
</p>
<h2 class="heading-md" style="margin-bottom:12px;">Kontakt</h2>
<p class="body-text" style="margin-bottom:24px;">
  Telefon: <a href="tel:015231810104" style="color:var(--rose);">0152 3181 0104</a><br>
  E-Mail: <a href="mailto:info@ulrike-bassenge.de" style="color:var(--rose);">info@ulrike-bassenge.de</a>
</p>
<h2 class="heading-md" style="margin-bottom:12px;">Haftungsausschluss</h2>
<p class="body-text" style="margin-bottom:24px;">Die Betreiberin übernimmt keine Garantie für Aktualität, Vollständigkeit und Richtigkeit der bereitgestellten Informationen. Haftungsansprüche entstehen erst nach konkreter Kenntnis von Rechtsverstößen.</p>
<h2 class="heading-md" style="margin-bottom:12px;">Urheberrecht</h2>
<p class="body-text">Alle Inhalte dieser Website sind urheberrechtlich geschützt. Fotografie: <a href="https://www.fotoart13.de" target="_blank" style="color:var(--rose);">fotoart13 · Heinrich-Mann-Allee 17, 14473 Potsdam</a>.</p>
```

- [ ] **Schritt 2: `datenschutz.html`**

Title: `Datenschutzerklärung — Ulrike Bassenge, Hebamme Potsdam`

Hauptinhalt (DSGVO-konform, Kurzversion — bitte vor Launch durch einen DSGVO-Generator ergänzen lassen):
```html
<h2 class="heading-md" style="margin-bottom:12px;">Verantwortliche</h2>
<p class="body-text" style="margin-bottom:20px;">Ulrike Bassenge, Haeckelstraße 1, 14471 Potsdam, info@ulrike-bassenge.de</p>

<h2 class="heading-md" style="margin-bottom:12px;">Erhobene Daten</h2>
<p class="body-text" style="margin-bottom:20px;">Diese Website erhebt beim Besuch keine personenbezogenen Daten. Das Kontaktformular überträgt Ihre Eingaben verschlüsselt an den Dienst Formspree (formspree.io). Kontaktformulardaten werden zur Bearbeitung Ihrer Anfrage gespeichert. Widerruf jederzeit per E-Mail möglich.</p>

<h2 class="heading-md" style="margin-bottom:12px;">Externe Dienste</h2>
<p class="body-text" style="margin-bottom:20px;">Google Fonts werden direkt von Google-Servern geladen. Dabei kann Ihre IP-Adresse an Google übertragen werden. Weitere Infos: <a href="https://policies.google.com/privacy" target="_blank" style="color:var(--rose);">Google Datenschutzrichtlinien</a>.</p>

<h2 class="heading-md" style="margin-bottom:12px;">Ihre Rechte</h2>
<p class="body-text">Sie haben das Recht auf Auskunft, Berichtigung, Löschung, Einschränkung der Verarbeitung und Datenübertragbarkeit. Wenden Sie sich dazu an: info@ulrike-bassenge.de</p>
```

- [ ] **Schritt 3: Prüfen** — beide Seiten laden ✓, Footer-Links funktionieren ✓

- [ ] **Schritt 4: Commit**

```bash
git add impressum.html datenschutz.html
git commit -m "feat: impressum + datenschutz"
```

---

### Task 14: Mobile-Optimierung Feinschliff + Finale Prüfung

**Files:**
- Modify: `styles.css`

- [ ] **Schritt 1: Mobile-Feinschliff in `styles.css`**

```css
/* Kontakt-CTA Mobile */
@media (max-width: 640px) {
  [style*="padding:80px 48px"] { padding: 48px 20px !important; }
  [style*="padding:60px 48px"] { padding: 40px 20px !important; }
  [style*="padding:70px 52px"] { padding: 40px 20px !important; }
  [style*="min-height:600px"]  { min-height: auto !important; }
  /* Hero auf Mobile: Foto-Bereich über Text */
  [style*="grid-template-columns:1fr 1fr"] > div:first-child { order: 2; }
  [style*="grid-template-columns:1fr 1fr"] > div:last-child  { order: 1; min-height: 300px !important; }
  /* Über-mich auf Mobile */
  [style*="grid-template-columns:1fr 1.3fr"] > div { min-height: 280px !important; }
  /* Kontakt-Buttons full-width */
  .btn-primary, .btn-outline { width: 100%; justify-content: center; }
  /* Info-Strip wrap */
  [style*="display:flex;align-items:center;justify-content:center;gap:0"] { gap: 0 !important; }
  [style*="border-right:1px solid rgba(255,255,255,.2)"] { border-right: none !important; border-bottom: 1px solid rgba(255,255,255,.1) !important; width: 100%; justify-content: center !important; }
}
```

- [ ] **Schritt 2: Alle Seiten auf Mobile prüfen (375px)**

In Chrome DevTools → Device Toolbar → iPhone SE (375px). Prüfe:
- `index.html`: Hero 1-spaltig ✓, Cards 1-spaltig ✓, Nav Hamburger ✓
- `geburtshilfe.html`: 3 Karten 1-spaltig ✓
- `kontakt.html`: Formular volle Breite ✓, Buttons volle Breite ✓
- `ueber-mich.html`: Grid 1-spaltig ✓

- [ ] **Schritt 3: SEO-Prüfung — alle Seiten**

Prüfe in DevTools → Elements für jede Seite:
- `<title>` enthält „Potsdam" oder „Berlin" ✓
- `<meta name="description">` vorhanden und 150–160 Zeichen ✓
- `<h1>` genau einmal pro Seite ✓
- `<link rel="canonical">` gesetzt ✓
- Bilder haben `alt`-Attribut mit Keywords ✓

- [ ] **Schritt 4: Commit**

```bash
git add styles.css
git commit -m "feat: mobile-optimierung feinschliff — alle seiten responsive"
```

---

## Self-Review Ergebnis

**Spec-Coverage:**
- ✓ 10 Seiten mit korrekten URLs
- ✓ Navigation mit Dropdown (Leistungen ▾)
- ✓ Hybrid-Struktur: Startseite + Unterseiten
- ✓ Alle 3 Geburtsorte (Hausgeburt, Geburtshaus, Waldfriede)
- ✓ Design-System: Cormorant Garamond + DM Sans, Altrosa-Palette
- ✓ Netzwerk-Seite (ersetzt Links-Seite) mit 6 Partner-Karten
- ✓ Kontaktformular (Formspree) mit DSGVO-Checkbox
- ✓ SEO: Title, Description, H1, JSON-LD, Canonical
- ✓ Mobile-first, responsive
- ✓ Dev-Server (`node serve.mjs`)
- ⚠️ Formspree-ID: Platzhalter `FORMSPREE_ID` muss nach Konto-Erstellung ersetzt werden
- ⚠️ Datenschutz: Vor Launch durch vollständigen DSGVO-Text eines Datenschutz-Generators ersetzen

**Placeholder-Scan:** Kein TBD/TODO in Implementierungsschritten. Einzige offene Punkte sind extern (Formspree-Registrierung, Datenschutztext, echte Fotos) — alle explizit markiert.

**Type-Consistency:** CSS Custom Properties konsistent benannt (`--rose`, `--rose-dark`, etc.) durch alle Tasks. Button-Klassen (`btn-primary`, `btn-ghost`, `btn-outline`) konsistent. Navigation-HTML identisch in allen Seiten.
