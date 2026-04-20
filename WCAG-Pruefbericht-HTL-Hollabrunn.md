# WCAG-Prüfbericht: HTL Hollabrunn Webseite

**Name** Ben Neunteufel 5AHITS
**Fach & Datum** MEDT am 21.4.2026
**URL:** https://www.htl-hl.ac.at/web/  
**Getestete Seiten:** Startseite, /kontakt/ ausbildung/ übersicht
**Methoden:** HTML-Analyse, manuelle Prüfung der Seitenstruktur

---

## Überblick

Wir haben die Website der HTL Hollabrunn auf Barrierefreiheit geprüft. Dabei haben wir uns den HTML-Code angeschaut, die Seitenstruktur untersucht und geschaut ob man die Seite auch ohne Maus bedienen könnte. Insgesamt haben wir 10 Probleme gefunden, davon sind 5 wirklich kritisch.

| Schweregrad | Anzahl |
|-------------|--------|
| Kritisch (Level A) | 5 |
| Wichtig (Level AA) | 3 |
| Empfehlung | 2 |
| **Gesamt** | **10** |

---

## Teil 1: Gefundene Probleme

---

### Problem 1 – Es gibt keine H1-Überschrift

| | |
|------|--------|
| **WCAG-Kriterium** | 1.3.1 Info und Beziehungen (Level A) / 2.4.6 Überschriften (Level AA) |
| **Prinzip** | Wahrnehmbar & Bedienbar |
| **Schweregrad** | Kritisch |

**Was ist das Problem?**  
Auf der Startseite und auch auf der Unterseite `/ausbildung/uebersicht/` gibt es kein einziges `<h1>`-Tag. Die Seite fängt direkt mit `<h3>`- und `<h4>`-Überschriften an, was eigentlcih überhaupt keinen Sinn macht weil die Hierarchie ja dann komplett fehlt.

**Was sieht man im Code?**  
```html
<!-- Was gefunden wurde: -->
<h3>Ausbildung am Puls der Zeit</h3>
<h3>Anmeldung für 2026/27</h3>
<!-- Es gibt kein <h1> oder <h2> irgendwo -->
```

**Was bedeutet das für Nutzer?**  
Wer einen Screenreader benutzt, navigiert mit der Taste `H` durch die Überschriften. Ohne `<h1>` weiß man halt nicht wo man überhaupt ist, also auf welcher Seite man sich befindet. Das ist ein echter Nachteil.

**Wie kann man es beheben?**  
Einfach einen `<h1>` mit dem Seitennamen ganz oben einfügen, und dann die restlichen Überschriften anpassen:

```html
<!-- So sollte es aussehen: -->
<h1>Herzlich willkommen an der HTL Hollabrunn</h1>
<h2>Ausbildung</h2>
  <h3>Höhere Lehranstalten</h3>
```

---

### Problem 2 – Bilder haben keinen Alt-Text

| | |
|------|--------|
| **WCAG-Kriterium** | 1.1.1 Nicht-Text-Inhalt (Level A) |
| **Prinzip** | Wahrnehmbar |
| **Schweregrad** | Kritisch |

**Was ist das Problem?**  
Fast alle Bilder auf der Seite haben entweder gar kein `alt`-Attribut oder ein leeres `alt=""`, obwohl die Bilder eigentlich Infos zeigen die man braucht. Zum Beispiel gibt es ein Diagramm zu den Ausbildungswegen und ein Karriere-Bild, bei denen einfach gar nichts drinsteht.

**Was sieht man im Code?**  
```html
<img src="fileadmin/_processed_/1/b/csm_Bild3_2_bf43609fbd.jpg" alt="">
<img src="fileadmin/_processed_/7/b/csm_202503_Karriere_als_HTL-LehrerIn.jpg_ce227b0e21.jpeg" alt="">
<img src="fileadmin/user_upload/Photovoltaik_150x150.gif" alt="">
<img src="fileadmin/_processed_/f/5/csm_Ausbildungsweg_V2_66961fe3a1.png">
<!-- ^ da fehlt das alt-Attribut komplett -->
```

**Was bedeutet das für Nutzer?**  
Blinde Nutzer hören dann einfach gar nichts, oder im schlimmsten Fall liest der Screenreader den kompletten Dateinamen vor, also sowas wie "csm_Ausbildungsweg_V2_66961fe3a1.png". Das hilft natürlich niemandem.

