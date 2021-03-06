# M300
Modul 300

# LB2

## 10 Toolumgebung

**GitHub**
GitHub brauchen wir für die Versionsverwaltung. Bei GitHub kann man Dokumente zentral hochladen und runterladen.

**Vagrant**
Vagrant ist die Software, mit welcher wir unsere Virtuellen Maschinen automatisiert aufsetzen können.

**Virtual Box**
Virtual Box ist unsere Virtualisierungsumgebung. Dort werden unsere Virtuellen Maschinen aufgesetzt und gespeichert.

## Wissensstand

**Linux**
Die Virtuellen Maschienen, welche ich automatisiert erstellt habe, waren Linux Maschienen.
In diesem Modul brauchte ich aber nicht viele Linux Commands. Folgende Befehle musste ich im Vagrant File angeben, damit der gewünschte Service installiert wird:

Mit diesem Befehl werden die Services alle auf den neusten Stand gebracht.
    sudo apt-get update

So kann man gewünschte Pakete installieren.
    sudo apt-get -y install "PAKET"

**Virtualisierung**
Das Ziel dieses Modules ist es, alles automatisiert zu Virtualisieren. Für die Virtualisierung verwenden wir Virtual Box. Man kann auf einem physischen Server, viele virtuelle Server haben, was natürlich sehr viele Ressourcen spart. Ebenfalls ist es sehr platzsparend und kostengünstiger.
Eine weitere sehr verbreitete virtualisierungs Methode ist Cloud Computing. Unter Cloud Computing versteht man, wenn man ein Programm ausführt, welches sich nicht auf dem lokalen Gerät befindet. Von Cloud Computing gibt es verschiedene Arten:

*Infrastructure as a Service (IaaS)*
Es bietet dem Nutzer die typischen Komponenten einer Rechenzentrumsinfrastruktur wie Hardware, Rechenleistung, Speicherplatz oder Netzwerkressourcen aus der Cloud.

*Platform as a Service (PaaS)*
PaaS bezeichnet eine Cloudumgebung, die eine Plattform für die Entwicklung von Anwendungen im Internet bereitstellt.

*Software as a Service (SaaS)*
SaaS bezeichnet ein Distributionsmodell für Anwendungen über den Webbrowser. SaaS wird als Teilbereich des Cloud Computings verstanden, da angeforderte Applikationen nie direkt auf dem Gerät des Nutzers vorhanden sind.

**Vagrant**
Vagrant brauchte ich in diesem Modul, um meine Virtuellen Maschienen automatisiert aufzusetzen und den gewünschten Service mit zu installieren. Vagrant ist eine Software, welche in BASH läuft. So kann ein Vagrant File aussehen:

    Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/xenial64"
    config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
    config.vm.synced_folder ".", "/var/www/html"  
    config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"  
    end
    config.vm.provision "shell", inline: <<-SHELL
    # Packages vom lokalen Server holen
    # sudo sed -i -e"1i deb {{config.server}}/apt-mirror/mirror/archive.ubuntu.com/ubuntu xenial main restricted" /etc/apt/sources.list 
    sudo apt-get update
    sudo apt-get -y install apache2 
    SHELL
    end

**Versionsverwaltung**
Die Versionsverwaltung braucht man, um zentral, Dokumente abzulegen und zu bearbeiten. Es ist sehr geeignet, wenn mehrere Benutzer auf einem Dokument oder Skript arbeiten müssen. Wenn man bei GitHub ein Dokument herunterlädt und bearbeitet, muss man beim wieder hochladen immer schreiben, was geändert wurde. So kann jede Veränderung nachgewiesen werden.
Hier sieht man die Help Page von Git:

    clone      Clone a repository into a new directory
    init       Create an empty Git repository or reinitialize an existing one

    add        Add file contents to the index
    mv         Move or rename a file, a directory, or a symlink
    reset      Reset current HEAD to the specified state
    rm         Remove files from the working tree and from the index

    bisect     Use binary search to find the commit that introduced a bug
    grep       Print lines matching a pattern
    log        Show commit logs
    show       Show various types of objects
    status     Show the working tree status

    branch     List, create, or delete branches
    checkout    Switch branches or restore working tree files
    commit     Record changes to the repository
    diff       Show changes between commits, commit and working tree, etc
    merge      Join two or more development histories together
    rebase     Reapply commits on top of another base tip
    tag        Create, list, delete or verify a tag object signed with GPG

    fetch      Download objects and refs from another repository
    pull       Fetch from and integrate with another repository or a local branch
    push       Update remote refs along with associated objects

