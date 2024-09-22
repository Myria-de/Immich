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
## Medien über ein Smartphone hochladen
Links zu Google Play und den Apple App Store finden Sie auf der Startseite von https://immich.app. Beim ersten Aufruf der App geben Sie die Server-URL in der Form „http://[Servername]:2283/api“ ein und melden sich an.



