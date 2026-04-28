# MealDesk – Essensplanung Pension

Eine schlanke Single-File-Webseite, mit der sich Mitreisende selbst eintragen können, an welchen Mahlzeiten (Frühstück / Abendessen, Do–So) sie teilnehmen. Daten werden zentral über [jsonbin.io](https://jsonbin.io) gespeichert, gehostet wird über GitHub Pages.

## Setup in 5 Schritten

### 1. jsonbin.io-Account anlegen

1. Auf [jsonbin.io](https://jsonbin.io) registrieren (kostenloser Plan reicht).
2. Im Dashboard auf **Create a Bin** klicken.
3. Als Inhalt einfach eintragen:
   ```json
   { "entries": [] }
   ```
4. Bin speichern. Auf der Bin-Übersicht oben die **Bin ID** kopieren (sieht aus wie `64f1a2b3c0e8a3a7b8f12345`).
5. Wichtig: Privacy auf **Private** belassen (Standard).

### 2. Access Key erstellen

1. Im Menü links auf **API Keys → Access Keys** klicken.
2. **+ Create Access Key** klicken.
3. Name vergeben (z. B. „MealDesk").
4. Bei den Berechtigungen unter **Bins** folgende beiden aktivieren:
   - ✅ **Read**
   - ✅ **Update**
   (Create und Delete bleiben aus.)
5. Access Key erstellen und den Wert kopieren (sieht aus wie `$2a$10$...`).

> Warum Access Key statt Master Key? Der Access Key landet im Quellcode der Seite und ist dort öffentlich sichtbar. Mit reduzierten Rechten kann höchstens dieser eine Bin manipuliert werden – nicht dein ganzer Account.

### 3. Werte in `index.html` eintragen

In `index.html` ganz oben im `<script>`-Block den `CONFIG`-Block bearbeiten:

```js
const CONFIG = {
  PASSWORD: 'Pledl',                 // bereits gesetzt
  BIN_ID: '64f1a2b3c0e8a3a7b8f12345', // ← deine Bin ID hier
  ACCESS_KEY: '$2a$10$abcdefg...'    // ← dein Access Key hier
};
```

### 4. Auf GitHub veröffentlichen

1. Neues GitHub-Repo anlegen, z. B. `pension-essen`.
2. `index.html` und (optional) diese `README.md` ins Repo pushen.
3. Im Repo unter **Settings → Pages**:
   - **Source**: `Deploy from a branch`
   - **Branch**: `main` (oder `master`), Folder: `/ (root)`
   - Speichern.
4. Nach 1–2 Minuten ist die Seite erreichbar unter:
   ```
   https://<dein-username>.github.io/pension-essen/
   ```

### 5. Link & Passwort an die Gruppe verteilen

URL und Passwort z. B. per WhatsApp an alle Mitreisenden schicken. Fertig.

## Bedienung

- **Einloggen** mit dem Passwort (einmal pro Sitzung).
- **Name eintragen** und Mahlzeiten ankreuzen → **Speichern**.
- Wer schon mal eingetragen ist und denselben Namen erneut eingibt, sieht seinen alten Eintrag und kann ihn aktualisieren.
- In der **Übersicht** sind alle Einträge inkl. Summen pro Mahlzeit sichtbar. Mit **↻ Aktualisieren** den neuesten Stand laden.
- Einträge können mit dem **×**-Knopf gelöscht werden.

## Hinweise

- **Sicherheit**: Da der Quellcode bei GitHub Pages öffentlich ist, sind Passwort und Access Key keine echten Geheimnisse. Für den einmaligen Gruppengebrauch ist das in Ordnung – wer wirklich will, kommt vorbei. Gegen versehentliche Treffer durch Suchmaschinen schützt das Passwort aber zuverlässig.
- **Konkurrenz**: Die Seite lädt vor jedem Speichern den aktuellen Stand neu und merged dann den eigenen Eintrag rein. Bei zwei gleichzeitigen Speichervorgängen im Sekundenbruchteil kann theoretisch eine Eintragung verloren gehen – in der Praxis bei 10–20 Personen quasi ausgeschlossen.
- **Nach der Reise**: Bin auf jsonbin.io löschen oder leeren (`{"entries": []}`), Access Key revoken. GitHub-Repo kann archiviert oder gelöscht werden.

## Lokal testen (optional)

`index.html` einfach im Browser öffnen – funktioniert ohne Webserver. Die jsonbin-API antwortet auch von `file://` aus.