**Mark Down**
Mark Down ist eine vereinfachte Auszeichnungssprache. Ein Ziel von Mark Down ist, dass schon die Ausgangsform ohne weitere Konvertierung leicht lesbar ist. Diese Dokumentation ist in der Mark Down Sprache geschrieben.

**Systemsicherheit**
Die Systemsicherheit kann man über verschiedene Methoden erhöhen:
- Firewall einrichten und nur die nötigen Ports öffnen
- Reverse-Proxy einrichten, so ist nur dieser vom Internet sichtbar und vom internen Netzwerk nichts.
- Benutzer- und Rechtvergabe ist eingerichtet, sodass nur bestimmte Personen Adminrechte haben.

Die Systemsicherheit ist ein sehr wichtiges Thema. In den grossen Firmen wird sehr viel Zeit und Geld in das investiert.

## Vagrant
Mit Vagrant setzen wir unsere VM automatisiert auf.

Als erstes muss man ein Vagrant File erstellen. In diesem werden diverse Sachen festgelegt, wie zum Beispiel RAM grösse, welches Betriebssystem, etc.
Ich habe ein Web Server aufgesetzt und ein MySQL Server aufgesetzt. Für diesen hat mein Vagrant File wie folgt ausgesehen:

    Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/xenial64"
    config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
    config.vm.synced_folder ".", "/var/www/html"  
    config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"  
    end
    config.vm.provision "shell", inline: <<-SHELL
    # Packages vom lokalen Server holen
    # sudo sed -i -e"1i deb {{config.server}}/apt-mirror/mirror/archive.ubuntu.com/ubuntu xenial main restricted" /etc/apt/sources.list 
    sudo apt-get update
    sudo apt-get -y install apache2 
    sudo apt-get -y install mysql-server
    sudo apt-get -y install php-mysql
    SHELL
    end

Danach kann man in irgendeiner Bash mit Hilfe von Vagrant eine VM aufsetzen.

    vagrant up

Mit diesem Befehl wird eine VM anhand des Vagrantfile erstellt.



**Vagrant Befehle**
Hier sind noch weitere Vagrant Befehle:

| Befehl                    | Beschreibung                                                      |
| ------------------------- | ----------------------------------------------------------------- | 
| `vagrant init`            | Initialisiert im aktuellen Verzeichnis eine Vagrant-Umgebung und erstellt, falls nicht vorhanden, ein Vagrantfile |
| `vagrant up`              |  Erzeugt und Konfiguriert eine neue Virtuelle Maschine, basierend auf dem Vagrantfile |
| `vagrant ssh`             | Baut eine SSH-Verbindung zur gewünschten VM auf                   |
| `vagrant status`          | Zeigt den aktuellen Status der VM an                              |
| `vagrant port`            | Zeigt die Weitergeleiteten Ports der VM an                        |
| `vagrant halt`            | Stoppt die laufende Virtuelle Maschine                            |
| `vagrant destroy`         | Stoppt die Virtuelle Maschine und zerstört sie.                   |

**Testing**
Wenn die VM richtig erstellt worden ist, sollte man jetzt unter de IP http://127.0.0.1:8080/ die Apache Standard Seite erreichen.

## Sicherheit
Ich habe einen Webserver und einen Datenbankserver aufgesetzt. Diese habe ich anhand eines bestehendem Vagrant File erstellt.
Für die Sicherheit habe ich zum einen eine Host Firewall konfiguriert. In dieser gebe ich definierte Ports an, welche offen sein sollten. Die anderen sind alle geschlossen. Dies aus dem Grund, da offene Ports, Eingänge für Hacker Angriffe sind.
Zusätzlich habe ich noch einen Reverse-Proxy aufgesetzt. Dieser leitet eine URL auf einen bestimmten Server weiter. Dadurch kann man zum Beispiel mit verschiedenen URL's auf einen einzigen Server zugreifen.

**Firewall Rules**
Danach habe ich auf dem Webserver den HTTP Port geöffnet

    sudo ufw allow 80/tcp

Auf dem Datenbankserver habe ich den MySQL Port für den Webserver geöffnet

    sudo ufw allow from 192.168.55.101 to any port 3306

Mit diesem Befehl habe ich dann kontrolliert, ob die Rules richtig erstellt wurden

    sudo ufw status

**Reverse-Proxy**
Für den Reverse-Proxy musste ich zwei Pakete installieren.

    sudo apt-get install lipapache2-mod-proxy-html
    sudo apt-get install libxml2-dev

Danach musste ich die Module in Apache aktivieren.

    sudo a2enmond proxy
    sudo a2enmond proxy_html
    sudo a2enmond proxy_http

Dann musste ich die Apache config (/etc/apache2/apache2.conf) anpassen.

    ServerName localhost

