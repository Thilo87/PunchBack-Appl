![](https://github.com/Thilo87/PunchBack-Appl/blob/main/img/Logo.png?raw=true)

*Punch Back* ist ein VR-Ryhthmus-Spiel, in dem der Spieler zum Takt der Musik auf ihn zufliegende Objekte boxen oder ihnen ausweichen muss. Der Fokus liegt bei diesem Spiel auf a) sportlicher Herausforderung und b) eine musikalische Club-/Disco-Atmosphäre zu erzeugen.




**Inhalt**

[TOC]

#Features
##Übersicht
- Online-High-Scores: die High-Scores der Spieler können hochgeladen und in einem Leaderboard angezeigt werden
- Statistiken: es werden zahlreiche Statistiken zum Spielerverhalten lokal in einer Datenbank gespeichert und dem Spieler in Tabellen und Funktionsgraphen angezeigt
- Profile: der Spieler hat die Möglichkeit, verschiedene Profile für sich mit jeweils eigenen Statistiken, High-Scores etc. zu erstellen
- Level-Editor: das Spiel enthält einen umfangreichen Level-Editor
- Modifier: der Spieler kann gewisse Aspekte des Spiels modifizieren und bekommt dafür mehr oder weniger Punkte
- Medaillen: der Spieler kann Medaillen gewinnen und sammeln
- Anbindung an Steam: das Spiel ist für Steam konzipiert und nutzt einige der Features von Steam
- Favouriten: der Spieler kann Levels zu seinen Favouriten hinzufügen und sich diese anzeigen lassen
- Playlists: der Spieler kann Playlists mit verschiedenen Leveln und verschiedenen Spieleinstellungen erstellen
- Fehlerberichterstattung: der Spieler kann mit seiner Zustimmung automatisch bei Fehlern Log-Daten an den Entwickler senden

###Online-High-Scores
Die High-Scores des Spielers für jedes Level und jede Schwierigkeitsstufe können auf einen Webserver geuploadet bzw. die High-Scores der anderen Spieler gedownloadet werden. Das Leaderboard-Feature von Steam wurde dabei nicht verwendet, da bei diesem die Anzahl der möglichen Leaderboards begrenzt ist und bei gegebener Popularität des Spiels aufgrund der besonderen Architektur des Spiels diese Obergrenze leicht überschritten werden könnte.

####Anti-Cheat-Maßnahmen
Es wurden einige Maßnahmen ergriffen, um das Uploaden veränderter High-Scores und die Veränderung von Leveln zu erschweren.

#####Level-Hashes
Würde ein Level einen festgelegten Identifier besitzen, unter dem die High-Scores online gespeichert werden, könnte man die Leveldaten so verändern, dass das Level leichter wird oder mehr Punkte abwirft. Um dies zu verhindern, wird mit Hilfe der *FMD5*-Klasse ein MD5-Hash über alle relevanten Leveldaten eines Levels erzeugt. Dieser Hashwert dient nun als Identifier für die online gespeicherten High-Scores verwendet. Eine Veränderung der Leveldaten hat nun eine Veränderung des Hash-Wertes zur Folge, d.h. sollte ein Spieler ein Level auch nur geringfügig verändern, wird das Level wie ein neues behandelt und eine neue High-Score-Tabelle dafür eingerichtet.

#####SCUE5
Das kostenlose Plugin *SCUE5* wurde verwendet, um sensible Daten wie den aktuellen Score des Spielers im Speicher zu verschlüsseln, damit sie mit Cheating-Software wie z.B. der *Cheat Engine* nicht bzw. nur sehr schwierig auszulesen und zu verändern sind.

#####Steam-Tickets
Über das *Identity-Interface* des *OnlineSubsystems* kann ein Steam-WebAPI-Ticket abgerufen werden und serverseitig über die URL https://api.steampowered.com/ISteamUserAuth/AuthenticateUserTicket/v1/ auf Gültigkeit überprüft werden. Dieses Feature von Steam wird bei verschiedenen Anfragen verwendet.

#####Verifikations-Hashes + Salt
Bei sensiblen Anfragen an den Server, z.B. dem Upload von Daten, wird zusätzlich zu den Daten ein MD5-Wert aus diesen Daten und einem Steam-WebAPI-Ticket, sowie ein geheimer Salt gesendet. Dies macht es wesentlich schwieriger, die Daten in der URL zu verfälschen.

