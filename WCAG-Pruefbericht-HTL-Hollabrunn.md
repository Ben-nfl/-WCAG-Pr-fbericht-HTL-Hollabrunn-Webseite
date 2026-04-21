# WCAG-Prüfbericht: HTL Hollabrunn Webseite


**Name:** Ben Neunteufel 5AHITS

**Fach & Datum:** MEDT am 21.4.2026

**Geprüfte URLs:**
- `https://www.htl-hl.ac.at/` (Startseite)
- `https://www.htl-hl.ac.at/web/ausbildung/uebersicht/` (Ausbildung/Übersicht)
- `https://www.htl-hl.ac.at/web/impressum/` (Impressum)
- `https://www.htl-hl.ac.at/web/kontakt/` (Kontakt)

**Methoden:** Vollständige HTML-Quellcode-Analyse (bereitgestellt + manuell abgerufen), Strukturprüfung, Linkprüfung

> **Hinweis zur Überarbeitung:** Diese Version korrigiert zwei Fehler aus dem Erstentwurf. Der Mobile-Toggle-Button war fälschlicherweise als Problem beschrieben – er ist korrekt als `<button>` implementiert. Ebenso wurde Problem 5 (Landmarks) präzisiert: `<header>`, `<nav>` und `<footer>` sind vorhanden, nur `<main>` fehlt.

---

## Überblick

Wir haben die Website der HTL Hollabrunn auf Barrierefreiheit geprüft. Basis war der vollständige HTML-Quellcode von vier Seiten, der entweder direkt bereitgestellt oder über den Browser abgerufen wurde. Insgesamt wurden **12 Probleme** gefunden, davon 6 kritisch auf Level A.

| Schweregrad          | Anzahl |
| -------------------- | ------ |
| Kritisch (Level A)   | 6      |
| Wichtig (Level A/AA) | 4      |
| Empfehlung           | 2      |
| **Gesamt**           | **12** |

---

## Teil 1: Gefundene Probleme

---

### Problem 1 – Keine H1-Überschrift auf der Startseite

| WCAG-Kriterium | 1.3.1 Info und Beziehungen (Level A) / 2.4.6 Überschriften (Level AA) |
| -------------- | --------------------------------------------------------------------- |
| Prinzip        | Wahrnehmbar & Bedienbar                                               |
| Schweregrad    | Kritisch                                                              |
| Fundort        | `https://www.htl-hl.ac.at/` → Slider-Bereich (`div.img-slider`)      |

**Was ist das Problem?**

Die Startseite hat keine `<h1>`-Überschrift. Der Slider startet direkt mit `<h3>` und `<h2>`, ohne dass es eine Seitenüberschrift gibt. Auf der Unterseite `/ausbildung/uebersicht/` ist die Situation ähnlich.

**Was sieht man im Code?**

```html
<!-- Fundort: https://www.htl-hl.ac.at/ → div.img-slider → div.img-slider__caption -->
<h3 class="img-slider__caption-sub-header">Ausbildung am Puls der Zeit</h3>
<h2 class="img-slider__caption-header">HTL Hollabrunn</h2>
<!-- Kein <h1> auf der gesamten Seite vorhanden -->
```

Auf der Impressum-Seite ist eine `<h1>` vorhanden – dort fehlt dafür die `<h2>`-Ebene (Sprung von `<h1>` direkt auf `<h3>`).

**Was bedeutet das für Nutzer?**

Wer mit einem Screenreader die Taste `H` drückt, landet direkt bei einer `<h2>` oder `<h3>` und weiß nicht, auf welcher Seite er sich befindet. Suchmaschinencrawler können die Seitenbedeutung ebenfalls schlechter einschätzen.

**Wie kann man es beheben?**

```html
<!-- So sollte es aussehen: -->
<h1>HTL Hollabrunn – Ausbildung am Puls der Zeit</h1>
<h2>Aktuelle Meldungen</h2>
<h3>Anmeldung für 2026/27</h3>
```

