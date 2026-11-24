# Blinder Spieltest-Bericht: COMMIT//24 (AGY)

- **Getesteter Stand:** Branch `claude/kind-maxwell-2zf1iq` (PR #1)
- **Testbedingungen:** 100 % blind gespielt im Browser (`http://127.0.0.1:8024`, frisches Chrome-Profil / leeres `localStorage`). `index.html`, Quelltext und DevTools wurden zu keinem Zeitpunkt gelesen oder genutzt.
- **Ergebnis:** **5/5 Siegel gebrochen** · **`hint`-Befehl: 0× genutzt** · Bonus-Funde: **Brunnen-Easter-Egg (`[Brunnenfreund]`)** & **`.schulhof`-Teaser** nach Siegel V.
- **Hinweis zur Testmethode:** Da der Test im Headless-Browser rein visuell (Screenshots + Maus/Tastatur) lief, konnte der Web-Audio-Ton beim Morse-Cursor nicht akustisch geprüft werden; alle visuellen/zeitlichen Signale (Neustart nach Klick, halbes Tempo nach Doppelklick, Farbwechsel, Pausenlängen) wurden direkt am gerenderten Bild gemessen.

---

## 1. Lösungsweg pro Siegel (Zeit, Fehlversuche & Schwierigkeit)

| Siegel | Rätsel / Kernmechanik | Aktive Zeit | Fehlversuche | `hint` | Schwierigkeit (1–5) |
|---|---|---:|---:|---:|---:|
| **Siegel I** | Die schräge Tür (`//` im Logo) | ~2 Min. | 3 | 0 | **2,5 / 5** |
| **Siegel II** | Versteckter Commit (`git branch -a` / `git log --all`) | ~1 Min. | 0 | 0 | **2,0 / 5** |
| **Siegel III** | Vigenère-Twist (`PHOENIX` → Irrlicht/Wald → `PHÖNIX`) | ~5 Min. | 2 (+ Sumpf-Abstecher) | 0 | **4,0 / 5** |
| **Siegel IV** | Morse-Cursor im Header (`FUNKE`) | ~4 Min. | 0 (1 Umweg über Footer) | 0 | **3,5 / 5** |
| **Siegel V** | Binäre Buch-Chiffre im Manifest + Schloss-Commit (`AUSASCHEWIRDLICHT`) | ~2,5 Min. | 0 | 0 | **3,5 / 5** |
| **Gesamt** | **Alle 5 Siegel + Brunnen-Easter-Egg** | **~14,5 Min.** | **5** | **0** | **Ø 3,1 / 5** |

### Detaillierter Lösungsweg

#### Siegel I – Die schräge Tür
1. Auf der Landingpage den Footer entdeckt: *„Fünf Siegel. Eine Tür. Sie steht schräg.“* → sofort auf das gelbe `//` im Logo `COMMIT//24` geschlossen.
2. **Fehlversuche (3):**
   - 1 Klick auf `//` → keine Reaktion.
   - 3 schnelle Klicks auf `//` → keine Reaktion.
   - 5 schnelle Klicks auf `//` → keine Reaktion (erst **7 schnelle Klicks** öffneten das Terminal und brachen Siegel I!).
3. Sicherheitsabfrage *„Are you Bo(o)t? [ja/nein]“* mit `nein` beantwortet → Zugang zur Shell.

#### Siegel II – Der versteckte Commit
1. `help` und `ls -la` ausgeführt → versteckte Datei `.siegel` gefunden.
2. `cat .siegel`: *„Ein Commit wurde versteckt. Nur wer ALLES sehen will, findet ihn.“*
3. `git branch -a` zeigte `refs/geheim/asche`, `refs/geheim/nebel`, `refs/geheim/echo` samt perfektem Nudge (*„Zweige allein sind nur Namen. Ihre Commits zeigt dir ein anderer Befehl.“*).
4. `git log --all` zeigte alle drei geheimen Commits → **Siegel II bricht** (0 Fehlversuche).

#### Siegel III – Phönix, Sumpf, Verlorener Wald & Umlaute
1. `refs/geheim/asche` (`3301c0de`): `SLJPC OHVJF XOXJZ GULGZ W` (*„Schlüssel: der Vogel, der aus der Asche steigt.“*). Mit `vig PHOENIX ...` entschlüsselt → `FOLGEDEMIRRLICHTINDENWALD`.
2. Parallel `refs/geheim/nebel` (`0b5e55ed`, ROT13 `HAYBPX FHZCS` → `unlock sumpf`) probiert → in der herrlichen Endlos-Irrlicht-Falle gelandet (6 Fragen gelöst, inklusive *„Du bist fast da.“* → durchschaut und mit `raus` verlassen).
3. **Fehlversuch 1:** `unlock wald` → hilfreicher Nudge: *„Der Wald antwortet nicht. Aber etwas darin leuchtet.“*
4. `unlock irrlicht` → **Verlorener Wald** betreten. Rinden-Gedicht direkt ohne Raten abgeleitet:
   - *„wo der Polarstern wacht“* → `geh nord`
   - *„dem Abend nach“* → `geh west`
   - *„der Mittagssonne entgegen“* → `geh sued`
   - *„noch einmal dem Abend nach“* → `geh west`
5. Auf der Lichtung: *„Hier draußen kennt niemand Umlaute. Daheim schon.“*
6. **Fehlversuch 2:** `unlock phönix` → klarer Nudge: *„Ein Vogel ist ein Schlüssel, kein Wort.“*
7. `vig PHÖNIX SLJPC OHVJF XOXJZ GULGZ W` → `DERCURSORSPRICHTMORSE` → **Siegel III bricht**.

#### Siegel IV – Der Morse-Cursor
1. Kurzer Umweg: Vor Siegel III blinkte der Header-Cursor noch gleichmäßig (650 ms Takt), weshalb kurz der 5-Siegel-Fortschrittsbalken im Footer verdächtigt wurde.
2. Nach Siegel III sendete der gelbe Header-Cursor neben `COMMIT//24` Morsecode.
3. Einzelklick startete sauber von vorn; visuell abgelesen: `..-.` (F) · `..-` (U) · `-.` (N) · `-.-` (K) · `.` (E) = **`FUNKE`** (anschließend ca. 2,2 s Pause bis zur Wiederholung).
4. Doppelklick getestet → Toast `Halbes Tempo`, Cursor färbte sich orange-rot (`#ff5a36`) und verdoppelte die Signalzeiten (dit ~400 ms, dah ~1250 ms).
5. `unlock funke` → **Siegel IV bricht** (0 Fehlversuche).

#### Siegel V – Buch-Chiffre & Finale
1. Hinweis nach Siegel IV: *„Seit dem Funken sieht man mehr.“*
2. `cat .siegel` zeigte die neue Aufgabe: *„Das Manifest ist ein Buch. Satz.Wort, aber nicht in deiner Zahlenwelt: 111.1 · 111.10 · 110.11 · 110.100“*.
3. Binär in Dezimal umgerechnet (`7.1 · 7.2 · 6.3 · 6.4`) und über `cat -n manifest.txt` abgelesen → `WER BRENNT WIRD GESEHEN`.
4. `git log --all` enthüllte den neuen Commit `0000f1a7 (refs/geheim/funke)`: `WYJBJ GURPE ZUOOG ZX` (*„Das Schloss. Der Schlüssel steht im Buch.“*).
5. `vig WERBRENNTWIRDGESEHEN WYJBJ GURPE ZUOOG ZX` → `AUSASCHEWIRDLICHT`.
6. `unlock ausaschewirdlicht` → **Siegel V bricht**, Finale startet (0 Fehlversuche).

#### Bonus: Brunnen & Schulhof
- **`refs/geheim/echo` (`5eaf00d5`, Base64 → `unlock brunnen`):** Rückwärts-Echo erkannt → Palindrom `anna` gerufen → *„Endlich jemand, der vorwärts wie rückwärts spricht.“* + gelber Toast `Der Brunnen kennt dich jetzt` + `[Brunnenfreund]` in `whoami`.
- **Nach Siegel V:** `cat .siegel` deutete auf mehr hin → `ls -la` zeigte `.schulhof` mit einem großartigen Pokémon-Rot/Blau-MissingNo-Gerücht als Post-Game-Teaser.

---

## 2. Prüfung der 7 gezielten Änderungen

1. **Siegel II ohne `--all` in `help` (`git branch -a` / `git log --all`):**
   **Genau richtig.** Für Entwickler (die Zielgruppe eines GitHub-Hackathons) ist *„Nur wer ALLES sehen will“* in `.siegel` eindeutig genug, und der Zusatz-Nudge bei `git branch -a` fängt alle auf, die erst Branches prüfen. `--all` in `help` aufzuführen hätte das Rätsel entwertet.

2. **Terminal schließen & wieder öffnen (`>_`, `` ` ``, `Strg+K`, 1 Klick auf `//`):**
   **Sehr intuitiv.** Alle vier Methoden funktionieren zuverlässig und fokussieren direkt das Eingabefeld. Besonders stark ist der Toast beim ersten Schließen (`Zurück ins Terminal: >_ unten rechts, ` oder Strg+K`). Kleiner DACH-Haken: Auf deutschen QWERTZ-Tastaturen ist `` ` `` eine Shift+Tottaste und weniger angenehm als `Strg+K` oder `>_`.

3. **Morse-Cursor (Klick = Neustart mit Ton, Doppelklick = halbes Tempo, Wortanfang):**
   - **Technisch stark:** Einzelklick startet zuverlässig am Wortanfang (`F` nach ~330 ms), Doppelklick halbiert das Tempo sauber inkl. Toast `Halbes Tempo` und Farbwechsel auf Orange-Rot.
   - **Wortanfang:** Durch die ~2,2-Sekunden-Pause nach dem letzten Buchstaben (`E`) erkennbar, könnte aber visuell noch einen Tick klarer sein (z. B. 2,8–3,0 s Pause oder ein kurzer Start-Impuls in anderer Farbe), weil `E` (`.`) + `F` (`..-.`) sonst beim rein visuellen Zählen leicht ineinander verschwimmen, wenn man mitten im Loop einsteigt.
   - **Entdeckbarkeit:** Ohne den Testauftrag wüsste ein Spieler kaum, dass der Cursor klick- und doppelklickbar ist – bei Einzelklick erscheint kein Toast und es gibt keinen Hinweis im Terminal nach Siegel III.

4. **Verlorener Wald (Rinden-Gedicht):**
   **100 % ohne Raten ableitbar.** Polarstern (Nord), Abend (West), Mittagssonne (Süd), Abend (West) sind klar, eindeutig und atmosphärisch. Die Rückmeldungen nach jedem Schritt (*„Das Licht flackert vor dir.“*, *„Es wird heller...“*, *„Du hörst es leise lachen.“*) geben perfektes Pacing.

5. **Brunnen (Easter Egg):**
   **Wenn ausgelöst, fantastisch** – Toast `Der Brunnen kennt dich jetzt`, Flüstertext und `[Brunnenfreund]` in `whoami` fallen sofort auf und belohnen Neugier. **Vor dem Auslösen** ist es allerdings leicht zu übersehen: Wer nur „Hallo“ ruft (`... ollaH ...`), hält den Brunnen schnell für eine reine Scherz-Sackgasse und geht wieder raus. Außerdem gibt es einen konkreten Bug mit durchschlagenden Shell-Befehlen (siehe unten).

6. **Finale nach Siegel V:**
   **Fühlt sich wirklich wie ein runder Abschluss an.** Prompt-Wechsel auf `eingeweiht@commit24`, das Banner `✦ ✦ ✦ ✦ ✦ Fünf Siegel. Die Tür ist offen. ✦ ✦ ✦ ✦ ✦`, der direkte Button `→ Zur Anmeldung` (schließt Terminal, scrollt hin, fokussiert das GitHub-Feld), der individuelle Code `C24-...-ASH`, das goldene Badge `Eingeweiht ✦ 5/5 Siegel` und der voll gefüllte Footer-Balken greifen nahtlos ineinander.

7. **Landingpage (Echtes Event? Was fehlt?):**
   **Optisch und sprachlich schon sehr überzeugend** – mutige Typografie, klare Struktur, griffiges Manifest, originelle PR-Anmeldung. Für ein echtes DACH-Event fehlen jedoch noch Vertrauensanker (Kontakt/Impressum/Veranstalter-Kontext trotz mysteriösem Stil), ein funktionierender Discord-/Repo-Link (bzw. sauberer „Bald offen“-Zustand statt totes Gefühl bei „Auf GitHub einreichen“), ein kurzes FAQ (insbesondere Team-Anmeldung & Preise) sowie ein ausbalanciertes Kategorien-Grid.

---

## 3. Was stark ist (unbedingt behalten)

- **Der `PHOENIX` vs. `PHÖNIX`-Twist in Siegel III:** Dass derselbe Geheimtext mit zwei Schreibweisen zwei völlig verschiedene, sinnvolle deutsche Botschaften ergibt (`FOLGEDEMIRRLICHTINDENWALD` vs. `DERCURSORSPRICHTMORSE`), ist Rätseldesign auf Top-Niveau.
- **Faire, atmosphärische Nudges statt Frust:** `unlock wald` (*„etwas darin leuchtet“*), `unlock phönix` (*„ein Schlüssel, kein Wort“*) und `git branch -a` (*„ein anderer Befehl“*) lenken Spieler sanft, ohne zu spoilern.
- **Der Sumpf als spielbare Irrlicht-Falle:** Die generierten Mini-Aufgaben mit *„Du bist fast da.“* als unendliche Nebelkerze sind witzig und thematisch perfekt.
- **Dynamische Welt:** `.siegel` aktualisiert sich nach Fortschritt, `git log --all` wächst nach Siegel IV um `refs/geheim/funke`, der Footer-Balken füllt sich pro Siegel, `whoami` zeigt Titel und `siegel: X/5` in der Anmeldedatei aktualisiert sich live.
- **Visuelle Identität & Flow ins Finale:** Der `→ Zur Anmeldung`-Button mit Auto-Scroll und Auto-Fokus auf das GitHub-Feld rundet das Erlebnis extrem sauber ab.

---

## 4. Probleme nach Priorität (je mit konkretem Fix)

### Priorität: HOCH

1. **Siegel I: 7 Klicks auf `//` ohne jegliches Zwischen-Feedback wirken kaputt**
   - **Beobachtung:** 1, 3 und selbst 5 schnelle Klicks auf `//` zeigen keinerlei Reaktion. Erst bei 7 schnellen Klicks öffnet sich das Terminal. Viele echte Spieler werden nach 1–3 Klicks glauben, `//` sei gar nicht interaktiv.
   - **Fix:** Entweder auf **3 Klicks** reduzieren oder ab Klick 1 deutliches Mikro-Feedback geben (z. B. `//` kippt/glüht bei jedem Klick stärker, leichter „Klopf“-Effekt oder Sub-Glitch), damit sofort klar ist: *„Da reagiert etwas – weiterklicken!“*

2. **Landingpage: Fehlende Vertrauensanker & FAQ für ein echtes DACH-Event**
   - **Beobachtung:** *„Absender unbekannt. Wer wir sind, spielt keine Rolle.“* passt toll zum ARG, lässt aber reale Entwickler und Sponsoren zweifeln, ob der Hackathon echt ist. Zudem fehlen Antworten auf offensichtliche Fragen (Team-PR-Ablauf, Kontaktadresse, rechtliches Impressum/Datenschutz für DE/AT/CH, konkreter Status des Discord-/Repo-Links).
   - **Fix:** Eine kompakte Sektion `05 FAQ` (4–5 Punkte: Team-Anmeldung, Preise/Sponsoren, Zeitzone/Abgabe, Kontakt) sowie dezente Footer-Links für `Kontakt / Impressum / GitHub` ergänzen – der anonyme Stil lässt sich im Manifest trotzdem behalten.

### Priorität: MITTEL

3. **Bug im Brunnen-Modus: Normale Shell-Befehle (`Hilfe`, `Echo`) brechen durch**
   - **Beobachtung:** Im Brunnen (`unlock brunnen`) rief die Eingabe `Hilfe` das normale `Befehle:`-Menü auf und `Echo` erzeugte eine leere Ausgabe, statt `... efliH ...` bzw. `... ohcE ...` zurückzuwerfen.
   - **Fix:** Im Brunnen-Handler ausschließlich `raus` (und optional `clear`/`exit`) abfangen und **alle anderen Eingaben** vor dem regulären Command-Dispatcher als Brunnen-Echo verarbeiten.

4. **Morse-Cursor: Klick- & Doppelklick-Bedienung ist kaum entdeckbar**
   - **Beobachtung:** Bei Einzelklick auf den Morse-Cursor erscheint kein Toast/Hinweis, und im Terminal nach Siegel III steht nur `DERCURSORSPRICHTMORSE`. Spieler entdecken die super hilfreichen Komfortfunktionen (Klick = Neustart mit Ton, Doppelklick = halbes Tempo) nur durch Zufall.
   - **Fix:** Dem Morse-Cursor ab Siegel III `cursor: pointer` + `title="Klick: Neustart mit Ton · Doppelklick: halbes Tempo"` geben und beim ersten Einzelklick einen kurzen Toast einblenden (z. B. `Morse-Neustart · Doppelklick = halbes Tempo`). Außerdem nach dem Lösen von Siegel IV/V den orange-roten Halbtempo-Status wieder zurücksetzen.

5. **Kategorien-Grid auf Desktop unausgewogen (4 + 1 verwaiste Karte)**
   - **Beobachtung:** Bei 1440 px Breite stehen 4 Kategorien in Zeile 1 und „Publikumspreis“ allein ganz links in Zeile 2 mit 3 leeren Spalten rechts daneben.
   - **Fix:** Entweder ein **3 + 2 zentriertes Grid** (oder 5 Spalten auf Wide-Desktop / „Publikumspreis“ als breite Karte über 2–4 Spalten oberhalb von „Gesamtsieg“) verwenden.

6. **Klick in den Terminal-Ausgabebereich nimmt dem Eingabefeld den Fokus**
   - **Beobachtung:** Klickt man irgendwo in den schwarzen Terminal-Body, verliert die Prompt-Zeile unten den Fokus und anschließendes Tippen geht ins Leere.
   - **Fix:** Ein `click`-Listener auf dem Terminal-Container, der das Input-Feld automatisch fokussiert (außer wenn der Nutzer gerade Text mit `window.getSelection().toString()` markiert hat).

### Priorität: NIEDRIG

7. **Brunnen-Easter-Egg vor dem Auslösen etwas zu versteckt**
   - **Beobachtung:** Ohne einen winzigen Hinweis verlassen die meisten Spieler den Brunnen nach 1–2 normalen Rufen, ohne je ein Palindrom auszuprobieren.
   - **Fix:** Nach 2–3 normalen Echos eine dezente Zusatzzeile flüstern lassen, z. B. *„... alles kommt verkehrt zurück. Gibt es nichts, das gleich bleibt?“*

8. **QWERTZ-Tastatur-Hinweis für `` ` `` im Reopen-Toast**
   - **Beobachtung:** Der Toast nennt `` ` `` neben `>_` und `Strg+K`. Auf deutschen Tastaturen erfordert `` ` `` Shift + Tottaste.
   - **Fix:** Im Toast `Strg+K` vor `` ` `` stellen (oder zusätzlich `^` / `e.code === 'Backquote'` ohne Shift unterstützen, falls noch nicht geschehen).

9. **Hex-Hinweis `51 75 65 6c 6c 74 65 78 74` („Quelltext“) im Footer prüfen**
   - **Beobachtung:** Der Hex-String im Footer dekodiert zu `Quelltext`, spielte aber im regulären 5-Siegel-Spieltest (ohne View-Source) keine Rolle und könnte Event-Besucher oder Rätselspieler auf die falsche Fährte (DevTools statt Terminal) locken.
   - **Fix:** Entweder bewusst als kleinen Extra-Gag im HTML-Kommentar verknüpfen oder – falls das Spiel komplett ohne View-Source spielbar sein soll – durch einen konsistenten In-World-Hinweis ersetzen.

10. **Anmeldeformular: `BITTE-WÄHLEN` und Doppelklick-Markierung**
    - **Beobachtung:** Solange kein Land gewählt ist, lässt sich eine Datei mit `land: BITTE-WÄHLEN` kopieren. Außerdem markierte ein Doppelklick auf den Footer-Balken versehentlich Text im Footer (`51`).
    - **Fix:** Beim Klick auf „Datei kopieren“ kurz prüfen, ob GitHub-Name und Land gesetzt sind (sonst sanften Hinweis anzeigen); `user-select: none` auf Cursor und Fortschrittsbalken setzen.

11. **Schnelle Eingaben direkt nach `unlock` / Brunnen-Echo**
    - **Beobachtung:** Werden zwei Befehle sehr schnell hintereinander abgeschickt, kann ein `Enter` kurzzeitig ignoriert werden und die Zeichen landen in der nächsten Eingabezeile.
    - **Fix:** Sicherstellen, dass das Eingabefeld während kurzer Ausgabeverzögerungen nicht still `Enter` verwirft (oder Eingaben sauber in einer Queue abarbeiten).

---

## 5. Top 3 Upgrades für die nächste Runde

1. **Spürbares Klopf-Feedback auf `//` (Siegel I) + Klick-Affordance am Morse-Cursor (Siegel IV):**
   Mikro-Feedback bei jedem Klick auf `//` (oder Absenkung auf 3 Klicks) und ein Tooltip + Erstklick-Toast am Morse-Cursor eliminieren die beiden größten vermeidbaren Abbruchstellen im Rätselverlauf.
2. **Event-Reife der Landingpage schärfen:**
   Ausbalanciertes Kategorien-Grid (3+2 oder Full-Width-Publikumspreis), kompaktes FAQ (Team-PRs, Preise, Zeitplan) sowie klare Kontakt-/Impressums-Infos machen aus einer starken Teaser-Seite eine rundum glaubwürdige DACH-Hackathon-Seite.
3. **Finales Highlight & Brunnen-Feinschliff:**
   Ein kurzer visueller „Funke/Asche-zu-Licht“-Moment auf dem Hintergrund-Canvas beim Brechen von Siegel V, eine klare Erklärung, was das `Eingeweiht`-Badge/der Code auf der Startliste bringt, sowie der Bugfix im Brunnen-Command-Routing heben das ohnehin schon großartige Finale auf 10/10.
