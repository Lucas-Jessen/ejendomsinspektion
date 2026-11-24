# CLAUDE.md – ejendomsinspektion-pages

Projektviden til nye sessioner. Svar altid på dansk.

## Formål og teknologi
- Udfyldeligt skema til visuelt tilsyn af bygning og udearealer for Forenede Service. Bruges af inspektører på iPad/iPhone (Safari) og pc (Edge/Chrome).
- Den publicerede side er ÉN statisk fil: `index.html` (ca. 890 linjer / 440 KB). Ingen backend, ingen build, ingen npm, ingen eksterne kald.
- PDF laves i browseren med jsPDF 2.5.1 (MIT), som er indlejret i `index.html` (de meget lange minificerede linjer ca. 104–476). Logoet er indlejret som base64-PNG (`const LOGO=...`).
- Siden gemmer intet (ingen localStorage/IndexedDB). Udfyldt indhold og fotos forsvinder ved lukning/genindlæsning; der er en `beforeunload`-advarsel ved ugemte ændringer.

## Sammenhæng med Forenede_Ejendomsinspektion (VIGTIGT)
- Kilden til siden er `..\Forenede_Ejendomsinspektion\Ejendomsinspektion_skema.html` (søstermappe, ikke git).
- `index.html` her = kildefilen + én ekstra linje i `<head>`: `<meta name="robots" content="noindex,nofollow">`. Ellers byte-identiske (verificeret 24.11.2026, ses bort fra CRLF).
- Arbejdsgang ved ændringer (fra README): ret kildefilen → kopiér til `index.html` → sørg for at robots-meta stadig er der → commit og push til `main`.
- Retter man direkte i `index.html`, skal ændringen også føres tilbage til kildefilen, ellers overskrives den ved næste kopiering.
- Søstermappen indeholder desuden en ældre, separat pilot (`web/` med IndexedDB) og en Python PDF-generator – de er IKKE det, der publiceres her.

## Kommandoer
- Lokal visning: åbn `index.html` direkte i browseren (filen er selvstændig), eller `python -m http.server 8000 --bind 127.0.0.1`.
- Ingen build/test-scripts findes.
- Deploy: push til `main` på https://github.com/Lucas-Jessen/ejendomsinspektion. `.nojekyll` findes, så det er udgivet som GitHub Pages fra roden. Den præcise Pages-URL er ikke verificeret (formodentlig `https://lucas-jessen.github.io/ejendomsinspektion/`).

## Struktur
- `index.html` – hele appen: CSS, HTML (værktøjslinje, A4-ark i `#sheets`, dialoger for fotos/afsendelse/skjulte punkter), jsPDF, app-script.
- `README.md` – brugervejledning og opdateringsprocedure.
- `robots.txt` – `Disallow: /` (siden skal ikke indekseres; behold sammen med robots-meta).
- `.nojekyll` – slår Jekyll fra på GitHub Pages.

## Vigtige dele af app-scriptet (sidste `<script>`)
- `MAILBOX` – fælles postkasse som PDF'en skal sendes til (siden sender ikke selv; brugeren vedhæfter manuelt).
- `GROUPS` – kontrolområder og punkter. `WEATHER` – vejr-rullemenu (`''`, Sol, Overskyet, Regn, Sne).
- `MAX_PHOTOS=2` pr. punkt; fotos komprimeres (`compress`) og kommer som bilag bagerst i PDF'en.
- `buildLayout`/`renderDom`/`fit` – tegner A4-siderne og skalerer dem ned på smalle skærme (CSS `zoom`).
- Adresse og dato kun på side 1 (felter `p1_ejendom`, `p1_dato`; `inspektoer`). Dato sættes til i dag (`setToday`).
- Skjul-knap pr. punkt; skjulte punkter kommer ikke med i PDF'en (`hidden`, dialog "Skjulte punkter").
- `buildPdf` – jsPDF (`unit:'pt'`, helvetica). `prepare` → `showReady` viser knapperne.
- Versionsmærke i værktøjslinjen: `<span class="ver">Version 11.11.2026 (3)</span>` – opdatér ved ændringer.

## PDF-navn og iPad-deling (seneste commit)
- PDF'en navngives efter ejendommen: `baseName()` renser `p1_ejendom` for `\ / : * ? " < > |`, trimmer og afkorter til 80 tegn; tom adresse → `Ejendomsinspektion.pdf`.
- Filen laves som `new File([blob], name, {type:'application/pdf'})`.
- På touch-enheder vises "Del / gem" (`navigator.share({files:[...]})`), som bevarer filnavnet (Gem i Filer / Mail). "Åbn PDF" vises også, men gemmes der derfra, kan navnet blive en kode.
- Deling slås fra, når siden vises i en iframe/forhåndsvisning (`framed`); så vises en note om at åbne siden direkte i Safari.
- "Hent PDF" (`<a download>`) er altid til stede; på iPad havner filen i Filer → Overførsler.
- Test ændringer i PDF/deling på en fysisk iPad – kan ikke verificeres fuldt på pc.

## Designsystem
- Farver (`:root` og `COLORS` i JS – hold dem ens):
  - `--blue #26677D`, `--green #3C795F`, `--ink #243B43`, `--muted #597078`, `--line #CAD7DB`, `--bg #e6ebed`
  - Hjælpefarver: hover `#f1f6f8`, fokus-ring `#67a8b9`, fejl/advarsel `#a3392f`.
- Skrift: `Helvetica, Arial, "Liberation Sans", sans-serif` (PDF: jsPDF `helvetica`).
- Al CSS ligger inline i `<style>` i `index.html`; ingen separate CSS-filer.
- Logo: base64 i `LOGO`. Originalen ligger i søstermappen (`assets/Forenede_Service_logo.ai`, `web/logo.png`).

## Konventioner og faldgruber
- Projektet ligger i OneDrive ("Forenede A S") med mellemrum i stien – citér stier.
- Filen er stor og har meget lange linjer: brug Grep/offset ved læsning; redigér ikke i jsPDF-blokken.
- Behold `'use strict'`, fejlvisning i statuslinjen (`showErr`) – den bruges til fejlsøgning på iPad.
- Siden må ikke hente noget udefra (jsPDF er indlejret netop derfor).
- Commit-beskeder skrives på dansk og beskriver brugerens oplevelse.

## Git-regler
- Arbejd på `main`; commit og push direkte til `main` (det udgiver siden).
- Før commit: tjek `git diff`, at ingen hemmeligheder, testudfyldninger, fotos eller kundedata (adresser, navne) er kommet med, og at robots-meta + `robots.txt` stadig er på plads.
- Commit/push kun når brugeren beder om det.

## Brugerens arbejdsregler
- Bekræft din forståelse af opgaven, før du ændrer noget.
- Vurdér risikoen for det eksisterende (siden bruges i drift af inspektører) og nævn den.
- Svar på dansk.