**Wie kann man es beheben?**  
Bei Bildern die wirklich was zeigen braucht man einen kurzen beschreibenden Text. Rein dekorative Bilder können `alt=""` haben, aber dann sollte man auch noch `aria-hidden="true"` dazuschreiben.

```html
<!-- Vorher: -->
<img src="csm_Ausbildungsweg_V2.png" alt="">

<!-- Nachher (das Bild zeigt was): -->
<img src="csm_Ausbildungsweg_V2.png" alt="Diagramm: Ausbildungswege an der HTL Hollabrunn von der Fachschule bis zum Kolleg">

<!-- Nachher (nur dekorativ): -->
<img src="csm_Bild3_2.jpg" alt="" aria-hidden="true">
```

---

### Problem 3 – Das Hamburger-Menü ist kein richtiger Button

| | |
|------|--------|
| **WCAG-Kriterium** | 4.1.2 Name, Rolle, Wert (Level A) |
| **Prinzip** | Robust |
| **Schweregrad** | Kritisch |

**Was ist das Problem?**  
Das Menü zum Aufklappen auf dem Handy ist kein `<button>`, sondern nur ein `<div>` mit einem Click-Handler. Außerdem gibt es kein `aria-expanded`, also weiß niemand ob das Menü gerade offen oder zu ist.

**Was sieht man im Code?**  
```html
<!-- Was gefunden wurde: -->
<div class="nav-toggle">☰</div>

<!-- Was fehlt: -->
<!-- <button aria-expanded="false" aria-controls="main-navigation"> -->
<!--   <span class="sr-only">Menü öffnen</span> -->
<!-- </button> -->
```

**Was bedeutet das für Nutzer?**  
Wenn man nur die Tastatur benutzt merkt man dass man das Menü überhaupt nicht öffnen kann, weil ein `<div>` keine Tastaturinteraktion hat. Enter oder Space funktionieren also nicht. Und Screenreader überspringen das Element dann einfach komplett.

**Wie kann man es beheben?**  
```html
<!-- So sollte es aussehen: -->
<button 
  aria-expanded="false" 
  aria-controls="main-navigation"
  aria-label="Navigationsmenü öffnen"
>
  <span aria-hidden="true">☰</span>
</button>
```
Dazu muss JavaScript dann noch `aria-expanded` von `false` auf `true` umschalten wenn man klickt.

---

### Problem 4 – Links heißen nur "Read more"

| | |
|------|--------|
| **WCAG-Kriterium** | 2.4.4 Linkzweck (Level A) |
| **Prinzip** | Bedienbar |
| **Schweregrad** | Kritisch |

**Was ist das Problem?**  
Auf der Startseite gibt es mehrere Links die alle nur "Read more" heißen. Man kann also nicht wissen wohin der jeweilige Link führt – und auf Englisch ist er auf einer deutschen Seite dann auch noch.

**Was sieht man im Code?**  
```html
<a href="ausbildung/uebersicht/">Read more</a>
<!-- und noch mehr davon auf der Seite -->
```

**Was bedeutet das für Nutzer?**  
Wer alle Links der Seite mit einem Screenreader auflistet, sieht dann halt fünfmal "Read more" und weiß nicht was sich dahinter verbirgt. Das ist schlecht weil man dann einfach nicht weiß was einen erwartet.

**Wie kann man es beheben?**  
```html
<!-- Option 1: einfach einen vernünftigen Linktext schreiben -->
<a href="ausbildung/uebersicht/">Mehr über unsere Ausbildungen</a>

<!-- Option 2: aria-label dazuschreiben -->
<a href="ausbildung/uebersicht/" aria-label="Mehr über das Ausbildungsangebot der HTL Hollabrunn">
  Read more
</a>

<!-- Option 3: versteckten Text einbauen -->
<a href="ausbildung/uebersicht/">
  Read more <span class="sr-only">über das Ausbildungsangebot</span>
</a>
```

---

### Problem 5 – Keine HTML-Landmark-Elemente

| | |
|------|--------|
| **WCAG-Kriterium** | 1.3.1 Info und Beziehungen (Level A) |
| **Prinzip** | Wahrnehmbar |
| **Schweregrad** | Kritisch |

**Was ist das Problem?**  
Die Seite benutzt keine semantischen HTML-Elemente wie `<main>`, `<nav>`, `<header>` oder `<footer>`. Stattdessen wird alles mit `<div>` gebaut. Man kann sich vorstellen das das für einen Screenreader ein ziemliches Problem ist weil er nicht weiß was Hauptinhalt ist und was Navigation.

