# Ejendomsinspektion – Forenede Service

Udfyldeligt skema til visuelt tilsyn af bygning og udearealer. Siden er én HTML-fil (`index.html`) uden backend.

## Brug

1. Åbn siden i Safari (iPad/iPhone) eller Edge/Chrome (pc).
2. Udfyld felterne, og tilføj op til 2 fotos pr. punkt.
3. Tryk **Lav PDF** → **Hent PDF**. På iPad ligger filen i Filer-appen under Overførsler.
4. Send PDF'en i en ny mail til den fælles mailbox.

## Opgaver i Propbinder

Et punkt kan markeres som **Opgave** (sammen med en status). Så vises feltet Opgavetitel. Ved **Lav PDF** åbner et vindue, hvor inspektøren logger ind med sit eget Propbinder-login, vælger ejendom og type (forvalgt "Inspektøropgaver") og opretter opgaverne: status "Skal udføres", frist på inspektionsdagen, tildelt inspektøren, beskrivelse = bemærkning + punkt, status og fotonumre. Derefter laves PDF'en. **Kun PDF** springer Propbinder over.

Siden kalder Propbinders interne web-API (`https://propbinder-api.azurewebsites.net/`) direkte fra browseren, ligesom siden "Automatisk opgave oprettelse i Propbinder" (`api/Website/Login`, `GET api/WebSite/Company/User/UserInfo`, `GET api/Website/Company/User`, `GET api/Activity/ActivityTypes`, `GET api/PropertyLease/GetPropertyList`, `POST api/Activity`). Der ligger ingen adgangskode eller nøgle i siden. Login-nøglen findes kun i hukommelsen, mens siden er åben. API'et er ikke et dokumenteret partner-API og kan ændre sig uden varsel. Fotos vedhæftes ikke opgaverne.

## Data

Siden gemmer intet. Det udfyldte og fotos findes kun i browseren, indtil PDF'en hentes. Lukkes eller genindlæses siden, forsvinder det udfyldte. Kun opgaver markeret som Opgave sendes til Propbinder, og kun når inspektøren selv opretter dem.

## Opdatering

Skemaets kilde er `Ejendomsinspektion_skema.html` i projektmappen. Ved ændringer kopieres den til `index.html` her (med `<meta name="robots" content="noindex,nofollow">` i `<head>`), og ændringen committes og pushes til `main`.

Siden er bygget med jsPDF 2.5.1 (MIT-licens), som er indbygget i `index.html`.