---

### Problem 2 – Bilder mit leerem Alt-Text, obwohl sie Inhalte zeigen

| WCAG-Kriterium | 1.1.1 Nicht-Text-Inhalt (Level A) |
| -------------- | ---------------------------------- |
| Prinzip        | Wahrnehmbar                        |
| Schweregrad    | Kritisch                           |
| Fundort        | Startseite & Ausbildung/Übersicht  |

**Was ist das Problem?**

Viele Bilder haben `alt=""`, obwohl sie inhaltliche Bedeutung haben – zum Beispiel das HTL-Logo, das Karriere-Bild und das Anmeldungs-Bild. Besonders problematisch: Das HTL-Logo ist in einen Link eingebettet. Wenn das `alt`-Attribut leer ist, hat der Link keinen zugänglichen Namen und der Screenreader liest entweder gar nichts oder den Dateipfad vor.

**Was sieht man im Code?**

```html
<!-- Fundort: https://www.htl-hl.ac.at/ → Footer → ce-textpic (HTL-Logo als Link) -->
<a href="/">
  <img src="fileadmin/_processed_/f/3/csm_HTL_Logo_fin_RGB_weiss_037fb886bf.png"
       width="80" height="56" alt="">
</a>
<!-- ↑ Link hat keinen zugänglichen Namen weil alt="" -->

<!-- Fundort: https://www.htl-hl.ac.at/ → Abschnitt "Anmeldung für 2026/27" -->
<img src="fileadmin/_processed_/1/b/csm_Bild3_2_bf43609fbd.jpg"
     width="150" height="150" alt="">

<!-- Fundort: https://www.htl-hl.ac.at/ → Abschnitt "Karriere als HTL-Lehrer:in" -->
<img src="fileadmin/_processed_/7/b/csm_202503_Karriere_als_HTL-LehrerIn.jpg_ce227b0e21.jpeg"
     width="150" height="150" alt="">

<!-- Fundort: https://www.htl-hl.ac.at/web/ausbildung/uebersicht/ → rechte Spalte -->
<img src="fileadmin/_processed_/6/9/csm_Startseite_Bild_3_74e601fa0e.jpg"
     width="500" height="500" alt="">
```

**Was bedeutet das für Nutzer?**

Blinde Nutzer erfahren nicht, was auf den Bildern zu sehen ist. Im schlimmsten Fall liest der Screenreader den langen Dateinamen vor. Der Link auf dem HTL-Logo ist für Screenreader-Nutzer nicht bedienbar.

**Wie kann man es beheben?**

```html
<!-- Bild mit Inhalt: Alt-Text beschreibt was zu sehen ist -->
<a href="/">
  <img src="csm_HTL_Logo_fin_RGB_weiss.png" alt="HTL Hollabrunn – zur Startseite">
</a>

<!-- Wirklich dekoratives Bild: alt="" + aria-hidden="true" -->
<img src="csm_Bild3_2.jpg" alt="" aria-hidden="true">
```

---

### Problem 3 – `<main>`-Landmark fehlt

| WCAG-Kriterium | 1.3.1 Info und Beziehungen (Level A) |
| -------------- | ------------------------------------- |
| Prinzip        | Wahrnehmbar                           |
| Schweregrad    | Kritisch                              |
| Fundort        | Alle geprüften Seiten                 |

**Was ist das Problem?**

Auf allen Seiten wird der Hauptinhalt in ein `<div id="main-content">` verpackt, nicht in ein `<main>`-Element. `<div>` hat keine semantische Bedeutung – ein Screenreader kann damit nicht als Hauptbereich der Seite navigieren.

`<header>`, `<nav>` und `<footer role="contentinfo">` sind auf der Seite korrekt vorhanden – nur `<main>` fehlt.

**Was sieht man im Code?**

