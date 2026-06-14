# Lauf-Rechner

Ein kleiner, eigenständiger Rechner für Lauf-Tempo: Distanz, Geschwindigkeit
(km/h), Pace (min/km) und Gesamtzeit lassen sich beliebig editieren – die
jeweils anderen Werte werden sofort neu berechnet. Die gesamte App steckt in
einer einzigen `index.html`-Datei, läuft offline und lässt sich auf Android
als App installieren.

## Funktionsweise

Die App rechnet mit zwei "Basiswerten":

- **Distanz** (km)
- **Geschwindigkeit** (km/h)

Daraus werden laufend abgeleitet:

- **Pace** = 60 / Geschwindigkeit (min/km)
- **Gesamtzeit** = Distanz / Geschwindigkeit

Wird **Pace** oder **Gesamtzeit** geändert, rechnet die App rückwärts und
passt die Geschwindigkeit an – die Distanz bleibt dabei immer unverändert.
Wird die **Distanz** geändert, bleibt die Geschwindigkeit gleich und nur
Pace/Gesamtzeit passen sich an. So ergibt sich ein konsistentes Vier-Felder-
System ohne widersprüchliche Werte.

## Eingabefelder

Jedes Zahlenfeld merkt sich während des Tippens einen eigenen Text-Zustand:

- Felder lassen sich vollständig leeren (kein "klebendes" `0`).
- Komma oder Punkt werden als Dezimaltrennzeichen akzeptiert.
- Pace-Sekunden und alle Zeit-Komponenten (Min/Sek) sind auf 0–59 begrenzt.
- Erst beim Verlassen des Feldes (Blur) wird der angezeigte Text wieder mit
  dem berechneten Wert synchronisiert.

Die Logik dafür steckt in der kleinen Hilfsfunktion `bindEditable(...)` im
`<script>`-Teil der `index.html`.

## Aufbau der Datei

Alles befindet sich in **einer** Datei (`index.html`):

- **HTML/CSS** – dunkles Layout im "Tartanbahn"-Look (Karte mit orangem
  Streifen, Monospace-Zahlen, dezente Trennlinien).
- **Vanilla JavaScript** – keine externen Bibliotheken, kein Build-Schritt.
- **Web-App-Manifest** – als Base64-codiertes JSON direkt im `<head>`
  eingebunden (`name`, `display: standalone`, Farben, Icons).
- **App-Icon** – ein Stoppuhr-Symbol, als PNG (192 px & 512 px) generiert und
  ebenfalls als Base64 eingebettet (Favicon, Apple-Touch-Icon und
  Manifest-Icon).

## Installation als App (Android)

1. `index.html` per HTTPS hosten (z. B. Netlify, GitHub Pages o. Ä.) – ohne
   Hosting funktioniert die Seite zwar auch über `file://`, aber der
   Installations-Banner erscheint bei manchen Android-Versionen nur über eine
   echte HTTPS-Quelle.
2. Seite in Chrome öffnen.
3. Über das Menü **„App installieren“** bzw. **„Zum Startbildschirm
   hinzufügen“** wählen.
4. Die App startet danach im Standalone-Modus (ohne Browser-Leiste) mit dem
   Stoppuhr-Icon auf dem Homescreen.

## Anpassen

- **Startwerte**: `let distance = 5;` und `let speed = 10;` im `<script>`.
- **Farben/Design**: Variablen direkt im `<style>`-Block (z. B. `#f97316` für
  den orangen Akzent, `#0a0a0a`/`#171717` für die Hintergründe).
- **Icon**: lässt sich durch ein eigenes PNG ersetzen, indem die
  Base64-Strings in den `<link>`-Tags und im Manifest ausgetauscht werden.
