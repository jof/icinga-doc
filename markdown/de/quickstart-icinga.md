 ![Icinga](../images/logofullsize.png "Icinga") 

* * * * *

2.4. Icinga-Schnellstart auf Linux
----------------------------------

2.4.1. [Einführung](quickstart-icinga.md#introduction)

2.4.2. [Voraussetzungen](quickstart-icinga.md#prerequisites)

2.4.3. [Installation der Pakete](quickstart-icinga.md#installpackages)

2.4.4. [Benutzerinformationen
erstellen](quickstart-icinga.md#createaccount)

2.4.5. [Icinga und die Plugins
herunterladen](quickstart-icinga.md#downloadicingaandplugins)

2.4.6. [Icinga kompilieren und
installieren](quickstart-icinga.md#compileinstall)

2.4.7. [Anpassen der
Konfiguration](quickstart-icinga.md#customiseconfig)

2.4.8. [Installieren und konfigurieren des klassischen
Web-Interface](quickstart-icinga.md#configclassicui)

2.4.9. [Kompilieren und installieren der Monitoring
Plugins](quickstart-icinga.md#compileinstallplugins)

2.4.10. [Anpassen der
SELinux-Einstellungen](quickstart-icinga.md#selinuxsettings)

2.4.11. [Icinga starten](quickstart-icinga.md#starticinga)

2.4.12. [Anmelden am klassischen
Web-Interface](quickstart-icinga.md#loginclassicui)

2.4.13. [Andere Anpassungen](quickstart-icinga.md#othermods)

2.4.14. [Fertig](quickstart-icinga.md#done)

### 2.4.1. Einführung

![[Anmerkung]](../images/note.png)

Anmerkung

Anstatt Icinga von Grund auf zu installieren möchten Sie vielleicht ein
Paket benutzen, das es möglicherweise für Ihr Betriebssystem gibt. Bitte
werfen Sie einen Blick auf die [Tabelle der
Pakete](https://www.icinga.org/download/packages).

Bitte bedenken Sie, dass die Upstream-Pakete evtl. relativ alt sind, so
dass die Verwendung von Backports-Paketen ein Weg ist, eine aktuelle
Version zu bekommen. Bitte werfen Sie einen Blick auf die
(englischsprachigen) Wiki-Artikel für detaillierte Informationen:




Falls Sie aus den Sourcen installieren möchten, dann benutzen Sie bitte
die offiziellen Release-Tarballs.

![[Wichtig]](../images/important.png)

Wichtig

Bitte benutzen Sie keine GIT-Snapshots, solange Sie kein Problem haben,
das in der aktuellen Entwicklerversion ggf. gelöst ist.

Diese Schnellstartanleitung ist dazu gedacht, Ihnen einfache Anweisungen
zu liefern, wie Sie Icinga innerhalb von 20 Minuten aus dem Quellcode
installieren und Ihren lokalen Rechner damit überwachen.

lediglich die Grundlagen, die für 95% aller Benutzer funktionieren, die
anfangen wollen.

Diese Anleitung enthält momentan Anweisungen für drei verschiedene
Linux-Distributionen: [Fedora](http://fedoraproject.org/),
[Ubuntu](http://www.ubuntu.com/) and
[openSuSE](http://www.opensuse.org/). Ähnliche Distributionen werden
wahrscheinlich auch funktionieren, darunter
[RedHat](http://www.redhat.com/), [CentOS](http://www.centos.org/),
[Debian](http://www.debian.org/) und
[SLES](http://www.novell.com/products/server/).

![[Wichtig]](../images/important.png)

Wichtig

Wenn Sie planen, eine Datenbank zusammen mit IDOUtils zu nutzen oder
wenn Sie das neue Web-Interface einsetzen möchten, dann lesen Sie statt
dessen die [Schnellstartanleitung mit
IDOUtils](quickstart-idoutils.md "2.6. Icinga-Schnellstart mit IDOUtils")!

**Was dabei herauskommt**

Wenn Sie diesen Anweisungen folgen, werden Sie am Ende folgendes haben:




### 2.4.2. Voraussetzungen

Während einiger Teile der Installation benötigen Sie **root**-Zugang zu
Ihrer Maschine.

Stellen Sie sicher, dass die folgenden Pakete installiert sind, bevor
Sie fortfahren.





**Optional**

Zu irgendeiner Zeit möchten Sie wahrscheinlich SNMP-basierte Prüfungen
verwenden, so dass es eine gute Idee ist, die benötigten Pakete gleich
zu installieren. Anderenfalls werden die Plugins nicht kompiliert und
sind nicht verfügbar, wenn Sie diese brauchen.

### 2.4.3. Installation der Pakete

Sie können diese Pakete mit Hilfe der folgenden Befehle installieren
(als root oder mit sudo).

![[Anmerkung]](../images/note.png)

Anmerkung

Unglücklicherweise ändern sich manchmal die Paketnamen zwischen
verschiedenen Ausgaben der gleichen Distribution, so dass Sie die
Suchoption Ihres Paket-Managers nutzen sollten, falls Sie die
Fehlermeldung bekommen, dass eins der Pakete nicht gefunden wurde.




*Fedora / RedHat / CentOS*

<pre><code>
 #> yum install httpd gcc glibc glibc-common gd gd-devel
 #> yum install libjpeg libjpeg-devel libpng libpng-devel
 #> yum install net-snmp net-snmp-devel net-snmp-utils
</code></pre>

![[Anmerkung]](../images/note.png)

Anmerkung

ggf. sind libjpeg-turbo bzw. libjpeg-turbo-devel zu installieren

*Debian / Ubuntu*

<pre><code>
 #> apt-get install apache2 build-essential libgd2-xpm-dev
 #> apt-get install libjpeg62 libjpeg62-dev libpng12 libpng12-dev
 #> apt-get install snmp libsnmp5-dev
</code></pre>

![[Anmerkung]](../images/note.png)

Anmerkung

Die Zahlen \<62/12\> können je nach Distribution abweichen.

![[Anmerkung]](../images/note.png)

Anmerkung

Ab Debian 6.0/Ubuntu 10.10 heißt das Paket libpng12-0, der Name des
dev-Pakets ändert sich nicht.

*openSuSE / SLES*

Bitte nutzen Sie YaST für die Installation der Pakete gd, gd-devel,
libjpeg, libjpeg-devel, libpng, libpng-devel und -optional- net-snmp,
net-snmp-devel und perl-Net-SNMP.

Die Nutzung von zypper sollte ebenfalls funktionieren:

<pre><code>
 #> zypper install gd gd-devel libjpeg libjpeg-devel libpng libpng-devel
 #> zypper install net-snmp net-snmp-devel perl-Net-SNMP
</code></pre>

![[Anmerkung]](../images/note.png)

Anmerkung

Abhängig von der Softwareauswahl bei der Installation des
Betriebssystems benötigen Sie evtl. weitere Pakete (z.B. apache2, gcc).
Die devel-Pakete sind ggf. auf den SDK-DVDs zu finden.

### 2.4.4. Benutzerinformationen erstellen

Werden Sie zum root-Benutzer.

<pre><code>
 $> su -l
</code></pre>

Erstellen Sie ein neues Benutzerkonto *icinga* und vergeben Sie ein
Passwort:

<pre><code>
 #> /usr/sbin/useradd -m icinga
</code></pre>

Bei einigen Distributionen müssen Sie die Gruppe in einem gesonderten
Schritt anlegen:

<pre><code>
 #> /usr/sbin/groupadd icinga
</code></pre>

Damit Sie über das klassische Web-Interface Befehle an Icinga senden
können, legen Sie noch eine neue Gruppe icinga-cmd an und fügen Sie den
Webbenutzer und den Icingabenutzer dieser Gruppe hinzu.

<pre><code>
 #> /usr/sbin/groupadd icinga-cmd
 #> /usr/sbin/usermod -a -G icinga-cmd icinga
 #> /usr/sbin/usermod -a -G icinga-cmd www-data
</code></pre>

(oder www, wwwrun, apache je nach Distribution)

![[Anmerkung]](../images/note.png)

Anmerkung

Bei einigen usermod-Versionen (z.B. bei OpenSuSE 11 bzw. SLES 11) fehlt
die Option -a. In diesen Fällen kann sie entfallen.

![[Anmerkung]](../images/note.png)

Anmerkung

Solaris unterstützt nur Gruppennamen bis max. 8 Zeichen, verwenden Sie
icingcmd anstelle von icinga-cmd.

### 2.4.5. Icinga und die Plugins herunterladen

Wechseln Sie in Ihr lokales Source-Verzeichnis, z.B. /usr/src

<pre><code>
 #> cd /usr/src
</code></pre>

Laden Sie die Sourcen von der [Icinga Website](http://www.icinga.org/).

Vergessen Sie nicht die [Monitoring
Plugins](https://www.monitoring-plugins.org).

### 2.4.6. Icinga kompilieren und installieren

Entpacken Sie das Icinga-Archiv (oder wechseln Sie in den GIT Snapshot)

<pre><code>
 #> cd /usr/src/
 #> tar xvzf icinga-1.13.tar.gz
 #> cd icinga-1.13
</code></pre>

Führen Sie das Icinga-configure-Script aus. Durch die Nutzung des
--help-Flags erhalten Sie Hilfe zu den Optionen.

![[Anmerkung]](../images/note.png)

Anmerkung

Beginnend mit Icinga 1.9 hat sich der Default geändert, so dass Sie die
Kompilation der IDOUtils explizit verhindern müssen.

<pre><code>
 #> ./configure --with-command-group=icinga-cmd --disable-idoutils
</code></pre>

![[Anmerkung]](../images/note.png)

Anmerkung

Beginnend mit Apache 2.4 hat sich der Standard-Konfigurationsordner von
`/etc/apache2/conf.d` in
`/etc/apache2/conf-available` geändert, so dass Sie abhängig
von Ihrer Distribution (testing-versionen von Debian / Ubuntu) dem
Aufruf von configure diese Option hinzufügen müssen

</code></pre> 
#> ./configure --with-httpd-conf=/etc/apache2/conf-available
</code></pre>

Aktuelle/kommende Distributionen (RedHat/CentOS 7, Fedora 20, Debian
8/Jessie, Gentoo, etc.) unterstützendie Nutzung von systemd statt des
SysVinit Systems.

Icinga 1.x enthält bereits die notwendigen Patches mit den
erforderlichen systemd-Dateien und die RPMs installieren sie ebenfalls.
Die Source-Installation erfordert ggf. etwa folgendes

</code></pre> 
#> ./configure [...] --with-systemd-unit-dir=/usr/lib/systemd/system --with-systemd-sysconfig-dir=/etc/sysconfig
#> make install-systemd
</code></pre>

Dann aktivieren Sie den systemd-Service und starten Icinga

</code></pre> 
#> systemctl enable icinga
#> systemctl start icinga
</code></pre>

Status

</code></pre> 
#> systemctl status icinga
</code></pre>

![[Anmerkung]](../images/note.png)

Anmerkung

Es gibt für systemd keine Unterstützung für den
"checkconfig/show-errors"-Parameter, wie es für SysVinit der Fall ist.

Kompilieren Sie den Icinga-Source-Code. Um mögliche Optionen zu sehen,
rufen Sie lediglich "make" auf.

<pre><code>
 #> make all
</code></pre>

Installieren Sie die Binaries, das Init-Script,
Beispiel-Konfigurationsdateien, Beispiel-Eventhandler und setzen Sie die
Berechtigungen für das External-Command-Verzeichnis.

<pre><code>
 #> make install
 #> make install-init
 #> make install-config
 #> make install-eventhandlers
</code></pre>

oder kürzer

<pre><code>
 #> make fullinstall
 #> make install-config
</code></pre>

![[Anmerkung]](../images/note.png)

Anmerkung

`make install-config` ist NICHT mehr in
`make fullinstall `enthalten, um ein versehentliches
Überschreiben der Konfigurationsdateien zu verhindern.

![[Anmerkung]](../images/note.png)

Anmerkung

Mit `make install-eventhandlers` werden einige
Beispiel-Eventhandler installiert. Das ist lediglich in
`make fullinstall` enthalten, um ein versehentliches
Überschreiben der Dateien zu verhindern.

Die Icinga-API wird beim Aufruf von "make install" installiert, wenn Sie
nur die Icinga-API (nach)installieren möchten, nutzen Sie:

<pre><code>
 # make install-api
</code></pre>

Die Icinga-API ist Voraussetzung für das Icinga-Web-Interface (nicht für
die klassische Ansicht!).

tun...

### 2.4.7. Anpassen der Konfiguration

Beispiel-Konfigurationsdateien werden durch

<pre><code>
 #> make install-config
</code></pre>

in /usr/local/icinga/etc/ installiert. Nun fehlt nur noch eine Änderung,
bevor Sie fortfahren können...

Ändern Sie die
*/usr/local/icinga/etc/objects/contacts.cfg*-Konfigurationsdatei mit
Ihrem bevorzugten Editor und passen die e-Mail-Adresse in der
*icingaadmin*-Kontaktdefinition an, so dass sie die Adresse enthält, die
im Falle von Alarmen benachrichtigt werden soll.

<pre><code>
 #> vi /usr/local/icinga/etc/objects/contacts.cfg
</code></pre>

### 2.4.8. Installieren und konfigurieren des klassischen Web-Interface

Icinga stellt das klassische Webinterface zur Verfügung ("Classic Web",
"die CGIs"). Sie können dieses wie folgt installieren:

<pre><code>
 #> make cgis
 #> make install-cgis
 #> make install-html
</code></pre>

Wenn Sie (zusätzlich) das neue Icinga Web installieren wollen, lesen Sie
bitte [Installation des
Web-Interface](icinga-web-scratch.md "6.5. Installation des Icinga Web Frontend").

Installieren Sie die Icinga-Web-Konfigurationsdatei im Apache
conf.d-Verzeichnis (bzw. conf-available ab Apache 2.4).

<pre><code>
 #> make install-webconf
</code></pre>

![[Anmerkung]](../images/note.png)

Anmerkung

Ab Icinga 1.9 installiert der Befehl 'make install-webconf-auth'
zusätzlich die Datei `htpasswd.users`, die
Anmeldeinformationen für den Benutzer *icingaadmin* enthält, so dass Sie
den nächsten Schritt überspringen können. Das Passwort lautet
*icingaadmin*.

![[Anmerkung]](../images/note.png)

Anmerkung

Beginnend mit Apache 2.4 (testing-Versionen von Debian / Ubuntu) müssen
Sie die Konfiguration aktivieren

</code></pre> 
#> a2enconf icinga
</code></pre>

Tun Sie das für das CGI-Modul

</code></pre> 
#> a2enmod cgi
</code></pre>

Legen Sie ein *icingaadmin*-Konto an, um sich am klassischen
Web-Interface anmelden zu können. Merken Sie sich das Passwort, das Sie

<pre><code>
 #> htpasswd -c /usr/local/icinga/etc/htpasswd.users icingaadmin
</code></pre>

![[Anmerkung]](../images/note.png)

Anmerkung

Abhängig von der Apache-Version müssen Sie ggf. *htpasswd2* verwenden.

Wenn Sie das Passwort später ändern oder einen weiteren Benutzer
hinzufügen möchten, verwenden Sie den folgenden Befehl:

<pre><code>
 #> htpasswd /usr/local/icinga/etc/htpasswd.users <USERNAME>
</code></pre>

Starten Sie Apache neu, damit die Änderungen wirksam werden.

*Fedora/RedHat/CentOS*

<pre><code>
 #> service httpd restart
</code></pre>

*Debian / Ubuntu / openSuSE*

<pre><code>
 #> /etc/init.d/apache2 reload
</code></pre>

![[Anmerkung]](../images/note.png)

Anmerkung

Prüfen Sie die Implementierung der verbesserten CGI-Sicherheitsmaßnahmen
wie
[hier](cgisecurity.md "8.2. Verbesserte Classic UI Modul-Sicherheit und Authentifizierung")
beschrieben, um sicherzustellen, dass Ihre
Web-Authentifizierungsinformationen nicht kompromittiert werden.

### 2.4.9. Kompilieren und installieren der Monitoring Plugins

Entpacken Sie die Monitoring Plugins-Quellcode-Archivdatei.

<pre><code>
 #> cd /usr/src
 #> tar xzf nagios-plugins-2.1.tar.gz
 #> cd nagios-plugins-2.1
</code></pre>

Kompilieren und installieren Sie die Plugins.

<pre><code>
 #> ./configure --prefix=/usr/local/icinga \
 #> make
 #> make install
</code></pre>

### 2.4.10. Anpassen der SELinux-Einstellungen

RHEL und ähnliche Distributionen wie Fedora oder CentOS werden mit
installiertem SELinux (Security Enhanced Linux) ausgeliefert und laufen
im "Enforcing"-Modus. Dies kann zu "Internal Server Error"-Fehlern
führen, wenn Sie versuchen, die Icinga-CGIs aufzurufen.

Schauen Sie, ob SELinux im Enforcing-Modus läuft.

</code></pre> 
 #> getenforce
</code></pre>

Setzen Sie SELinux in den "Permissive"-Modus.

</code></pre> 
 #> setenforce 0
</code></pre>

Damit diese Änderung dauerhaft wird, müssen Sie diese Einstellung in
*/etc/selinux/config* anpassen und das System neustarten.

Statt SELinux zu deaktivieren oder es in den Permissive-Modus zu
versetzen, können Sie den folgenden Befehl benutzen, um die CGIs im
Enforcing/Targeted-Modus laufen zu lassen. Der *semanage*-Befehl wird
automatisch Einträge in die Datei
`/etc/selinux/targeted/contexts/files/file_contexts.local`
einfügen:

<pre><code>
 #> semanage fcontext -a -t httpd_sys_script_exec_t '/usr/local/icinga/sbin(/.*)?'
 #> semanage fcontext -a -t httpd_sys_content_t '/usr/local/icinga/share(/.*)?'
 #> semanage fcontext -a -t httpd_sys_rw_content_t '/usr/local/icinga/var(/.*)?'
</code></pre>

Sobald Sie die notwendigen Kontexte definiert haben, müssen die Einträge
aktiviert werden.

<pre><code>
 #> chcon -R /usr/local/icinga/sbin
 #> chcon -R /usr/local/icinga/share
 #> chcon -R /usr/local/icinga/var
</code></pre>

Einzelheiten finden Sie unter
[http://www.linuxquestions.org/questions/blog/sag47-492023/selinux-and-icinga-34926/](http://www.linuxquestions.org/questions/blog/sag47-492023/selinux-and-icinga-34926/)
(englisch).

### 2.4.11. Icinga starten

Fügen Sie Icinga zu der Liste der System-Services hinzu und sorgen Sie
für einen automatischen Start, wenn das System hochfährt (stellen Sie
sicher, dass Sie vorher das Init-Script installiert haben).

*Fedora / RedHat / CentOS / openSuSE*

<pre><code>
</code></pre>

*Debian / Ubuntu*

<pre><code>
 #> update-rc.d icinga defaults
</code></pre>

Überprüfen Sie die Icinga-Beispielkonfigurationsdateien.

<pre><code>
 #> /usr/local/icinga/bin/icinga -v /usr/local/icinga/etc/icinga.cfg
</code></pre>

Anstatt die Pfade für das Binary und die Konfigurationsdatei anzugeben
können Sie auch den folgenden Befehl eingeben:

<pre><code>
 #> /etc/init.d/icinga show-errors
</code></pre>

Die Ausführung ergibt einen OK-Meldung, wenn alles in Ordnung ist, oder
eine Reihe von Zeilen, die zeigen, wo der/die Fehler zu finden sind.

Wenn es dabei keine Fehler gibt, starten Sie Icinga.

*Fedora / openSuSE*

<pre><code>
 #> service icinga start
</code></pre>

*Debian / Ubuntu*

<pre><code>
 #> /etc/init.d/icinga start
</code></pre>

### 2.4.12. Anmelden am klassischen Web-Interface

Sie sollten nun auf das klassische Icinga-Web-Interface zugreifen
können. Sie werden nach dem Benutzernamen (*icingaadmin*) und Passwort
gefragt, das Sie vorhin angegeben haben.

<pre><code>
 http://localhost/icinga/
</code></pre>

oder

<pre><code>
 http://yourdomain.com/icinga/
</code></pre>

Klicken Sie auf den "Service Detail"-Verweis in der Navigationsleiste,
um Details darüber zu erhalten, was auf Ihrer lokalen Maschine überwacht
wird. Es wird ein paar Minuten dauern, bis Icinga alle mit Ihrer
Maschine verbundenen Services geprüft hat, weil die Prüfungen über eine
gewisse Zeit verteilt werden.

### 2.4.13. Andere Anpassungen

Stellen Sie sicher, dass die Firewall-Einstellungen Ihrer Maschine einen
Zugriff auf das klassische Web-Interface ermöglichen, wenn Sie von
anderen Rechnern darauf zugreifen wollen.

<pre><code>
 #> iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
</code></pre>

Die Konfiguration von e-Mail-Benachrichtigungen ist nicht Gegenstand
dieser Anleitung. Icinga ist konfiguriert, um e-Mail-Benachrichtigungen
zu versenden, aber möglicherweise ist auf Ihrem System noch kein
Mail-Programm installiert bzw. konfiguriert. Schauen Sie in Ihre
Systemdokumentation oder suchen Sie im Web nach genauen Anweisungen, wie
Ihr System konfiguriert werden muss, damit es e-Mail-Mitteilungen an
externe Adressen versendet. Mehr Informationen zu Benachrichtigungen
finden Sie [hier](notifications.md "5.11. Benachrichtigungen").

### 2.4.14. Fertig

Glückwunsch! Sie haben erfolgreich Icinga installiert. Ihre Reise in die
Überwachung hat gerade begonnen. Sie werden ohne Zweifel mehr als nur
Ihre lokale Maschine überwachen wollen, so dass Sie u.a. das folgende
[Kapitel](ch02.md "Kapitel 2. Los geht's") lesen sollten...

* * * * *


© 1999-2009 Ethan Galstad, 2009-2015 Icinga Development Team,
http://www.icinga.org