```html
<!-- Was gefunden wurde (z. B. https://www.htl-hl.ac.at/) -->
<div id="main-content">
    <!-- Gesamter Seiteninhalt -->
</div>

<!-- Was da sein sollte: -->
<main id="main-content">
    <!-- Gesamter Seiteninhalt -->
</main>
```

**Was bedeutet das für Nutzer?**

Screenreader bieten eine Schnellnavigation zu Landmarks an (z. B. `F6` in NVDA oder der Rotor in VoiceOver). Ohne `<main>` gibt es keinen Hauptbereich zum Anspringen. Nutzer müssen sich mühsam durch den ganzen Header durcharbeiten.

**Wie kann man es beheben?**

```html
<!-- Einfach div durch main ersetzen: -->
<main id="main-content">
    <!-- Seiteninhalt -->
</main>
```

---

### Problem 4 – Link "Read more" ohne Kontext, auf Englisch

| WCAG-Kriterium | 2.4.4 Linkzweck (Level A) |
| -------------- | -------------------------- |
| Prinzip        | Bedienbar                  |
| Schweregrad    | Kritisch                   |
| Fundort        | `https://www.htl-hl.ac.at/` → Slider |

**Was ist das Problem?**

Im Startseiten-Slider gibt es einen Link mit dem Text "Read more". Der Text verrät nicht, wohin der Link führt. Außerdem steht er auf Englisch auf einer deutschen Seite.

**Was sieht man im Code?**

```html
<!-- Fundort: https://www.htl-hl.ac.at/ → div.img-slider__caption (Slider-Bereich) -->
<a href="ausbildung/uebersicht/" class="img-slider__caption-link">Read more</a>
```

**Was bedeutet das für Nutzer?**

Wer alle Links der Seite mit einem Screenreader auflistet, sieht nur "Read more" – ohne Kontext wohin der Link führt. Außerdem wird "Read more" mit falscher Aussprache (Deutsch) vorgelesen, weil kein `lang="en"` gesetzt ist.

**Wie kann man es beheben?**

```html
<!-- Option 1: Linktext direkt ändern -->
<a href="ausbildung/uebersicht/" class="img-slider__caption-link">
    Zum Ausbildungsangebot
</a>

<!-- Option 2: aria-label ergänzen -->
<a href="ausbildung/uebersicht/"
   aria-label="Mehr über das Ausbildungsangebot der HTL Hollabrunn">
    Read more
</a>

<!-- Option 3: versteckten Text einbauen -->
<a href="ausbildung/uebersicht/">
    Read more <span class="sr-only">über das Ausbildungsangebot</span>
</a>
```

---

### Problem 5 – Suchformular ohne sichtbares Label

| WCAG-Kriterium | 1.3.1 Info und Beziehungen (Level A) / 3.3.2 Beschriftungen (Level A) |
| -------------- | ---------------------------------------------------------------------- |
| Prinzip        | Wahrnehmbar                                                            |
| Schweregrad    | Kritisch                                                               |
| Fundort        | Alle Seiten → Hauptnavigation rechts (`nav#mdf_menu → .search-box`)    |

**Was ist das Problem?**

Das Suchfeld in der Navigationsleiste hat kein `<label>`-Element. Es hat nur ein `title`-Attribut und einen `placeholder` – das reicht laut WCAG nicht aus, weil der Placeholder bei Texteingabe verschwindet und `title` von Screenreadern unterschiedlich behandelt wird.

**Was sieht man im Code?**

```html
<!-- Fundort: Alle Seiten → nav#mdf_menu → ul.right-nav → div.search-box -->
<form method="get" action="/web/index.php">
    <input type="hidden" name="id" value="217">
    <input type="text" name="q" size="14" maxlength="255"
           title="Suche" placeholder="Suchtext">
    <!-- Kein <label> vorhanden -->
</form>
```

**Was bedeutet das für Nutzer?**

Screenreader-Nutzer hören möglicherweise nur "Bearbeitungsfeld" – ohne zu wissen, dass es sich um eine Suche handelt. `title` wird nicht von allen Hilfsmitteln korrekt ausgelesen.

