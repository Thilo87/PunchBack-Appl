<p align="center">
  <img src="https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Logo.png?raw=true" />
</p>

*Punch Back* ist ein VR-Ryhthmus-Spiel, in dem der Spieler zum Rhythmus der Musik auf ihn zufliegende Objekte boxen oder ihnen ausweichen muss. Der Fokus liegt bei diesem Spiel auf a) sportlicher Herausforderung und b) eine musikalische Club-/Disco-Atmosphäre zu erzeugen.


<p align="center">
Gameplay-Video:

<a href="https://www.youtube.com/watch?v=mOvrBCflcGI"  target="_blank"><img src="https://img.youtube.com/vi/mOvrBCflcGI/0.jpg"></a>
</p>

<p align="center">
Features-Übersichtsvideo:

<a href="https://www.youtube.com/watch?v=WVc1ZVRSO6A"  target="_blank"><img src="https://img.youtube.com/vi/WVc1ZVRSO6A/0.jpg"></a>
</p>

**Inhalt**

[TOC]

# Online-Funktionalität
Der Spieler hat die Möglichkeit, einen Spielernamen auszuwählen, welcher mit seiner Steam-ID verknüpft in einer MySQL-Datenbank auf einem Web-Server gespeichert wird. Unter dieser Steam-ID werden auch auch andere Daten wie High-Scores und Bewertungen gespeichert.

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/SQLOnline.jpg?raw=true)

Bei Anfragen an den Web-Server werden PHP-Scripte ausgeführt, welche die Parameter auf Gültigkeit überprüft und Querys in der MySQL-Datenbank ausführt. Es wurde viel Wert auf die Sicherheit der Datenbank gesetzt, so werden z.B. alle gewählten Spielernamen AES-verschlüsselt und nur ein Hash der Steam-ID gespeichert.

Für das Senden von GET- und POST-Anfragen an den Server wurde das HTTP-Modul der Unreal Engine verwendet. Zumeist gibt die aufgerufene Methode dabei ein Handle mit Delegates zurück, an die in Blueprints die Funktionalität bei Erfolg bzw. Misserfolg gebunden wird.

# Lokale Speicherung von Daten
Lokale Daten wurden zumeist im JSON-Format bzw. mit Hilfe des Json-Moduls gesichert, beispielsweise die zuletzt ausgewählten Modifiers, die gewonnenen Medaillen, Einstellungen etc.

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/LevelData.jpg?raw=true)

# Features
## Übersicht
- Registrierung: der Spieler kann einen Spielernamen auswählen, der mit seiner Steam-ID verknüpft wird
- Online-High-Scores: die High-Scores der Spieler können geuploaded und in einem Leaderboard angezeigt werden
- Bewertungen: die Bewertungen für Levels können geuploaded und eine durchschnittliche Community-Bewertung gedownloaded werden
- Anbindung an Steam: das Spiel ist für Steam konzipiert und nutzt einige der Features von Steam, zum Beispiel die *Steam-Achievements*
- Statistiken: es werden zahlreiche Statistiken zum Spielerverhalten lokal in einer Datenbank gespeichert und dem Spieler in Tabellen und Funktionsgraphen angezeigt
- Profile: der Spieler hat die Möglichkeit, verschiedene Profile für sich mit jeweils eigenen Statistiken, High-Scores etc. zu erstellen
- Modifier: der Spieler kann gewisse Aspekte des Spiels modifizieren und bekommt dafür mehr oder weniger Punkte
- Medaillen: der Spieler kann Medaillen gewinnen und sammeln
- Favouriten: der Spieler kann Levels zu seinen Favouriten hinzufügen und sich diese anzeigen lassen
- Playlists: der Spieler kann Playlists mit verschiedenen Leveln und verschiedenen Spieleinstellungen selber erstellen
- Level-Editor: das Spiel enthält einen umfangreichen Level-Editor
- Localization: das Spiel ist zweisprachig auf Deutsch und Englisch
- Fehlerberichterstattung: der Spieler kann mit seiner Zustimmung automatisch bei Fehlern Log-Daten an den Entwickler senden

## Online-High-Scores
Die High-Scores des Spielers für jedes Level und jede Schwierigkeitsstufe können auf einen Webserver geuploadet bzw. die High-Scores der anderen Spieler gedownloadet werden. Das Leaderboard-Feature von Steam wurde dabei nicht verwendet.

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/OnlineHighScores.jpg?raw=true)

