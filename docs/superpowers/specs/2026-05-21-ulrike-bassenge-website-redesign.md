# Website Redesign — Ulrike Bassenge, Hebamme Potsdam
**Datum:** 2026-05-21  
**Status:** Design genehmigt, bereit zur Implementierung

---

## Kontext & Ziel

Ulrike Bassenge ist freiberufliche Hebamme und Beleghebamme in Potsdam und Berlin. Ihre bestehende Website (ulrike-bassenge.de) ist technisch veraltet, nicht mobil-optimiert und nicht SEO-optimiert. Das Redesign soll:

- Die **bestehende Seitenstruktur** weitgehend beibehalten
- Mobil-first responsiv werden
- SEO für lokale Keywords optimieren (Hebamme Potsdam, Beleghebamme Potsdam, etc.)
- Persönliches Branding stärken — Besucher sollen sich sofort sicher und aufgehoben fühlen
- Als einzelne `index.html` (+ Unterseiten) mit Tailwind CDN und Inline-Styles ausgeliefert werden

---

## Kundendaten

| Feld | Wert |
|---|---|
| Name | Ulrike Bassenge |
| Beruf | Freiberufliche Hebamme, Beleghebamme & Hausgeburtshebamme |
| Adresse | Haeckelstraße 1, 14471 Potsdam |
| E-Mail | info@ulrike-bassenge.de |
| Telefon/SMS | 0152 3181 0104 |
| Geburtsorte | 1. Krankenhaus Waldfriede, Berlin-Zehlendorf (Beleghebamme) |
| | 2. Geburtshaus am Neuen Garten, Potsdam |
| | 3. Hausgeburten in Potsdam & Umgebung |
| Tätig seit | 2005 |
| Fotos | Platzhalter — echte Fotos folgen später |

---

## Seitenstruktur (Hybrid)

Startseite mit Leistungsübersicht (Cards) + eigene Unterseiten für jede Leistung.

### Seiten & URLs

| Seite | URL | Inhalt |
|---|---|---|
| Startseite | `/` | Hero, Anspruch-Kurzversion, Leistungskacheln, Über-mich-Preview, Netzwerk, Kontakt-CTA |
| Hebammenhilfe | `/hebammenhilfe.html` | Vollständige Infos zum gesetzlichen Anspruch, Kassenübernahme, Empfehlung zur frühen Kontaktaufnahme |
| Schwangerschaft | `/schwangerschaft.html` | Vorsorge, Akupunktur, Homöopathie, Bachblüten, Ernährungsberatung, Geburtsvorbereitungskurse |
| Geburtshilfe | `/geburtshilfe.html` | Alle 3 Geburtsorte: Hausgeburt (Potsdam & Umgebung), Geburtshaus am Neuen Garten, Beleghebamme Krankenhaus Waldfriede |
| Wochenbett | `/wochenbett.html` | Tägliche Besuche, Neugeborenenbetreuung, Rückbildung, Stillunterstützung |
| Stillen | `/stillen.html` | Stillberatung, Vorteile, Unterstützung auch nach den 8 Wochen |
| Über mich | `/ueber-mich.html` | Vollständige Bio, Ausbildung, Erfahrung, Philosophie |
| Netzwerk | `/netzwerk.html` | Partner-Cards (Waldfriede, Geburtshaus, Vertretung, Verband, Fotograf) |
| Kontakt | `/kontakt.html` | Kontaktformular, Telefon, E-Mail, Adresse |
| Impressum | `/impressum.html` | Rechtliche Pflichtangaben |
| Datenschutz | `/datenschutz.html` | DSGVO-Datenschutzerklärung |

### Navigation

```
Logo (Ulrike Bassenge) | Hebammenhilfe | Leistungen ▾ | Über mich | Netzwerk | Kontakt  [Jetzt anfragen →]
                                           Schwangerschaft
                                           Geburtshilfe
                                           Wochenbett
                                           Stillen
```

---

## Design-System

### Typografie

| Rolle | Font | Größe | Gewicht |
|---|---|---|---|
| Display/Headings | Cormorant Garamond (serif) | 32–54px | 600–700 |
| Subheadings | Cormorant Garamond italic | 18–28px | 400 italic |
| Body | DM Sans | 13–15px | 400 |
| Labels/Eyebrows | DM Sans | 10–11px | 600, uppercase, letter-spacing 0.18em |
| Zitate | Cormorant Garamond italic | 15–17px | 400 |

