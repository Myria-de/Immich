# Immich-Installation und Konfiguration

## Docker unter Ubuntu/Linux Mint installieren
Der Immich-Server wird in einem Docker-Container installiert , was einige Vorbereitungen erfordert. Wir empfehlen die Verwendung von Docker zusammen mit der Verwaltungsoberfläche Portainer (https://www.portainer.io). Alternative Installationsanleitungen finden Sie unter https://immich.app/docs, etwa für Unraid oder einen Netzwerkspeicher mit True NAS. Soll der Server im heimischen Netzwerk auch über das Internet erreichbar sein, ist zusätzlich Nginx Proxy Manager (https://nginxproxymanager.com) erforderlich. 

Immich benötigt eine aktuellere Docker-Version, als sich über die Repositorien von Ubuntu 24.04 oder Linux Mint installieren lässt. Unter https://docs.docker.com/engine/install/ubuntu finden Sie Anleitungen zu Deinstallation älterer Docker-Versionen – wenn vorhanden – und zur Einrichtung des Docker-Repositoriums. Die nötigen Befehlszeilen haben wir im Script „docker_install.sh“ usammengefasst, das sich für Ubuntu 20.04/22.04 und Linux Mint 21.3/22 eignet. Starten Sie es im Terminal im Downloadverzeichnis mit
```
sh ./docker_install.sh
```
Das Script installiert Docker und Portainer.

Damit sich Docker als Standardbenutzer verwenden lässt, fügt das Script den aktuellen Nutzer zur Gruppe „docker“ hinzu. Damit Linux die neue Gruppe berücksichtigt, starten Sie das System neu.

## Immich in einem Docker-Container installieren
Rufen Sie die URL https://localhost:9443 im Webbrowser auf. Da Portainer ein selbst signiertes Zertifikat verwendet erhalten Sie eine Warnung. In Firefox klicken Sie auf „Erweitert“ und dann auf „Risiko akzeptieren und fortfahren“.

**Schritt 1:** Geben Sie Benutzername und Passwort für den administrativen Benutzer ein, klicken Sie auf „Create user“, dann auf „Get started“ und anschließend auf „Live connect“. 

**Schritt 2:** Laden Sie die Dateien „docker-compose.yml“ und „example.env“ herunter (https://m6u.de/IMCO und https://m6u.de/IMEN). Öffne Sie beide Dateien in einem Texteditor.

**Schritt 3:*** In Portainer gehen Sie auf „Stacks“ und klicken auf „+ Add stack“. Hinter „Name“ tippen Sie „immich“ ein. Kopieren Sie den Inhalt von „docker-compose.yml“in den Bereich unter „Web editor“. Ersetzen Sie im Text „.env“ durch „stack.env“ (zwei Vorkommen). Klicken Sie weiter unten auf „Advanced mode“ und fügen Sie den Inhalt von „example.env“ ein. Setzen Sie hinter „DB_PASSWORD=“ ein sicheres Passwort ein und klicken Sie ganz unten auf „Deploy the stack“.

**Schritt 4 (optional):** Wenn Sie Nginx Proxy Manager für den Zugriff über das Internet Installieren wollen, laden Sie über https://m6u.de/DOCK die Datei „NPM-Dockerfile“ herunter. Erstellen Sie damit einen Docker-Container, entsprechend wie in Schritt 3 beschrieben.

![502_01_Portainer](https://github.com/user-attachments/assets/d027c4e5-704a-454d-a6f5-8b1bf6e352eb)

![502_00_Immich](https://github.com/user-attachments/assets/880836a7-67b0-49b9-8068-db7508b49c8e)

## Immich über das Internet nutzen
Wer auch unterwegs auf Immich zugreifen will, baut am einfachsten eine VPN-Verbindung ins Heimnetz auf. Sie können dann direkt auf die lokalen Dienste beziehungsweise IP-Adressen zugreifen. Für Nutzer einer Fritzbox empfiehlt es sich, dafür Wireguard zu aktivieren. Eine ausführliche Anleitung lesen Sie unter https://www.pcwelt.de/1181301.

Wer den klassischen Weg bevorzugt, benötigt einen Domainnamen für dynamisches IP und eine Portweiterleitung im Router. Aus Sicherheitsgründen ist es ratsam, einen Reverse Proxy zu installieren, damit sich Immich nicht direkt aus dem Internet angreifen lässt. Wenn Docker ohnehin schon installiert ist, verwendet man dafür den Nginx Proxy Manager (NPM). Die Datei für die Installation laden Sie über https://m6u.de/DOCK herunter. Auf der Seite finden Sie auch eine kurze Anleitung zu Konfiguration über Portainer.

Einen dynamischen Domainnamen erhalten Fritzbox-Besitzer über den AVM-Dienst Myfritz. Öffnen Sie die Konfigurationsoberfläche über http://fritz.box oder http://192.168.178.1. Gehen Sie auf „Internet -> „MyFRITZ!-Konto“. Im folgenden Dialog klicken Sie auf die Schaltfläche „Neues MyFRITZ!-Konto erstellen“. Geben Sie im nächsten Schritt Ihre E-Mail-Adresse ein und vergeben Sie ein MyFRITZ!-Kennwort. Diese Zugangsdaten dienen später zur Authentifizierung auf http://myfritz.net. Der Domainname ist anschließend unter „MyFRITZ!-Internetzugriff“ zu sehen.

Unter „Internet -> Freigaben“ legen Sie die Portweiterleitung auf den Server im lokalen Netzwerk fest. Es genügt, die Ports 80 und 443 zu konfigurieren, wenn der Nginx Proxy Manager zum Einsatz kommt. Darüber kann man auch ein kostenloses SSL-Zertifikat von Letsencrypt beziehen.

Auch andere Router-Hersteller bieten oft einen eigenen Dienst für dynamische Domainnamen an. Wenn nicht, lässt sich in der Regel beispielsweise https://www.noip.com oder https://freedns.afraid.org konfigurieren.

**Nginx Proxy Manager konfigurieren:** 
Mit Nginx Proxy Manager können Sie den Port von Docker-Webanwendungen dem lokalen Hostnamen oder eine über das Internet erreichbare Domain zuordnen.

**Schritt 1:** Klicken Sie auf „Add Custom Template“. Für den Nginx Proxy Manager tragen Sie in die Felder hinter „Title“ und „Description“ jeweils npm ein.

**Schritt 2:** Gehen Sie auf https://m6u.de/DOCK, klicken Sie auf „NPM-Dockerfile“ und dann auf der rechten Seite auf „Copy raw file“ (das Icon hinter „Raw“).

**Schritt 3:** Zurück in Portainer fügen Sie den Inhalt mit Strg-V in den Eingabebereich unter „Web editor“ ein. Die Konfiguration enthält Definitionen für das Docker-Netzwerk „npm“, über das Nginx Proxy Manager später auf Anwendungen in anderen Containern zugreifen kann.

**Schritt 4:** Klicken Sie unter „Custom Templates“ auf „npm“ und danach auf „Deploy the stack“.


## Medien über ein Smartphone hochladen
Links zu Google Play und den Apple App Store finden Sie auf der Startseite von https://immich.app. Beim ersten Aufruf der App geben Sie die Server-URL in der Form „http://[Servername]:2283/api“ ein und melden sich an.