###Statistiken
Für jedes gestartete Spiel werden Statistiken wie z.B. der erreichte Score, der härteste Boxschlag uvm. lokal in einer *MySQL*-Datenbank gespeichert. Dafür werden diese Werte zunächst im Speicher angelegt und bei Beendigung des Levels in die Datenbank übertragen. Hierfür wurde [S. Rombauts SQLiteCpp-Library](https://github.com/SRombauts/SQLiteCpp "S. Rombauts SQLiteCpp-Library") verwendet. Über entsprechende *MySQL*-Querys werden diese Daten abgerufen und dem Spieler präsentiert.

###Profile
Der Spieler hat die Möglichkeit, verschiedene Profile für sich mit jeweils eigenen Playlists, Favouriten, Medaillen und Statistiken zu erstellen und zu verwalten. Dabei wird für jede Steam-ID und für jedes Profil ein eigener Ordner mit den entsprechenden Daten erstellt.

###Level-Editor
Das Spiel enthält einen umfangreichen Level-Editor, mit dem der Spieler neue Levels mit eigens ausgewählter Musik erstellen kann.

####Features
- Ein Undo-Redo-System
- Erzeugung aller notwendigen Leveldaten, inkl. der Waveform einer MP3-Datei im *JSON*-Format mittels externer Software, die über einen *FMonitoredProcess* ausgeführt wird
- Bearbeitung von Objekten, Festlegung ihrer Positionen und Einstellen ihrer Eigenschaften
- Kopieren, Einfügen und Löschen von Bereichen auf der Timeline
- Erstellen und Abspeichern von Blaupausen
- Autosaves in festgelegten Intervallen

###Modifier
Der Spieler hat vor dem Start eines Spiels die Möglichkeit, verschiedene Modifier auszuwählen, welche das Spiel schwieriger oder leichter machen können, wofür er mehr bzw. weniger Punkte bekommt.

###Medaillen
Der Spieler kann Bronze-, Silber- und Goldmedaillen gewinnen. Die benötigte Punktezahl dafür wird in den Leveldaten gespeichert.

###Anbindung an Steam
Das Spiel nutzt die *Steam-Achievements* 

###Fehlerberichterstattung
Das Spiel verfügt über ein eigenes Fehlerberichterstattungssystem, welches auch im *Shipping Build* vorhanden ist. Das Abspeichern von Log-Daten wurde aus Performance-Gründen ausgeschaltet. Diese werden nur im Arbeitsspeicher zwischengespeichert und im Falle eines Fehlers nach Zustimmung des Spielers auf den Server geuploaded, nachdem sie in einem temporären Ordner abgespeichert und mittels des Plugins *ZipIt* komprimiert wurden.



##Headers (Underline)

H1 Header (Underline)
=============

H2 Header (Underline)
-------------

###Characters
                
----

~~Strikethrough~~ <s>Strikethrough (when enable html tag decode.)</s>
*Italic*      _Italic_
**Emphasis**  __Emphasis__
***Emphasis Italic*** ___Emphasis Italic___

Superscript: X<sub>2</sub>，Subscript: O<sup>2</sup>

**Abbreviation(link HTML abbr tag)**

The <abbr title="Hyper Text Markup Language">HTML</abbr> specification is maintained by the <abbr title="World Wide Web Consortium">W3C</abbr>.

###Blockquotes

> Blockquotes

Paragraphs and Line Breaks
                    
> "Blockquotes Blockquotes", [Link](http://localhost/)。

###Links

[Links](http://localhost/)

[Links with title](http://localhost/ "link title")

`<link>` : <https://github.com>

[Reference link][id/name] 

[id/name]: http://link-url/

GFM a-tail link @pandao

###Code Blocks (multi-language) & highlighting

####Inline code

`$ npm install marked`

####Code Blocks (Indented style)

Indented 4 spaces, like `<pre>` (Preformatted Text).

    <?php
        echo "Hello world!";
    ?>
    
Code Blocks (Preformatted text):

    | First Header  | Second Header |
    | ------------- | ------------- |
    | Content Cell  | Content Cell  |
    | Content Cell  | Content Cell  |

####Javascript　

```javascript
function test(){
	console.log("Hello world!");
}
 
(function(){
    var box = function(){
        return box.fn.init();
    };

    box.prototype = box.fn = {
        init : function(){
            console.log('box.init()');

			return this;
        },

		add : function(str){
			alert("add", str);

			return this;
		},

		remove : function(str){
			alert("remove", str);

			return this;
		}
    };
    
    box.fn.init.prototype = box.fn;
    
    window.box =box;
})();

var testBox = box();
testBox.add("jQuery").remove("jQuery");
```

####HTML code

```html
<!DOCTYPE html>
<html>
    <head>
        <mate charest="utf-8" />
        <title>Hello world!</title>
    </head>
    <body>
        <h1>Hello world!</h1>
    </body>
</html>
```

###Images

Image:

![](https://pandao.github.io/editor.md/examples/images/4.jpg)

> Follow your heart.

![](https://pandao.github.io/editor.md/examples/images/8.jpg)

> 图为：厦门白城沙滩 Xiamen

图片加链接 (Image + Link)：

[![](https://pandao.github.io/editor.md/examples/images/7.jpg)](https://pandao.github.io/editor.md/examples/images/7.jpg "李健首张专辑《似水流年》封面")

> 图为：李健首张专辑《似水流年》封面
                
----

###Lists

####Unordered list (-)

- Item A
- Item B
- Item C
     
####Unordered list (*)

* Item A
* Item B
* Item C

####Unordered list (plus sign and nested)
                
+ Item A
+ Item B
    + Item B 1
    + Item B 2
    + Item B 3
+ Item C
    * Item C 1
    * Item C 2
    * Item C 3

####Ordered list
                
1. Item A
2. Item B
3. Item C
                
----
                    
###Tables
                    
First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell 

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |

| Function name | Description                    |
| ------------- | ------------------------------ |
| `help()`      | Display the help window.       |
| `destroy()`   | **Destroy your computer!**     |

| Item      | Value |
| --------- | -----:|
| Computer  | $1600 |
| Phone     |   $12 |
| Pipe      |    $1 |

| Left-Aligned  | Center Aligned  | Right Aligned |
| :------------ |:---------------:| -----:|
| col 3 is      | some wordy text | $1600 |
| col 2 is      | centered        |   $12 |
| zebra stripes | are neat        |    $1 |
                
----

####HTML entities

&copy; &  &uml; &trade; &iexcl; &pound;
&amp; &lt; &gt; &yen; &euro; &reg; &plusmn; &para; &sect; &brvbar; &macr; &laquo; &middot; 

X&sup2; Y&sup3; &frac34; &frac14;  &times;  &divide;   &raquo;

18&ordm;C  &quot;  &apos;

##Escaping for Special Characters

\*literal asterisks\*

##Markdown extras

###GFM task list

- [x] GFM task list 1
- [x] GFM task list 2
- [ ] GFM task list 3
    - [ ] GFM task list 3-1
    - [ ] GFM task list 3-2
    - [ ] GFM task list 3-3
- [ ] GFM task list 4
    - [ ] GFM task list 4-1
    - [ ] GFM task list 4-2

###Emoji mixed :smiley:

> Blockquotes :star:

####GFM task lists & Emoji & fontAwesome icon emoji & editormd logo emoji :editormd-logo-5x:

- [x] :smiley: @mentions, :smiley: #refs, [links](), **formatting**, and <del>tags</del> supported :editormd-logo:;
- [x] list syntax required (any unordered or ordered list supported) :editormd-logo-3x:;
- [x] [ ] :smiley: this is a complete item :smiley:;
- [ ] []this is an incomplete item [test link](#) :fa-star: @pandao; 
- [ ] [ ]this is an incomplete item :fa-star: :fa-gear:;
    - [ ] :smiley: this is an incomplete item [test link](#) :fa-star: :fa-gear:;
    - [ ] :smiley: this is  :fa-star: :fa-gear: an incomplete item [test link](#);
            
###TeX(LaTeX)
   
$$E=mc^2$$

Inline $$E=mc^2$$ Inline，Inline $$E=mc^2$$ Inline。

$$\(\sqrt{3x-1}+(1+x)^2\)$$
                    
$$\sin(\alpha)^{\theta}=\sum_{i=0}^{n}(x^i + \cos(f))$$
                
### FlowChart

```flow
st=>start: Login
op=>operation: Login operation
cond=>condition: Successful Yes or No?
e=>end: To admin

st->op->cond
cond(yes)->e
cond(no)->op
```

###Sequence Diagram
                    
```seq
Andrew->China: Says Hello 
Note right of China: China thinks\nabout it 
China-->Andrew: How are you? 
Andrew->>China: I am good thanks!
```

###End