**Was sieht man im Code?**  
- Kein `<main>` für den Hauptinhalt
- Navigation ist ein `<div>` statt `<nav>`
- Kein `<header>` oder `<footer>` erkennbar

**Wie kann man es beheben?**  
```html
<header role="banner">
  <nav role="navigation" aria-label="Hauptnavigation">...</nav>
</header>

<main role="main" id="main-content">
  <!-- Hauptinhalt hier -->
</main>

<footer role="contentinfo">...</footer>
```

---

### Problem 6 – Fokus ist wahrscheinlich nicht sichtbar

| | |
|------|--------|
| **WCAG-Kriterium** | 2.4.7 Fokus sichtbar (Level AA) |
| **Prinzip** | Bedienbar |
| **Schweregrad** | Wichtig |

**Was ist das Problem?**  
Im CSS gibt es keine Hinweise auf einen eigenen Fokus-Stil. Viele Seiten entfernen den fokus einfach mit `outline: none`, ohne ihn durch etwas Eigenes zu ersetzen. Das ist dann natürlich für Tastaturnutzer sehr unangenehm.

**Was bedeutet das für Nutzer?**  
Wenn man mit Tab durch die Seite navigiert, sieht man nicht wo man gerade ist. Das ist so als würde man blind durch ein Zimmer laufen.

**Wie kann man es beheben?**  
```css
/* Fokus niemals einfach entfernen */
:focus {
  outline: 3px solid #005fcc;
  outline-offset: 2px;
}

/* Moderne Variante, nur bei Tastaturnavigation */
:focus-visible {
  outline: 3px solid #005fcc;
  outline-offset: 2px;
}
```

---

### Problem 7 – Skip-Links funktionieren vielleicht nicht richtig

| | |
|------|--------|
| **WCAG-Kriterium** | 2.4.1 Blöcke überspringen (Level A) |
| **Prinzip** | Bedienbar |
| **Schweregrad** | Wichtig |

**Was ist das Problem?**  
Die Seite hat zwar zwei Skip-Links ("Direkt zur Hauptnavigation" und "Direkt zum Inhalt"), was wichitg und gut ist. Aber wir konnten nicht bestätigen ob die Ziel-IDs `#main-navigation` und `#main-content` wirklich im HTML existieren. Außerdem ist unklar ob die Skip-Links sichtbar werden wenn man sie per Tab erreicht.

**Was sieht man im Code?**  
```html
<a href="/#main-navigation">Direkt zur Hauptnavigation springen</a>
<a href="/#main-content">Direkt zum Inhalt springen</a>
```
Die Elemente mit den passenden IDs konnten nicht gefunden werden.

**Wie kann man es beheben?**  
```html
<!-- Ziel-IDs müssen wirklich da sein: -->
<main id="main-content">...</main>
<nav id="main-navigation">...</nav>
```
```css
/* Skip-Link beim Tab-Fokus einblenden: */
.skip-link {
  position: absolute;
  left: -9999px;
}
.skip-link:focus {
  left: 0;
  top: 0;
  z-index: 9999;
  background: #000;
  color: #fff;
  padding: 8px 16px;
}
```

---

### Problem 8 – Videos haben vielleicht keine Untertitel

| | |
|------|--------|
| **WCAG-Kriterium** | 1.2.2 Untertitel aufgezeichnet (Level A) |
| **Prinzip** | Wahrnehmbar |
| **Schweregrad** | Wichtig |

**Was ist das Problem?**  
Es gibt Links zu Videos auf YouTube und PeerTube, aber man kann aus dem HTML-Code nicht erkennen ob diese Videos Untertitel haben. Automatische YouTube-Untertitel zählen hier nicht, die müssen manuell geprüft und korrigiert werden.

**Was bedeutet das für Nutzer?**  
Gehörlose Nutzer können sich die Videos dann nicht ansehen. Gleiches gilt auch für Leute die gerade in einer ruhigen Umgebung sind und keinen Ton machen wollen.

**Wie kann man es beheben?**  
Untertitel über YouTube Studio aktivieren und manuell prüfen. Alternativ kann man auch ein Transkript als Text unter das Video schreiben.

---

### Problem 9 – "Read more" ist auf Englisch ohne Sprachkennzeichnung

