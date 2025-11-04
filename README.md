# KeyCloak Test Server

Mit diesem Docker-Container kann ein KeyCloak-Server 
für Test- und Entwicklungsumgebungen simuliert werden.
Mit dem Reserve-Proxy [oauth2-proxy](https://oauth2-proxy.github.io/oauth2-proxy/) 
wird die Authentifizierung (Single Sign On) in einer Webanwendung möglich.  

## Einrichtung und Verwendung

### Docker-Container starten

1. ggf. Docker Desktop installieren
2. Dieses Projekt mit git clonen
3. Im Terminal ins Projektverzeichnis wechseln
4. Zunächst nur KeyCloak starten, dazu im Terminal ausführen: `docker compose up keycloak`
5. KeyCloak-Adminoberfläche im Browser öffnen: http://localhost:8080
6. Login mit `admin/admin`

### Realm `dhbw` in KeyCloak erstellen

1. `Manage realms` > `Create realm` 
2. Im Dialog unter `Resource file` mit `Browse…` die Datei [`dhbw-realm.json`](dhbw-realm.json) aus diesem Projekt auswählen.
3. `Create` drücken
4. Benutzer erstellen:
  - Unter `Users` den Button `Add user` drücken
  - `Email verified` aktivieren
  - Gewünschten `Username` für Testbenutzer eintragen (z.B. `user1`)
  - `Email`, `First name` und `Last name` eintragen (z.B. `user1@example.com`, `Hans`, `Mustermann`)
  - In `department` den Kurs eintragen (z.B. `TIF25A`)
  - Benutzer mit `Create` erstellen
  - Für den Benutzer nun im Tab `Credentials` mit `Set password` das Passwort setzen und im angezeigten Dialog den Schalter `Temporary` auf `Off` setzen bzw. deaktivieren
  - Falls der Benutzer ein Mitarbeiter sein soll (z.B. für die Campus Rallye Admin Web-App), dann unter `Role mappings` die Rolle `staff` hinzufügen:
    - `Assign role` und dann `Realm roles` klicken, um dort `staff (Mitarbeiter der DHBW)` zu selektieren und hinzuzufügen

### oauth2-proxy mit keycloak starten 

Nun können wir beide Container zuerst stoppen und dann starten, z.B. mit `docker compose down && docker compose up -d`. 

In einer Webapp mit dem passenden Setup (siehe Web-Projekte an der DHBW Lörrach) können wir uns nun mit den in KeyCloak erstellten Benutzern einloggen.