Die Weiterleitung wird in der Datei (etc/apache2/sites-enabled/001-reverseproxy.conf) angegeben.

    # Allgemeine Proxy Einstellungen
    ProxyRequests Off
    <Proxy *>
        Order deny,allow
        Allow from all
    </Proxy>

    # Weiterleitungen master
    ProxyPass /master http://master
    ProxyPassReverse /master http://master



## Schlusswort
**Reflexion**
Die LB2 des Modul 300 wahr sehr umfangreich. Diese Umgebung war neu für mich und ich musste mich zuerst zurecht finden. Nach einigen versuchen kam ich immer besser mit der Automatisierung zurecht. Ich konnte viel neues lernen. Leider verwenden wir keines der Programme im Arbeitsalltag. Daher wird mir das neue Wissen momentan nichts bringen. Jedoch kenne ich nun die Programme und weiss um was es geht. Ich hoffe ich kann mein neu gelerntes Wissen wiedermal brauchen.

**Vergleich Vorwissen - Wissenszuwachs**
Vor diesem Modul hatte ich noch nie was mit Vagrant, Visual Studio Code oder Virtual Box zu tun. Mit Vagrant kann ich nun VM's automatisiert aufsetzten. Mit Visual Studio Code habe ich die Dokumentation gestalltet. Diese mussten wir im Mark-Down style schreiben. Auch dies habe ich hier im Modul das erste mal gemacht. Vorher wusste ich nicht was das ist. Ich nun auch ein bisschen besser mit Linux zurecht.

**Probleme**
Ich bemerkte das Vagrant mühe hat, zwei Webserver gleichzeitig zu hosten. Ich wollte während dem ich bereits einen Webserver hatte, einen zweiten installieren. Bereits währnd der Installation gab es Probleme. Zum testen löschte ich den ersten Web-Server. Danach konnte ich wieder einen anderen Webserver ohne Probleme aufsetzten. 


# LB3

## K1
**GitHub**
GitHub brauchen wir für die Versionsverwaltung. Bei GitHub kann man Dokumente zentral hochladen und runterladen.

**Vagrant**
Vagrant ist die Software, mit welcher wir unsere Virtuellen Maschinen automatisiert aufsetzen können.

**Virtual Box**
Virtual Box ist unsere Virtualisierungsumgebung. Dort werden unsere Virtuellen Maschinen aufgesetzt und gespeichert.

**Visual Studio Code**
Visual Studio Code brauche ich um die Dokumentation zu schreiben. Dies mache ich im Mark-Down style. Der Vorteil von Visual Studio Code ist das ich das geschriebene direkt in Github hochladen kann. 



## K2


**Containerisierung** 
Was ist ein Container?
Ein Linux Container ist ein Satz an Prozessen, die vom Rest des Systems isoliert sind und auf einem eigenen Image ausgeführt werden, das alle benötigten Dateien zur Unterstützung der Prozesse bereitstellt. Durch Bereitstellung eines Image, das alle Abhängigkeiten einer Anwendung enthält, ist er portabel und bleibt konsistent und lässt sich so problemlos von der Entwicklung über die Prüfung und schließlich in die Produktion überführen.
 
**Container vs. Virtualisierung**
 
Container
Container teilen sich den gleichen Betriebssystem-Kernel und isolieren die Anwendungsprozesse vom Rest des Systems. 
Virtualisierung
Die Virtualisierung ermöglicht, dass mehrere Betriebssysteme gleichzeitig auf einem einzigen System laufen.
 
Was bedeutet das? Erst einmal: Mehrere Betriebssysteme auf einem Hypervisor laufen zu lassen – der Software, die Virtualisierung möglich macht – belastet das System mehr als die Verwendung von Containern. Wenn Sie nur über endliche Ressourcen mit endlichen Fähigkeiten verfügen, benötigen Sie kompakte Apps, die in großer Dichte eingesetzt werden können. Linux-Container arbeiten aus diesem einzelnen Betriebssystem heraus und nutzen es gemeinsam für alle Ihre Container. So bleiben Ihre Apps und Dienste leicht und können zügig parallel laufen.


**Docker**
 Mit Docker ist es möglich, Container zu implementieren. Ausserdem kann man Services inklusive deren Abhängikeiten in den Container verpacken. Container sind sehr nützlich, da jeder individuell gestalltet werden kann. Damit eine Dockerumgebung läuft, braucht es immer ein Image.


## K3
Als erstes musste ich einen Container erstellen. Darin hat es einen Webserver. Das ganze habe ich so gemacht:

Zu beginn muss ein Image erstellt werden.

    build image -t websrv .