| | |
|------|--------|
| **WCAG-Kriterium** | 3.1.2 Sprache von Teilen (Level AA) |
| **Prinzip** | Verständlich |
| **Schweregrad** | Empfehlung |

**Was ist das Problem?**  
Die Seite hat `lang="de"` gesetzt, aber die Links heißen "Read more". Das `lang`-Attribut am Link selbst fehlt, also liest der Screenreader "Read more" mit deutscher Aussprache vor.

**Wie kann man es beheben?**  
```html
<!-- Am besten: einfach Deutsch verwenden -->
<a href="ausbildung/uebersicht/">Mehr erfahren</a>

<!-- Oder: Sprache kennzeichnen -->
<a href="ausbildung/uebersicht/" lang="en">Read more</a>
```

---

### Problem 10 – Farbkontrast wurde nicht geprüft

| | |
|------|--------|
| **WCAG-Kriterium** | 1.4.3 Kontrast Minimum (Level AA) |
| **Prinzip** | Wahrnehmbar |
| **Schweregrad** | Empfehlung |

Wir konnten den Farbkontrast ohne das vollständige CSS nicht prüfen. Es ist aber empfohlen das mit Lighthouse oder axe nachzuholen, besonders bei grauen Texten auf weißem Hintergrund oder bei Text der über Bildern liegt.

WCAG fordert mindestens 4,5:1 für normalen Text und 3:1 für großen Text.

Einfach mal Lighthouse in Chrome aufmachen und den Accessibility-Audit laufen lassen, dann sieht man sofort welche Farben Probleme machen.

---

## Teil 2: Was gut funktioniert

| # | Kriterium | Befund |
|---|-----------|--------|
| 1 | 3.1.1 Sprache der Seite (Level A) | `<html lang="de">` ist korrekt gesetzt |
| 2 | 2.4.1 Blöcke überspringen (Level A) | Skip-Links sind vorhanden (Ziel-IDs müssen noch geprüft werden) |
| 3 | Listen-Semantik | Navigation benutzt `<ul>`/`<li>` richtig |

---

## Teil 3: Was als erstes gemacht werden sollte

### Sofort (Kritisch – Level A)

| Priorität | Maßnahme | WCAG |
|-----------|----------|------|
| 1 | Alt-Texte für alle inhaltlichen Bilder ergänzen | 1.1.1 |
| 2 | H1-Überschrift auf jeder Seite einfügen | 1.3.1 |
| 3 | Menü-Toggle von `<div>` auf `<button>` mit `aria-expanded` umbauen | 4.1.2 |
| 4 | "Read more"-Links mit sinnvollem Text oder `aria-label` versehen | 2.4.4 |
| 5 | `<main>`, `<nav>`, `<header>`, `<footer>` statt `<div>` verwenden | 1.3.1 |

### Bald danach (Level AA)

| Priorität | Maßnahme | WCAG |
|-----------|----------|------|
| 6 | Fokus-Stil mit `:focus-visible` definieren | 2.4.7 |
| 7 | Skip-Link-Ziele und Einblendung beim Fokus sicherstellen | 2.4.1 |
| 8 | Untertitel für Videos prüfen | 1.2.2 |

### Empfehlungen

| Priorität | Maßnahme | WCAG |
|-----------|----------|------|
| 9 | "Read more" auf Deutsch umstellen oder `lang="en"` ergänzen | 3.1.2 |
| 10 | Farbkontrast mit Lighthouse oder axe prüfen | 1.4.3 |

---

## Teil 4: Welche Testmethoden wir verwendet haben

| Methode                              | Durchgeführt |
|-------------------------------------|-------------|
| HTML-Strukturanalyse (manuell)      | Ja |
| Screenreader (NVDA/VoiceOver)       | Nein – wäre aber empfohlen |
| Nur-Tastatur-Navigation             | Nein – wäre empfohlen (Tab, Enter, Space, Pfeiltasten) |
| Chrome DevTools Accessibility Tree  | Nein – wäre empfohlen |
| Lighthouse Accessibility Audit      | Nein – wäre empfohlen |
| axe Browser Extension               | Nein – wäre empfohlen |

---

- **Geprüft gemäß WCAG 2.1:** https://www.w3.org/TR/WCAG21/  
- **HTML-Analyse durchgeführt mit:** https://claude.ai  
- **Weitere Informationen zu Barrierefreiheitskriterien:** https://www.digitalbarrierefrei.at/de/verstehen/barrierefreiheitskriterien