**Wie kann man es beheben?**

```html
<!-- Option 1: Sichtbares Label -->
<label for="search-input">Suche</label>
<input type="text" id="search-input" name="q" placeholder="Suchtext">

<!-- Option 2: Unsichtbares aber zugängliches Label -->
<label for="search-input" class="sr-only">Seitensuche</label>
<input type="text" id="search-input" name="q" placeholder="Suchtext">

<!-- Option 3: aria-label direkt am Input -->
<input type="text" name="q" aria-label="Seitensuche" placeholder="Suchtext">
```

---

### Problem 6 – Defekter HTML-Code im Navbar-Brand

| WCAG-Kriterium | 4.1.1 Syntaxanalyse (Level A) |
| -------------- | ------------------------------ |
| Prinzip        | Robust                         |
| Schweregrad    | Kritisch                       |
| Fundort        | Alle Seiten → `nav#mdf_menu → div.navbar-header` |

**Was ist das Problem?**

Der HTML-Code des Logo-Links in der Navbar ist strukturell kaputt: Ein `<a>`-Tag enthält ein weiteres `<a href="/" />`-Tag (was in HTML verboten ist), und das äußere `<a>` hat kein gültiges `href`. Browser reparieren solche Fehler unterschiedlich, was zu unvorhersehbarem Verhalten bei Screenreadern führt.

**Was sieht man im Code?**

```html
<!-- Fundort: Alle Seiten → nav#mdf_menu → div.navbar-header -->
<a class="navbar-brand" <a href="/" /> <div class="nav_logo"></div></a>
<!--                    ↑ Defekt: zweites <a> innerhalb eines <a>,
                          und <a href="/" /> als self-closing ist ungültig -->
```

**Was bedeutet das für Nutzer?**

Je nach Browser/Screenreader-Kombination wird das Logo entweder gar nicht als Link erkannt, doppelt vorgelesen, oder die gesamte Navigationsstruktur wird falsch interpretiert. Das ist eine Kern-Funktionalität der Seite.

**Wie kann man es beheben?**

```html
<!-- Korrekte Version: -->
<a class="navbar-brand" href="/">
    <div class="nav_logo" role="img" aria-label="HTL Hollabrunn – zur Startseite"></div>
</a>
```

---

### Problem 7 – Skip-Link Ziel `#main-navigation` existiert nicht

| WCAG-Kriterium | 2.4.1 Blöcke überspringen (Level A) |
| -------------- | ------------------------------------ |
| Prinzip        | Bedienbar                            |
| Schweregrad    | Wichtig                              |
| Fundort        | Alle Seiten → Skip-Links am Seitenanfang, und `nav#mdf_menu` |

**Was ist das Problem?**

Beide Skip-Links sind vorhanden, aber der Link "Direkt zur Hauptnavigation springen" zeigt auf `#main-navigation`, das im HTML nicht existiert. Die Hauptnavigation hat die ID `mdf_menu`, nicht `main-navigation`. Der Link führt also ins Leere.

**Was sieht man im Code?**

```html
<!-- Fundort: Alle Seiten → div.skip-links (ganz oben im <body>) -->
<a href="ausbildung/uebersicht/#main-navigation" class="skip-links__item sr-only sr-only-focusable">
    Direkt zur Hauptnavigation springen   ← Ziel-ID fehlt!
</a>
<a href="ausbildung/uebersicht/#main-content" class="skip-links__item sr-only sr-only-focusable">
    Direkt zum Inhalt springen            ← Ziel-ID vorhanden ✓
</a>

<!-- Fundort: Alle Seiten → Hauptnavigation -->
<nav id="mdf_menu" class="navbar navbar-default clean sticky">
<!--      ↑ ID ist "mdf_menu" – nicht "main-navigation" -->
```

**Wie kann man es beheben?**