Google Fonts: `Cormorant+Garamond:ital,wght@0,400;0,600;0,700;1,400;1,600` + `DM+Sans:wght@300;400;500;600`

### Farbpalette — Altrosa & Creme

```css
--rose:       #c4867a;   /* Hauptfarbe, CTAs, Akzente */
--rose-light: #e8c5bb;   /* Borders, dezente Hintergründe */
--rose-pale:  #f5e8e4;   /* Sehr helle Sektionen */
--rose-dark:  #6b3f36;   /* Überschriften, dunkle Texte */
--cream:      #fdf8f6;   /* Haupthintergrund */
--cream-mid:  #f7ede9;   /* Hero-Hintergrund, Karten-BG */
--sand:       #edddd7;   /* Borders, Trennlinien */
--muted:      #8a6960;   /* Fließtext, Beschreibungen */
```

### Abstands-Tokens

- Section padding: `70px 48px` (Desktop), `40px 20px` (Mobile)
- Card gap: `20px`
- Border-radius: Cards `20px`, Badges `12px`, Buttons `40px` (pill)

### Animationen

- Nur `transform` und `opacity` animieren
- Hover-Cards: `translateY(-5px)` + `box-shadow` intensivieren
- Transition: `all .25s ease`
- Buttons: `translateY(-1px)` on hover

---

## Homepage-Sektionen (Reihenfolge)

### 1. Navigation (sticky)
- Logo links: „Ulrike Bassenge" (Cormorant Garamond) + „Hebamme · Potsdam & Berlin" (klein, uppercase)
- Nav-Links Mitte (Hebammenhilfe, Leistungen Dropdown, Über mich, Netzwerk, Kontakt)
- CTA-Button rechts: „Jetzt anfragen" (Rose, pill-shape)

### 2. Hero (2-spaltig)

**Links:**
- `Ulrike Bassenge` — groß, Cormorant Garamond, 32px
- Eyebrow: `── HEBAMME IN POTSDAM & BERLIN`
- H1: „Mit Herz / und Erfahrung / *begleiten.*"
- Zitat (linke Border): „Wo wahre Liebe hinführt, beginnt die Arbeit der Hebamme."
- Buttons: [☎ Jetzt Kontakt aufnehmen] [Leistungen entdecken →]

**Rechts:**
- Gradient-Hintergrund (rose-pale → cream-mid → sand)
- 3 organische Blob-Formen als Deko
- Organisch geformter Foto-Rahmen (pill-shape) mit Schatten
- 3 schwebende Trust-Badges: „Erfahrung seit 2005", „Kostenübernamhe durch deine Krankenkasse", „Tätig in Potsdam & Berlin"

### 3. Info-Strip (dunkel rose-dark)
4 Icons mit Text: Gesetzlicher Anspruch · Krankenkasse übernimmt Kosten · Beleghebamme Waldfriede · Hausgeburt oder Geburtshaus möglich

### 4. Hebammenhilfe — Kurzversion
2-spaltig: Links H2 + kurze Erklärung, rechts vollständiger Text + Tag „✓ Krankenkasse". Link zur vollständigen Seite.

### 5. Leistungen (4 Cards)
Cards: Schwangerschaft, Geburtshilfe, Wochenbett, Stillen — je mit Icon, Titel, Kurztext, „Mehr erfahren →"-Link zur Unterseite.

### 6. Über mich (2-spaltig)
Links: Foto-Fläche mit Gradient + schwebendes Zitat-Glasmorphism-Element
Rechts: Label, H2, 2 Textabsätze, 3 Fact-Zahlen (20+ Jahre, Geburtsorte), CTA „Lass uns kennenlernen →"

### 7. Netzwerk (6 Cards, 3×2 Grid)
Elegante Partner-Karten statt nackter Linkliste:
- Krankenhaus Waldfriede (Geburtsklinik)
- Geburtshaus am Neuen Garten (Geburtshaus Potsdam)
- Peggy Jahnel & Martina Schulze (Meine Vertretung — 1 Card mit 2 Links)
- Hebammenverband Brandenburg (Berufsverband)
- fotoart13 (Fotografie)
- Infos-Card (intern, verlinkt auf Hebammenhilfe-Sektion)

