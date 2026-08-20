# Meine Funde — Linktree-Style Katalog + Admin

Statische Seite für GitHub Pages: öffentliche Übersichtsseite im Linktree-Stil
(`index.html`) und eine Adminseite (`admin.html`), über die du Produkte
hinzufügen, bearbeiten und löschen kannst — ohne eigenes Backend.

## 1. Repository einrichten

1. Neues **privates oder öffentliches** GitHub-Repository anlegen, z. B. `meine-funde`.
2. Inhalt dieses Ordners hochladen (per `git push` oder Drag&Drop im Browser).
3. In den Repo-Einstellungen: **Settings → Pages → Source: `main` / root** wählen.
4. Nach kurzer Zeit ist die Seite unter `https://<dein-username>.github.io/meine-funde/` erreichbar.

## 2. Wie die Adminseite geschützt ist — wichtig zu verstehen

GitHub Pages liefert **nur statische Dateien aus**, es gibt keinen Server, der
ein echtes Passwort prüfen könnte. Jeder JavaScript-Code auf einer
statischen Seite ist im Browser einsehbar — ein reines "Login mit Passwort"
wäre daher nur Show und keine echte Sicherheit.

**Deshalb funktioniert der Schutz hier anders, aber echt:**
Zum Speichern verlangt `admin.html` einen **GitHub Personal Access Token**
mit Schreibrecht auf dein Repository. Dieser Token ist der tatsächliche
Schlüssel — ohne ihn kann niemand `data/products.json` verändern, egal ob er
den Quellcode der Adminseite sieht oder nicht. Das ist dieselbe
Sicherheitsgrenze, die z. B. auch `git push` schützt.

**Token erstellen (dauert 1 Minute):**

1. GitHub → Einstellungen → **Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
2. **Repository access:** "Only select repositories" → dein Repo auswählen (NICHT alle Repos).
3. **Permissions → Contents:** auf **Read and write** stellen. Alle anderen Rechte auf "No access" lassen.
4. Ablaufdatum setzen (z. B. 90 Tage) und Token generieren.
5. Token kopieren und in `admin.html` einfügen. Er wird **nie** an einen anderen Server als `api.github.com` gesendet.

**Praktische Empfehlungen:**

- Verlinke `admin.html` nirgends öffentlich (kein Link auf `index.html`, keine Sitemap-Eintragung). Sie ist zusätzlich mit `<meta name="robots" content="noindex, nofollow">` markiert.
- Wenn du "Auf diesem Gerät merken" aktivierst, wird der Token unverschlüsselt im `localStorage` des Browsers abgelegt — nur an eigenen, vertrauenswürdigen Geräten aktivieren, niemals an öffentlichen PCs.
- Läuft der Token ab oder wird er kompromittiert: einfach in den GitHub-Einstellungen widerrufen und einen neuen erstellen.
- Für noch mehr Sicherheit: eigenes privates Repo für die Daten nutzen und die Pages-Seite in einem separaten öffentlichen Repo per Fetch auf eine Public-API/GitHub-Raw-URL zeigen lassen (fortgeschritten, optional).

## 3. Bedienung

- **`index.html`** liest `data/products.json` und zeigt jede Position als Karte im Linktree-Stil, inklusive Kategorie-Filter.
- **`admin.html`** liest denselben JSON direkt aus GitHub, lässt dich Einträge hinzufügen/bearbeiten/löschen und schreibt jede Änderung als eigenen **Commit** direkt in dein Repository zurück. GitHub Pages baut die Seite danach automatisch neu — Änderungen sind nach ca. 30–60 Sekunden live.

## 4. Struktur

```
index.html          Öffentliche Übersicht
admin.html           Geschützte Verwaltung
css/style.css        Gemeinsames Design
js/app.js             Logik der Übersichtsseite
js/admin.js           Logik der Adminseite (GitHub API)
data/products.json   Produktdaten (Quelle der Wahrheit)
```

## 5. Design

Editorial-Katalog-Look: tiefes Tinten-Navy, Messing-Gold-Akzente, Fraunces
(Serif) für Überschriften/Preise, Manrope für Fließtext. Jede Karte trägt
eine fortlaufende „FUND № …“-Nummer wie ein Katalogeintrag.