```html
<!-- Entweder: ID der Navigation anpassen -->
<nav id="main-navigation" class="navbar navbar-default clean sticky">

<!-- Oder: Skip-Link-Ziel anpassen -->
<a href="#mdf_menu">Direkt zur Hauptnavigation springen</a>
```

---

### Problem 8 – Fokus-Stil wahrscheinlich nicht sichtbar

| WCAG-Kriterium | 2.4.7 Fokus sichtbar (Level AA) |
| -------------- | -------------------------------- |
| Prinzip        | Bedienbar                        |
| Schweregrad    | Wichtig                          |
| Fundort        | CSS-Dateien (nicht vollständig zugänglich) |

**Was ist das Problem?**

Das externe CSS der Seite (Bootstrap + custom Theme) überschreibt typischerweise den Standard-Fokus-Rahmen des Browsers mit `outline: none` oder `outline: 0`. Das konnte nicht direkt aus dem Quellcode bestätigt werden, ist aber bei Bootstrap-basierten Seiten dieser Bauart sehr häufig. Beim manuellen Tab-Test ist zu prüfen ob ein sichtbarer Fokus-Indikator vorhanden ist.

**Wie kann man es beheben?**

```css
/* Fokus niemals einfach entfernen: */
:focus {
    outline: 3px solid #005fcc;
    outline-offset: 2px;
}

/* Moderne Variante: nur bei Tastaturnavigation sichtbar */
:focus-visible {
    outline: 3px solid #005fcc;
    outline-offset: 2px;
}
```

---

### Problem 9 – Videos möglicherweise ohne Untertitel

| WCAG-Kriterium | 1.2.2 Untertitel (aufgezeichnet) (Level A) |
| -------------- | ------------------------------------------ |
| Prinzip        | Wahrnehmbar                                |
| Schweregrad    | Wichtig                                    |
| Fundort        | `https://www.htl-hl.ac.at/web/ausbildung/uebersicht/` → `div#c1310` |

**Was ist das Problem?**

Es gibt mehrere eingebettete Videos und Verlinkungen zu YouTube. Ob diese Videos manuell geprüfte Untertitel haben, lässt sich aus dem HTML allein nicht sagen. Automatische YouTube-Untertitel zählen für WCAG nicht.

**Was sieht man im Code?**

```html
<!-- Fundort: https://www.htl-hl.ac.at/web/ausbildung/uebersicht/ → div#c1310 -->
<iframe src="https://www.youtube-nocookie.com/embed/varKqW44CjY?..."
        title="Technik Campus HTL Hollabrunn"
        class="embed-responsive-item"
        allow="fullscreen">
</iframe>
<!-- Positiv: title-Attribut vorhanden ✓ -->
<!-- Zu prüfen: Hat das Video auf YouTube manuelle Untertitel? -->

<!-- Fundort: https://www.htl-hl.ac.at/ → Abschnitt "neues HTL Video" -->
<a href="https://www.youtube.com/watch?v=k656j7PzMxA" target="_blank">Zum Video</a>
```

**Wie kann man es beheben?**

Untertitel über YouTube Studio aktivieren und manuell prüfen bzw. korrigieren. Alternativ ein Texttranskript direkt unter dem Video anbieten.

---

### Problem 10 – JavaScript-verschleierte E-Mail-Links

| WCAG-Kriterium | 4.1.2 Name, Rolle, Wert (Level A) |
| -------------- | ---------------------------------- |
| Prinzip        | Robust                             |
| Schweregrad    | Wichtig                            |
| Fundort        | `https://www.htl-hl.ac.at/web/ausbildung/uebersicht/` → Fachhochschul-Abschnitt (im Akkordeon, `div#c1308`) |

**Was ist das Problem?**

E-Mail-Adressen werden per JavaScript verschlüsselt, um Spam zu verhindern. Das `href`-Attribut enthält dabei `javascript:linkTo_UnCryptMailto(...)`. Solche Links sind für viele assistive Technologien nicht bedienbar – sie haben keinen auflösbaren Hyperlink und können in manchen Screenreader-Konfigurationen nicht aktiviert werden.