### 8. Kontakt-CTA
Dunkle Sektion (rose-dark). Headline: „Sie sind nicht *allein damit.*"
- 2 große Buttons: [📞 0152 3181 0104 anrufen oder SMS] [✉ E-Mail schreiben]
- Hinweis-Box mit Link zum Kontaktformular
- Adresse darunter klein

### 9. Footer
Links: © 2026 Ulrike Bassenge · Hebamme Potsdam  
Rechts: Impressum · Datenschutz

---

## Kontaktformular (kontakt.html)

Felder:
- Name (required)
- E-Mail (required)
- Telefon (optional)
- Nachricht (textarea, required)
- Checkbox: DSGVO-Zustimmung (required)
- Absenden-Button

Formular sendet via **Formspree** (formspree.io, kostenlos, kein Backend nötig). Action-URL im `<form action="https://formspree.io/f/[ID]">`. Fallback: `mailto:info@ulrike-bassenge.de`.

---

## SEO-Strategie

### Primäre Keywords (in `<title>`, `<h1>`, `<meta description>` einbauen)
- Hebamme Potsdam
- Beleghebamme Potsdam
- Hebamme Berlin Zehlendorf
- Geburtshilfe Potsdam

### Sekundäre Keywords (natürlich in Fließtexte einweben)
- Wochenbettbetreuung Potsdam
- Stillberatung Potsdam
- Hausgeburt Potsdam
- Hausgeburt Hebamme Potsdam
- Schwangerschaftsbegleitung Potsdam
- Hebamme Waldfriede Berlin
- Geburtshaus Neuer Garten Potsdam
- Beleghebamme Berlin

### Technisches SEO
- `<meta name="description">` für jede Seite (150–160 Zeichen)
- `<title>` Pattern: „[Seitenthema] | Ulrike Bassenge – Hebamme Potsdam"
- Strukturierte Daten: `LocalBusiness` + `MedicalBusiness` JSON-LD auf Startseite
- `<h1>` genau einmal pro Seite, Keywords enthalten
- `alt`-Texte für alle Bilder mit Keyword-Kontext
- Canonical URLs
- Mobile-first (Google Mobile-First-Indexing)
- Alle internen Links mit sprechenden Ankertexten

---

## Netzwerk-Seite (/netzwerk.html)

Volle Partner-Seite mit denselben 6 Cards wie auf der Startseite, aber mit mehr Text pro Partner. Ersetzt die alte „Links"-Seite.

---

## Mobile-Verhalten

- Nav: Hamburger-Menü ab 768px
- Hero: 1-spaltig, Foto über dem Text oder darunter
- Cards: 1-spaltig auf Mobile, 2-spaltig ab 640px, 4-spaltig ab 1024px
- Netzwerk-Grid: 1-spaltig auf Mobile, 2-spaltig ab 640px
- Buttons: Full-width auf Mobile
- Schriftgrößen: H1 36px (Mobile) → 54px (Desktop)

---

## Datei-Ausgabe & Implementierungsphasen

### Phase 1 — Startseite + Design-System
- `index.html` — vollständige Startseite mit allen Sektionen
- `styles.css` — geteiltes CSS mit Custom Properties (Farben, Fonts, Spacing-Tokens) für alle Seiten
- Design-System fertigstellen bevor Unterseiten gebaut werden

### Phase 2 — Unterseiten
Je eine HTML-Datei: `hebammenhilfe.html`, `schwangerschaft.html`, `geburtshilfe.html`, `wochenbett.html`, `stillen.html`, `ueber-mich.html`, `netzwerk.html`, `kontakt.html`, `impressum.html`, `datenschutz.html`

### Technische Grundlagen (beide Phasen)
- Tailwind CSS via CDN: `<script src="https://cdn.tailwindcss.com"></script>`
- Google Fonts via CDN (Cormorant Garamond + DM Sans)
- Vanilla JS: nur für Hamburger-Menü + Leistungen-Dropdown (< 30 Zeilen)
- Placeholder-Bilder: `https://placehold.co/WxH` bis echte Fotos vorliegen
- Shared Navigation + Footer als HTML-Snippet — in jede Seite kopiert (kein Build-System)
