# KeyCloak Test Server

Mit diesem Docker-Container kann ein KeyCloak-Server 
für Test- und Entwicklungsumgebungen simuliert werden.
Mit dem Reserve-Proxy [oauth2-proxy](https://oauth2-proxy.github.io/oauth2-proxy/) 
wird die Authentifizierung (Single Sign On) in einer Webanwendung möglich.  

## Einrichtung und Verwendung

### Docker-Container starten

1. ggf. Docker Desktop installieren
2. Zunächst nur KeyCloak starten, dazu im Terminal ausführen: `docker compose up keycloak`
3. KeyCloak-Adminoberfläche im Browser öffnen: http://localhost:8080
4. Login mit `admin/admin`


### Realm `dhbw` in KeyCloak erstellen

1. `Manage realms` > `Create realm` 
2. Im Dialog unter `Resource file` mit `Browse…` die Datei [`dhbw-realm.json`](dhbw-realm.json) auswählen.
3. `Create` drücken
4. Benutzer erstellen:
  - Unter `Users` den Button `Add user` drücken
  - `Email verified` aktivieren
  - Gewünschten `Username` für Testbenutzer eintragen
  - `Email`, `First name` und `Last name` eintragen
  - In `department` den Kurs eintragen (z.B. `TIF25A`)
  - Benutzer mit `Create` erstellen
  - Für den Benutzer nun im Tab `Credentials` mit `Set password` das Passwort setzen und im angezeigten Dialog `Temporary` deaktivieren
  - Falls der Benutzer ein Mitarbeiter ist, dann unter `Role mappings` die Rolle `staff` hinzufügen:
    - `Assign role` und dann `Filter by realm roles` aktivieren, um dort `staff` zu selektieren und hinzuzufügen

### oauth2-proxy mit keycloak starten 

Nun können wir beide Container zuerst stoppen und dann starten, z.B. mit `docker compose down && docker compose up -d`. 

In einer Webapp mit dem passenden Setup (siehe Web-Projekte an der DHBW Lörrach) können wir uns nun mit den in KeyCloak erstellten Benutzern einloggen.