### Anti-Cheat-Maßnahmen
Es wurden einige Maßnahmen ergriffen, um das Uploaden veränderter High-Scores und die Veränderung von Leveln zu erschweren.

#### Level-Hashes
Würde ein Level einen festgelegten Identifier besitzen, unter dem die High-Scores online gespeichert werden, könnte man die Leveldaten so verändern, dass das Level leichter wird oder mehr Punkte abwirft. Um dies zu verhindern, wird mit Hilfe der FMD5-Klasse ein MD5-Hash über alle relevanten Leveldaten eines Levels erzeugt. Dieser Hashwert wird als Identifier für die online gespeicherten High-Scores verwendet. Eine Veränderung der Leveldaten hat nun eine Veränderung des Hash-Wertes zur Folge, d.h. sollte ein Spieler ein Level auch nur geringfügig verändern, wird das Level wie ein neues Level behandelt und eine neue High-Score-Tabelle dafür eingerichtet.

#### SCUE5
Das kostenlose Plugin SCUE5 wurde verwendet, um sensible Daten wie den aktuellen Score des Spielers im Speicher zu verschlüsseln, damit sie mit Cheating-Software wie z.B. der Cheat Engine nicht bzw. nur sehr schwierig auszulesen und zu verändern sind.

#### Steam-Tickets
Über das Identity-Interface des OnlineSubsystems kann ein Steam-WebAPI-Ticket abgerufen werden und serverseitig über eine Anfrage an  https://api.steampowered.com/ISteamUserAuth/AuthenticateUserTicket/v1/ auf Gültigkeit überprüft werden. Dieses Feature von Steam wird bei verschiedenen Anfragen verwendet.

#### Verifikations-Hashes + Salt
Bei sensiblen Anfragen an den Server, z.B. dem Upload von Daten, wird zusätzlich zu den Daten ein MD5-Wert aus diesen Daten und einem Steam-WebAPI-Ticket, sowie ein geheimer Salt gesendet. Dies macht es wesentlich schwieriger, die Daten direkt in der URL zu verfälschen.

## Bewertungen
Das Spiel lädt automatisch Bewertungen für Levels hoch bzw. eine durchschnittliche Community-Bewertung herunter.

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Ratings.jpg?raw=true)

## Statistiken
Für jedes gestartete Spiel werden Statistiken wie z.B. der erreichte Score, der härteste Boxschlag uvm. lokal in einer MySQL-Datenbank gespeichert. Dafür werden diese Werte zunächst im Speicher angelegt und bei Beendigung des Levels in die Datenbank übertragen. Hierfür wurde [S. Rombauts SQLiteCpp-Library](https://github.com/SRombauts/SQLiteCpp "S. Rombauts SQLiteCpp-Library") verwendet. Über entsprechende MySQL-Querys werden diese Daten abgerufen und dem Spieler präsentiert.

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/LevelStatistics.jpg?raw=true)

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/GlobalStats1.jpg?raw=true)

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/GlobalStats2.jpg?raw=true)

## Profile
Der Spieler hat die Möglichkeit, verschiedene Profile für sich mit jeweils eigenen Playlists, Favouriten, Medaillen und Statistiken zu erstellen und zu verwalten. Dabei wird für jede Steam-ID und für jedes Profil ein eigener Ordner mit den entsprechenden Daten erstellt.

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Profiles1.jpg?raw=true)

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Profiles2.jpg?raw=true)

## Modifier
Der Spieler hat vor dem Start eines Spiels die Möglichkeit, verschiedene Modifier auszuwählen, welche das Spiel schwieriger oder leichter machen können, wofür er mehr bzw. weniger Punkte bekommt.

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Modifiers.jpg?raw=true)

## Medaillen
Der Spieler kann Bronze-, Silber- und Goldmedaillen gewinnen. Die benötigte Punktezahl dafür wird in den Leveldaten gespeichert. Als kleines optisches Gimmick fallen mittels Physik-Simulation die gewonnenen Medaillen aus 1-2 Metern Höhe auf den Boden, ihre Locations werden nach dem *Sleep*-Aufruf der Physik-Engine abgespeichert und drei Instanced-Static-Mesh-Components hinzugefügt (für drei Medaillenarten - Bronze, Silber und Gold).

## Level-Editor
Das Spiel enthält einen umfangreichen Level-Editor, mit dem der Spieler neue Levels mit eigens ausgewählter Musik erstellen kann.

