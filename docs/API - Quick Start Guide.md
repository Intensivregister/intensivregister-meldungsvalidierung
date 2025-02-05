# Schnittstellenbeschreibung des Intensivregisters - Quick Start Guide


# Getting Started

Sie können sich selbstständig im [Partner-Portal](https://partner.intensivregister.de) registrieren. Sobald die Registrierung abgeschlossen ist, können Sie die benötigten Zugangsinformation über das [Partner-Portal](https://partner.intensivregister.de) unter *Zugänge* einsehen. Mit den aufgeführten Credentials können Sie ein "Access Token" bei unserer Authentifizierungssoftware [^1] erstellen.
Dieses Access-Token muss in jedem HTTP-Request im Header namens ```Authorization``` samt des Prefix "Bearer " (Leerzeichen beachten!) mitgesendet werden.

[^1]: Wir nutzen Keycloak, ein OpenID-Connect fähige Lösung.


## Beispiel:
1. Access-Token abfragen
```
curl --location --request POST \
 'https://auth.intensivregister.de/[...]/connect/token' \
 --header 'Content-Type: application/x-www-form-urlencoded' \
 --data-urlencode 'grant_type=client_credentials' \
 --data-urlencode 'client_id=<CLIENT_ID>' \
 --data-urlencode 'client_secret=<CLIENT_SECRET>'
 ```

Die Antwort ist ein JSON-Dokument, welches u.a. den Eintrag ```access_token``` enthält. Dieses bitte extrahieren. 
Die Gültigkeitsdauer des Tokens ist ebenfalls in der Antwort enthalten (alternativ kann auch das Token entpackt werden, es ist ein normales JWT).

2. Beispiel-Request an das Intensivregister
```
curl --location --request GET \
  'https://www.intensivregister.de/api/stammdaten/meldebereich' \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer ACCESS_TOKEN'
```

Das Ergebnis ist ein JSON-Dokument mit der Menge der Meldebereiche, die Ihnen zugewiesen ist.



# Allgemeines

## Umgebungen und Freigaben

Das Intensivregister hat mehrere, komplett unabhängige Umgebungen. Neuen API-Partnern werden Accounts und OICD-Clients auf der Testumgebung und Produktiv-Umgebung zugewiesen.

Tests können in der Testumgebung durchgeführt werden, dabei können die folgenden Pfade verwendet werden:

* Access-Token-Endpunkt - `https://auth.intensivregister.de/realms/ir-prod-alike/protocol/openid-connect/token`
* Basispfad der Serveranwendung - `https://prod-alike.intensivregister.de/api/`

Nach Test wird der OICD-Client für die Produktiv-Umgebung freigeschaltet. Die Pfade dort sind:

* Access-Token-Endpunkt - `https://auth.intensivregister.de/realms/intensivregister/protocol/openid-connect/token`
* Basispfad der Serveranwendung - `https://www.intensivregister.de/api/`

In der Testumgebung können selbstständig Meldebereiche zum Testen über das [Partner-Portal](https://partner.intensivregister.de) unter *Zugänge* > *Meldebereiche verwalten* angelegt werden.
Diese Meldebereiche dienen nur zum Testen und sind unabhängig von den Meldebereichen in Produktion - die Umgebungen sind strikt voneinander getrennt. Dementsprechend können auch die IDs der Meldebereiche voneinander abweichen.

In der Produktiv-Umgebung müssen die Meldebereiche erst zugeordnet werden, dafür muss zunächst eine Meldebereichsanfrage über das [Partner-Portal](https://partner.intensivregister.de) unter *Zugänge* > *Meldebereiche anzeigen* gestellt werden. Danach kann die Freigabe durch das RKI ebenfalls über das [Partner-Portal](https://partner.intensivregister.de) angefragt werden.

## Login-Modi

Für die Authentifizierung wird das "client credentials" Verfahren verwendet. Hierzu wird kein username/passwort benötigt, sondern für die Authentifizierung muss das ```client_secret``` und der ```grant_type``` auf "client_credentials" gesetzt werden. 

Die folgenden Informationen werden benötigt und können dem [Partner-Portal](https://partner.intensivregister.de) unter *Zugänge* entnommen werden:

* ```client-id```
* ```client_secret```

Beispiel:
```
curl --location --request POST \
 <URL des Token-Endpunktes, s.o.> \
 --header 'Content-Type: application/x-www-form-urlencoded' \
 --data-urlencode 'grant_type=client_credentials' \
 --data-urlencode 'client_id=<CLIENT_ID>' \
 --data-urlencode 'client_secret=<CLIENT_SECRET>'
 ```

Als Ergebnis erhält man jeweils einen JSON-formatierten Body mit folgenden Feldern (gekürzt):

```json
{
  "access_token": "eyJhb...Yx2w",
  "expires_in": 900,
  "refresh_expires_in": 1800,
  "refresh_token": "eyJh...eJo",
  "token_type": "bearer",
  "not-before-policy": 0,
  "session_state": "a3c4...b3d4",
  "scope": "profile email"
}
 ```

Der Wert im Feld ```access_token``` muss in folgenden API-Calls an das Intensivregister mitgesendet werden.


## Intensivregister-API

Die Dokumentation des (semi-)öffentlichen Teils dieser Schnittstelle findet man unter

* Swagger UI - [https://www.intensivregister.de/api/public/api-docs-ui](https://www.intensivregister.de/api/public/api-docs-ui)
* OpenAPI-Spezifikation - [https://www.intensivregister.de/api/public/api-docs](https://www.intensivregister.de/api/public/api-docs)

Darüber hinaus gibt es noch weitere Endpunkte, die nicht in der öffentlichen API-Spec beschrieben werden.

### Kompatibilität

Wir versuchen die API "backwards compatible" zu entwickeln bzw. garantieren eine Übergangsphase von einigen 
Monaten, in denen alte Requests technisch gültig sind.

Voraussetzung ist allerdings, dass Sie beim Verarbeiten der Antworten "ignore unknowns" aktivieren, da wir
jederzeit beginnen könnten, in den Antworten neue Felder zu senden.

#### Beispiel:
Im folgenden ein Beispiel für eine alte Antwort einer Meldung:
```json
{
  "meldung_id": "abcd",
  "kapazitaeten": { ... }
}
```

Wenn wir den Fragebogen erweitern, erweitern wir i.d.R. das Meldungsobjekt um ein neues Sub-Feld, im folgenden 
beispielhaft mit "neuaufnahmen" dargestellt:
```json
{
  "meldung_id": "abcd",
  "kapazitaeten": { ... },
  "neuaufnahmen": {
    "erstaufnahmen": null,
    "verlegungen": null
  }
}
```

Selbst wenn Sie die API ohne das unbekannte Sub-Feld aufrufen, wird die Antwort dieses Feld (mit dem Wert NULL)
enthalten. Wenn Ihr API-Client kein "ignore unknowns" kann, wird dieser in diesem Fall einen Fehler werfen (da die Antwort
ein für den Client unbekanntes Feld enthält).

Changes der API-Endpunkte, die nicht in der öffentlichen OpenAPI-Spezifikation beschrieben sind, werden nicht kommuniziert. Die Benutzung dieser Endpunkte ist möglich, wird aber offiziell nicht unterstützt.

## OpenAPI-Spezifikation

Es wird dringend angeraten, die Client-seitige API anhand der OpenAPI-Spezifikation automatisch zu generieren[^4]. Das Intensivregister-Frontend nutzt diese Variante ebenfalls. Der Vorteil hierbei ist, dass Änderungen an der API einfacher nachzuvollziehen sind (da nur der Client-seitige API-Code neu generiert werden muss). Dies ist für alle gängigen Programmiersprachen möglich.

[^4]: [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator)


# Fachliche Anwendungsfälle

## Meldebereiche des Nutzers abfragen

Die Meldebereiche, die für den Client freigegeben sind, sind mit dem folgenden Endpunkt abrufbar:
* Abrufen der Meldebereiche - ```GET /stammdaten/meldebereich```

## Letzte Meldung eines Meldebereichs abfragen.

Die letzte (aktive/freigegebene) Meldung eines Meldebereichs ist mit dem folgenden Endpunkt abrufbar:
* Abrufen der letzten Meldung - ```GET /stammdaten/meldebereich/{meldebereichId}/letzte-meldung```

## Meldungen senden
Zum Abgeben von Meldungen bzw. zum Aktualisieren von Meldungen [^5] sind die folgenden Endpunkte relevant:
* Abgeben einer Meldung - ```POST /meldungen```
* Aktualisierung einer Meldung - ```PUT /meldungen/{meldungId}``` 

Notwendige technische Informationen zum Benutzen sind: ein Access-Token (s.o.), die ID des Meldebereichs, für den man melden will (s.u.) und eine zufällig generierte UUID für die neue Meldung (bzw.
die ID der zu aktualisierenden Meldung).

[^5]: Ist die ID der Meldung bereits in Benutzung durch einen anderen Meldebereich wird die neue Meldung abgelehnt (Status-Code 403).

Die Beschreibung der zu sendenden Struktur findet sich in der OpenAPI-Spezifikation [^6]

[^6]: [Intensivregister OpenAPI Docukumentation - Meldungen](https://www.intensivregister.de/api/public/swagger-ui/index.html?configUrl=/api/public/api-docs/swagger-config#/meldungs-controller/meldeKapazitaet) 

Folgende Hinweise für bestimmte Felder:

* ```id```: eine selbst generierte UUID (für neue Meldungen) oder die ID der zu aktualisierenden Meldung
* ```api_version```: muss ```V2``` sein (```V1``` wird nicht mehr akzeptiert)

NULL-Werte (bzw. nicht gesendet) sind (bis auf die unten aufgeführte Felder)
zugelassen und bedeuten, dass der Anwender nicht um den Zustand des entsprechenden Feldes weiß. Die Ausprägungen von ```versorgungskategorie```, ```bettenVerfuegbarkeit``` und ```betriebssituation``` sind in der OpenAPI-Spezifikation hinterlegt.

Die Verordnung zur Krankenhauskapazitätssurveillance [^7] definiert die Meldepflicht für verschiedene Datenfelder. 

Technisch verpflichtende Eingabefelder sind:

* ```id```
* ```meldebereich.id```
* ```kapazitaeten.intensivBetten```
* ```kapazitaeten.intensivBettenBelegt```

[^7]: [Verordnung zur Krankenhauskapazitätssurveillance](https://www.gesetze-im-internet.de/khkapsurv/BJNR626200022.html)

### Hinweis:
Ist die Betriebssituation ```KEINE_ANGABE``` oder ```REGULAERER_BETRIEB```, so sollten die vier Felder für
Betriebseinschränkungen (```betriebseinschraenkungPersonal```, ```betriebseinschraenkungRaum```,
```betriebseinschraenkungBeatmungsgeraet```, ```betriebseinschraenkungVerbrauchsmaterial```) NULL sein. In anderen Fällen können beliebig viele dieser Betriebseinschränkungsgründe gesetzt werden.

Für eine genaue Datenfeld-Definition wenden Sie sich bitte an das RKI.

# Meldungsfreigabe

Um dem RKI eine fachliche Validierung der gesendeten Meldungen zu ermöglichen inkl. eines Parallelbetriebs wurde
folgendes Verfahren ermöglicht:

* Mit der Kommunikation von Client-Id/-Secret ist es für die entsprechende Umgebung möglich,
  Meldungen abzugeben (sofern man dem entsprechenden Meldebereich zugeordnet ist).
* Ohne weitere Konfiguration werden die Meldungen zwar angenommen und gespeichert, aber werden nicht für inhaltliche
  Zwecke ausgewertet und in der Oberfläche nicht angezeigt. Die Meldungen sind "nicht freigegeben"
  und im Meldungsobjekt ist der Wert des Feldes "aktiv" auf 0.
* Nachdem das RKI die Qualität überprüft hat, wird ihr Client durch das RKI "freigegeben". Alle nachfolgenden
  Meldungen werden dadurch bei Meldungseingang freigegeben. Alte Meldungen bleiben im Zustand "nicht freigegeben".

Auf der Testumgebung wird ihr Client i.d.R. sofort freigegeben; obige Hinweise betreffen nur die **Produktiv-Umgebung**.

### Validierungen

Meldungen werden vor der Speicherung oder Aktualisierung zunächst validiert/plausibilisiert. Diese Validierungen können sich auf einzelne Felder beziehen (z.B. für den Wert von ```faelleCovidAktuell``` wird die folgende Validierung durchgeführt: ```0 <= faelleCovidAktuell <= 999```), oder auf mehrere Felder (z.B. muss die Anzahl der aktuellen COVID-19-Fälle kleiner gleich der Gesamtzahl der belegten Betten sein).

Schlägt die Validierung fehl, wird ein HTTP-Response mit dem Status Code 400 (Bad Request) zurückgegeben, welcher die
Validierungsfehler als ```errors``` enthält. Für jedes in der fehlgeschlagenen Validierung beteiligte Feld wird ein
separater ```error``` zurückgegeben.

Ein ```error``` besteht aus einem ```errorCode```, der den Fehler identifiziert, einem ```propertyPath```, welcher das beteiligte Feld benennt und einer ```errorMessage```, welche den Fehler beschreibt.

Ein Beispiel-Response für einen Request mit einer negativen Anzahl verstorbener und mehr aktuellen COVID-19-Fällen als Gesamtzahl der belegten Betten:

```json
{
  "statusCode": 400,
  "timestamp": "2020-05-15T16:24:19.186772",
  "errors": [
    {
      "errorCode": "VALIDATIONERROR",
      "propertyPath": "faelleCovidVerstorben",
      "errormessage": "must be greater than or equal to 0"
    },
    {
      "errorCode": "VALIDATIONERROR",
      "propertyPath": "intensivBettenBelegt",
      "errormessage": "faelleCovidAktuell must be less or equal to intensivBettenBelegt."
    },
    {
      "errorCode": "VALIDATIONERROR",
      "propertyPath": "faelleCovidAktuell",
      "errormessage": "faelleCovidAktuell must be less or equal to intensivBettenBelegt."
    }
  ]
}
 ``` 

Für die Meldung-Erfassen-Endpunkte gibt es ein separates Dokument mit den Plausibilisierungsregeln, zu finden unter 
[https://github.com/Intensivregister/intensivregister-meldungsvalidierung](https://github.com/Intensivregister/intensivregister-meldungsvalidierung)

## Kontakt

Für die Unterstützung bei der Einrichtung von Zugängen wenden Sie sich bitte an den Helpdesk: `intensivregister-hilfe@rki.de`

Für technische Fragen zu diesem Dokument/der Schnittstelle wenden Sie sich bitte an das Support-Team:
`Tech-support-IntensivRegister@prodyna.com`. 

Das RKI ist erreichbar unter `intensivregister@rki.de`
