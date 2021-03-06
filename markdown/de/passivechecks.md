 ![Icinga](../images/logofullsize.png "Icinga") 

* * * * *

5.7. Passive Prüfungen (Passive Checks)
---------------------------------------

5.7.1. [Einführung](passivechecks.md#introduction)

5.7.2. [Einsatzmöglichkeiten für passive
Prüfungen](passivechecks.md#usecases)

5.7.3. [Wie passive Prüfungen arbeiten](passivechecks.md#howitworks)

5.7.4. [Passive Prüfungen aktivieren](passivechecks.md#enable)

5.7.5. [Übermitteln von passiven
Service-Prüfungsergebnissen](passivechecks.md#servicecheckresults)

5.7.6. [Übermitteln von passiven
Host-Prüfungsergebnissen](passivechecks.md#hostcheckresults)

5.7.7. [Passive Prüfungen und
Host-Zustände](passivechecks.md#hoststates)

5.7.8. [Übermitteln von passiven Prüfungsergebnissen von entfernten
Hosts](passivechecks.md#checkresultsfromremotehosts)

### 5.7.1. Einführung

In den meisten Fällen werden Sie Icinga nutzen, um Ihre Hosts und
Services mit Hilfe von regelmäßig geplanten [aktiven
Prüfungen](activechecks.md "5.6. Aktive Prüfungen (Active Checks)") zu
überwachen. Aktive Prüfungen können genutzt werden, um ein Gerät oder
Service gelegentlich "abzufragen". Icinga unterstützt auch einen Weg,
Hosts und Services passiv zu überwachen statt aktiv. Die Hauptmerkmale
von passiven Prüfungen sind wie folgt:



Der Hauptunterschied zwischen aktiven und passiven Prüfungen ist, dass
aktive Prüfungen von Icinga veranlasst und ausgeführt werden, während
passive Prüfungen von externen Applikationen durchgeführt werden.

### 5.7.2. Einsatzmöglichkeiten für passive Prüfungen

passive Prüfungen sind nützlich, um Services zu überwachen, die



Beispiele für asynchrone Services, bei denen sich eine passive
Überwachung lohnt, sind u.a. SNMP-Traps und Sicherheits-Alarme. Sie
wissen nie, wie viele (falls überhaupt) Traps oder Alarme Sie innerhalb
eines vorgegebenen Zeitfensters erhalten, so dass es nicht sinnvoll ist,
ihren Status alle paar Minuten zu überwachen.

Passive Prüfungen werden auch genutzt, um
[verteilte](distributed.md "7.6. Verteilte Überwachung") oder
[redundante](redundancy.md "7.7. Redundante und Failover-Netzwerk-Überwachung")
Überwachungsinstallationen zu konfigurieren.

### 5.7.3. Wie passive Prüfungen arbeiten

![](../images/passivechecks.png)

Hier nun mehr Details, wie passive Prüfungen arbeiten...





Die Verarbeitung von aktiven und passiven Prüfungsergebnissen ist
tatsächlich identisch. Dies erlaubt eine nahtlose Integration von
externen Applikationen mit Icinga.

### 5.7.4. Passive Prüfungen aktivieren

Um passive Prüfungen in Icinga zu aktivieren, müssen Sie folgendes tun:



Wenn Sie die Verarbeitung von passiven Prüfungen global deaktivieren
wollen, setzen Sie die
[accept\_passive\_service\_checks](configmain.md#configmain-accept_passive_service_checks)-Direktive
auf 0.

Wenn Sie die Verarbeitung von passiven Prüfungen nur für ein paar Hosts
oder Services deaktivieren wollen, nutzen Sie die
*passive\_checks\_enabled*-Direktive in den Host- und/oder
Service-Definitionen.

### 5.7.5. Übermitteln von passiven Service-Prüfungsergebnissen

Externe Applikationen können passive Prüfungsergebisse an Icinga
übermitteln, indem sie ein PROCESS\_SERVICE\_CHECK\_RESULT [external
command](extcommands.md "7.1. Externe Befehle") in das "external
command file" schreiben.

Das Format des Befehls lautet wie folgt:

</code></pre> 
 [<Zeitstempel>] PROCESS_SERVICE_CHECK_RESULT;<host_name>;<svc_description>;<return_code>;<plugin_output>
</code></pre>

wobei...






![](../images/note.gif) Anmerkung: ein Service muss in Icinga definiert
sein, bevor Sie passive Prüfungen für ihn abliefern können! Icinga wird
alle Prüfergebnisse für Services ignorieren, die nicht konfiguriert
waren, bevor es das letzte Mal (neu) gestartet wurde.

![](../images/tip.gif) Ein Beispiel-Shell-Script, wie man passive
Service-Prüfungsergebnisse an Icinga übermittelt, finden Sie in der
Dokumentation zu [sprunghaften
Services](volatileservices.md "7.4. sprunghafte Services").

### 5.7.6. Übermitteln von passiven Host-Prüfungsergebnissen

Externe Applikationen können passive Host-Prüfungsergebisse an Icinga
übermitteln, indem sie ein PROCESS\_HOST\_CHECK\_RESULT [external
command](extcommands.md "7.1. Externe Befehle") in das "external
command file" schreiben.

Das Format des Befehls lautet wie folgt:

</code></pre> 
 [<timestamp>] PROCESS_HOST_CHECK_RESULT;<host_name>;<host_status>;<plugin_output>
</code></pre>

wobei...





![](../images/note.gif) Anmerkung: ein Host muss in Icinga definiert
sein, bevor Sie passive Prüfungen für ihn abliefern können! Icinga wird
alle Prüfergebnisse für Hosts ignorieren, die nicht konfiguriert waren,
bevor es das letzte Mal (neu) gestartet wurde.

### 5.7.7. Passive Prüfungen und Host-Zustände

nicht festzustellen, ob der Host DOWN oder UNREACHABLE ist. Statt dessen
nimmt Icinga das passive Prüfergebnis als den wahren Status des Hosts
und versucht nicht, den wahren Host-Status mit Hilfe der
[Erreichbarkeitslogik](networkreachability.md "5.10. Ermitteln des Zustands und der Erreichbarkeit von Netzwerk-Hosts")
zu ermitteln. Dies kann Probleme verursachen, wenn Sie passive Prüfungen
von einem entfernten Host übermitteln oder Sie ein [verteiltes
Überwachungs-Setup](distributed.md "7.6. Verteilte Überwachung")
haben, in dem Eltern/Kind-Verhältnisse unterschiedlich sind.

Sie können Icinga anweisen, die passiven Prüfergebnisse
DOWN/UNREACHABLE-Zustände mit Hilfe der
[translate\_passive\_host\_checks](configmain.md#configmain-translate_passive_host_checks)-Variable
in ihre "sauberen" Zustände zu übersetzen. Mehr Informationen wie dies
funktioniert, finden Sie
[hier](passivestatetranslation.md "7.22. Passive Host-Zustandsübersetzung").

![](../images/note.gif) Anmerkung: Passive Host-Prüfungen werden
normalerweise als [HARD-Zustände](statetypes.md "5.8. Statustypen")
behandelt, falls nicht die
[passive\_host\_checks\_are\_soft](configmain.md#configmain-passive_host_checks_are_soft)-Option
aktiviert ist.

### 5.7.8. Übermitteln von passiven Prüfungsergebnissen von entfernten Hosts

![](../images/nsca.png)

Wenn eine Applikation, die sich auf dem gleichen Host wie Icinga
befindet, passive Host- oder Service-Prüfungsergebnisse sendet, kann es
die Ergebisse einfach direkt in das "external command file" schreiben
wie oben skizziert. Allerdings können entfernte Hosts das nicht so
einfach tun.

Um es entfernten Hosts zu erlauben, passive Prüfungsergebnisse an den
überwachenden Host zu senden, hat Ethan Galstad das
[NSCA](addons.md#addons-nsca)-Addon entwickelt. Das NSCA-Addon besteht
aus einem Daemon, der auf dem Icinga-Host läuft und einem Client, der
auf entfernten Hosts ausgeführt wird. Der Daemon lauscht auf
Verbindungen von entfernten Hosts, führt mit den Ergebnissen einige
grundlegende Gültigkeitsprüfungen durch und schreibt die Prüfergebnisse
direkt in das "external command file" (wie oben beschrieben). Mehr
Informationen über das NSCA-Addon finden Sie
[hier](addons.md#addons-nsca).

* * * * *


© 1999-2009 Ethan Galstad, 2009-2015 Icinga Development Team,
http://www.icinga.org
