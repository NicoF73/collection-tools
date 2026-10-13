# XPath-Chat Anleitung

Gilt für XPath-Chat und für XPath-GH-Chat. Was nur XPath-GH-Chat kann, ist mit GH markiert.

[a) Funktionen](#a) [b) Import und Export](#b) [c) GitHub einrichten](#c)

## a) Funktionen

Links stellst du XPath-Fragen an die geladene XML-Datei, rechts siehst du ihren Inhalt.

### Chat links

- Gib einen XPath-1.0-Ausdruck ein, zum Beispiel `//item[@id='1']` oder `count(//item)`, und bestätige mit Enter. Die Antwort erscheint als Chat-Nachricht.
- Knoten werden mit Trefferzahl und nummeriert ausgegeben. Zahlen, Text und Wahrheitswerte kommen direkt.
- Antworten mit mehr als 10 Zeilen sind zugeklappt. „Aufklappen“ zeigt alles.
- Frühere Eingaben holst du mit ↑ und ↓ zurück. Tab vervollständigt aus der History, weiteres Tab springt zum nächsten Treffer. Die History bleibt im Browser gespeichert.
- **Chat leeren** löscht nur die Nachrichten.

### Bereich rechts

| Tab | Inhalt |
| --- | --- |
| XML | Der geladene Text, bei mehreren Dateien das zusammengeführte XML. |
| Struktur | Alle Pfade von Elementen, die Kindelemente haben, in der Form `name1.name2.name3`. Ohne Wurzelelement, alphabetisch, ohne Doppelte. |
| XML-Elemente | Zwei Listen nebeneinander mit der Anzahl je Name: **Nodes** (haben Kinder) und **Leafes** (haben keine). |
| GitHub GH | Einstellungen für die Verbindung zu GitHub (siehe c). |

**Clear** (rechts oben) entfernt nur das geladene XML. Der Chat bleibt erhalten.

## b) Import und Export

### XML importieren

- **Import** öffnet die Dateiauswahl. Du kannst mehrere Dateien auf einmal wählen.
- Alternativ ziehst du die Dateien per Drag & Drop irgendwo auf die Seite.
- Haben alle Dateien dasselbe Wurzelelement, werden sie verkettet: Die Kinder der weiteren Dateien kommen unter das Wurzelelement der ersten.
- Dateien mit anderem Wurzelelement oder mit XML-Fehlern überspringt die App und meldet sie einzeln im Chat.
- Jeder Import ersetzt das bisher geladene XML.

### Chat exportieren

- **Als .md exportieren** bietet den Chat als Markdown-Datei zum Speichern an, Name `xpath-chat-JJJJ-MM-TT-HHMM.md`. Du musst den Speichern-Dialog bestätigen.
- Darin stehen deine Eingaben und die vollständigen Antworten, auch die zugeklappten.

### Über GitHub GH

Beide Schaltflächen sind erst nach erfolgreicher Verbindung aktiv (siehe c).

- **GitHub-Import** zeigt die .xml-Dateien deines Ordners im Repository. Wähle eine oder mehrere und klicke „Laden“. Danach läuft alles wie beim normalen Import.
- **Nach GitHub** speichert den Chat als .md im selben Ordner. Du gibst den Dateinamen an. Gibt es die Datei schon, musst du „Überschreiben“ bestätigen. Jeder Export ist ein Commit, ältere Stände bleiben in GitHub abrufbar.

## c) GitHub einrichten GH

### 1. Repository anlegen

1. Melde dich bei github.com an. Klicke oben rechts auf **+** und dann auf **New repository**.
2. Gib einen Namen ein, zum Beispiel `xpath-daten`.
3. Stelle die Sichtbarkeit auf **Private**. In einem öffentlichen Repository könnte jeder deine Dateien lesen.
4. Setze das Häkchen bei **Add a README file**, damit das Repository schon einen Branch hat. Klicke auf **Create repository**.
5. Optional für den Import: Klicke auf **Add file**, dann **Create new file**. Als Name gibst du `xpath-chat/beispiel.xml` ein. Der Schrägstrich legt den Ordner an. Füge XML ein und klicke **Commit changes**. Ohne diesen Schritt entsteht der Ordner beim ersten Export.

### 2. Zugriffstoken erzeugen

1. Klicke auf dein Profilbild, dann **Settings**, dann ganz unten links **Developer settings**.
2. Wähle **Personal access tokens**, dann **Fine-grained tokens**, dann **Generate new token**.
3. Gib einen Namen ein, zum Beispiel `xpath-chat`, und wähle ein Ablaufdatum (zum Beispiel 90 Tage).
4. Bei **Repository access** wählst du **Only select repositories** und dort nur dein Datenrepository.
5. Öffne **Repository permissions** und stelle **Contents** auf **Read and write**. Mehr ist nicht nötig.
6. Klicke auf **Generate token** und kopiere das Token (beginnt mit `github_pat_`). GitHub zeigt es nur einmal.

### 3. Verbindung in der App

1. Öffne in XPath-GH-Chat rechts den Tab **GitHub**.
2. Trage ein:

   | Feld | Eintrag |
   | --- | --- |
   | Repository | `deinname/xpath-daten` |
   | Ordner | `xpath-chat` (leer lassen für die Hauptebene) |
   | Branch | leer lassen für den Standard-Branch |
   | Zugriffstoken | das kopierte Token |
3. Setze das Häkchen **Token in diesem Browser merken**, wenn du es nicht jedes Mal neu eingeben willst. Ohne Häkchen gilt es nur für diesen Tab.
4. Klicke **Speichern und testen**. Bei Erfolg steht dort „Verbunden mit … (privat)“, und **GitHub-Import** und **Nach GitHub** werden aktiv.

### Wenn etwas nicht klappt

| Meldung | Ursache und Lösung |
| --- | --- |
| Token ungültig oder abgelaufen | Token falsch kopiert oder abgelaufen. Neues Token erzeugen. |
| Nicht gefunden | Repository, Ordner oder Branch falsch geschrieben, oder das Token hat keinen Zugriff auf dieses Repository. |
| Zugriff verweigert | Dem Token fehlt „Contents: Read and write“, oder ein Limit ist erreicht. |
| Konflikt beim Schreiben | Die Datei wurde zwischenzeitlich geändert. Export noch einmal ausführen. |
| GitHub nicht erreichbar | Die Umgebung blockiert Verbindungen zu api.github.com. Öffne die App stattdessen als lokale HTML-Datei oder hoste sie auf GitHub Pages (siehe unten). |

**Sicherheit:** Behandle das Token wie ein Passwort. Merke es nicht auf einem geteilten Rechner. Ein verlorenes Token widerrufst du unter Settings, Developer settings, Fine-grained tokens.

### Optional: App auf GitHub Pages hosten

1. Lege ein zweites, öffentliches Repository für die Seite an, zum Beispiel `xpath-app`. Das Datenrepository bleibt privat.
2. Lade die Datei `xpath-gh-chat.html` hoch und nenne sie `index.html`.
3. Öffne **Settings**, dann **Pages**. Wähle bei **Source** **Deploy from a branch**, Branch `main`, Ordner `/ (root)`, und speichere.
4. Nach etwa einer Minute ist die App unter `https://deinname.github.io/xpath-app/` erreichbar. Token und Repository trägst du dort wie in Schritt 3 ein.