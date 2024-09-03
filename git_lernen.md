
ist eine übersetzung von https://github.com/UnseenWizzard/git_training unter unter der Attribution-ShareAlike 4.0 International lizenz

# Git-Konzepte lernen, nicht nur Befehle

**Ein interaktives Git-Tutorial, das dir beibringt, wie Git funktioniert, und nicht nur, welche Befehle du ausführen musst.**

Du möchtest also Git verwenden, richtig?

Aber du willst nicht nur Befehle lernen, sondern verstehen, was du verwendest?

Dann ist dies genau das Richtige für dich!

Lass uns loslegen!

---

> Basierend auf dem allgemeinen Konzept von Rachel M. Carmenas Blogbeitrag [How to teach Git](https://rachelcarmena.github.io/2018/12/12/how-to-teach-git.html).

> Während viele Git-Tutorials im Internet meiner Meinung nach zu sehr darauf fokussiert sind, was zu tun ist, anstatt wie die Dinge funktionieren, sind die wertvollsten Ressourcen für beides (und Grundlage für dieses Tutorial!) das [Git-Buch](https://git-scm.com/book/en/v2) und die [Referenzseite](https://git-scm.com/docs).

> Wenn du also nach diesem Tutorial noch interessiert bist, schau dir diese unbedingt an! Ich hoffe, dass das etwas andere Konzept dieses Tutorials dir hilft, all die anderen Git-Funktionen, die dort im Detail beschrieben sind, besser zu verstehen.

---
- [Git-Konzepte lernen, nicht nur Befehle](#git-konzepte-lernen-nicht-nur-befehle)
  - [- Verlauf lesen](#--verlauf-lesen)
  - [Übersicht](#übersicht)
  - [Ein _Remote-Repository_ erhalten](#ein-remote-repository-erhalten)
  - [Neue Dinge hinzufügen](#neue-dinge-hinzufügen)
  - [Änderungen vornehmen](#änderungen-vornehmen)
  - [Branching](#branching)
  - [Mergen](#mergen)
    - [Fast-Forward-Merging](#fast-forward-merging)
    - [Zusammenführen divergenter Branches](#zusammenführen-divergenter-branches)
    - [Konflikte lösen](#konflikte-lösen)
  - [Rebasing](#rebasing)
    - [Konflikte lösen](#konflikte-lösen-1)
  - [Die _Entwicklungsumgebung_ mit Remote-Änderungen aktualisieren](#die-entwicklungsumgebung-mit-remote-änderungen-aktualisieren)
    - [_Fetching_ von Änderungen](#fetching-von-änderungen)
    - [_Pulling_ von Änderungen](#pulling-von-änderungen)
    - [Änderungen stashen](#änderungen-stashen)
    - [Pulling mit Konflikten](#pulling-mit-konflikten)
  - [Cherry-Picking](#cherry-picking)
  - [Geschichte umschreiben](#geschichte-umschreiben)
    - [Den letzten Commit ändern](#den-letzten-commit-ändern)
    - [Interaktives Rebase](#interaktives-rebase)
    - [Einen älteren Commit korrigieren](#einen-älteren-commit-korrigieren)
    - [Öffentliche Historie: Warum du sie nicht umschreiben solltest und wie du es trotzdem sicher machen kannst](#öffentliche-historie-warum-du-sie-nicht-umschreiben-solltest-und-wie-du-es-trotzdem-sicher-machen-kannst)
  - [Verlauf lesen](#verlauf-lesen)
---

## Übersicht

Auf dem Bild unten siehst du vier Kästchen. Eines davon steht allein, während die anderen drei zusammen gruppiert sind und das bilden, was ich deine _Entwicklungsumgebung_ nenne.

![Git-Komponenten](img/components.png)

Wir beginnen jedoch mit dem Kästchen, das allein steht. Das _Remote-Repository_ ist der Ort, an den du deine Änderungen sendest, wenn du sie mit anderen teilen möchtest, und von dem du deren Änderungen erhältst. Wenn du bereits andere Versionskontrollsysteme verwendet hast, ist das nichts Neues.

Die _Entwicklungsumgebung_ befindet sich auf deinem lokalen Rechner. Sie besteht aus drei Teilen: deinem _Arbeitsverzeichnis_ (Working Directory), der _Staging Area_ und dem _lokalen Repository_. Diese werden wir im Laufe der Git-Nutzung genauer kennenlernen.

Wähle einen Ort aus, an dem du deine _Entwicklungsumgebung_ einrichten möchtest. Gehe einfach zu deinem Home-Verzeichnis oder an einen Ort, an dem du deine Projekte normalerweise speicherst. Es ist nicht notwendig, einen neuen Ordner für deine _Entwicklungsumgebung_ zu erstellen.

## Ein _Remote-Repository_ erhalten

Nun möchten wir ein _Remote-Repository_ abrufen und den Inhalt auf deinen Rechner kopieren.

Ich schlage vor, dass wir dieses hier verwenden ([https://github.com/UnseenWizzard/git_training.git](https://github.com/UnseenWizzard/git_training.git), falls du dies nicht bereits auf GitHub liest).

> Dafür kann ich `git clone https://github.com/UnseenWizzard/git_training.git` verwenden.
> 
> Da du die Änderungen, die du in deiner _Entwicklungsumgebung_ machst, zurück ins _Remote-Repository_ bringen musst und GitHub nicht jedem erlaubt, in jedes beliebige Repository zu schreiben, solltest du am besten jetzt einen _Fork_ davon erstellen. Es gibt einen Button dafür oben rechts auf dieser Seite.

Jetzt, wo du eine Kopie meines _Remote-Repositorys_ hast, ist es an der Zeit, diese auf deinen Rechner zu holen.

Dafür verwenden wir `git clone https://github.com/{DEIN BENUTZERNAME}/git_training.git`.

Wie im untenstehenden Diagramm zu sehen, kopiert dieser Befehl das _Remote-Repository_ an zwei Stellen: in dein _Arbeitsverzeichnis_ und in das _Lokale Repository_.
Hier wird deutlich, wie Git als _verteiltes_ Versionskontrollsystem funktioniert. Das _Lokale Repository_ ist eine Kopie des _Remote-Repositorys_ und funktioniert genauso wie dieses. Der einzige Unterschied ist, dass du es nicht mit anderen teilst.

Was `git clone` auch macht, ist das Erstellen eines neuen Ordners an dem Ort, an dem du den Befehl ausgeführt hast. Es sollte jetzt einen `git_training`-Ordner geben. Öffne ihn.

![Das Remote-Repository klonen](img/clone.png)

## Neue Dinge hinzufügen

Im _Remote Repository_ befindet sich bereits eine Datei namens `Alice.txt`. Diese ist dort ziemlich einsam, also lass uns eine neue Datei erstellen und sie `Bob.txt` nennen.

Was du gerade gemacht hast, ist das Hinzufügen der Datei zu deinem _Arbeitsverzeichnis_. Es gibt zwei Arten von Dateien in deinem _Arbeitsverzeichnis_: _verfolgte_ Dateien, die Git kennt, und _unverfolgte_ Dateien, die Git noch nicht kennt.

Um zu sehen, was in deinem _Arbeitsverzeichnis_ vor sich geht, führe `git status` aus. Dieser Befehl zeigt dir an, auf welchem Branch du dich befindest, ob dein _Lokales Repository_ vom _Remote_ abweicht, und den Status von _verfolgten_ und _unverfolgten_ Dateien.

Du wirst sehen, dass `Bob.txt` unverfolgt ist, und `git status` gibt dir sogar Hinweise, wie du das ändern kannst. Im Bild unten siehst du, was passiert, wenn du dem Hinweis folgst und `git add Bob.txt` ausführst: Du hast die Datei zur _Staging Area_ hinzugefügt, wo du alle Änderungen sammelst, die du ins _Repository_ einfügen möchtest.

![Änderungen zur Staging Area hinzufügen](img/add.png)

Wenn du alle deine Änderungen hinzugefügt hast (was derzeit nur das Hinzufügen von Bob ist), bist du bereit, diese in das _Lokale Repository_ zu _committen_.

Die gesammelten Änderungen, die du _committest_, stellen eine sinnvolle Arbeitseinheit dar. Wenn du jetzt `git commit` ausführst, öffnet sich ein Texteditor, in dem du eine Nachricht schreiben kannst, die beschreibt, was du gerade gemacht hast. Wenn du die Nachricht speicherst und die Datei schließt, wird dein _Commit_ in das _Lokale Repository_ aufgenommen.

![In das lokale Repository committen](img/commit.png)

Du kannst deine _Commit-Nachricht_ auch direkt in der Kommandozeile hinzufügen, indem du `git commit` so ausführst: `git commit -m "Add Bob"`. Da du jedoch [gute Commit-Nachrichten](https://chris.beams.io/posts/git-commit/) schreiben möchtest, solltest du dir wirklich Zeit nehmen und den Editor verwenden.

Jetzt befinden sich deine Änderungen in deinem lokalen Repository, was ein guter Ort für sie ist, solange niemand anderes sie benötigt oder du noch nicht bereit bist, sie zu teilen.

Um deine Commits mit dem _Remote Repository_ zu teilen, musst du sie `pushen`.

![In das lokale Repository pushen](img/push.png)

Sobald du `git push` ausführst, werden die Änderungen an das _Remote Repository_ gesendet. Im folgenden Diagramm siehst du den Zustand nach deinem `push`.

![Zustand aller Komponenten nach dem Pushen der Änderungen](img/after_push.png)

## Änderungen vornehmen

Bisher haben wir nur eine neue Datei hinzugefügt. Der interessantere Teil der Versionskontrolle besteht jedoch darin, Dateien zu ändern.

Schau dir die Datei `Alice.txt` an. Sie enthält bereits Text, aber `Bob.txt` ist noch leer. Also ändern wir das und fügen `Hi!! I'm Bob. I'm new here.` hinzu.

Wenn du jetzt `git status` ausführst, wirst du sehen, dass `Bob.txt` als _modified_ angezeigt wird. In diesem Zustand befinden sich die Änderungen nur in deinem _Arbeitsverzeichnis_.

Um zu sehen, was in deinem _Arbeitsverzeichnis_ geändert wurde, kannst du `git diff` ausführen. Derzeit wird Folgendes angezeigt:

```Diff
diff --git a/Bob.txt b/Bob.txt
index e69de29..3ed0e1b 100644
--- a/Bob.txt
+++ b/Bob.txt
@@ -0,0 +1 @@
+Hi!! I'm Bob. I'm new here.
```

Führe `git add Bob.txt` aus, wie du es zuvor getan hast. Dadurch werden deine Änderungen in die _Staging Area_ verschoben.

Um die Änderungen zu sehen, die wir gerade _staged_ haben, führen wir erneut `git diff` aus. Diesmal wird die Ausgabe leer sein, da `git diff` nur die Änderungen im _Arbeitsverzeichnis_ anzeigt.

Um die Änderungen zu sehen, die bereits _staged_ sind, verwenden wir `git diff --staged`. Dadurch wird die gleiche Diff-Ausgabe wie zuvor angezeigt.

Ich habe gerade bemerkt, dass wir zwei Ausrufezeichen nach dem 'Hi' gesetzt haben. Das gefällt mir nicht, also ändern wir `Bob.txt` so, dass es nur noch 'Hi!' ist.

Wenn du jetzt `git status` ausführst, wirst du sehen, dass es zwei Änderungen gibt: die eine, die wir bereits _staged_ haben, und die gerade gemachte, die sich noch im Arbeitsverzeichnis befindet.

Wir können uns den `git diff` zwischen dem _Arbeitsverzeichnis_ und dem, was wir bereits in die _Staging Area_ verschoben haben, ansehen, um zu sehen, was sich seit dem letzten _Staging_ geändert hat.

```Diff
diff --git a/Bob.txt b/Bob.txt
index 8eb57c4..3ed0e1b 100644
--- a/Bob.txt
+++ b/Bob.txt
@@ -1 +1 @@
-Hi!! I'm Bob. I'm new here.
+Hi! I'm Bob. I'm new here.
```

Da die Änderung so ist, wie wir sie wollten, lass uns `git add Bob.txt` ausführen, um den aktuellen Stand der Datei zu _stagen_.

Jetzt sind wir bereit, unsere Änderungen mit `git commit` zu sichern. Ich habe mich für `git commit -m "Add text to Bob"` entschieden, da ich finde, dass für eine so kleine Änderung eine kurze Nachricht ausreicht.

Die Änderungen befinden sich nun im _Lokalen Repository_. Vielleicht möchtest du noch sehen, welche Änderungen du gerade _committed_ hast und was vorher da war.

Das können wir tun, indem wir Commits vergleichen. Jeder Commit in Git hat eine eindeutige Hash-ID.

Wenn wir uns das `git log` ansehen, sehen wir eine Liste aller Commits mit ihren _Hashes_, sowie _Author_ und _Datum_, und den Zustand unseres _Lokalen Repositories_ und die neuesten Informationen über _Remote Branches_.

Derzeit sieht das `git log` in etwa so aus:

```ShellSession
commit 87a4ad48d55e5280aa608cd79e8bce5e13f318dc (HEAD -> master)
Author: {YOU} <{YOUR EMAIL}>
Date:   Sun Jan 27 14:02:48 2019 +0100

    Add text to Bob

commit 8af2ff2a8f7c51e2e52402ecb7332aec39ed540e (origin/master, origin/HEAD)
Author: {YOU} <{YOUR EMAIL}>
Date:   Sun Jan 27 13:35:41 2019 +0100

    Add Bob

commit 71a6a9b299b21e68f9b0c61247379432a0b6007c 
Author: UnseenWizzard <nicola.riedmann@live.de>
Date:   Fri Jan 25 20:06:57 2019 +0100

    Add Alice

commit ddb869a0c154f6798f0caae567074aecdfa58c46
Author: Nico Riedmann <UnseenWizzard@users.noreply.github.com>
Date:   Fri Jan 25 19:25:23 2019 +0100

    Add Tutorial Text

      Changes to the tutorial are all squashed into this commit on master, to keep the log free of clutter that distracts from the tutorial

      See the tutorial_wip branch for the actual commit history
```

Hier sind einige interessante Dinge zu sehen:
* Die ersten beiden Commits wurden von dir gemacht.
* Dein initialer Commit zum Hinzufügen von Bob ist der aktuelle _HEAD_ des _master_ Branches im _Remote Repository_. Wir werden dies später erneut betrachten, wenn wir über Branches und das Abrufen von Remote-Änderungen sprechen.
* Der neueste Commit im _Lokalen Repository_ ist der, den wir gerade gemacht haben, und wir kennen jetzt seinen Hash.

> Beachte, dass die tatsächlichen Commit-Hashes bei dir unterschiedlich sein werden. Wenn du wissen möchtest, wie Git genau zu diesen Revisions-IDs kommt, wirf einen Blick auf [diesen interessanten Artikel](https://blog.thoughtram.io/git/2014/11/18/the-anatomy-of-a-git-commit.html).

Um diesen Commit mit einem vorherigen zu vergleichen, können wir `git diff <commit>^!` ausführen (wobei das `^!` Git anweist, den Commit mit dem vorherigen zu vergleichen). In diesem Fall führe ich `git diff 87a4ad48d55e5280aa608cd79e8bce5e13f318dc^!` aus.

Wir können auch `git diff 8af2ff2a8f7c51e2e52402ecb7332aec39ed540e 87a4ad48d55e5280aa608cd79e8bce5e13f318dc` ausführen, um das gleiche Ergebnis zu erzielen und im Allgemeinen beliebige zwei Commits zu vergleichen. Beachte, dass das Format hier `git diff <von commit> <zu commit>` ist, sodass unser neuer Commit an zweiter Stelle steht.

Im Diagramm unten siehst du erneut die verschiedenen Phasen einer Änderung und die entsprechenden Diff-Befehle.

![Zustände einer Änderung und die zugehörigen Diff-Befehle](img/diffs.png)

Da wir sicher sind, dass wir die gewünschte Änderung vorgenommen haben, führe `git push` aus. 

## Branching

Ein weiteres tolles Feature von Git ist, dass das Arbeiten mit Branches wirklich einfach ist und ein integraler Bestandteil der Arbeit mit Git darstellt.

Tatsächlich haben wir seit Beginn an an einem Branch gearbeitet.

Wenn du ein _Remote Repository_ klonst, startet deine _Dev Environment_ automatisch auf dem Hauptbranch des Repositories, also dem _master_.

Die meisten Workflows mit Git beinhalten das Arbeiten an einem _Branch_, bevor du ihn wieder in den _master_ `merge`st. 
Normalerweise arbeitest du an deinem eigenen _Branch_, bis du fertig bist und von deinen Änderungen überzeugt bist, die dann in den _master_ gemerged werden können.

> Viele Git-Repository-Manager wie _GitLab_ und _GitHub_ erlauben es auch, Branches _geschützt_ zu machen, was bedeutet, dass nicht jeder einfach Änderungen dort `pushen` darf. Der _master_ ist normalerweise standardmäßig geschützt.

Keine Sorge, wir werden auf all diese Dinge später noch detaillierter eingehen.

Im Moment wollen wir einen Branch erstellen, um dort einige Änderungen vorzunehmen. Vielleicht möchtest du etwas auf eigene Faust ausprobieren und nicht den Arbeitszustand auf deinem _master_ Branch ändern, oder du darfst nicht auf _master_ `pushen`.

Branches existieren sowohl im _Local_ als auch im _Remote Repository_. Wenn du einen neuen Branch erstellst, wird der Inhalt des Branches eine Kopie des aktuell committeden Zustands des Branches sein, an dem du gerade arbeitest.

Lass uns eine Änderung an `Alice.txt` vornehmen! Wie wäre es, wenn wir etwas Text in die zweite Zeile setzen?

Wir möchten diese Änderung teilen, sie aber nicht sofort auf _master_ anwenden, also erstellen wir einen Branch dafür, indem wir `git branch <branch name>` verwenden.

Um einen neuen Branch namens `change_alice` zu erstellen, kannst du `git branch change_alice` ausführen.

Dies fügt den neuen Branch dem _Local Repository_ hinzu.

Während dein _Working Directory_ und deine _Staging Area_ sich nicht wirklich um Branches kümmern, commitst du immer zu dem Branch, auf dem du dich gerade befindest.

Du kannst dir _Branches_ in Git als Zeiger vorstellen, die auf eine Reihe von Commits zeigen. Wenn du `commit`st, fügst du zu dem hinzu, worauf du gerade zeigst.

Das Hinzufügen eines Branches bringt dich nicht direkt dorthin, es erstellt nur einen solchen Zeiger. 
Tatsächlich kann der Zustand deines _Local Repository_ als ein anderer Zeiger betrachtet werden, genannt _HEAD_, der zeigt, auf welchem Branch und Commit du dich gerade befindest.

Wenn das kompliziert klingt, sollten die folgenden Diagramme hoffentlich helfen, das Ganze etwas klarer zu machen:

![State after adding branch](img/add_branch.png)

Um zu unserem neuen Branch zu wechseln, musst du `git checkout change_alice` verwenden. Was dies bewirkt, ist einfach das Verschieben von _HEAD_ auf den angegebenen Branch.

> Da du normalerweise direkt nach dem Erstellen eines Branches zu diesem wechseln möchtest, gibt es die praktische `-b` Option für den `checkout` Befehl, die es dir ermöglicht, direkt einen _neuen_ Branch auszuchecken, ohne ihn vorher erstellen zu müssen.

> Um unseren `change_alice` Branch zu erstellen und darauf zu wechseln, hätten wir also auch einfach `git checkout -b change_alice` verwenden können.

![State after switching branch](img/checkout_branch.png)

Du wirst feststellen, dass sich dein _Working Directory_ nicht verändert hat und dass die Tatsache, dass wir `Alice.txt` _modifiziert_ haben, noch nicht mit dem Branch, auf dem wir uns befinden, in Verbindung steht. Jetzt kannst du die Änderung an `Alice.txt` genauso `add`en und `commit`en wie vorher im _master_, wodurch die Änderung in den `change_alice` Branch _gestaged_ (wobei sie noch nicht mit dem Branch verknüpft ist) und schließlich _committed_ wird.

Es gibt jedoch eine Sache, die du noch nicht tun kannst. Versuche, deine Änderungen mit `git push` auf das _Remote Repository_ zu übertragen.

Du wirst den folgenden Fehler sehen und - da Git immer bereit ist zu helfen - einen Vorschlag zur Behebung des Problems:

```ShellSession
fatal: The current branch change_alice has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin change_alice
```

Aber wir wollen das nicht blind machen. Wir sind hier, um zu verstehen, was tatsächlich vor sich geht. Also, was sind _Upstream-Branches_ und _Remotes_?

Erinnere dich, als wir vor einer Weile das _Remote Repository_ geklont haben? Zu diesem Zeitpunkt enthielt es nicht nur dieses Tutorial und `Alice.txt`, sondern tatsächlich zwei Branches.

Der _master_, mit dem wir gerade angefangen haben zu arbeiten, und der Branch, den ich "tutorial_wip" genannt habe, in dem ich alle Änderungen am Tutorial committe.

Als wir die Dinge im _Remote Repository_ in dein _Dev Environment_ kopiert haben, sind einige zusätzliche Schritte im Hintergrund abgelaufen.

Git hat das _remote_ deines _Local Repository_ so eingerichtet, dass es das _Remote Repository_ ist, das du geklont hast, und ihm den Standardnamen `origin` gegeben.

> Dein _Local Repository_ kann mehrere _Remotes_ verfolgen, und diese können unterschiedliche Namen haben, aber für dieses Tutorial bleiben wir bei `origin` und sonst nichts.

Dann wurden die beiden Remote-Branches in dein _Local Repository_ kopiert und schließlich wurde _master_ für dich ausgecheckt.

Beim Auschecken eines Branch-Namens, der eine genaue Übereinstimmung mit den Remote-Branches hat, erhältst du einen neuen _local_ Branch, der mit dem _remote_ Branch verknüpft ist. Der _remote_ Branch ist der _Upstream-Branch_ deines _local_ Branches.

In den Diagrammen vorher siehst du nur die lokalen Branches, die du hast. Du kannst diese Liste der lokalen Branches anzeigen, indem du `git branch` ausführst.

Wenn du auch die _remote_ Branches sehen möchtest, die dein _Local Repository_ kennt, kannst du `git branch -a` verwenden, um alle aufzulisten.

![Remote and local branches](img/branches.png)

Jetzt können wir den vorgeschlagenen Befehl `git push --set-upstream origin change_alice` ausführen und die Änderungen auf unseren Branch zu einem neuen _Remote_ übertragen. Dies wird einen `change_alice` Branch im _Remote Repository_ erstellen und unseren _local_ `change_alice` so einrichten, dass er diesen neuen Branch verfolgt.

> Es gibt eine weitere Option, wenn wir tatsächlich möchten, dass unser Branch etwas verfolgt, das bereits im _Remote Repository_ existiert. Vielleicht hat ein Kollege bereits einige Änderungen gepusht, während wir an einem verwandten Problem auf unserem lokalen Branch gearbeitet haben, und wir möchten die beiden integrieren. Dann könnten wir einfach den _Upstream_ für unseren `change_alice` Branch auf einen neuen _Remote_ setzen, indem wir `git branch --set-upstream-to=origin/change_alice` verwenden und von dort aus den _Remote_ Branch verfolgen.

Nachdem das abgeschlossen ist, schau dir dein _Remote Repository_ auf GitHub an, dein Branch wird dort sein, bereit für andere Personen, um ihn zu sehen und daran zu arbeiten.

Wir werden bald darauf eingehen, wie du die Änderungen anderer Personen in dein _Dev Environment_ übernehmen kannst, aber zuerst werden wir noch ein wenig mehr mit Branches arbeiten, um alle Konzepte einzuführen, die auch beim Abrufen neuer Dinge aus dem _Remote Repository_ eine Rolle spielen.

## Merging

Da du und alle anderen normalerweise an Branches arbeiten werden, müssen wir besprechen, wie man Änderungen von einem Branch in den anderen durch _Merging_ integriert.

<!-- NO CONFLICT -->
Wir haben gerade `Alice.txt` auf dem `change_alice` Branch geändert und ich würde sagen, wir sind zufrieden mit den Änderungen, die wir gemacht haben.

Wenn du jetzt `git checkout master` ausführst, werden die Commits, die wir auf dem anderen Branch gemacht haben, dort nicht vorhanden sein. Um die Änderungen in den _master_ zu bringen, müssen wir den `change_alice` Branch in den _master_ _mergen_.

Beachte, dass du immer einen bestimmten Branch _in_ den Branch mergen musst, auf dem du dich gerade befindest.

### Fast-Forward Merging

Da wir bereits _master_ `checked out` haben, können wir jetzt `git merge change_alice` ausführen.

Da es keine anderen _konfligierenden_ Änderungen an `Alice.txt` gibt und wir nichts an _master_ verändert haben, wird dies ohne Probleme durchgehen, in dem, was als _fast-forward_ Merge bezeichnet wird.

In den folgenden Diagrammen siehst du, dass dies einfach bedeutet, dass der _master_ Zeiger einfach auf den Punkt verschoben werden kann, auf dem sich der _change_alice_ Branch bereits befindet.

Das erste Diagramm zeigt den Zustand vor unserem `merge`, _master_ befindet sich noch auf dem Commit, auf dem er ursprünglich war, und auf dem anderen Branch haben wir einen weiteren Commit gemacht.

![Before fast forward merge](img/before_ff_merge.png)

Das zweite Diagramm zeigt, was sich mit unserem `merge` geändert hat.

![After fast forward merge](img/ff_merge.png)

### Merging Divergenter Branches

Lass uns etwas Komplexeres ausprobieren.

Füge eine neue Zeile mit Text in `Bob.txt` auf _master_ hinzu und committe es.

Wechsle dann zu `change_alice`, ändere `Alice.txt` und committe.

Im Diagramm unten siehst du, wie unser Commit-Verlauf jetzt aussieht. Sowohl _master_ als auch `change_alice` stammen ursprünglich vom selben Commit, haben sich aber seitdem _divergiert_, jeder mit seinem eigenen zusätzlichen Commit.

![Divergent commits](img/branches_diverge.png)

Wenn du nun wieder zu _master_ (`git checkout master`) wechselst und `git merge change_alice` ausführst, ist ein fast-forward Merge nicht möglich. Stattdessen wird dein bevorzugter Texteditor geöffnet und ermöglicht es dir, die Nachricht des `merge commit`, den Git gerade erstellen möchte, anzupassen, um die beiden Branches wieder zusammenzubringen. Du kannst jetzt einfach die Standardnachricht verwenden. Das folgende Diagramm zeigt den Zustand unserer Git-Historie nach dem `merge`.

![Merging branches](img/merge.png)

Der neue Commit führt die Änderungen, die wir am `change_alice` Branch vorgenommen haben, in _master_ ein.

Wie du dich vielleicht erinnerst, sind Revisionen in Git nicht nur ein Schnappschuss deiner Dateien, sondern enthalten auch Informationen darüber, woher sie stammen. Jeder `commit` hat einen oder mehrere Eltern-Commits. Unser neuer `merge` Commit hat sowohl den letzten Commit von _master_ als auch den Commit, den wir auf dem anderen Branch gemacht haben, als Eltern.

### Konflikte Lösen

Bis jetzt haben sich unsere Änderungen nicht gegenseitig beeinflusst.

Lass uns einen _Konflikt_ einführen und dann _lösen_.

Erstelle und `checkoute` einen neuen Branch. Du weißt, wie das geht, aber versuche vielleicht, `git checkout -b` zu verwenden, um dir das Leben zu erleichtern. 
Ich habe meinen `bobby_branch` genannt.

Auf diesem Branch machen wir eine Änderung an `Bob.txt`. 
Die erste Zeile sollte immer noch `Hi!! I'm Bob. I'm new here.` sein. Ändere das in `Hi!! I'm Bobby. I'm new here.`

Stage und committe deine Änderung, bevor du wieder zu _master_ wechselst. Hier ändern wir dieselbe Zeile in `Hi!! I'm Bob. I've been here for a while now.` und committen diese Änderung.

Jetzt ist es Zeit, den neuen Branch in _master_ zu `mergen`.
Wenn du das versuchst, siehst du die folgende Ausgabe:

```ShellSession
Auto-merging Bob.txt
CONFLICT (content): Merge conflict in Bob.txt
Automatic merge failed; fix conflicts and then commit the result.
```
Die gleiche Zeile wurde in beiden Branches geändert, und Git kann das nicht alleine lösen.

Wenn du `git status` ausführst, erhältst du alle üblichen hilfreichen Anweisungen, wie du fortfahren kannst.

Zuerst müssen wir den Konflikt von Hand lösen.

> Für einen einfachen Konflikt wie diesen reicht dein bevorzugter Texteditor aus. Beim Merging großer Dateien mit vielen Änderungen wird ein leistungsfähigeres Tool dein Leben erheblich erleichtern, und ich nehme an, dein bevorzugtes IDE bietet Versionskontrolltools und eine nützliche Ansicht für das Merging.

Wenn du `Bob.txt` öffnest, siehst du etwas Ähnliches wie das folgende (ich habe alles, was wir möglicherweise in der zweiten Zeile vorher eingegeben haben, gekürzt):

```Diff
<<<<<<< HEAD
Hi! I'm Bob. I've been here for a while now.
=======
Hi! I'm Bobby. I'm new here.
>>>>>>> bobby_branch
[... was auch immer du in Zeile 2 eingefügt hast]
```

Oben siehst du, was sich in `Bob.txt` auf dem aktuellen HEAD geändert hat, unten siehst du, was sich in dem Branch geändert hat, den wir mergen.

Um den Konflikt von Hand zu lösen, musst du nur sicherstellen, dass du mit einem vernünftigen Inhalt endest und ohne die speziellen Zeilen, die Git in die Datei eingefügt hat.

Ändere die Datei also auf etwas wie das folgende:

```
Hi! I'm Bobby. I've been here for a while now.
[...]
```

Von hier aus verfahren wir genau wie bei anderen Änderungen. 
Wir _stagen_ sie, wenn wir `add Bob.txt` ausführen, und dann _committen_ wir.

Wir wissen bereits, dass der Commit für die Änderungen, die wir zur Lösung des Konflikts vorgenommen haben, der _merge commit_ ist, der immer bei einem Merge vorhanden ist.

Solltest du während des Lösens von Konflikten feststellen, dass du tatsächlich nicht fortfahren möchtest, kannst du den Merge einfach abbrechen, indem du `git merge --abort` ausführst.

## Rebasing

Git bietet eine andere saubere Methode, um Änderungen zwischen zwei Branches zu integrieren, die als `rebase` bezeichnet wird.

Wir erinnern uns, dass ein Branch immer auf einem anderen basiert. Wenn du ihn erstellst, _zweigst_ du von irgendwo ab.

In unserem einfachen Merging-Beispiel haben wir vom _master_ zu einem bestimmten Commit verzweigt und dann Änderungen sowohl im _master_ als auch im `change_alice` Branch vorgenommen.

Wenn sich ein Branch von dem, auf dem er basiert, abzweigt und du die neuesten Änderungen wieder in deinen aktuellen Branch integrieren möchtest, bietet `rebase` eine sauberere Methode als `merge`.

Wie wir gesehen haben, führt ein `merge` zu einem _Merge-Commit_, bei dem die beiden Historien wieder integriert werden.

Einfach gesagt, ändert Rebasing nur den Punkt in der Geschichte (den Commit), auf den dein Branch basiert.

Um das auszuprobieren, lass uns zuerst den _master_ Branch wieder auschecken, dann einen neuen Branch darauf erstellen und auschecken. 
Ich habe meinen `add_patrick` genannt, eine neue `Patrick.txt`-Datei hinzugefügt und das mit der Nachricht 'Add Patrick' committed.

Nachdem du einen Commit zu dem Branch hinzugefügt hast, gehe zurück zu _master_, mache eine Änderung und committe diese. Ich habe etwas mehr Text zu `Alice.txt` hinzugefügt.

Wie in unserem Merging-Beispiel divergiert die Historie dieser beiden Branches an einem gemeinsamen Vorfahren, wie du im Diagramm unten sehen kannst.

![History before a rebase](img/before_rebase.png)

Jetzt lass uns `checkout add_patrick` wieder ausführen und die Änderung, die am _master_ gemacht wurde, in den Branch, an dem wir arbeiten, einfügen!

Wenn wir `git rebase master` ausführen, basieren wir unseren `add_patrick` Branch auf dem aktuellen Stand des _master_ Branches.

Die Ausgabe dieses Befehls gibt uns einen guten Hinweis darauf, was dabei passiert:

```ShellSession
First, rewinding head to replay your work on top of it...
Applying: Add Patrick
```

Wie wir uns erinnern, ist _HEAD_ der Zeiger auf den aktuellen Commit, an dem wir uns in unserer _Dev Environment_ befinden.

Es zeigt auf denselben Ort wie `add_patrick` vor dem Beginn des Rebase. Für das Rebase bewegt es sich dann zuerst zurück zum gemeinsamen Vorfahren, bevor es auf den aktuellen Head des Branches wechselt, auf den wir rebasen wollen.

Also bewegt sich _HEAD_ vom _0cfc1d2_ Commit zum _7639f4b_ Commit, der am Kopf von _master_ ist. 
Dann wird Rebase jeden einzelnen Commit, den wir in unserem `add_patrick` Branch gemacht haben, darauf anwenden.

Genauer gesagt, was _git_ nach dem Zurückbewegen von _HEAD_ zum gemeinsamen Vorfahren der Branches macht, ist das Speichern von Teilen jedes einzelnen Commits, den du im Branch gemacht hast (die `diff` der Änderungen, und der Commit-Text, Autor usw.).

Danach wird ein `checkout` des letzten Commits des Branches durchgeführt, auf den du rebased, und dann werden die gespeicherten Änderungen __als neuer Commit__ obenauf angewendet.

In unserer ursprünglichen vereinfachten Ansicht würden wir annehmen, dass nach dem `rebase` der _0cfc1d2_ Commit nicht mehr auf den gemeinsamen Vorfahren in seiner Historie zeigt, sondern auf den Kopf von master. 
Tatsächlich ist der _0cfc1d2_ Commit verschwunden, und der `add_patrick` Branch beginnt mit einem neuen _0ccaba8_ Commit, der den neuesten Commit von _master_ als Vorfahren hat. 
Wir haben es so aussehen lassen, als ob unser `add_patrick` auf dem aktuellen _master_ basiert, nicht auf einer älteren Version davon, aber dabei haben wir die Geschichte des Branches umgeschrieben.  
Am Ende dieses Tutorials werden wir noch etwas mehr über das Umschreiben der Historie lernen und wann es angemessen und unangemessen ist, dies zu tun.

![History after rebase](img/rebase.png)

`Rebase` ist ein unglaublich mächtiges Werkzeug, wenn du an deinem eigenen Entwicklungs-Branch arbeitest, der auf einem gemeinsamen Branch basiert, z.B. dem _master_.

Durch die Verwendung von Rebase kannst du sicherstellen, dass du häufig die Änderungen integrierst, die andere Leute machen und an _master_ pushen, während du eine saubere, lineare Historie beibehältst, die es dir ermöglicht, ein `fast-forward merge` durchzuführen, wenn es Zeit ist, deine Arbeit in den gemeinsamen Branch zu integrieren.

Eine lineare Historie macht das Lesen oder Betrachten (versuche `git log --graph` oder schaue dir die Branch-Ansicht bei _GitHub_ oder _GitLab_ an) von Commit-Logs viel nützlicher, als eine Historie voller _Merge-Commits_, die normalerweise nur den Standardtext verwenden.

### Konflikte Lösen

Ähnlich wie beim `merge` kannst du auch beim `rebase` auf Konflikte stoßen, wenn zwei Commits dieselben Teile einer Datei ändern.

Wenn du während eines `rebase` auf einen Konflikt triffst, musst du diesen jedoch nicht in einem zusätzlichen _merge commit_ beheben. Stattdessen kannst du den Konflikt direkt im Commit lösen, der gerade angewendet wird. Dabei basierst du deine Änderungen direkt auf dem aktuellen Zustand des ursprünglichen Branches.

Das Lösen von Konflikten beim `rebase` ist tatsächlich sehr ähnlich wie beim `merge`, also schau dir den Abschnitt zum `merge` an, falls du dir unsicher bist, wie das geht.

Der einzige Unterschied besteht darin, dass du beim `rebase` keinen _merge commit_ einführst, daher musst du deine Lösung nicht `committen`. Stattdessen kannst du die Änderungen einfach in das _Staging Environment_ _add_en und dann `git rebase --continue` ausführen. Der Konflikt wird im Commit gelöst, der gerade angewendet wird.

Wie beim Merging kannst du auch beim Rebase jederzeit alles bisherige abbrechen, indem du `git rebase --abort` ausführst.

## Aktualisieren der _Dev Environment_ mit Änderungen vom Remote

Bis jetzt haben wir nur gelernt, wie man Änderungen macht und teilt.

Das passt, wenn du nur alleine arbeitest. In der Praxis werden jedoch viele Leute am selben Code arbeiten, und wir wollen ihre Änderungen aus dem _Remote Repository_ in unsere _Dev Environment_ übernehmen.

Da es eine Weile her ist, schauen wir uns noch einmal die Komponenten von Git an:

![git components](img/components.png)

Genauso wie deine _Dev Environment_ gibt es auch viele andere _Dev Environments_, die an demselben Quellcode arbeiten.

![many dev environments](img/many_dev_environments.png)

Alle diese _Dev Environments_ haben ihre eigenen _Working Directory_ und _Staging Area_, die irgendwann `committed` in das _Local Repository_ und schließlich `pushed` ins _Remote Repository_ werden.

Für unser Beispiel nutzen wir die Online-Tools von _GitHub_, um zu simulieren, dass jemand anderes Änderungen am _Remote_ vorgenommen hat, während wir weiterarbeiten.

Gehe zu deinem `Fork` dieses Repos auf [github.com](https://www.github.com) und öffne die Datei `Alice.txt`.

Finde den Bearbeiten-Button, mache eine Änderung und committe sie über die Website.

![github edit](img/github.png)

In diesem Repository habe ich eine Remote-Änderung an `Alice.txt` auf einem Branch namens `fetching_changes_sample` hinzugefügt, aber in deiner Version des Repositories kannst du natürlich auch einfach die Datei auf `master` ändern.

### _Fetching_ Änderungen

Wir erinnern uns noch, dass beim `git push` Änderungen aus dem _Local Repository_ ins _Remote Repository_ synchronisiert werden.

Um Änderungen, die am _Remote_ vorgenommen wurden, in dein _Local Repository_ zu holen, verwendest du `git fetch`.

Dieser Befehl holt alle Änderungen am Remote - sowohl Commits als auch Branches - in dein _Local Repository_.

Beachte, dass die Änderungen zu diesem Zeitpunkt noch nicht in die lokalen Branches integriert sind und somit noch nicht in das _Working Directory_ und die _Staging Area_ übernommen wurden.

![Fetching changes](img/fetch.png)

Wenn du jetzt `git status` ausführst, erhältst du eine klare Nachricht darüber, was passiert ist:

```ShellSession
> git status
On branch fetching_changes_sample
Your branch is behind 'origin/fetching_changes_sample' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)
```

Hier wird angezeigt, dass dein Branch `fetching_changes_sample` hinter dem Remote-Branch `origin/fetching_changes_sample` zurückliegt und durch ein `fast-forward` aktualisiert werden kann.

### _Pulling_ Änderungen

Da wir keine _working_ oder _staged_ Änderungen haben, könnten wir jetzt einfach `git pull` ausführen, um die Änderungen aus dem _Repository_ direkt in unser Arbeitsverzeichnis zu holen.

> Beim `pull` wird implizit auch ein `fetch` des _Remote Repository_ durchgeführt, aber manchmal ist es sinnvoll, ein `fetch` separat auszuführen. 
> Zum Beispiel, wenn du neue _Remote_ Branches synchronisieren möchtest oder sicherstellen willst, dass dein _Local Repository_ aktuell ist, bevor du einen `git rebase` auf etwas wie `origin/master` durchführst.

![Pulling in changes](img/pull.png)

Bevor wir `pull`, lass uns eine Datei lokal ändern, um zu sehen, was passiert.

Ändere jetzt auch `Alice.txt` in deinem _Working Directory_!

Wenn du nun versuchst, `git pull` auszuführen, wirst du folgenden Fehler sehen:

```ShellSession
> git pull
Updating df3ad1d..418e6f0
error: Your local changes to the following files would be overwritten by merge:
        Alice.txt
Please commit your changes or stash them before you merge.
Aborting
```

Du kannst keine Änderungen `pullen`, während es Modifikationen an Dateien im _Working Directory_ gibt, die auch von den Commits betroffen sind, die du `pull`en möchtest.

Eine Möglichkeit, dies zu umgehen, besteht darin, deine Änderungen bis zu einem Punkt zu bringen, an dem du sicher bist, `add` sie in das _Staging Environment_, und sie dann schließlich zu `committen`. Dies ist jedoch auch ein guter Moment, um ein weiteres nützliches Werkzeug kennenzulernen: `git stash`.

### Änderungen Stashen

Wenn du zu irgendeinem Zeitpunkt lokale Änderungen hast, die du noch nicht committen möchtest oder die du vorübergehend aufbewahren willst, während du eine andere Herangehensweise ausprobierst, kannst du diese Änderungen `stashen`.

Ein `git stash` ist im Wesentlichen ein Stapel von Änderungen, auf dem du alle Änderungen am _Working Directory_ ablegen kannst.

Die Befehle, die du hauptsächlich verwenden wirst, sind `git stash`, um alle Modifikationen am _Working Directory_ auf dem Stash zu speichern, und `git stash pop`, um die neuesten gestashten Änderungen wieder auf das _Working Directory_ anzuwenden.

Ähnlich wie die [Stack-Kommandos](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)), nach denen es benannt ist, entfernt `git stash pop` die neuesten gestashten Änderungen, bevor sie auf das Arbeitsverzeichnis angewendet werden. Wenn du die gestashten Änderungen behalten möchtest, kannst du `git stash apply` verwenden. Dieser Befehl wendet die neuesten gestashten Änderungen auf das Arbeitsverzeichnis an, ohne sie vom Stash zu entfernen.

Um deinen aktuellen `stash` zu inspizieren, kannst du `git stash list` verwenden, um die einzelnen Einträge aufzulisten, und `git stash show`, um die Änderungen im neuesten Eintrag im `stash` anzuzeigen.

> Ein weiteres nützliches Komfortkommando ist `git stash branch {BRANCH NAME}`, das einen neuen Branch erstellt, der vom HEAD zum Zeitpunkt des Stashens ausgeht, und die gestashten Änderungen auf diesen Branch anwendet.

Jetzt, da wir wissen, wie man `git stash` verwendet, lass uns diesen Befehl ausführen, um unsere lokalen Änderungen an `Alice.txt` aus dem _Working Directory_ zu entfernen, damit wir die Änderungen, die wir über die Website vorgenommen haben, per `git pull` holen können.

Danach verwenden wir `git stash pop`, um die Änderungen zurückzuholen. Da sowohl der Commit, den wir gepullt haben, als auch die gestashten Änderungen `Alice.txt` modifiziert haben, musst du den Konflikt lösen, genau wie bei einem `merge` oder `rebase`. Wenn du fertig bist, _add_ und _commit_ die Änderungen. 

### _Pulling_ with Konflikten

Jetzt, da wir verstanden haben, wie man _Remote Changes_ in unser _Dev Environment_ holt, ist es Zeit, einige Konflikte zu erzeugen!

**Schritt 1: Lokale und Remote-Änderungen vorbereiten**

- Vergewissere dich, dass der Commit, der `Alice.txt` geändert hat, noch nicht gepusht wurde.
- Gehe zurück zu deinem _Remote Repository_ auf [github.com](https://www.github.com).

Dort wirst du `Alice.txt` erneut ändern und diese Änderung committen.

**Schritt 2: Remote-Änderungen abrufen**

- Vergiss nicht, `git fetch` auszuführen, um die Remote-Änderung zu sehen, ohne sie sofort mit `pull` abzurufen.

Wenn du jetzt `git status` ausführst, siehst du, dass beide Branches (lokal und remote) jeweils einen Commit enthalten, der sich von dem des anderen unterscheidet:

```ShellSession
> git status
On branch fetching_changes_sample
Your branch and 'origin/fetching_changes_sample' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)
```

**Schritt 3: Konflikte beim Pull**

Wenn du `git pull` ausführst, während es Unterschiede zwischen dem _Local_ und _Remote Repository_ gibt, passiert genau das Gleiche wie bei einem `merge` zweier Branches. Dies bedeutet, dass ein _merge_ Commit eingeführt wird, wenn Konflikte auftreten.

Zusätzlich kannst du dir die Beziehung zwischen Branches im _Remote_ und dem im _Local Repository_ als einen speziellen Fall des Erstellens eines Branches basierend auf einem anderen vorstellen. Ein lokaler Branch basiert auf dem Zustand des Branches im _Remote_, als du das letzte Mal `fetched` hast.

**Schritt 4: Alternative Pull-Strategie mit Rebase**

Wenn du `git pull` ausführst, werden die _Local_ und _Remote_ Versionen eines Branches zusammengeführt. Alternativ kannst du auch `git pull --rebase` (oder die Kurzform `git pull -r`) verwenden. 

Beim Rebase werden deine lokalen Änderungen so umgeschrieben, als ob sie auf der neuesten Version im _Remote Repository_ basieren. Dies führt zu einem linearen Verlauf, der die Historie sauberer hält.

> Du kannst Git auch so konfigurieren, dass `rebase` statt `merge` die Standardstrategie beim `git pull` verwendet, indem du das `pull.rebase`-Flag mit einem Befehl wie `git config --global pull.rebase true` setzt.

**Schritt 5: Pull mit Rebase**

Wenn du `git pull` noch nicht ausgeführt hast, führe jetzt `git pull -r` aus, um die Remote-Änderungen abzurufen und die Historie so zu gestalten, als ob dein neuer Commit nach diesen Änderungen passiert ist.

Wie bei einem normalen `rebase` (oder `merge`) musst du die Konflikte, die wir eingeführt haben, lösen, damit der `git pull` abgeschlossen werden kann.

**Zusammenfassung der Schritte:**

1. **Stashen**: Speichere deine lokalen Änderungen, bevor du die Remote-Änderungen abrufst.
   ```ShellSession
   git stash
   ```

2. **Fetch**: Hole die Änderungen vom Remote-Repository.
   ```ShellSession
   git fetch
   ```

3. **Pull mit Rebase**: Ziehe die Remote-Änderungen mit Rebase ein, um eine lineare Historie zu erhalten.
   ```ShellSession
   git pull -r
   ```

4. **Konflikte lösen**: Falls Konflikte auftreten, löse diese wie bei einem normalen Rebase oder Merge.
   - Öffne die betroffenen Dateien.
   - Bearbeite die Konflikte.
   - Stage und committe die gelösten Konflikte.
   ```ShellSession
   git add <konfliktdatei>
   git rebase --continue
   ```

Durch diese Schritte kannst du sicherstellen, dass deine Änderungen sowohl lokal als auch remote synchronisiert werden, und Konflikte effektiv verwaltet werden.

## Cherry-Picking

Herzlichen Glückwunsch! Du hast die fortgeschritteneren Funktionen erreicht!

Du verstehst nun die typischen Git-Befehle und, was noch wichtiger ist, wie sie funktionieren. Dies sollte die folgenden Konzepte viel einfacher zu verstehen machen, als wenn ich dir nur gesagt hätte, welche Befehle du eingeben sollst.

### Was ist Cherry-Picking?

Das `cherry-pick`-Kommando in Git ermöglicht es dir, spezifische Commits aus einem Branch herauszunehmen und auf einen anderen Branch anzuwenden. Dies ist besonders nützlich, wenn du nur eine bestimmte Änderung aus einem Branch auf einen anderen übertragen möchtest, ohne alle Änderungen des Branches zu übernehmen.

### Einfache Anwendung: Cherry-Picking eines einzelnen Commits

Nehmen wir an, du möchtest einen bestimmten Commit von einem Branch in einen anderen übernehmen. Schau dir die Abbildung unten an, die drei Branches vor dem Cherry-Picking zeigt. Angenommen, du möchtest Änderungen vom Branch `add_patrick` in den Branch `change_alice` übernehmen. Da diese Änderungen noch nicht in `master` gemerged wurden, kannst du nicht einfach auf `master` rebased, um diese Änderungen zu erhalten.

![Branches before cherry-picking](img/cherry_branches.png)

Um den Commit mit der ID `63fc421` zu übernehmen, führst du den folgenden Befehl aus:

```ShellSession
git cherry-pick 63fc421
```

Das Ergebnis wird ein neuer Commit auf dem Branch `change_alice` sein, der die gewünschten Änderungen enthält.

![Cherry-picking a single commit](img/cherry_pick.png)

Wie bei anderen Git-Befehlen musst du, falls Konflikte auftreten, diese selbst lösen, bevor der `cherry-pick`-Befehl abgeschlossen werden kann. Du kannst entweder den `--continue`-Schalter verwenden, um fortzufahren, nachdem die Konflikte gelöst sind, oder den `--abort`-Schalter, um den Befehl abzubrechen.

### Cherry-Picking eines Commit-Bereichs

Wenn du mehrere aufeinanderfolgende Commits übernehmen möchtest, kannst du dies ebenfalls tun. Verwende dazu den Befehl `git cherry-pick` in der Form `git cherry-pick <from>..<to>`. Im folgenden Beispiel übernehmen wir die Commits im Bereich von `0cfc1d2` bis `41fbfa7`:

```ShellSession
git cherry-pick 0cfc1d2..41fbfa7
```

![Cherry-picking commit range](img/cherry_pick_range.png)

### Zusammenfassung

- **Einzelner Commit**: Verwende `git cherry-pick <commit-id>`, um einen einzelnen Commit auf den aktuellen Branch zu übernehmen.
- **Commit-Bereich**: Verwende `git cherry-pick <from>..<to>`, um eine Reihe von Commits auf den aktuellen Branch zu übernehmen.

Cherry-Picking ist eine mächtige Technik, um gezielt Änderungen zu übernehmen, ohne den gesamten Branch zu integrieren. Achte darauf, Konflikte sorgfältig zu lösen und den Überblick über die angewendeten Änderungen zu behalten.

## Rewriting History

Du hast den Rebase-Bereich gut verstanden, und jetzt schauen wir uns an, wie du die Geschichte von Git noch weiter anpassen kannst. 

### Was bedeutet „Geschichte neu schreiben“?

Die Geschichte eines Branches besteht aus all seinen Commits. Manchmal, nachdem du einen Commit gemacht hast, stellst du vielleicht fest, dass du eine Datei vergessen hast oder einen Tippfehler gemacht hast. Zum Glück gibt es Möglichkeiten, diese Fehler zu korrigieren und die Geschichte so aussehen zu lassen, als wären sie nie passiert.

### Wechseln zu einem neuen Branch

Um zu demonstrieren, wie man die Geschichte neu schreibt, wechseln wir zunächst zu einem neuen Branch:

```ShellSession
git checkout -b rewrite_history
```

Mach einige Änderungen an `Alice.txt` und `Bob.txt`, füge dann `Alice.txt` zur Staging Area hinzu:

```ShellSession
git add Alice.txt
```

Committe die Änderungen mit einer Nachricht wie „This is history“:

```ShellSession
git commit -m "This is history"
```

Nun haben wir einige Fehler gemacht:

- Wir haben vergessen, die Änderungen an `Bob.txt` hinzuzufügen.
- Die Commit-Nachricht könnte verbessert werden.

### Amending (Ändern) des letzten Commits

Eine Möglichkeit, beide Probleme auf einmal zu beheben, besteht darin, den letzten Commit zu ändern.

Das Ändern des letzten Commits funktioniert im Wesentlichen wie das Erstellen eines neuen Commits, jedoch mit einigen zusätzlichen Funktionen:

1. **Änderungen zur Staging Area hinzufügen**: Füge `Bob.txt` zur Staging Area hinzu:

    ```ShellSession
    git add Bob.txt
    ```

2. **Commit Amend**: Führe den folgenden Befehl aus, um den letzten Commit zu ändern:

    ```ShellSession
    git commit --amend
    ```

3. **Commit Message Anpassen**: Ein Editor öffnet sich, in dem du die Commit-Nachricht sehen und ändern kannst. Fühle dich frei, die Nachricht zu verbessern.

4. **Bestätigen**: Nach dem Bearbeiten der Nachricht speichere und schließe den Editor.

### Überprüfen des geänderten Commits

Sieh dir den neuesten Commit erneut an, um zu überprüfen, was geändert wurde:

```ShellSession
git show HEAD
```

Wie erwartet, hat der Commit jetzt einen neuen Hash. Der ursprüngliche Commit wurde durch den neuen ersetzt, der die kombinierten Änderungen und die verbesserte Commit-Nachricht enthält.

> Beachte, dass andere Commit-Daten wie Autor und Datum unverändert bleiben. Wenn du diese ebenfalls ändern möchtest, kannst du dies tun, indem du die zusätzlichen `--author={AUTHOR}` und `--date={DATE}`-Flags beim Amend-Befehl verwendest.

Herzlichen Glückwunsch! Du hast erfolgreich die Geschichte für den ersten Commit neu geschrieben!

### Interaktives Rebase

Normalerweise, wenn wir `git rebase` verwenden, rebasen wir auf einen Branch. Wenn wir zum Beispiel `git rebase origin/master` ausführen, passiert tatsächlich ein Rebase auf den _HEAD_ dieses Branches.

Tatsächlich könnten wir, wenn wir wollten, auf jeden Commit rebases.

> Denke daran, dass ein Commit Informationen über die Historie enthält, die davor kam.

Wie viele andere Befehle hat auch `git rebase` einen _interaktiven_ Modus.

Im Gegensatz zu den meisten anderen Befehlen wirst du den _interaktiven_ `rebase` wahrscheinlich häufig verwenden, da er es dir ermöglicht, die Historie nach Belieben zu ändern.

Besonders wenn du einem Workflow folgst, bei dem du viele kleine Commits deiner Änderungen machst, die es dir ermöglichen, leicht zurückzuspringen, falls du einen Fehler gemacht hast, wird der _interaktive_ `rebase` dein engster Verbündeter sein.

_Genug geredet! Lass uns etwas tun!_

Wechsle zurück zu deinem _master_-Branch und `git checkout` einen neuen Branch, um daran zu arbeiten.

Wie zuvor, werden wir Änderungen an `Alice.txt` und `Bob.txt` vornehmen und dann `git add Alice.txt` ausführen.

Dann werden wir `git commit` mit einer Nachricht wie "Add text to Alice".

Jetzt, statt diesen Commit zu ändern, fügen wir `Bob.txt` hinzu (`git add Bob.txt`) und committen diese Änderung ebenfalls. Als Nachricht habe ich "Add Bob.txt" verwendet.

Um es noch interessanter zu machen, werden wir eine weitere Änderung an `Alice.txt` vornehmen, die wir `git add` und `git commit` werden. Als Nachricht habe ich "Add more text to Alice" verwendet.

Wenn wir jetzt den Verlauf des Branches mit `git log` (oder für einen schnellen Überblick vorzugsweise mit `git log --oneline`) ansehen, werden wir unsere drei Commits oben auf dem sehen, was auch immer auf deinem _master_ war.

Für mich sieht es so aus:
```ShellSession
> git log --oneline
0b22064 (HEAD -> interactiveRebase) Add more text to Alice
062ef13 Add Bob.txt
9e06fca Add text to Alice
df3ad1d (origin/master, origin/HEAD, master) Add Alice
800a947 Add Tutorial Text
```

Es gibt zwei Dinge, die wir hier ändern möchten, die sich für Lernzwecke ein wenig von der vorherigen Sektion über `amend` unterscheiden:

* Fasse beide Änderungen an `Alice.txt` in einem einzigen Commit zusammen.
* Benenne die Dinge konsistent und entferne die _.txt_ aus der Nachricht über `Bob.txt`.

Um die drei neuen Commits zu ändern, wollen wir auf den Commit direkt vor ihnen rebasen. Dieser Commit für mich ist `df3ad1d`, aber wir können ihn auch als den dritten Commit von dem aktuellen _HEAD_ als `HEAD~3` referenzieren.

Um ein _interaktives_ `rebase` zu starten, verwenden wir `git rebase -i {COMMIT}`, also führen wir `git rebase -i HEAD~3` aus.

Was du sehen wirst, ist dein bevorzugter Editor, der etwas wie folgt anzeigt:

```bash
pick 9e06fca Add text to Alice
pick 062ef13 Add Bob.txt
pick 0b22064 Add more text to Alice

# Rebase df3ad1d..0b22064 onto df3ad1d (3 commands)
#
# Commands:
# p, pick = verwende Commit
# r, reword = verwende Commit, aber bearbeite die Commit-Nachricht
# e, edit = verwende Commit, aber halte zum Ändern an
# s, squash = verwende Commit, aber füge ihn in den vorherigen Commit ein
# f, fixup = wie "squash", aber verwirf die Log-Nachricht dieses Commits
# x, exec = führe Befehl (den Rest der Zeile) im Shell aus
# d, drop = entferne Commit
#
# Diese Zeilen können neu angeordnet werden; sie werden von oben nach unten ausgeführt.
#
# Wenn du hier eine Zeile entfernst, GEHT DIESER COMMIT VERLOREN.
#
# Wenn du jedoch alles entfernst, wird das Rebase abgebrochen.
#
# Beachte, dass leere Commits auskommentiert sind
```

Wie immer erklärt `git` alles, was du tun kannst, direkt, wenn du den Befehl aufrufst.

Die _Befehle_, die du wahrscheinlich am häufigsten verwenden wirst, sind `reword`, `squash` und `drop`. (Und `pick`, aber das ist standardmäßig vorhanden.)

Nimm dir einen Moment Zeit, um darüber nachzudenken, was du siehst und was wir tun müssen, um unsere beiden Ziele zu erreichen. Ich werde warten.

Hast du einen Plan? Perfekt!

Bevor wir mit den Änderungen beginnen, beachte, dass die Commits von ältesten zu neuesten aufgelistet sind und somit in die entgegengesetzte Richtung der `git log`-Ausgabe.

Ich starte mit der einfachen Änderung und ermögliche es uns, die Commit-Nachricht des mittleren Commits zu ändern.

```bash
pick 9e06fca Add text to Alice
reword 062ef13 Add Bob.txt
pick 0b22064 Add more text to Alice

# Rebase df3ad1d..0b22064 onto df3ad1d (3 commands)
[...]
```

Jetzt gehen wir dazu über, die beiden Änderungen an `Alice.txt` in einen Commit zusammenzufassen.

Offensichtlich wollen wir den späteren der beiden Commits in den ersten Commit `squashen`, also setzen wir diesen Befehl anstelle von `pick` beim zweiten Commit, der `Alice.txt` ändert. Für mich im Beispiel ist das _0b22064_.

```bash
pick 9e06fca Add text to Alice
reword 062ef13 Add Bob.txt
squash 0b22064 Add more text to Alice

# Rebase df3ad1d..0b22064 onto df3ad1d (3 commands)
[...]
```

Sind wir fertig? Wird das das tun, was wir wollen?

Nein, das wird es nicht. Wie die Kommentare in der Datei uns sagen:

```bash
# s, squash = verwende Commit, aber füge ihn in den vorherigen Commit ein
```

Was wir bisher getan haben, wird die Änderungen des zweiten Alice-Commits mit dem Bob-Commit zusammenführen. Das ist nicht das, was wir wollen.

Eine weitere leistungsstarke Funktion in einem _interaktiven_ `rebase` ist das Ändern der Reihenfolge von Commits.

Wenn du die Kommentare sorgfältig gelesen hast, weißt du bereits, wie das geht: Einfach die Zeilen verschieben!

Glücklicherweise bist du in deinem bevorzugten Texteditor, also geh voran und verschiebe den zweiten Alice-Commit nach dem ersten.

```bash
pick 9e06fca Add text to Alice
squash 0b22064 Add more text to Alice
reword 062ef13 Add Bob.txt

# Rebase df3ad1d..0b22064 onto df3ad1d (3 commands)
[...]
```

Das sollte das Problem lösen, also schließe den Editor, um `git` zu sagen, dass es die Befehle ausführen soll.

Was als nächstes passiert, ist genau wie bei einem normalen `rebase`: Beginnend mit dem Commit, den du beim Start referenziert hast, werden die Commits, die du aufgelistet hast, nacheinander angewendet.

> Im Moment wird es nicht passieren, aber wenn du tatsächliche Code-Änderungen neu anordnest, kann es vorkommen, dass du während des `rebase` auf Konflikte stößt. Schließlich hast du möglicherweise Änderungen durcheinandergebracht, die aufeinander aufbauen.
>
> Einfach [Konflikte lösen](#resolving-conflicts), wie du es normalerweise tun würdest.

Nachdem der erste Commit angewendet wurde, öffnet sich der Editor und ermöglicht dir, eine neue Nachricht für den Commit zu verfassen, der die Änderungen an `Alice.txt` kombiniert. Ich habe den Text beider Commits entfernt und "Add a lot of very important text to Alice" eingegeben.

Nachdem du den Editor geschlossen hast, um diesen Commit abzuschließen, wird er erneut geöffnet, damit du die Nachricht des `Add Bob.txt`-Commits ändern kannst. Entferne das ".txt" und fahre fort, indem du den Editor schließt.

Das war's! Du hast die Geschichte wieder umgeschrieben – diesmal deutlich mehr als beim `amend`-Befehl!

Wenn du dir das `git log` erneut ansiehst, wirst du feststellen, dass es jetzt zwei neue Commits anstelle der drei gibt, die wir vorher hatten. Aber inzwischen weißt du, was ein `rebase` mit den Commits macht und hast das erwartet.

```
> git log --oneline
105177b (HEAD -> interactiveRebase) Add Bob
ed78fa1 Add a lot very important text to Alice
df3ad1d (origin/master, origin/HEAD, master) Add Alice
800a947 Add Tutorial Text
```

### Einen weiter zurückliegenden Commit korrigieren

Jetzt wissen wir, wie man den neuesten Commit einfach `amend`en kann und wie man mit einem interaktiven `rebase` Commits umschreibt.

Es gibt jedoch eine weitere hilfreiche Funktion, die es einfach macht, Commits, die weiter in der Vergangenheit liegen, zu korrigieren: `fixup`-Commits.

Lass uns unser [Beispiel von zuvor](#amending-the-last-commit) wiederherstellen und von dem Haupt-Branch auf einen neuen Branch mit `git checkout -b fixup_commits` wechseln.

Jetzt mache einige Änderungen an `Alice.txt` und füge `Bob.txt` hinzu. Führe dann `git add Alice.txt` aus.

Danach `git commit` mit einer Nachricht wie "Add important things to Alice and Bob".

Und jetzt lass uns eine weitere Änderung an `Alice.txt` vornehmen, `git add Alice.txt` ausführen und einen weiteren `git commit` mit einer Nachricht deiner Wahl machen.

Wenn du dich noch an das `amend`-Beispiel erinnerst, wirst du festgestellt haben, dass wir erneut vergessen haben, die Änderungen an `Bob.txt` in unserem ersten Commit aufzunehmen.

Wenn du weißt, wie man ein interaktives Rebase durchführt, könntest du einfach einen neuen Commit hinzufügen, rebasen, ihn vor den zweiten Commit verschieben und in den ersten squashen.

Aber das ist viel manuelle Arbeit und `git` hat eine praktische Kurzform für das Korrigieren von Commits.

Zuerst lass uns den Hash des ersten Commits herausfinden – ich nutze gerne `git log --oneline`, um schnell den kurzen Hash zu finden:

```sh
> git log --oneline
20e59b5 (HEAD -> fixup_commits) Add some more changes to Alice
7acb4bc Add important things to Alice and Bob
e701c3e (origin/master, origin/HEAD, master) made stashing section a little more understandable (#83)
[...]
```

Also, für mich ist der Commit, dem ich die `Bob.txt`-Änderungen hinzufügen möchte, `7acb4bc`.

Um das zu tun, lass uns `git add Bob.txt` ausführen und dann einen neuen Commit erstellen, der `7acb4bc` korrigiert.

Das machen wir, indem wir einfach `git commit --fixup={der Hash des zu korrigierenden Commits}` ausführen. Für mich wird also `git commit --fixup=7acb4bc` das erledigen.

Nach diesem Schritt sieht mein Log so aus:

```sh
> git log --oneline
8b4b1af (HEAD -> fixup_commits) fixup! Add important things to Alice and Bob
20e59b5 Add some more changes to Alice
7acb4bc Add important things to Alice and Bob
e701c3e (origin/master, origin/HEAD, master) made stashing section a little more understandable (#83)
[...]
```

Wie du sehen kannst, ist dieser neue Commit speziell und seine Nachricht ist einfach die des Commits, den er korrigiert, mit dem Präfix `fixup!`.

Mit solchen `fixup`-Commits in unserer Historie können wir das interaktive Rebase vereinfachen, indem wir `git` sagen, dass es sie für uns `autosquashen` soll.

Wenn wir also jetzt `git rebase -i --autosquash origin/master` ausführen, sieht die Rebase-Eingabeaufforderung so aus:

```sh
pick 7acb4bc Add important things to Alice and Bob
fixup 8b4b1af fixup! Add important things to Alice and Bob
pick 20e59b5 Add some more changes to Alice

# Rebase e701c3e..8b4b1af onto e701c3e (3 commands)
#
# Commands:
# p, pick <commit> = verwende Commit
# r, reword <commit> = verwende Commit, aber bearbeite die Commit-Nachricht
# e, edit <commit> = verwende Commit, aber halte zum Ändern an
# s, squash <commit> = verwende Commit, aber füge ihn in den vorherigen Commit ein
# f, fixup <commit> = wie "squash", aber verwirf die Log-Nachricht dieses Commits
# x, exec <command> = führe Befehl (den Rest der Zeile) im Shell aus
# b, break = halte hier an (setze Rebase später mit 'git rebase --continue' fort)
# d, drop <commit> = entferne Commit
# l, label <label> = benenne den aktuellen HEAD
# t, reset <label> = setze HEAD auf ein Label zurück
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       erstelle einen Merge-Commit mit der Nachricht des ursprünglichen Merge-Commits
# .       (oder der oneline, wenn kein ursprünglicher Merge-Commit angegeben wurde). 
# .       Verwende -c <commit>, um die Commit-Nachricht zu ändern.
#
# Diese Zeilen können neu angeordnet werden; sie werden von oben nach unten ausgeführt.
#
# Wenn du hier eine Zeile entfernst, GEHT DIESER COMMIT VERLOREN.
#
# Wenn du jedoch alles entfernst, wird das Rebase abgebrochen.
#
# Beachte, dass leere Commits auskommentiert sind
```

Wie du sehen kannst, wurde unser `fixup`-Commit an die richtige Stelle unterhalb unseres ersten Commits verschoben und wird mit dem `fixup`-Befehl angewendet – der "stumm" die Änderungen in den vorhergehenden Commit integriert.

Nach dem Rebase sieht unsere Historie so aus:

```sh
> git log --oneline
429c09a (HEAD -> fixup_commits) Add some more changes to Alice
55f424e Add important things to Alice and Bob
e701c3e (origin/master, origin/HEAD, master) made stashing section a little more understandable (#83)
[...]
```

Und der geänderte Commit (`55f424e` für mich) sieht so aus:

```Diff
> git show 55f424e
commit 55f424ee6f7e2ebdc265628aaca9c3f7222b8c47
Author: UnseenWizzard <nico@riedmann.dev>
Date:   Sun Jul 17 12:30:08 2022 +0200

    Add important things to Alice and Bob

diff --git a/Alice.txt b/Alice.txt
index b4006e7..0c0d2c7 100644
--- a/Alice.txt
+++ b/Alice.txt
@@ -1 +1,2 @@
 Hi! I'm Alice. I'm a file someone added to this repository a while ago.
+Then someone added a line.
diff --git a/Bob.txt b/Bob.txt
new file mode 100644
index 0000000..317a2d6
--- /dev/null
+++ b/Bob.txt
@@ -0,0 +1 @@
+Hi, I'm Bob. I was just added on this branch.
```

Und das ist alles, was es über die Verwendung von `fixup`-Commits zu wissen gibt – eine praktische Methode, um die Git-Historie für weiter zurückliegende Commits umzuschreiben, wo `commit --amend` nicht mehr hilft.

<!-- force pushing -->
### Öffentliche Historie: Warum du sie nicht umschreiben solltest und wie du es sicher tun kannst

Wie bereits erwähnt, ist das Ändern von Historie ein äußerst nützlicher Bestandteil eines Workflows, der viele kleine Commits umfasst, während du arbeitest.

Obwohl alle kleinen atomaren Änderungen es dir sehr leicht machen, z.B. zu überprüfen, dass deine Test-Suite nach jeder Änderung weiterhin besteht, und wenn nicht, nur diese spezifischen Änderungen zu entfernen oder zu ändern, sind die 100 Commits, die du gemacht hast, um `HelloWorld.java` zu schreiben, wahrscheinlich nicht etwas, das du mit anderen teilen möchtest.

Wahrscheinlich möchtest du ihnen einige gut formulierte Änderungen mit schönen Commit-Nachrichten zeigen, die deinen Kollegen erklären, was du aus welchem Grund gemacht hast.

Solange all diese kleinen Commits nur in deiner _Dev Environment_ existieren, kannst du `git rebase -i` verwenden und die Historie nach Herzenslust ändern. 

Problematisch wird es, wenn es darum geht, _öffentliche Historie_ zu ändern. Das bedeutet alles, was bereits ins _Remote Repository_ gelangt ist.

An diesem Punkt ist es _öffentlich_ geworden und die Branches anderer Personen könnten auf dieser Historie basieren. Das macht es wirklich zu etwas, mit dem man im Allgemeinen nicht herumspielen möchte.

Der übliche Rat ist: „Ändere niemals öffentliche Historie!“ und während ich das hier wiederhole, muss ich zugeben, dass es eine beträchtliche Anzahl von Fällen gibt, in denen du trotzdem öffentliche Historie umschreiben möchtest.

In all diesen Fällen ist diese Historie jedoch nicht wirklich _öffentlich_. Du möchtest sicherlich keine Historie im _master_-Branch eines Open-Source-Projekts oder in einem Branch wie dem _release_-Branch deines Unternehmens umschreiben.

Wo du Historie umschreiben möchtest, sind Branches, die du nur geteilt hast, um sie mit einigen Kollegen zu teilen.

Vielleicht machst du trunk-based development, möchtest aber etwas teilen, das noch nicht einmal kompiliert, daher möchtest du das offensichtlich nicht absichtlich in den Hauptbranch stellen. 
Oder du hast einen Workflow, bei dem du Feature-Branches teilst.

Besonders bei Feature-Branches hoffentlich `rebasest` du sie regelmäßig auf den aktuellen _master_. Aber wie wir wissen, fügt ein `git rebase` die Commits unseres Branches als _neue_ Commits oben auf das, worauf wir sie stützen, hinzu. Dies ändert die Historie. Und im Falle eines gemeinsam genutzten Feature-Branches ändert es die _öffentliche Historie_. 

Was sollten wir also tun, wenn wir dem Mantra „Ändere niemals öffentliche Historie“ folgen?

Sollten wir unseren Branch niemals rebasen und hoffen, dass er am Ende noch in _master_ gemergt wird?

Keine geteilten Feature-Branches verwenden?

Zugegeben, letzteres ist tatsächlich eine vernünftige Antwort, aber möglicherweise kannst du das trotzdem nicht tun. Daher bleibt dir nur, das Umschreiben der _öffentlichen Historie_ zu akzeptieren und die geänderte Historie ins _Remote Repository_ zu `pushen`.

Wenn du einfach `git push` machst, wirst du darauf hingewiesen, dass du das nicht tun darfst, da dein _lokaler_ Branch vom _remote_ Branch abgewichen ist.

Du musst die Änderungen mit `--force` pushen und den Remote-Branch durch deine lokale Version überschreiben.

Da ich das so suggestiv hervorgehoben habe, bist du wahrscheinlich bereit, jetzt `git push --force` auszuprobieren. Wenn du jedoch öffentliche Historie sicher umschreiben möchtest, solltest du das wirklich nicht tun!

Du bist viel besser beraten, den vorsichtigeren Bruder von `--force`, nämlich `--force-with-lease`, zu verwenden!

`--force-with-lease` überprüft, ob deine _lokale_ Version des _remote_ Branches und der tatsächliche _remote_ Branch übereinstimmen, bevor er `push`-t. 

So kannst du sicherstellen, dass du keine Änderungen von jemandem versehentlich überschreibst, die dieser möglicherweise während deiner Historienänderung `push`te!

![Was passiert bei einem push --force-with-lease](img/force_push.png)

Und damit hinterlasse ich dir ein leicht verändertes Mantra:

_Ändere öffentliche Historie nur, wenn du dir wirklich sicher bist, was du tust. Und wenn du es tust, sei sicher und benutze force-with-lease._


## Historie lesen

Wenn du die Unterschiede zwischen den Bereichen in deiner _Dev Environment_ – insbesondere dem _Local Repository_ – und wie Commits und die Historie funktionieren, verstehst, sollte dir das `rebase`-Kommando nicht mehr beängstigend erscheinen.

Trotzdem gehen manchmal Dinge schief. Du hast vielleicht ein `rebase` durchgeführt und versehentlich die falsche Version einer Datei beim Lösen eines Konflikts akzeptiert.

Jetzt hast du statt des von dir hinzugefügten Features nur noch die hinzugefügte Zeile Logging deiner Kollegen in einer Datei.

Glücklicherweise hat `git` eine eingebaute Sicherheitsfunktion namens _Reference Logs_ oder `reflog`, die dir hilft.

Immer wenn ein _Reference_ wie der Kopf eines Branches in deinem _Local Repository_ aktualisiert wird, wird ein Eintrag im _Reference Log_ hinzugefügt.

Es gibt also einen Verlauf von jedem Mal, wenn du einen `commit` machst, aber auch von jedem `reset` oder wenn du sonstwie den `HEAD` verschiebst.

Nachdem du dieses Tutorial bisher gelesen hast, siehst du wahrscheinlich, wie das nützlich sein könnte, wenn wir ein `rebase` vermasselt haben, oder?

Wir wissen, dass ein `rebase` den `HEAD` unseres Branches auf den Punkt verschiebt, auf dem wir ihn basieren, und dann unsere Änderungen anwendet. Ein interaktives `rebase` funktioniert ähnlich, könnte aber Dinge wie _squashing_ oder _rewording_ dieser Commits tun.

Falls du noch nicht auf dem Branch bist, auf dem wir [interaktives Rebase](#interactive-rebase) geübt haben, wechsle wieder dorthin, da wir gleich noch mehr üben werden.

Lass uns den `reflog` der Dinge ansehen, die wir auf diesem Branch gemacht haben, indem du – du hast es erraten – `git reflog` ausführst.

Du wirst wahrscheinlich eine Menge Ausgabe sehen, aber die ersten paar Zeilen oben sollten etwa so aussehen:

```bash
> git reflog
105177b (HEAD -> interactiveRebase) HEAD@{0}: rebase -i (finish): returning to refs/heads/interactiveRebase
105177b (HEAD -> interactiveRebase) HEAD@{1}: rebase -i (reword): Add Bob
ed78fa1 HEAD@{2}: rebase -i (squash): Add a lot very important text to Alice
9e06fca HEAD@{3}: rebase -i (start): checkout HEAD~3
0b22064 HEAD@{4}: commit: Add more text to Alice
062ef13 HEAD@{5}: commit: Add Bob.txt
9e06fca HEAD@{6}: commit: Add text to Alice
df3ad1d (origin/master, origin/HEAD, master) HEAD@{7}: checkout: moving from master to interactiveRebase
```

Da ist es. Jede einzelne Aktion, die wir durchgeführt haben, vom Wechseln des Branches bis zum `rebase`.

Es ist ziemlich cool, die Dinge zu sehen, die wir gemacht haben, aber es ist an sich nutzlos, wenn wir irgendwo einen Fehler gemacht haben, wenn es nicht die Referenzen am Anfang jeder Zeile wären.

Wenn du die `reflog`-Ausgabe mit dem `log` vergleichst, den wir das letzte Mal angesehen haben, siehst du, dass diese Punkte auf Commit-Referenzen verweisen, und wir können sie genauso verwenden.

Angenommen, wir wollten den `rebase` tatsächlich nicht durchführen. Wie bekommen wir die Änderungen wieder weg?

Wir verschieben den `HEAD` auf den Punkt vor dem Start des `rebase` mit einem `git reset 0b22064`.

> `0b22064` ist in meinem Fall der Commit vor dem `rebase`. Allgemeiner kannst du ihn auch als _HEAD vor vier Änderungen_ mit `HEAD@{4}` referenzieren. Beachte, dass du, falls du zwischendurch Branches gewechselt oder etwas anderes getan hast, das einen Log-Eintrag erstellt, eine höhere Nummer haben könntest.

Wenn du dir jetzt den `log` ansiehst, wirst du den ursprünglichen Zustand mit den drei einzelnen Commits wiederhergestellt sehen.

Aber nehmen wir an, wir stellen jetzt fest, dass das nicht das war, was wir wollten. Der `rebase` ist in Ordnung, wir mögen nur nicht, wie wir die Nachricht des Bob-Commits geändert haben.

Wir könnten einfach ein weiteres `rebase -i` im aktuellen Zustand durchführen, genau wie wir es ursprünglich gemacht haben.

Oder wir verwenden den `reflog` und springen zurück zum Zeitpunkt nach dem `rebase` und `amend` den Commit von dort aus.

Aber du weißt jetzt, wie man das eine oder andere macht, also lasse ich dich das selbst ausprobieren. Und zusätzlich weißt du auch, dass der `reflog` dir erlaubt, die meisten Dinge rückgängig zu machen, die du möglicherweise versehentlich gemacht hast.