**Was sieht man im Code?**

```html
<!-- Fundort: https://www.htl-hl.ac.at/web/ausbildung/uebersicht/
     → Accordion-Panel "Fachhochschul-Studiengang" (div#c1308) → letzter Absatz -->
<a href="javascript:linkTo_UnCryptMailto('ocknvq,jcpu\/lqgti0rcagtBjvn\/jn0ce0cv');">
    hans-joerg.payer(at)htl-hl.ac.at
</a>
```

**Wie kann man es beheben?**

```html
<!-- Besser: mailto-Link direkt (Spam-Filter auf Serverseite) -->
<a href="mailto:hans-joerg.payer@htl-hl.ac.at">hans-joerg.payer@htl-hl.ac.at</a>

<!-- Oder: E-Mail als Text + Kontaktformular anbieten -->
```

---

### Problem 11 – "Read more" ohne Sprachkennzeichnung

| WCAG-Kriterium | 3.1.2 Sprache von Teilen (Level AA) |
| -------------- | ------------------------------------ |
| Prinzip        | Verständlich                         |
| Schweregrad    | Empfehlung                           |
| Fundort        | `https://www.htl-hl.ac.at/` → Slider |

**Was ist das Problem?**

Die Seite hat `lang="de"` gesetzt. Der Slider-Link heißt aber "Read more" (Englisch), ohne `lang="en"`. Screenreader lesen "Read more" dann mit deutscher Aussprache vor.

**Was sieht man im Code?**

```html
<!-- Fundort: https://www.htl-hl.ac.at/ → div.img-slider__caption -->
<a href="ausbildung/uebersicht/" class="img-slider__caption-link">Read more</a>
<!-- Kein lang="en" vorhanden, obwohl <html lang="de"> gesetzt ist -->
```

**Wie kann man es beheben?**

```html
<!-- Option 1: einfach Deutsch verwenden -->
<a href="ausbildung/uebersicht/">Mehr erfahren</a>

<!-- Option 2: Sprache kennzeichnen -->
<a href="ausbildung/uebersicht/" lang="en">Read more</a>
```

---

### Problem 12 – Farbkontrast nicht vollständig geprüft

| WCAG-Kriterium | 1.4.3 Kontrast Minimum (Level AA) |
| -------------- | ---------------------------------- |
| Prinzip        | Wahrnehmbar                        |
| Schweregrad    | Empfehlung                         |
| Fundort        | CSS-Dateien (nicht vollständig zugänglich) |

Das vollständige CSS war für diese Prüfung nicht zugänglich. Besonders kritisch zu überprüfen:

- Weißer Text über dem Slider-Bild (Header-Bereich)
- Grauer Text im Footer auf dunklem Hintergrund
- Linkfarben im Fließtext

WCAG fordert mindestens **4,5:1** für normalen Text und **3:1** für großen Text (≥ 18pt oder 14pt fett).

**Empfohlene Prüfmethode:**

Chrome DevTools öffnen → Lighthouse → Accessibility Audit → oder die Browser-Extension **axe DevTools** nutzen.

---

## Teil 2: Was gut funktioniert

