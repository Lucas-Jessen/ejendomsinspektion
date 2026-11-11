# Ejendomsinspektion – Forenede Service

Udfyldeligt skema til visuelt tilsyn af bygning og udearealer. Siden er én HTML-fil (`index.html`) uden backend.

## Brug

1. Åbn siden i Safari (iPad/iPhone) eller Edge/Chrome (pc).
2. Udfyld felterne, og tilføj op til 2 fotos pr. punkt.
3. Tryk **Lav PDF** → **Hent PDF**. På iPad ligger filen i Filer-appen under Overførsler.
4. Send PDF'en i en ny mail til den fælles mailbox.

## Data

Siden gemmer intet. Det udfyldte og fotos findes kun i browseren, indtil PDF'en hentes. Lukkes eller genindlæses siden, forsvinder det udfyldte.

## Opdatering

Skemaets kilde er `Ejendomsinspektion_skema.html` i projektmappen. Ved ændringer kopieres den til `index.html` her (med `<meta name="robots" content="noindex,nofollow">` i `<head>`), og ændringen committes og pushes til `main`.

Siden er bygget med jsPDF 2.5.1 (MIT-licens), som er indbygget i `index.html`.