### Features
- Ein Undo-Redo-System
- Erzeugung aller notwendigen Leveldaten, inkl. der Waveform einer MP3-Datei im JSON-Format mittels externer Software, die über einen FMonitoredProcess ausgeführt wird
- Bearbeitung von Objekten, Festlegung ihrer Positionen und Einstellen ihrer Eigenschaften
- Kopieren, Einfügen und Löschen von Bereichen auf der Timeline
- Erstellen und Abspeichern von Blaupausen
- Autosaves in festgelegten Intervallen

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Editor1.jpg?raw=true)

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Editor2.jpg?raw=true)

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Editor3.jpg?raw=true)

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Editor4.jpg?raw=true)

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Editor5.jpg?raw=true)

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Editor6.jpg?raw=true)

## Fehlerberichterstattung
Das Spiel verfügt über ein eigenes Fehlerberichterstattungssystem, welches auch im Shipping Build vorhanden ist. Das Abspeichern von Log-Daten wurde aus Performance-Gründen ausgeschaltet. Diese werden nur im Arbeitsspeicher zwischengespeichert und im Falle eines Fehlers nach Zustimmung des Spielers auf den Server geuploaded, nachdem sie in einem temporären Ordner abgespeichert und mittels des Plugins ZipIt komprimiert wurden.

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Error.jpg?raw=true)

# Das Spiel
Im eigentlichen Spiel werden zunächst die aus dem UI bzw. der GameInstance festgelegten Spieleinstellungen dazu verwendet, das ausgewählte Level bzw. die Schwierigkeitsstufe aus einer JSON-Datei nach dem Übergang zu der neuen Map zu laden.

Die fliegenden Objekte werden dabei einem Object Pool entnommen. Dieser wurde eigens für den Zweck implementiert, um fliegende Objekte wiederverwenden und mögliche Hitches beim Spawnen vieler Objekte auf einmal verhindern zu können.

Für akkurates Timing bzw. zur Umgehung von Audio-Latenz-Problemen wurde die Klasse Quartz der Unreal Engine verwendet.

Ebenfalls geladen wird eine JSON-Datei, welche im Level-Editor erzeugt wurde und Amplitudenwerte der gesamten Audio-Datei enthält. Mit Hilfe dieser Werte können Animationen in Blueprints gesteuert werden, welche im Rhythmus der Musik ablaufen sollen.
Für das Laden der Audio-Datei wurde das kostenlose [RuntimeAudioImporter](https://github.com/gtreshchev/RuntimeAudioImporter "RuntimeAudioImporter")-Plugin verwendet. Die dazugehörigen [AudioAnalysisTools](https://github.com/gtreshchev/AudioAnalysisTools "AudioAnalysisTools") bieten ebenfalls einige Möglichkeiten, Eigenschaften der abgespielten Musik in Echtzeit abzurufen. Zusätzlich dazu wurde in Niagara-Systemen das AudioSpectrum-Modul verwendet.

Da von Anfang an weitere Spielumgebungen geplant waren, wurde viel Wert auf maximal mögliche Generik gesetzt: alle Actors in der Spielumgebung sind problemlos austauschbar mit anderen Actors mit der gleichen Basisklasse.

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/InGameScreenshot1.jpg?raw=true)

# Widgets
Es wurde eine Vielzahl an Widgets für das Spiel programmiert. Im Folgenden werden einige davon etwas näher erklärt.

## Keyboard
Für maximale Flexibilität musste ein eigenes virtuelles Keyboard implementiert werden. Dieses gibt es sowohl für Buchstaben (Deutsch, Englisch) als auch für Ganz- und Gleitkommazahlen. Bei den eigens erstellten bzw. abgeleiteten UMultilineEditableText-Widgets kann bei einem Klick auf dieses das Keyboard automatisch gespawnt werden.

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Keyboard1.jpg?raw=true)

![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Keyboard2.jpg?raw=true)

## ScrollBox
Es musste ein ScrollBox-Widget implementiert werden, welches eine Sortier- und Suchfunktion beinhaltet. Hierfür wurde eine Basisklasse für die Items mit IsGreaterThan-, IsEqualTo- und DoesFilterApply-Methoden erstellt. Diese Methoden dienen dazu, die Elemente einer ScrollBox zu sortieren oder zu filtern. Die eigentliche Sortierung findet dann über Algo::Sort oder, falls Items einzeln hinzugefügt werden, über eine binäre Suche statt, welche den Index findet, an dem das Item eingefügt werden muss.

# Tests
Für Tests wurde in kleinerem Umfang das Unreal Automation-Test-Framework verwendet. Die meisten Tests fanden allerdings *per Hand* statt.