Mit folgenden Befehl kann überprüft werden ob das Image erstellt wurde.

    docker images

Danach muss der Container erstellt werden. Dazu muss das Image, welches ich erstellt habe angegeben werden. 

    docker run --rm -d --name websrv websrv

Kontollieren kann man das ganze mit folgendem Befehlen

    docker ps
    docker exec -it mysql bash

Unter URL:http://localhost:8080 war ist der Webserver erreichbar. 


**Volumes**
Um ein Volume zu ertstellen, muss folgender Befehl angewendet werden.

    docker volume create volumeLB3

Um alle Volumes anzuschauen folgender Befehl.

    docker volume ls

So habe ich das Volume gemountet.

    docker run -d \
    --name devtest \
    --mount source=myvol2,target=/app \
    nginx:latest

**Docker Befehle**
Hier sind noch weitere Docker Befehle:

| Befehl                    | Beschreibung                                                      |
| ------------------------- | ----------------------------------------------------------------- | 
| `docker build image -t "image-name"`            | Erstellt Image |
| `docker images`              |  Zeigt alles Images an|
| `docker run --rm -d- --name "image-name" "name"`             | Container erstellen                   |
| `docker ps`          | Zeigt Container an                              |
| `docker exec -it "name" bash`            | Zugriff auf erstellten Web-Server                        |
| `docker rm "name"`            | Docker Container löschen                            |
| `docker rm -f docker ps -a -q`         | Alle Container, auch aktive, löschen                  |
| `docker volume create "name"`         | Erstellt Volume               |
| `docker volume ls`         | Ausgabe aller Volumes                 |


## K4

**cAdvisor**
Um zur Sicherheit beizutragen, habe ich den cAdvisor von Google installiert. Dies habe ich mit diesem Befehl gemacht. 

    docker run -d --name cadvisor -v /:/rootfs:ro -v /var/run:/var/run:rw -v /sys:/sys:ro -v /var/lib/docker/:/var/lib/docker:ro -p 8080:8080 google/cadvisor:latest


**Standard-Logging (JSON-File)**

Ausserdem habe ich das Standard-Logging (JSON-File) auch konfiguriert. Hier wird alles Protokolliert was an den STDOUT und STDERR geschickt wird. Das ganze kann man auch jede Sekunde ausgeben lassen. Dazu wäre der zweite Befehl.

    docker run --name logtest ubuntu bash -c 'echo "stdout"; echo "stderr" >>2'
    docker run -d --name streamtest ubuntu bash -c 'while true; do echo "tick"; sleep 1; done;'

Mit folgdenem Befehl kann ich das Log anschauen.

    docker logs logtest

Löschen.

    docker rm logtest

**Syslog**

Unter /var/log/syslog wird ebenfalls alles Dokumentiert.

    docker run -d --log-driver=syslog ubuntu bash -c 'i=0; while true; do i=$((i+1)); echo "docker $i"; sleep 1; done;'
    tail -f /var/log/syslog


## K5
**Vergleich Vorwissen / Nachwissen**
Ich hatte vor diesem Modul noch nie was mit Container zu tun. Dementsprechend hatte ich kein Vorwissen. Für mich war alles mehr oder weniger neu. Ich habe bereits vieles über Container gehört jedoch hatte ich noch nie was damit zu tun. Jetzt konnte ich selber damit arbeiten und erkenne dessen Vorteile.

**Reflexion**
In diesem Modul konnte ich viel neues Lernen. Ich habe gemerkt das Container ein wichtiges Thema ist, welches auch in Zukunft eine grosse Rolle spielen wird. Daher bin ich forh das ich bereits jetzt eine kleinen Einblick in das Thema bekommen habe. Ich finde es gut wie die LB3 aufgebaut ist. Es ist klar geschrieben was gefordert ist.

## K6
**Kubernetes**
Zuerst musste ich einen eingenen Namespace erstellen. Diesen habe ich test genannt.

    kubectl create namespace test

Danach habe ich den einen Pod erzeugt. In diesem Fall war dieser der Apache Web Server.

    kubectl run apache --image=httpd --restart=Never --namespace test

Kubernetes erzeugt zu jeder Ressource immer eine YAML Datei. So müssen wir nicht jede YAML Datei von Hand erstellen.

    kubectl get pod apache -o yaml --namespace test

Das unser Web Server von aussen sichtbar wird, müssen wir eine weiteren Service installieren.

    kubectl expose pod/apache --type="LoadBalancer" --port 80 --namespace test

Auch hier müssen wir eine YAML Datei ausgeben.

    kubectl get service apache -o yaml --namespace test

Mit der default IP-Adresse (192.168.137.100) können wir auf den Service zugreifen.
