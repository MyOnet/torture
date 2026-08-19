# TASTENRAGE — GitHub Pages Deployment

Dieser Ordner enthält alles, was du für eine öffentliche GitHub-Pages-Version von TASTENRAGE brauchst: `index.html` ist die komplette Single-File-App (kein Build-Schritt, keine Abhängigkeiten außer Google Fonts über CDN).

## 1. Auf GitHub Pages veröffentlichen (5 Minuten)

1. Geh auf [github.com/new](https://github.com/new) und erstelle ein neues **öffentliches** Repository, z. B. `tastenrage`.
2. Lade `index.html` aus diesem Ordner hoch ("Add file" → "Upload files" → Datei reinziehen → Commit).
3. Geh in den Repo-Settings auf **Pages** (linkes Menü unter "Code and automation").
4. Bei "Source" wähle **Deploy from a branch**, Branch `main`, Ordner `/ (root)`. Speichern.
5. Nach ca. 1 Minute ist die Seite live unter `https://<dein-username>.github.io/tastenrage/`.

Das reicht bereits für ein voll funktionsfähiges Spiel — alle 3 Geschichten, Endlos-Modus, Musik, Admin-Cheat. Nur die **geräteübergreifende Bestenliste** braucht noch Schritt 2.

## 2. Geteilte Bestenliste aktivieren (kvdb.io, kostenlos)

**Diese `index.html` hat deine Bucket-ID (`57PfX3hnukcpZQZu6pc1C1`) bereits eingetragen** — die Schritte unten musst du nur nachvollziehen, falls du noch nicht auf den Bestätigungslink in deiner E-Mail geklickt hast (dann klappt aktuell nur Lesen, nicht Schreiben) oder falls du später einen neuen Bucket willst.

Ohne einen bestätigten Bucket läuft die Bestenliste lokal (jeder Browser sieht nur seine eigenen Einträge). Mit einem bestätigten Bucket sehen alle Besucher der Seite dieselbe Bestenliste.

1. Geh auf [kvdb.io](https://kvdb.io) und erstelle einen Bucket mit deiner **echten** E-Mail-Adresse (kvdb.io lehnt Wegwerf-/Test-Adressen mittlerweile ab). Kein Passwort nötig.
2. Du bekommst eine Bucket-ID zurück (ein String wie `NdexL1i6uf6eoVvFJYPmhx`) — kopier sie dir.
3. Check dein Postfach: kvdb.io schickt einen Bestätigungslink. **Ohne Klick auf den Link funktioniert nur Lesen, nicht Schreiben** — die Bestenliste würde dann nur leer angezeigt, aber niemand könnte Einträge hinzufügen.
4. Öffne `index.html` in einem Texteditor, such nach dieser Zeile (ganz am Anfang des `<script>`-Blocks):
   ```js
   const KVDB_BUCKET = 'YOUR_BUCKET_ID';
   ```
   Ersetze `YOUR_BUCKET_ID` durch deine Bucket-ID, z. B.:
   ```js
   const KVDB_BUCKET = 'NdexL1i6uf6eoVvFJYPmhx';
   ```
5. Lade die geänderte `index.html` wieder auf GitHub hoch (überschreibt die alte Version). Nach ein paar Sekunden ist die Bestenliste live.

**Wichtig zu wissen:**
- kvdb.io ist ein kostenloser Drittanbieter-Dienst ohne Zugriffskontrolle auf Bucket-Ebene — jeder, der die Bucket-ID kennt (z. B. durch Ansehen deines Quellcodes), kann theoretisch Einträge schreiben oder löschen. Für eine Spaß-Bestenliste unter Freunden ist das ok, für irgendetwas Wichtiges wäre es das nicht.
- Es gibt kein Anti-Cheat — jeder kann sich selbst eine beliebige Zeit/einen beliebigen Score eintragen (die Werte werden clientseitig berechnet und dann einfach hochgeladen).
- Bei zwei gleichzeitigen Einsendungen kann in seltenen Fällen ein Eintrag verloren gehen (kein Datenbank-Lock, nur "lesen → ändern → schreiben"). Für ein Hobby-Projekt ist das ein akzeptabler Kompromiss.
- Willst du die Bestenliste zurücksetzen: einfach in kvdb.io die Keys `leaderboard_story` und `leaderboard_endless` in deinem Bucket löschen oder überschreiben.

## Admin-Cheat-Code

Im Spiel gibt es einen 🔑-Button, der jedes aktuelle Level sofort freischaltet — auch das Finale. Das Passwort ist das, was du mir genannt hast. Es ist im Code als SHA-256-Hash hinterlegt statt im Klartext, aber das ist **keine echte Absicherung**: In einer statischen HTML-Datei kann niemand ein echtes Geheimnis vor jemandem verstecken, der sich die Devtools ansieht. Reicht für einen Insider-Spaß unter Freunden, nicht für irgendetwas, das wirklich geschützt sein muss.