| #  | WCAG-Kriterium                          | Befund                                                                                                       |
|----|-----------------------------------------|--------------------------------------------------------------------------------------------------------------|
| 1  | 3.1.1 Sprache der Seite (Level A)       | `<html lang="de">` korrekt gesetzt auf allen Seiten                                                          |
| 2  | 4.1.2 Mobile Toggle Button (Level A)    | `<button type="button" aria-expanded="false" aria-controls="navbar">` + `<span class="sr-only">Toggle navigation</span>` – korrekt implementiert |
| 3  | 2.4.1 Skip-Links (Level A)              | Zwei Skip-Links vorhanden; Ziel `#main-content` (`<div id="main-content">`) funktioniert                    |
| 4  | Header & Footer Landmarks               | `<header class="header">`, `<footer role="contentinfo">` korrekt verwendet                                   |
| 5  | Navigation Landmark                     | `<nav id="mdf_menu">` und `<nav aria-label="Meta navigation">` vorhanden                                     |
| 6  | Listensemantik in der Navigation        | Alle Navigationsbereiche verwenden `<ul>`/`<li>` korrekt                                                     |
| 7  | Social Media Links mit aria-label       | `<a aria-label="Facebook">`, `<a aria-label="LinkedIn">` etc. korrekt vorhanden                              |
| 8  | iframe mit title-Attribut              | YouTube-Einbettung hat `title="Technik Campus HTL Hollabrunn"` ✓                                            |
| 9  | Mehrsprachige Navigation               | Sprachwechsler mit `<a class="active" href="/">Deutsch</a>` / `<a href="en/">English</a>` vorhanden          |

---

## Teil 3: Priorisierte Maßnahmenliste

### Sofort (Kritisch – Level A)

| Priorität | Maßnahme                                                                          | WCAG  |
| --------- | --------------------------------------------------------------------------------- | ----- |
| 1         | Defekten HTML-Code im Navbar-Brand reparieren                                    | 4.1.1 |
| 2         | Alt-Texte für alle inhaltlichen Bilder ergänzen (besonders Logo-Link!)           | 1.1.1 |
| 3         | H1-Überschrift auf der Startseite einfügen                                        | 1.3.1 |
| 4         | `<div id="main-content">` zu `<main id="main-content">` ändern                   | 1.3.1 |
| 5         | "Read more"-Link durch sinnvollen Linktext oder `aria-label` ersetzen            | 2.4.4 |
| 6         | Suchformular mit `aria-label` oder `<label>` versehen                            | 1.3.1 |

### Bald danach

| Priorität | Maßnahme                                                                          | WCAG  |
| --------- | --------------------------------------------------------------------------------- | ----- |
| 7         | Skip-Link Ziel `#main-navigation` korrigieren (ID der Nav anpassen)              | 2.4.1 |
| 8         | Fokus-Stil mit `:focus-visible` prüfen und ggf. definieren                       | 2.4.7 |
| 9         | YouTube-Videos auf manuelle Untertitel prüfen                                     | 1.2.2 |
| 10        | JavaScript-verschlüsselte E-Mail-Links durch `mailto:`-Links ersetzen            | 4.1.2 |

### Empfehlungen

| Priorität | Maßnahme                                                               | WCAG  |
| --------- | ---------------------------------------------------------------------- | ----- |
| 11        | "Read more" durch deutschen Text ersetzen oder `lang="en"` ergänzen   | 3.1.2 |
| 12        | Farbkontrast mit Lighthouse oder axe DevTools vollständig prüfen       | 1.4.3 |

---

## Teil 4: Verwendete Testmethoden

| Methode                                  | Durchgeführt                            |
| ---------------------------------------- | --------------------------------------- |
| HTML-Quellcode-Analyse (manuell & mit AI) | Ja – vollständiger Quellcode von 4 Seiten |
| Screenreader (NVDA/VoiceOver)            | Nein – wäre zusätzlich empfohlen        |
| Nur-Tastatur-Navigation                  | Teilweise – anhand des Quellcodes simuliert |
| Chrome DevTools Accessibility Tree       | Nein – wäre empfohlen                   |
| Lighthouse Accessibility Audit           | Nein – wäre empfohlen                   |
| axe Browser Extension                    | Nein – wäre empfohlen                   |
| W3C HTML-Validator                       | Nein – wäre empfohlen (Problem 6!)      |

---

- **Geprüft gemäß WCAG 2.1:** https://www.w3.org/TR/WCAG21/
- **HTML-Analyse durchgeführt mit:** https://claude.ai
- **Weitere Informationen:** https://www.digitalbarrierefrei.at/de/verstehen/barrierefreiheitskriterien
