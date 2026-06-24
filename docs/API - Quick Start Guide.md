# Schnittstellenbeschreibung des Intensivregisters - Quick Start Guide

## 1. Ziel dieses Dokuments

Dieses Dokument beschreibt den schnellen Einstieg in die Intensivregister-API: Zugang, Authentifizierung mit Bearer-Token, erste Aufrufe sowie die wichtigsten fachlichen Endpunkte.

## 2. Getting Started

### 2.1 Zugang einrichten

1. Registrieren Sie sich im [Partner-Portal](https://partner.intensivregister.de).
2. Nach erfolgreicher Registrierung finden Sie unter **Zugaenge** Ihre Zugangsdaten (Client ID und Client Secret).
3. Mit diesen Credentials fordern Sie ein Access-Token bei der Authentifizierung (Keycloak/OpenID Connect) an.

### 2.2 Umgebungen

Das Intensivregister bietet zwei getrennte Umgebungen:

- **TEST**: Entwicklung und Integrationstests, nicht fuer den produktiven Betrieb.
- **PROD**: Produktive Umgebung fuer die Erfassung realer Meldungen.

Fuer API-Partner werden Accounts und OIDC-Clients auf beiden Umgebungen angelegt.

### 2.3 Basis-URLs

**TEST**

- `ACCESS_TOKEN_URL`: `https://auth.intensivregister.de/realms/intensivregister-alike/protocol/openid-connect/token`
- `API_URL`: `https://alike.intensivregister.de/api/`

**PROD**

- `ACCESS_TOKEN_URL`: `https://auth.intensivregister.de/realms/intensivregister/protocol/openid-connect/token`
- `API_URL`: `https://www.intensivregister.de/api/`

## 3. Authentifizierung und Bearer-Token

### 3.1 Access-Token anfordern

Zur Authentifizierung wird der `client_credentials`-Flow verwendet. Die Token-Antwort enthaelt u. a. das Feld `access_token` und die Gueltigkeitsdauer.

```bash
curl --location --request POST \
  'https://auth.intensivregister.de/realms/intensivregister/protocol/openid-connect/token' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=client_credentials' \
  --data-urlencode 'client_id=<CLIENT_ID>' \
  --data-urlencode 'client_secret=<CLIENT_SECRET>'
```

### 3.2 Bearer-Token verwenden

Das Access-Token muss bei **jedem** API-Aufruf im Header `Authorization` mit dem Prefix `Bearer ` gesendet werden:

`Authorization: Bearer <access_token>`

Ohne gueltiges Token werden Requests abgewiesen.

### 3.3 Postman Collection

Verwenden Sie die Collection `Intensivregister - Meldung erfassen - Quick-Start.postman_collection.json`.

- Sie enthaelt die relevanten Requests fuer Authentifizierung und Meldungsabgabe.
- Die Collection nutzt Platzhalter fuer die API-Basis-URL. Setzen Sie je nach Umgebung:

## 4. Erster API-Aufruf (Smoke Test)

Mit folgendem Request koennen Sie direkt pruefen, ob Authentifizierung und Zugriff funktionieren:

```bash
curl --location --request GET \
  'https://www.intensivregister.de/api/stammdaten/meldebereich' \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer <ACCESS_TOKEN>'
```

Die Antwort ist ein JSON-Dokument mit den dem Client zugewiesenen Meldebereichen.

## 5. Fachliche Anwendungsfaelle

Generell gilt, dass in der api-docs.json die notwendigen Endpunkte (exklusive dem Keycloak-Endpunkt) enthalten sind.
Diese api-docs.json kann verwendet werden, um sich daraus in der bevorzugten Programmiersprache source files generieren zu lassen, mit welchen man dann sein automatisiertes Meldungssystem aufbauen kann.

### 5.1 Meldebereiche des Nutzers abfragen

- `GET /stammdaten/meldebereich`

### 5.2 Letzte Meldung eines Meldebereichs abfragen

- `GET /stammdaten/meldebereich/{meldebereichId}/letzte-meldung`

### 5.3 Meldungen senden oder aktualisieren

- `POST /meldungen` (neue Meldung)
- `PUT /meldungen/{meldungId}` (bestehende Meldung aktualisieren)

Technisch benoetigt werden:

- ein gueltiges Access-Token,
- die ID des Meldebereichs,
- eine selbst erzeugte UUID fuer neue Meldungen bzw. die bestehende `meldungId` fuer Updates.

Hinweis: Ist die `meldungId` bereits durch einen anderen Meldebereich belegt, wird die Meldung mit Status-Code `403` abgelehnt.

Wichtige Feldhinweise:

- `id`: selbst generierte UUID (neu) oder ID der zu aktualisierenden Meldung
- `api_version`: muss `V2` sein (`V1` wird nicht mehr akzeptiert)

`null`-Werte (oder nicht gesendete optionale Felder) sind grundsaetzlich erlaubt und bedeuten, dass kein verifizierter Zustand vorliegt.

Die Verordnung zur Krankenhauskapazitaetssurveillance definiert die Meldepflicht fuer bestimmte Inhalte:
[Verordnung zur Krankenhauskapazitaetssurveillance](https://www.gesetze-im-internet.de/khkapsurv/BJNR626200022.html)

Technisch verpflichtende Eingabefelder:

- `id`
- `meldebereich.id`
- `kapazitaeten.intensivBetten`
- `kapazitaeten.intensivBettenBelegt`

Spezialfall Betriebssituation:

Ist `betriebssituation` gleich `KEINE_ANGABE` oder `REGULAERER_BETRIEB`, sollten die folgenden Felder `null` sein:

- `betriebseinschraenkungPersonal`
- `betriebseinschraenkungRaum`
- `betriebseinschraenkungBeatmungsgeraet`
- `betriebseinschraenkungVerbrauchsmaterial`

In anderen Betriebssituationen koennen beliebig viele dieser Gruende gesetzt werden.

Fuer eine genaue fachliche Datenfeld-Definition wenden Sie sich bitte an das RKI.

## 6. Meldungsfreigabe (fachliche Aktivierung)

Meldungen koennen technisch angenommen werden, ohne sofort fachlich freigegeben zu sein:

- Mit Client ID/Secret koennen Meldungen gesendet werden (bei zugeordnetem Meldebereich).
- Ohne Freigabe werden Meldungen gespeichert, aber nicht fachlich ausgewertet und nicht in der Oberflaeche angezeigt (`aktiv = 0`).
- Nach Qualitaetspruefung durch das RKI wird der Client freigegeben. Nachfolgende Meldungen sind dann automatisch freigegeben.
- Bereits zuvor gespeicherte Meldungen bleiben im Zustand "nicht freigegeben".

Auf der TEST-Umgebung erfolgt die Freigabe in der Regel direkt; die obigen Einschraenkungen betreffen primaer die **PROD-Umgebung**.

## 7. Validierungen und Fehlerformat

Vor Speicherung oder Aktualisierung werden Meldungen plausibilisiert.

- Es gibt Feldvalidierungen (z. B. `0 <= faelleCovidAktuell <= 999`).
- Es gibt felduebergreifende Regeln (z. B. `faelleCovidAktuell <= intensivBettenBelegt`).

Bei Verstoessen antwortet die API mit `400 Bad Request` und einer `errors`-Liste.
Jeder Eintrag enthaelt typischerweise:

- `errorCode`
- `propertyPath`
- `errorMessage` (in manchen Responses als `errormessage` ausgegeben)

Beispiel-Response:

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

Weiterfuehrende Plausibilisierungsregeln:
[https://github.com/Intensivregister/intensivregister-meldungsvalidierung](https://github.com/Intensivregister/intensivregister-meldungsvalidierung)

## 8. Kompatibilitaet

Die API wird grundsaetzlich rueckwaertskompatibel weiterentwickelt; fuer aeltere Requests gibt es in der Regel eine Uebergangsphase von mehreren Monaten.

Wichtig fuer Clients:

- Aktivieren Sie bei der Antwortverarbeitung ein "ignore unknowns"-Verhalten.
- Neue Felder koennen jederzeit in Responses auftauchen.

Beispiel alt:

```json
{
  "meldung_id": "abcd",
  "kapazitaeten": { ... }
}
```

Beispiel erweitert:

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

Nicht oeffentlich kommunizierte Endpunkte koennen technisch nutzbar sein, werden aber nicht offiziell unterstuetzt.

## 9. Kontakt

- Hilfe bei der Einrichtung von Zugaengen: `intensivregister-hilfe@rki.de`
- Technische Fragen zur Schnittstelle/diesem Dokument: `ir-tech-support@rki.de`
- Allgemeiner Kontakt RKI: `intensivregister@rki.de`
