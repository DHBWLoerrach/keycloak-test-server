1. Docker Desktop installieren
2. Zunächst nur KeyCloak starten, dazu im Terminal ausführen: `docker compose up keycloak`
3. KeyCloak-Adminoberfläche im Browser öffnen: http://localhost:8080
4. Login mit `admin/admin`


Realm `dhbw` erstellen:
1. `Manage realms` > `Create realm` und dann `dhbw` als Namen eingeben und `Create` drücken
2. `dhbw` sollte als `Current realm` automatisch ausgewählt sein
3. Unter `Clients` mit `Create client` einen Client namens `carpool` erstellen:
  - `Client ID` ist `carpool` (Rest bleibt gleich) und dann `Next`
  - `Client authentication` aktivieren (Rest bleibt gleich) und dann `Next`
  - In `Valid redirect URIs` den Wert `http://localhost:5000` eintragen und dann mit `Save` speichern
4. Unter `Realm roles` mit `Create role` die Rolle `staff` erstellen.
5. Unter `Realm settings` im Tab `User profile` mit `Create attribute` ein Attribut erstellen:
  - `Attribute [Name]` soll `department` sein
  - `Required field` aktivieren
  - Mit `Create` das Attribut erstellen   
6. Nun können wir Benutzer erstellen:
  - Unter `Users` den Button `Add user` drücken
  - `Email verified` aktivieren
  - Gewünschten `Username` für Testbenutzer eintragen
  - `Email`, `First name` und `Last name` eintragen
  - In `department` den Kurs eintragen (z.B. `TIF25A`)
  - Benutzer mit `Create` erstellen
  - Für den Benutzer nun im Tab `Credentials` mit `Set password` das Passwort setzen und im angezeigten Dialog `Temporary` deaktivieren
  - Falls der Benutzer ein Mitarbeiter ist, dann unter `Role mappings` die Rolle `staff` hinzufügen

## oauth2-proxy einrichtien

Wir müssen nun das Client-Secret des `carpool`-Clients aus KeyCloak in die Konfiguration des Docker-Containers von oauth2-proxy eintragen:

1. Im Webinterface von KeyCloak unter `Clients` > `carpool` > `Credentials` das `Client Secret` in die Zwischenablage kopieren
2. Die Datei `docker-compose.yml` zum Bearbeiten im Editor öffnen.
3. Unter `OAUTH2_PROXY_CLIENT_SECRET` das eben kopierte Client-Secret eintragen.

Nun können wir beide Container zuerst stoppen und dann starten, z.B. mit `docker compose down && docker compose up -d`. 


