# Requirements und Design für die Türschließanlage "Türlich"

**Dokument-Version**: 0.0.1

## Inhaltsverzeichnis
1. [Motivation](#h1-motivation)
2. [Vision](#h1-vision)
3. [Anwendungsszenarien (Use Cases)](#h1-use-cases)
4. [Requirements](#h1-requirements)
5. [Abnahmetests](#h1-abnahmetests)
6. [Glossar](#h1-glossar)

----

<a name="h1-motivation"></a>
## Motivation
In der alten Mietung im Mexikoring wurden Metallschlüssel an die Mitglieder ausgegeben. Die Rückgabe dieser Schlüssel im Zusammenhang mit dem Umzug verlief mehr als schleppend, sodass auch noch Monate nach dem Umzug Schlüssel im Umlauf sind. Deshalb kann die Schließanlage nicht wiederverwendet werden.

Bei Metallschlüsseln besteht weiterhin der Nachteil, dass diese unkontrolliert ohne großen Aufwand kopiert und verbreitet werden können.

In der aktuellen Mietung im Eschelsweg wurde deshalb nur ein kleiner Schlüsselsatz an Mitglieder ausgegeben, die besonders häufig anwesend sind und bei Bedarf die Tür öffnen können. Das hat allerdings den Nachteil, dass Mitglieder ohne Schlüssel nicht 24/7 die Mietung betreten können.

<a name="h1-vision"></a>
## Vision

Um diese Nachteile zu umgehen und eine problemlosere Aus- und Rückgabe der Schlüssel zu ermöglichen, soll eine elektronische Türschließanlage entwickelt werden. Als Schlüssel der Anlage werden iButton-Schlüsselanhänger verwendet, die nur mit sehr hohem Aufwand kopierbar und bei Bedarf deaktivierbar sind.

Außen an jeder Tür der Mietfläche (Haupteingang, Küche, Lounge, Mitgliederbereich) sowie an der Haustür (Haupteingang des Gebäudes) werden dafür iButton-Leser angebracht. Auf der Innenseite jeder Mietungs-Tür sollen Türschlossantriebe das Ver- und Entriegeln übernehmen. Der Haupteingang hat von außen keine Türklinke, dafür aber einen Summer, der zum Öffnen betätigt wird, wenn die Tür entriegelt ist. Zusätzlich befindet sich an jeder Tür ein Lautsprecher, über den Status-Nachrichten ausgegeben werden.

TODO: Haustür (Stefan: Welches System?)

Durch kurzes Berühren des Lesers mit dem iButton soll die entsprechende Tür entriegelt und ggf. geöffnet werden. Durch langes Berühren eines Lesers an einer der Mietungs-Türen sollen alle Mietungs-Türen verriegelt werden. Langes Berühren des Lesers am Hauseingang bewirkt das Verriegeln desselbigen.

Zusätzlich zum Normalbetrieb soll eine Ärzteschaltung Besuchern ohne Schlüssel das Öffnen der Tür ohne weitere Aktion von Mitgliedern ermöglichen. Sobald ein Besucher auf einen Klingelknopf drückt, wird die entsprechende Tür geöffnet. Die Lautsprecher der Klingel bleiben dabei stumm.

Das Anlernen neuer Schlüssel erfolgt durch Schlüsselträger, die berechtigt sind, neue Schlüssel hinzuzufügen. Dazu hält der Schlüsselträger seinen iButton an ein dafür vorgesehenes Lesegerät. Danach wird der anzulernende iButton an den Leser gehalten. Dadurch wird das Web-Interface freigeschaltet, was den Schlüsselträger nach seinem Passwort fragt. Nach einem erfolgreichen Login kann der Schlüsselträger den gerade angelernten Schlüssel auswählen und zum System hinzufügen. Dabei hat er die Möglichkeit, die Gültigkeitsdauer einzustellen. Dabei kann ein Datum mit Uhrzeit für den Beginn und das Ende der Gültigkeit angegeben werden. Zusätzlich soll eine Einschränkung aufgrund des Wochentages vorgenommen werden können (z. B. Donnerstags zwischen 15 und 23 Uhr).

Falls das System einmal einen Defekt haben sollte, soll es komplett abgeschaltet werden können. Die Türen können dann nur mit herkömmlichen Metallschlüsseln geöffnet werden. Dazu sind Schließzylinder mit Notfall-Funktion nötig, die noch installiert werden müssen.

Um eine möglichst hohe Ausfallsicherheit zu erreichen, soll das System auch bei Stromausfall funktionieren. Zudem sollen alle Türen autonom funktionieren und selbst über die Gültigkeit des Schlüssels entscheiden können. Sollte eine Störung an einer der Türen auftreten, sollen alle verbleibenden Türen davon unbeeinträchtigt bleiben. Selbst wenn die Basisstation ausfällt oder die Verbindung zu dieser unterbrochen wird, soll es möglich sein, die Türen zu entriegeln.

Um einer Manipulation zuvor zu kommen, sollen sich alle Geräte innerhalb der Mietung befinden. Die iButton-Reader stellen eine elektrische Verbindung nach außen her und werden direkt berührt. Deshalb ist es nötig, die Geräte an den Lesern gegen ESD zu schützen. Wünschenswert wäre auch ein Schutz gegen Vandalismus (mutwilliger Anschluss einer hohen Spannung an den Leser).

<a name="h1-use-cases"></a>
## Anwendungsszenarien (Use Cases)
Im Folgenden werden einige typische Szenarien aus der Sicht des Anwenders beschrieben.

### UC-01: Entriegeln der Haustür

#### Kurzbeschreibung
TODO: Ein Benutzer entriegelt mit seinem persönlichen iButton die Haustür (Haupteingang des Gebäudes).

### UC-03: Entriegeln und Öffnen einer verriegelten Mietungs-Tür

#### Kurzbeschreibung
Ein Benutzer öffnet mit seinem persönlichen iButton eine beliebige Mietungs-Tür, die verriegelt ist.

#### Auslösendes Ereignis
Ein Benutzer steht vor einer Mietungs-Tür und möchte diese öffnen.

#### Vorbedingung
Das System befindet sich im Normalbetrieb. Die Tür ist verriegelt.

#### Nachbedingung
Die Tür ist entriegelt und wurde vom Benutzer geöffnet. Im Falle einer Tür ohne Klinke von außen (Haupteingangstür) wurde dafür der Summer betätigt.

#### Erfolgsszenario
1. Der Benutzer hält seinen iButton kurz (< 3 Sekunden) an das Lesegerät.
2. Der Controller stellt die Gültigkeit des Schlüssels fest.
3. Folgende Aktionen werden gleichzeitg durchgeführt: Die grüne LED am Lesegerät beginnt zu blinken,
4. der Lautsprecher informiert den Benutzer über den Öffnungsvorgang,
5. und der Türschlossantrieb entriegelt das Schloss.
6. Der Controller stellt fest, dass das Schloss entriegelt ist.
7. Die grüne LED erlischt.
8. Der Lautsprecher informiert den Benutzer über das Entriegeln des Schlosses.
9. Der Benutzer öffnet die Tür.

#### Erweiterungen
* 2a: Der Controller stellt fest, dass der Schlüssel bekannt, aber ungültig ist.
  * 2a1: Die rote LED leuchtet für 5 Sekunden.
  * 2a2: Gleichzeitig teilt der Lautsprecher dem Benutzer mit, dass sein Schlüssel zur Zeit ungültig ist.
  * 2a3: Abbruch.
* 2b: Der Controller stellt fest, dass der Schlüssel dem System nicht bekannt ist.
  * 2b1: Die rote LED leuchtet für 5 Sekunden.
  * 2b2: Gleichzeitig teilt der Lautsprecher dem Benutzer mit, dass sein Schlüssel dem System nicht bekannt ist.
  * 2b3: Abbruch.
* 9a: Die Tür hat keine Klinke von außen, aber einen Summer.
  * 9a1: Die grüne LED leuchtet für 3 Sekunden.
  * 9a2: Gleichzeitig wird der Summer für 3 Sekunden betätigt.
  * 9a3: Der Benutzer öffnet die Tür.

#### Fehlerfälle
* 6a: Der Öffnungsvorgang des Türschlossantriebs konnte innerhalb von 10 Sekunden nicht bestätigt werden.
  * 6a1: Der Lautsprecher teilt dies dem Benutzer mit.
  * 6a2: Trotzdem wird ggf. der Summer betätigt.
  * 6a3: Das System meldet diesen Fehler per E-Mail.

#### Häufigkeit
Voraussichtlich zwischen 1- und 3-mal am Tag.

### UC-04: Öffnen einer entriegelten Mietungs-Tür

#### Kurzbeschreibung
Ein Benutzer öffnet eine entriegelte Mietungs-Tür.

#### Auslösendes Ereignis
Ein Benutzer möchte die Mietfläche betreten.

#### Vorbedingung
Das System befindet sich im Normalbetrieb. Die Mietungs-Tür ist bereits entriegelt.

#### Nachbedingung
Der Benutzer hat die Tür geöffnet und kann die Mietfläche betreten.

#### Erfolgsszenario
1. Der Benutzer hält seinen persönlichen iButton an ein Lesegerät.
2. Der Lautsprecher weist den Benutzer darauf hin, dass die Tür bereits entriegelt ist und er diese öffnen kann.
3. Der Benutzer öffnet die Tür.

#### Erweiterungen
* 2a: Die Tür hat keine Klinke von außen, aber einen Summer.
  * 2a1: Der Controller betätigt den Summer für 3 Sekunden.
  * 2a2: Der Benutzer öffnet die Tür.

#### Fehlerfälle
keine

### UC-05: Manuelles Verriegeln aller Mietungs-Türen

#### Kurzbeschreibung
Ein Benutzer verriegelt mit seinem persönlichen iButton von außen alle Türen der Mietfläche gleichzeitig.

#### Auslösendes Ereignis
Ein Benutzer verlässt als Letzter die Mietfläche und möchte deshalb alle Türen verriegeln.

#### Vorbedingung
Das System befindet sich im Normalbetrieb. Mindestens eine Mietungs-Tür ist entriegelt.

#### Nachbedingung
Alle Mietungs-Türen sind verriegelt.

#### Erfolgsszenario
1. Der Benutzer hält seinen iButton lange (>= 3 Sekunden) an ein Lesegerät.
2. Der Controller stellt fest, dass der Schlüssel dem System bekannt ist. Die Gültigkeit wird nicht ausgewertet.
4. Die grüne LED beginnt zu blinken.
5. Der Controller stellt fest, dass alle Türen geschlossen sind.
6. Der Lautsprecher bittet den Benutzer zu warten, bis alle Türen verriegelt sind.
7. Der Controller stellt fest, dass alle Türen verriegelt sind.
8. Der Lautsprecher informiert den Benutzer darüber.

#### Erweiterungen
keine

#### Fehlerfälle
* 2a: Der Controller stellt fest, dass der Schlüssel dem System nicht bekannt ist.
  * 2a1: Der Lautsprecher informiert den Benutzer darüber.
  * 2a2: Abbruch.
* 5a: Das System stellt fest, dass mindestens eine Tür nicht geschlossen ist.
  * 5a1: Der Lautsprecher informiert den Benutzer darüber, bittet ihn, alle Türen zu schließen, und weist darauf hin, dass die Türen nicht verriegelt werden.
  * 5a2: Abbruch.
* 7a: Das System stellt fest, dass nach 10 Sekunden nicht alle Türen verriegelt sind.
  * 7a1: Der Lautsprecher informiert den Benutzer darüber, bittet ihn, die Türen manuell zu verriegeln, und weist ihn darauf hin, dass die Türen nicht verriegelt werden.
  * 7a2: Abbruch.


### UC-06: Automatisches Verriegeln aller Mietungs-Türen

#### Kurzbeschreibung
Nach 45 Minuten Inaktivität werden alle Mietungs-Türen automatisch verriegelt. Dies tritt z. B. ein, wenn der Letzte die Mietfläche verlassen hat, ohne die Türen zu verriegeln. Die Inaktivität wird durch die eigenen Sensoren (Tür offen/geschlossen, verriegelt) festgestellt und kann durch ggf. vorhandene weitere Sensoren (Bewegungsmelder, ...; z. B. über MQTT) ergänzt werden.

#### Auslösendes Ereignis
Es wurde 45 Minuten lang keine Aktivität erkannt.

#### Vorbedingung
Das System befindet sich im Normalbetrieb. Nicht alle Mietungs-Türen sind verriegelt.

#### Nachbedingung
Alle Mietungs-Türen sind verriegelt.

#### Erfolgsszenario
1. Das System stellt eine 45-minütige Inaktivität fest.
2. Das System stellt fest, dass alle Mietungs-Türen geschlossen sind.
3. Alle Türen werden verriegelt.
4. Das System stellt fest, dass alle Türen verriegelt sind.
5. Das System meldet das erfolgreiche automatische Verriegeln als Warnung per E-Mail.

#### Erweiterungen
keine

#### Fehlerfälle
* 2a: Das System stellt fest, dass mindestens eine Mietungs-Tür offen steht.
  * 2a1: Alle verbleibenden Türen werden verriegelt.
  * 2a2: Die Basisstation stellt fest, dass alle verbleibenden Türen verriegelt sind.
  * 2a3: Das System meldet das automatische Verriegeln und die offen stehenden Türen als Fehler per E-Mail.
* 2a2a: Das System stellt nach 10 Sekunden fest, dass nicht alle verbleibenden Türen verriegelt sind.
  * 2a2a1: Das System meldet das automatische Verriegeln, das Offenstehen mindestens einer Tür und diesen Fehler per E-Mail.
* 4a: Die Basisstation stellt nach 10 Sekunden fest, dass nicht alle verbleibenden Türen verriegelt sind.
  * 4a1: Das System meldet das automatische Verriegeln, das Offenstehen mindestens einer Tür und diesen Fehler per E-Mail.

### UC-07: Öffnen der Haustür oder des Haupteingangs mittels Ärzteschaltung

#### Kurzbeschreibung
Mit aktivierter Ärzteschaltung wird der Summer betätigt, sobald ein Benutzer auf den Klingelknopf drückt.

#### Auslösendes Ereignis
Ein Benutzer drückt auf einen Klingelknopf an der Haustür oder am Haupteingang. Dazu benötigt er keinen persönlichen Schlüssel.

#### Vorbedingung
Das System befindet sich im Normalbetrieb und die Ärzteschaltung wurde aktiviert.

#### Nachbedingung
Der Benutzer hat die Tür geöffnet.

#### Erfolgsszenario
1. Der Benutzer betätigt den Klingelknopf.
2. Das System betätigt den Summer für 3 Sekunden.
3. Der Benutzer öffnet die Tür.

#### Erweiterungen
* 2a: Die Tür ist die Haustür (Haupteingang des Gebäudes).
  * 2a1: Der Lautsprecher verkündet eine kurze Wegbeschreibung.
  * 2a2: Das System betätigt den Summer für 3 Sekunden.

#### Fehlerfälle
keine

### UC-08: Anlernen eines Schlüssels
#### Kurzbeschreibung
Ein neuer Schlüssel soll dem System hinzugefügt werden.

#### Auslösendes Ereignis
Ein Benutzer möchte einen persönlichen Schlüssel erhalten.

#### Vorbedingung
Das System befindet sich im Normalbetrieb. Der zukünftige Besitzer des neuen Schlüssels hat das Antragsformular unterschrieben und die anfallenden Gebühren entrichtet.

#### Nachbedingung
Der Schlüssel des Benutzer wurde dem System hinzugefügt. Der Benutzer kann nun im Gültigkeitszeitraum die Türen öffnen.

#### Erfolgsszenario
1. Ein Schlüsselträger, der dazu berechtigt ist, neue Schlüssel hinzuzufügen, hält seinen iButton an den System-Leser.
2. Das System aktiviert das Web-Interface.
3. Der Schlüsselträger öffnet das Web-Interface und wird dort persönlich begrüßt.
4. Der Schlüsselträger gibt im Web-Interface sein persönliches Passwort ein und meldet sich erfolgreich an.
5. Das Web-Interface präsentiert dem Schlüsselträger eine Liste unbekannter iButtons mit Datum und Uhrzeit, die an den System-Leser gehalten wurden.
6. Der Schlüsselträger wählt den richtigen iButton aus, um ihn zum System hinzuzufügen.
7. Der Schlüsselträger legt für den neuen Schlüssel den Namen des Besitzers, seine E-Mail-Adresse und die Gültigkeit fest. Wenn der Besitzer des neuen Schlüssels ebenfalls die Rechte bekommen soll, neue Schlüssel hinzuzufügen, wird ein Passwort vergeben.
8. Der Schlüsselträger klickt auf den "Speichern"-Button, sodass der Schlüssel im System gespeichert wird.
9. Das System sendet dem Besitzer des neuen Schlüssel eine E-Mail mit den gespeicherten Details.
10. Der Schlüsselträger klickt auf den "Ausloggen"-Button, sodass von ihm keine Änderungen mehr vorgenommen werden können.
11. Das System deaktiviert das Web-Interface.

#### Erweiterungen
* 1a: Dem System sind keine Schlüsselträger bekannt, die das Recht haben, weitere Schlüssel hinzuzufügen.
  * 1a1: Ein neuer Benutzer hält seinen persönlichen iButton an den System-Leser.
  * 1a1: Dieser Benutzer öffnet das Web-Interface.
  * 1a2: Weiter mit Punkt 5.

#### Fehlerfälle
keine


<a name="h1-requirements"></a>
## Requirements

* **REQ-02**: Das System muss alle im Folgenden beschriebenen Modi vollständig unterstützen.
  * **REQ-02-01**: Das System muss den Modus *Normalbetrieb* vollständig unterstützen.
    * **REQ-02-01-01**: Wenn ein gültiger iButton kurz (< 3s) an einen Leser gehalten wird, öffnet sich die entsprechende Tür.
      * Die grüne LED blinkt.
      * Eine verriegelte Tür wird entriegelt.
      * Sobald die Entriegelung abgeschlossen ist, leuchtet die grüne LED dauerhaft für 3 Sekunden.
      * Bei einer Tür mit Summer wird dieser für 3 Sekunden betätigt, sobald die Tür entriegelt ist.
    * Wenn ein unberechtigter iButton an einen Leser gehalten wird, leuchtet die rote LED für 3 Sekunden.
    * Im Mitgliederbetrieb kann eine Arztschaltung aktiviert werden.
      * Diese wird durch einen Taster innen an der Mietungs-Tür aktiviert.
      * Die Funktionen des Mitgliederbetriebs bleiben erhalten. 
      * Nach Betätigen der Klingel an der Haustür wird der Summer der Haustür betätigt.
      * Nach Betätigen der Klingel an der Mietungs-Tür wird der Summer der Mietungs-Tür betätigt.
      * Dieser Modus kann durch erneuten Druck auf den Taster verlassen werden.
      * Dieser Modus wird verlassen, sobald die Türen verriegelt werden.
  * Modus "Deaktiviert"
    * Dieser kann nur von innen durch einen Schlüsselschalter betätigt werden.
    * Das System ist heruntergefahren und stromlos.
    * Die Schlösser können von außen nur durch Metallschlüssel entriegelt werden.
  * Modus "Zombie-Apokalypse"
    * Kann nur von innen durch einen Schlüsselschalter betätigt werden.
    * Alle Schlösser werden sofort verriegelt.
    * Sobald ein Sensor erkennt, dass der Riegel zurückgezogen wird, verriegelt der Türschlossantrieb das Schloss wieder.

* Rechteverwaltung

* Datenerfassung
  * Daten direkt zur Person
    * Name
    * iButton-ID
    * 

* Web-Interface  
  * Das Web-Interface wird von einer autorisierten Person durch das Einlesen ihres iButtons am Service-Leser aktiviert.
  * Die Funktionen des Web-Interfaces stehen erst zur Verfügung, wenn sich diese Person mit ihrem persönlichen Passwort erfolgreich authentisiert hat.
  * Das Web-Interface deaktiviert sich nach 10 Minuten Inaktivität oder nach Klick auf einen "Ausloggen"-Button.
  * Funktionsumfang des Web-Interfaces
    * 

* Schutz gegen Ausfall / Manipulation
  * Jede Tür hat einen eigenen Controller, der autonom agieren kann.
    * Jeder Controller speichert die Schlüssel und Zutrittszeiten.
    * Jeder Controller kennt die aktuelle Uhrzeit und das aktuelle Datum.
    * Jeder Controller loggt die folgenden Aktivitäten mit der Uhrzeit und Datum:
      * die erfolgreichen Lesevorgänge des iButton-Leser,
      * das Betätigen des Türschlossantriebs (öffnen/schließen),
      * die Änderungen des Sensors, der den ausgefahrenen Riegel erkennt (Tür verriegelt).
  * Alle Komponenten des Systems werden durch eine zentrale Spannungsquelle versorgt.
    * Jede Komponente des Systems (Controller, Basisstation) sind gegen Stromausfall geschützt.
    * Die Zuleitungen zu den Komponenten sind je gegen Kurzschluss (Sicherungen) und gegen Überspannung (Z-Dioden) gesichert.

* Web-Interface: wenn login, dann kein anderer darauf zugreifen

<a name="h1-abnahmetests"></a>
## Abnahmetests

### AT-01:
#### Abgedeckte Requirements
#### Umgebungsbedingungen
#### Ablauf
#### Ablaufvarianten

### AT-02: Entriegeln und Öffnen der Mietungs-Türen
#### Abgedeckte Requirements
#### Umgebungsbedingungen
Alle Mietungs-Türen sind verriegelt. Das System ist aktiviert und befindet sich im Normalbetrieb. Es existiert mindestens ein gültiger iButton.

#### Ablauf
Der Folgende Test wird für alle Mietungs-Türen durchgeführt:

1. Ein gültiger iButton wird kurz an einen Leser einer Mietungs-Tür gehalten.
2. Die grüne LED des Lesers muss blinken. Der Lautsprecher muss verkünden, dass die Tür entriegelt wird.
3. Sobald die Tür entriegelt ist muss dies durch den Lautsprecher mitgeteilt werden. Die LED erlischt.

#### Ablaufvarianten
* 3a: Die Tür hat keine Klinke von außen, sondern einen Summer.
  * 3a1: Die LED erlischt. Der Summer wird für 3 Sekunden betätigt.

### AT-03: Öffnen der Mietungs-Türen
#### Abgedeckte Requirements
#### Umgebungsbedingungen
#### Ablauf
#### Ablaufvarianten


<a name="h1-glossar"></a>
## Glossar
Terminus              | Erklärung
--------------------- | -----------------------------------------------
Haustür               | Haupteingang des Gebäudes zum Treppenhaus (unten)
Mietungs-Türen        | Eingangstüren zur Mietfläche (Haupteingang, Küche, Lounge, Mitgliederbereich)
Türschlossantrieb     | Gerät, das das Türschloss betätigt. KeyMatic HM-Sec-Key-S oder vergleichbar.
Falle                 | Auch Schlossfalle (Schnapper): Hält die Tür geschlossen, nachdem die Tür "ins Schloss gefallen ist". 
Riegel                | Der Bolzen, der ausgefahren wird, wenn die Tür abgeschlossen wird.
ver-/entriegelt       | ab-/aufgeschlossen
geschlossen           | Tür ist geschlossen, aber nicht verriegelt
geöffnet              | Tür steht offen und ist entriegelt
Controller            | Modul an jeder Tür, das u. a. den Türschlossantrieb steuert und den iButton-Leser auswertet.
Basisstation          | Zentraler Computer, der die Daten aggregiert und das Web-Interface betreibt

## Fragen
* Bus-System zwischen Controllern und Basisstation? Wenn Basisstation ausfällt, funktioniert das gemeinsame Abschließen noch.
* Welches System wird in die Haustür eingebaut? Wann?
