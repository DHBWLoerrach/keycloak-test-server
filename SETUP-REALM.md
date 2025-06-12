Wenn der Zugriff auf die Admin-Oberfläche im Browser (`localhost:8080`) https erfordern sollte, dann folgende Kommandos ausführen, um http zu erlauben:

```
docker exec -it keycloak-dev /opt/keycloak/bin/kcadm.sh config credentials --server http://localhost:8080   --realm master --user admin --password admin
docker exec -it keycloak-dev /opt/keycloak/bin/kcadm.sh update realms/master -s sslRequired=NONE
``` 

## Realm, Client usw. in KeyCloak manuell erstellen

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
7. Mapper für das Attribut `department` erstellen, damit es nach dem Login an die Webapp mitgeliefert wird:
  - `Client scopes` > `Create client scope`
  - `Name` ist `department`
  - `Type` auswählen: `Default`
  - `Include in token scope` aktivieren
  - Mit `Save` speichern
  - Dann zum Tab `Mappers` gehen und `Configure a new mapper` drücken
  - In der Liste `User Attribute` wählen
  - In `Name` und `Token Claim Name` den Wert `department` eingeben
  - Unter `User Attribute` den Eintrag `department` wählen 
  - Mit `Save` speichern
  - In der Seitenleiste zu `Clients` gehen und dort `carpool` auswählen
  - Im Tab `Client scopes` den Button `Add client scope` drücken
  - Dort einen Haken bei `department` setzen und dann `Add>Default` drücken

## oauth2-proxy einrichtien

Wir müssen nun das Client-Secret des `carpool`-Clients aus KeyCloak in die Konfiguration des Docker-Containers von oauth2-proxy eintragen:

1. Im Webinterface von KeyCloak unter `Clients` > `carpool` > `Credentials` das `Client Secret` in die Zwischenablage kopieren
2. Die Datei `docker-compose.yml` zum Bearbeiten im Editor öffnen.
3. Unter `OAUTH2_PROXY_CLIENT_SECRET` das eben kopierte Client-Secret eintragen.

Nun können wir beide Container zuerst stoppen und dann starten, z.B. mit `docker compose down && docker compose up -d`. 

In der Webapp können wir uns nun mit den in KeyCloak erstellten Benutzern einloggen.
