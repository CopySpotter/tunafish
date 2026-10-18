# TunaFish

TunaFish ist ein kleines Programm, das Schachstellungen untersucht. Es ist eine **Schach-Analyse-Engine**: Es rechnet mögliche Züge durch und sucht den voraussichtlich besten Zug. TunaFish läuft auf dem eigenen Computer oder im Browser und braucht dafür keinen Schachserver.

> **Einfach erklärt:** TunaFish ist eine Rechenhilfe im Hintergrund. Es schaut sich das Brett an und sagt, welcher Zug vermutlich am besten ist.

Eine echte Schach-Analyse-Engine kennt alle erlaubten Züge, spielt viele Möglichkeiten im Hintergrund durch und nimmt an, dass der Gegner gut antwortet. Danach bewertet sie die entstehenden Stellungen und wählt den Zug mit dem besten Ergebnis. Sie arbeitet also nicht nur mit einzelnen Regeln wie „nimm eine angegriffene Figur“, sondern rechnet Zugfolgen voraus.

> **Einfach erklärt:** Die Engine denkt mehrere Züge voraus: „Wenn ich das tue, kann der Gegner so antworten – und dann passiert das.“ Deshalb kann sie Gefahren besser erkennen.

TunaFish wurde aus dem Projekt [Tonnetto](https://github.com/marcobuontempo/tonnetto) geforkt. Ein *Fork* ist eine eigene Kopie eines bestehenden offenen Programms. Diese Kopie wird nun unter dem Namen TunaFish unabhängig weiterentwickelt.

> **Einfach erklärt:** Wir nehmen ein vorhandenes, erlaubtes Programm als Anfang und bauen es nach unseren eigenen Zielen weiter aus.

Das Ziel ist nicht, eine Weltklasse-Engine wie Stockfish nachzubauen. TunaFish soll leicht, verständlich und schnell genug für eine Lernhilfe im Browser sein. Die Engine soll zum Beispiel eine Gefahr erkennen, nachdem eine Person einen Zug geplant hat.

> **Einfach erklärt:** TunaFish muss nicht die stärkste Schachmaschine der Welt sein. Es soll gut genug sein, um zu sagen: „Achtung, nach deinem Zug kann das passieren.“

## Wofür TunaFish verwendet wird

TunaFish soll später die freiwillige Analysehilfe **„Gefahrencheck“** in [Einfach-Schach](https://github.com/CopySpotter/Einfach-Schach) unterstützen. Einfach-Schach ist ein inklusives Schachprojekt: Es soll Menschen mit unterschiedlichen Vorkenntnissen einen verständlichen, selbstbestimmten Zugang zum Schach ermöglichen. Die Spielerin oder der Spieler plant zuerst selbst einen Zug. Erst wenn die Hilfe gewünscht wird, untersucht TunaFish die Stellung und sucht die stärkste Antwort der Gegenseite.

> **Einfach erklärt:** Zuerst denkt die Person selbst. Nur wenn sie auf Hilfe klickt, schaut TunaFish nach, was der Gegner danach machen könnte.

Der bisherige einfache Computergegner von Einfach-Schach bleibt erhalten und bleibt bewusst getrennt von TunaFish. Er ist eine einfache, teilweise zufällige Gegnerin für Anfänger:innen. TunaFish ist dagegen eine echte Analyse-Engine und soll nicht ungefragt die Partie übernehmen.

> **Einfach erklärt:** Es gibt zwei verschiedene Programme: Die freundliche, einfache Gegnerin zum Spielen – und TunaFish zum Nachschauen und Lernen.

Alle Berechnungen sollen lokal stattfinden. *Lokal* heißt: Die Stellung bleibt im Browser beziehungsweise auf dem Gerät der Person. Sie wird nicht an einen fremden Server geschickt und braucht keine Internetverbindung.

> **Einfach erklärt:** Das Schachbrett verlässt den Computer nicht. Auch ohne Internet soll die Hilfe funktionieren.

## Was das Programm technisch kann

TunaFish ist in **TypeScript** geschrieben. TypeScript ist eine Programmiersprache, die auf JavaScript aufbaut und zusätzliche Prüfungen für Programmierer:innen bietet. Beim Bauen wird daraus JavaScript. JavaScript ist die Sprache, die direkt im Browser ausgeführt wird.

> **Einfach erklärt:** TypeScript hilft beim sauberen Programmieren. Am Ende entsteht JavaScript, das Chrome, Firefox und andere Browser verstehen.

Das Schachbrett wird intern als **10×12-Mailbox** gespeichert. Das bedeutet nicht, dass das sichtbare Brett zehn mal zwölf Felder hat. Sichtbar bleiben die normalen 8×8 Felder. Rundherum speichert das Programm zusätzliche Randfelder, damit es beim Rechnen leichter erkennt, wann eine Figur nicht weiterziehen darf.

> **Einfach erklärt:** Im Hintergrund hat das Brett einen Sicherheitsrand. Dadurch kann das Programm einfacher merken: „Hier ist das Brett zu Ende.“

Die Daten liegen in einem Uint8Array. Das ist ein besonders platzsparender Speicherbereich in JavaScript. Er enthält nur kleine ganze Zahlen; damit kann TunaFish viele Brettfelder schnell verarbeiten.

> **Einfach erklärt:** Die Figuren werden als kurze Zahlen gespeichert. Das spart Platz und macht das Rechnen schneller.

TunaFish erzeugt alle legalen Züge. *Legal* heißt: Der Zug hält die Schachregeln ein und lässt den eigenen König nicht im Schach stehen. Rochade, en passant, Bauernumwandlung, Schach, Schachmatt und Patt sind im Kern vorhanden.

> **Einfach erklärt:** TunaFish kennt die wichtigen Schachregeln und schlägt nur Züge vor, die erlaubt sind.

## Wie TunaFish einen Zug auswählt

Zuerst erzeugt TunaFish die möglichen erlaubten Züge. Danach spielt das Programm diese Züge nur im Speicher probeweise weiter. Es schaut dabei abwechselnd auf den eigenen besten Zug und auf die beste Antwort des Gegners.

> **Einfach erklärt:** TunaFish verschiebt die Figuren nicht wirklich auf dem Bildschirm. Es spielt verschiedene Möglichkeiten heimlich im Kopf durch.

Das Suchverfahren heißt **Negamax**. Negamax ist eine vereinfachte Form von *Minimax*. Minimax bedeutet: „Ich suche den besten Zug für mich und nehme an, dass auch mein Gegner den besten Zug für sich sucht.“ Negamax verwendet dafür dieselbe Rechenregel für Weiß und Schwarz.

> **Einfach erklärt:** TunaFish denkt nicht nur: „Was ist für mich gut?“ Es denkt auch: „Wie kann der Gegner am besten antworten?“

Die Suche verwendet **Alpha-Beta-Schnitt**. Das ist ein Verfahren zum Weglassen unnötiger Varianten. Wenn TunaFish schon weiß, dass ein Weg schlechter als ein bereits gefundener Weg ist, muss es diesen Weg nicht vollständig weiterrechnen.

> **Einfach erklärt:** Das Programm spart Zeit. Es hört bei schlechten Möglichkeiten früher auf, statt jede Kleinigkeit bis zum Ende zu prüfen.

Bei unruhigen Stellungen rechnet TunaFish Schlagzüge noch etwas weiter. Das heißt **Quiescence Search**, auf Deutsch ungefähr „Suche nach einer ruhigen Stellung“. Dadurch soll die Engine nicht irrtümlich glauben, eine Figur sei sicher, obwohl sie im nächsten Zug geschlagen wird.

> **Einfach erklärt:** Wenn gerade Figuren geschlagen werden können, schaut TunaFish genauer hin. So übersieht es weniger einfache Taktiken.

Für die Bewertung verwendet TunaFish **PeSTO**-Tabellen. Das sind vorbereitete Werte für Figuren auf verschiedenen Feldern. Eine Dame ist zum Beispiel mehr wert als ein Bauer; zusätzlich können zentrale oder sichere Felder besser bewertet werden. Die Werte für Mittelspiel und Endspiel werden unterschiedlich gewichtet.

> **Einfach erklärt:** Das Programm weiß: Figuren haben verschiedene Werte, und manche Felder sind für sie besser als andere.

Schlagzüge werden zuerst betrachtet. Dafür wird **MVV-LVA** verwendet. Die Abkürzung bedeutet *Most Valuable Victim – Least Valuable Attacker*: Eine wertvolle gegnerische Figur mit einer weniger wertvollen eigenen Figur zu schlagen, wird zuerst untersucht.

> **Einfach erklärt:** Ein Zug wie „Bauer schlägt Dame“ wird besonders früh angesehen, weil er sehr wichtig sein könnte.

## Welche Informationen hinein- und herausgehen

TunaFish erhält eine Stellung als **FEN**. FEN bedeutet *Forsyth–Edwards Notation*. Das ist eine kurze Textzeile, die alle Figuren, die Seite am Zug, Rochaderechte, das en-passant-Feld und die Zugnummer beschreibt.

> **Einfach erklärt:** FEN ist ein kurzer Text, der ein ganzes Schachbrett beschreibt. Damit kann Einfach-Schach die Stellung an TunaFish übergeben.

Die öffentliche Programmschnittstelle heißt **API**. API bedeutet *Application Programming Interface*, auf Deutsch: eine festgelegte Verbindung zwischen Programmen. Die wichtigste Methode ist derzeit getBestMove(depth). Sie gibt den besten gefundenen Zug im Format wie e2e4 zurück.

> **Einfach erklärt:** Die API ist die vereinbarte Sprache zwischen TunaFish und Einfach-Schach. Einfach-Schach fragt zum Beispiel: „Was ist der beste Zug bei Tiefe 3?“

depth heißt Rechentiefe. Eine Tiefe von 3 bedeutet drei Halbzüge: eigener Zug, gegnerischer Zug, eigener Zug. Höhere Zahlen können bessere Ergebnisse bringen, brauchen aber deutlich mehr Rechenzeit.

> **Einfach erklärt:** Je weiter TunaFish vorausdenken soll, desto länger muss es rechnen.

~~~ts
import ChessEngineAPI from './dist/index.js';

const engine = new ChessEngineAPI({
  fen: 'rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1'
});

const besterZug = engine.getBestMove(3);
~~~

Das Beispiel erzeugt zuerst eine Engine mit einer Startstellung. Danach sucht getBestMove(3) einen guten Zug. Der Name ChessEngineAPI stammt noch aus dem übernommenen Programm und wird im Rahmen der Umbenennung zu TunaFish angepasst.

> **Einfach erklärt:** Das Beispiel fragt TunaFish: „Hier ist das Brett. Welchen Zug würdest du nach kurzem Nachdenken machen?“

Die FEN-Eingabe wird im Moment noch nicht vollständig geprüft. Deshalb darf Einfach-Schach nur Stellungen übergeben, die aus seiner eigenen, bereits geprüften Schachlogik kommen.

> **Einfach erklärt:** TunaFish vertraut derzeit darauf, dass die Brettbeschreibung richtig ist. Wir müssen später noch einen Sicherheitscheck dafür bauen.

Neben der Browser-Schnittstelle gibt es einen Ansatz für **UCI**. UCI bedeutet *Universal Chess Interface*. Das ist eine Standardsprache, über die Schachoberflächen mit einer Engine sprechen können.

> **Einfach erklärt:** UCI ist eine Art gemeinsame Sprache für Schachprogramme. Sie wäre später nützlich, damit TunaFish auch mit anderen Schachoberflächen reden kann.

## Was bereits gut ist

Die Zugerzeugung wurde mit **Perft**-Stellungen geprüft. Perft bedeutet *Performance Test* und zählt, wie viele erlaubte Zugfolgen es ab einer Stellung gibt. Wenn die Anzahl genau stimmt, ist das ein guter Hinweis, dass die Schachregeln und Sonderzüge korrekt umgesetzt sind.

> **Einfach erklärt:** Perft ist ein Regeltest. Das Programm zählt sehr viele mögliche Züge und vergleicht die Zahl mit bekannten richtigen Ergebnissen.

Schnelle Perft-Prüfungen bis Tiefe 3 wurden beim Start der Übernahme erfolgreich ausgeführt. Damit wurden drei bekannte Teststellungen überprüft: die Anfangsstellung, „Kiwi Pete“ und eine Talkchess-Stellung.

> **Einfach erklärt:** Drei wichtige Schachregel-Tests haben funktioniert. Das ist ein guter Anfang, aber noch keine vollständige Qualitätsgarantie.

Der Kern ist mit rund zweitausend Zeilen TypeScript überschaubar. Das ist für ein Lernprojekt ein Vorteil: Wir können verstehen, verändern und testen, was TunaFish tut.

> **Einfach erklärt:** Das Programm ist nicht riesig. Deshalb können wir es wirklich selbst weiterentwickeln, statt nur eine Black Box zu benutzen.

## Original-Live-Demo und Dank

Die ursprüngliche Live-Demo von Tonnetto wurde von Marco Buontempo erstellt. Sie ist eine gute Möglichkeit, die Ausgangs-Engine direkt im Browser auszuprobieren. Vielen Dank an Marco Buontempo für die Veröffentlichung des Quellcodes, die Dokumentation und die Demo.

> **Einfach erklärt:** Der ursprüngliche Autor hat nicht nur den Programmcode veröffentlicht, sondern auch eine fertige Webseite zum Ausprobieren gebaut. Das hilft uns sehr beim Verstehen und Weiterentwickeln.

You can play the bot directly in the browser, using the custom-built GUI at [chess.marcobuontempo.com](https://chess.marcobuontempo.com/).

> **Einfach erklärt:** Über diesen Link kann man die ursprüngliche Tonnetto-Engine direkt spielen. Die Seite gehört zum Originalprojekt und ist noch nicht unsere TunaFish-Version.

Die Live-Demo benötigt Internet und lädt die Originalversion von Tonnetto. Sie ist daher kein Offline-Test von TunaFish und wird nicht in Einfach-Schach eingebaut. TunaFish soll später eine eigene lokale und offline nutzbare Prüfmöglichkeit erhalten.

> **Einfach erklärt:** Die Demo ist zum Anschauen und Vergleichen da. Unser TunaFish soll später ohne Internet auf dem eigenen Computer arbeiten.

## Was wir zuerst verbessern müssen

Die Datei package-lock.json passt derzeit nicht vollständig zu package.json. package.json beschreibt, welche Zusatzprogramme benötigt werden. package-lock.json soll die exakt passenden Versionen festhalten. Darum schlägt npm ci zurzeit fehl.

> **Einfach erklärt:** Die Liste der benötigten Bausteine ist nicht ganz ordentlich. Deshalb kann man das Projekt noch nicht immer mit einem einzigen Standardbefehl sauber installieren.

**npm** steht für *Node Package Manager*. Es ist das Programm, das JavaScript-Bausteine installiert. npm ci bedeutet eine saubere, genau wiederholbare Installation für Tests und Veröffentlichungen. Bis wir die Listen repariert haben, funktioniert nur der vorläufige Befehl npm install --legacy-peer-deps.

> **Einfach erklärt:** npm lädt die Werkzeuge für das Programm herunter. Der einfache Notbehelf funktioniert, aber wir wollen bald wieder den sauberen Standardweg verwenden.

Die sehr langen Perft-Tests laufen bis Tiefe 6 und zählen dabei mehrere Milliarden Stellungen. Sie sind als gründliche Prüfung wertvoll, aber für jeden normalen Programmlauf zu langsam. Wir trennen sie deshalb später in schnelle Standardtests und seltene Langtests.

> **Einfach erklärt:** Manche Tests dauern sehr lange, weil sie fast unvorstellbar viele Züge zählen. Sie sollen nicht jedes Mal automatisch starten.

Der vorhandene Browser-Worker lädt momentan noch Tonnetto über ein **CDN**. CDN bedeutet *Content Delivery Network*: ein Servernetz, von dem Dateien aus dem Internet geladen werden. Das widerspricht unserem Ziel einer lokalen Offline-Analyse und prüft außerdem nicht wirklich den TunaFish-Fork.

> **Einfach erklärt:** Der alte Test lädt noch ein fremdes Programm aus dem Internet. Das ändern wir, damit wirklich unser TunaFish ohne Internet arbeitet.

Ein **Web Worker** ist ein kleiner Hintergrundprozess im Browser. Er rechnet, ohne dass das sichtbare Programm einfriert. Für die spätere Analyse ist das wichtig, weil die Person während der Berechnung weiter auf das Brett schauen können soll.

> **Einfach erklärt:** Ein Worker ist ein zweiter Helfer im Browser. Er denkt im Hintergrund, damit das Spiel nicht stehen bleibt.

Die aktuelle Schnittstelle gibt nur den besten Zug zurück. Für den Gefahrencheck benötigen wir außerdem eine Bewertung, eine kurze Hauptvariante und Hinweise darauf, was der gegnerische Zug bedroht.

> **Einfach erklärt:** Es reicht nicht, nur „Springer nach f6“ anzuzeigen. Die Hilfe soll auch sagen können: „Danach ist deine Dame angegriffen.“

Eine **Hauptvariante** ist die Zugfolge, die die Engine für am wahrscheinlichsten oder besten hält. Beispiel: „Dein geplanter Zug – beste Antwort des Gegners – deine mögliche Abwehr.“

> **Einfach erklärt:** Die Hauptvariante ist ein kurzer möglicher Film der nächsten Züge.

TunaFish hat noch keine Transpositionstabelle und kein Zobrist-Hashing. Eine **Transpositionstabelle** merkt sich bereits berechnete Stellungen. **Zobrist-Hashing** ist eine schnelle Methode, einer Stellung eine eindeutige Kennzahl zu geben. Beides kann die Suche später stark beschleunigen.

> **Einfach erklärt:** TunaFish vergisst derzeit viele Rechnungen wieder. Später soll es sich gleiche Brettstellungen merken, damit es nicht doppelt arbeiten muss.

TunaFish hat außerdem noch keine iterative Vertiefung und keine feste Zeitgrenze. **Iterative Vertiefung** heißt: Die Engine rechnet zuerst kurz, dann etwas tiefer und behält zwischendurch immer ein brauchbares Ergebnis. Eine Zeitgrenze beendet die Rechnung nach zum Beispiel einer Sekunde.

> **Einfach erklärt:** Später soll TunaFish nach kurzer Zeit schon eine Antwort haben und nur weiterdenken, wenn noch Zeit übrig ist.

## Plan der Übernahme

1. Wir benennen technische Reste von Tonnetto um: Paketname, Beschreibung, UCI-Name und Worker. Der ursprüngliche Copyright-Hinweis bleibt trotzdem erhalten.

   **Einfach erklärt:** Der Name wird überall TunaFish, aber wir verschweigen nicht, wer das ursprüngliche Programm geschrieben hat.

2. Wir reparieren die Installation und modernisieren die Entwicklungswerkzeuge. Danach soll npm ci wieder zuverlässig funktionieren.

   **Einfach erklärt:** Jeder soll TunaFish später einfach und gleich installieren können.

3. Wir teilen die Tests in schnell und lang auf und ergänzen **GitHub Actions**. GitHub Actions ist ein Dienst von GitHub, der nach Änderungen automatisch bauen und testen kann.

   **Einfach erklärt:** GitHub kann später selbst prüfen, ob ein neuer Programmstand noch funktioniert.

4. Wir bauen einen lokalen TunaFish-Worker. Er bekommt eine FEN-Stellung und eine Rechentiefe und gibt den besten Zug, die Bewertung und später die Hauptvariante zurück.

   **Einfach erklärt:** Einfach-Schach schickt TunaFish ein Brettbild. TunaFish schickt eine kurze, brauchbare Antwort zurück.

5. Erst wenn TunaFish für sich zuverlässig läuft, bauen wir einen kleinen Adapter für Einfach-Schach. Ein *Adapter* übersetzt zwischen zwei Programmen, damit beide eine klare und stabile Verbindung haben.

   **Einfach erklärt:** Wir bauen zuerst den Motor fertig. Erst danach verbinden wir ihn sauber mit dem Schachspiel.

6. Zum Schluss übersetzt Einfach-Schach die technische Antwort in Lernhilfe: zuerst eine Denkfrage, dann auf Wunsch Brettmarkierungen, dann die konkrete Gefahr und mögliche Abwehr.

   **Einfach erklärt:** TunaFish rechnet Schach. Einfach-Schach erklärt das Ergebnis freundlich und in kleinen Schritten.

## Verbindung mit Einfach-Schach

Die Repositories werden jetzt **nicht fest gekoppelt**. Insbesondere verwenden wir kein Git-Submodule. Ein Git-Submodule ist ein Repository innerhalb eines anderen Repositories; das macht Updates und Downloadversionen unnötig kompliziert.

> **Einfach erklärt:** Wir stecken TunaFish nicht einfach als zweiten Ordner in Einfach-Schach. Das wäre später schwer zu warten.

Zuerst bleibt TunaFish ein eigenständiges, testbares Projekt. Nach einem ersten stabilen TunaFish-Release bindet Einfach-Schach eine geprüfte Version über einen kleinen Adapter ein. Dadurch kann TunaFish verbessert werden, ohne das spielbare Einfach-Schach zu gefährden.

> **Einfach erklärt:** Erst wird TunaFish verlässlich. Dann bekommt Einfach-Schach genau diese geprüfte Version. So geht nichts kaputt, wenn wir später weiterbauen.

~~~text
Einfach-Schach: Spiel, Brett, einfache Computergegnerin
        │
        │ freiwillige Analyseanfrage mit FEN
        ▼
TunaFish-Worker: rechnet lokal und offline
        │
        │ bester Gegenzug, Bewertung, Hauptvariante
        ▼
Gefahrencheck: Erklärung in einfacher Sprache
~~~

Die Grafik zeigt die geplante Rollenverteilung. Einfach-Schach bleibt für Spiel und Bedienung zuständig. TunaFish bleibt für die Schachrechnung zuständig. Der Gefahrencheck macht daraus eine hilfreiche Erklärung.

> **Einfach erklärt:** Jeder Teil hat nur eine Aufgabe: spielen, rechnen oder erklären.

## Starten und Bauen

Derzeit wird TunaFish vorläufig so installiert und gebaut:

~~~bash
npm install --legacy-peer-deps
npm run build
~~~

npm run build baut aus TypeScript die Datei dist/index.js. *Build* bedeutet hier: Der Quelltext wird in eine Form umgewandelt, die andere Programme und der Browser verwenden können.

> **Einfach erklärt:** Der erste Befehl holt die benötigten Werkzeuge. Der zweite macht aus dem Programmcode eine verwendbare TunaFish-Datei.

Der Standardbefehl npm ci wird erst empfohlen, wenn wir die Abhängigkeitslisten repariert haben. Ein **Dependency** ist ein zusätzliches Programmteil, von dem TunaFish beim Entwickeln oder Bauen abhängt.

> **Einfach erklärt:** Manche Teile werden von anderen Projekten mitbenutzt. Diese Verbindung müssen wir zuerst ordentlich aufräumen.

## Lizenz, Herkunft und Grafik-Dateien

Der Engine-Kern steht unter der **MIT-Lizenz**. MIT steht für *Massachusetts Institute of Technology*; die Lizenz ist nach dieser Hochschule benannt. Sie ist eine sehr freie Softwarelizenz und erlaubt Verwenden, Ändern und Weitergeben, verlangt aber, dass der Copyright-Hinweis und der Lizenztext erhalten bleiben.

> **Einfach erklärt:** Wir dürfen TunaFish weiterbauen und verteilen. Wir müssen aber ehrlich dazuschreiben, von wem der ursprüngliche Code kommt, und die Lizenz beilegen.

Der ursprüngliche Hinweis lautet: „Copyright (c) 2024 Marco Buontempo“. Er bleibt in der Datei [LICENSE](LICENSE) erhalten. Eigene Ergänzungen von TunaFish sollen ebenfalls unter MIT stehen, sofern bei einer einzelnen Datei nichts anderes angegeben ist.

> **Einfach erklärt:** Marco Buontempo wird als ursprünglicher Autor genannt. Unsere eigenen Teile dürfen andere Menschen ebenfalls frei benutzen.

Im alten Demo-Ordner liegen Figuren mit anderen Lizenzen: **GPL** und **CC BY-NC-SA**. GPL bedeutet *GNU General Public License* und stellt stärkere Bedingungen an die Weitergabe. CC BY-NC-SA bedeutet *Creative Commons Attribution–NonCommercial–ShareAlike*: Name nennen, nicht kommerziell verwenden und unter gleichen Bedingungen weitergeben.

> **Einfach erklärt:** Nicht alle alten Figuren dürfen so einfach übernommen werden. Deshalb verwenden wir diese Demo-Grafiken nicht in Einfach-Schach.

Einfach-Schach behält seine eigene Brettdarstellung. TunaFish liefert nur Rechenergebnisse und keine Figuren, Farben oder Bildschirmoberfläche.
