# Schnittstellenbeschreibung des Intensivregisters - Quick Start Guide

## 1. Ziel dieses Dokuments

Dieses Dokument beschreibt den schnellen Einstieg in die Intensivregister-API: Zugang, Authentifizierung mit Bearer-Token, erste Aufrufe sowie die wichtigsten fachlichen Endpunkte.

## 2. Getting Started

### 2.1 Zugang einrichten

1. Registrieren Sie sich im [Partner-Portal](https://partner.intensivregister.de).
2. Nach erfolgreicher Registrierung finden Sie unter **Zugänge** Ihre Zugangsdaten (Client ID und Client Secret).
3. Mit diesen Credentials fordern Sie ein Access-Token bei der Authentifizierung (Keycloak/OpenID Connect) an.

### 2.2 Umgebungen

Das Intensivregister bietet zwei getrennte Umgebungen:

- **TEST**: Entwicklung und Integrationstests, nicht für den produktiven Betrieb.
- **PROD**: Produktive Umgebung für die Erfassung realer Meldungen.

Für API-Partner werden Accounts und OIDC-Clients auf beiden Umgebungen angelegt.

### 2.3 Basis-URLs

**TEST**

- `ACCESS_TOKEN_URL`: `https://auth.intensivregister.de/realms/intensivregister-alike/protocol/openid-connect/token`
- `API_URL`: `https://prod-alike.intensivregister.de/api`

**PROD**

- `ACCESS_TOKEN_URL`: `https://auth.intensivregister.de/realms/intensivregister/protocol/openid-connect/token`
- `API_URL`: `https://www.intensivregister.de/api/`

## 3. Authentifizierung und Bearer-Token

### 3.1 Access-Token anfordern

Zur Authentifizierung wird der `client_credentials`-Flow verwendet (*keycloak). Die Token-Antwort enthält u. a. das Feld `access_token` und die Gültigkeitsdauer.

```bash
curl --location --request POST \
  'https://auth.intensivregister.de/realms/intensivregister/protocol/openid-connect/token' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=client_credentials' \
  --data-urlencode 'client_id=<CLIENT_ID>' \
  --data-urlencode 'client_secret=<CLIENT_SECRET>'
```

Typisches Antwortmuster (gekürzt):

```json
{
  "access_token": "ey...",
  "expires_in": 1800,
  "refresh_expires_in": 1800,
  "refresh_token": "ey...",
  "token_type": "Bearer",
  "not-before-policy": 0,
  "session_state": "...",
  "scope": "profile email"
}
```

### 3.2 Bearer-Token verwenden

Das Access-Token muss bei **jedem** API-Aufruf im Header `Authorization` mit dem Prefix `Bearer ` gesendet werden:

`Authorization: Bearer <access_token>`

Ohne gültiges Token werden Requests abgewiesen.

### 3.3 Postman Collection

Verwenden Sie die Collection `Intensivregister - Meldung erfassen - Quick-Start.postman_collection.json`.

- Sie enthält die relevanten Requests für Authentifizierung und Meldungsabgabe.
- Die Collection nutzt Platzhalter für die API-Basis-URL. Setzen Sie je nach Umgebung die Variable `API_URL` auf.
- `https://prod-alike.intensivregister.de/api` (TEST)
- `https://www.intensivregister.de/api/` (PROD)

## 4. Erster API-Aufruf (Smoke Test)

Mit folgendem Request können Sie direkt prüfen, ob Authentifizierung und Zugriff funktionieren:

```bash
curl --location --request GET \
  'https://www.intensivregister.de/api/stammdaten/meldebereich' \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer <ACCESS_TOKEN>'
```

Die Antwort ist ein JSON-Dokument mit den dem Client zugewiesenen Meldebereichen.

## 5. Fachliche Anwendungsfälle

Generell gilt, dass in der `api-docs.json` die notwendigen Endpunkte (exklusive dem Keycloak-Endpunkt) enthalten sind.
Diese `api-docs.json` kann verwendet werden, um sich daraus in der bevorzugten Programmiersprache Source Files generieren zu lassen, mit welchen man dann sein automatisiertes Meldungssystem aufbauen kann (*openapi-generator).

### 5.1 Meldebereiche des Nutzers abfragen

- `GET /stammdaten/meldebereich`

### 5.2 Letzte Meldung eines Meldebereichs abfragen

- `GET /stammdaten/meldebereich/{meldebereichId}/letzte-meldung`

### 5.3 Meldungen senden oder aktualisieren

- `POST /meldungen` (neue Meldung)
- `PUT /meldungen/{meldungId}` (bestehende Meldung aktualisieren)

Technisch benötigt werden:

- ein gültiges Access-Token,
- die ID des Meldebereichs,
- eine selbst erzeugte UUID für neue Meldungen bzw. die bestehende `meldungId` für Updates.

Hinweis: Ist die `meldungId` bereits durch einen anderen Meldebereich belegt, wird die Meldung mit Status-Code `403` abgelehnt (*meldung-403).

Wichtige Feldhinweise:

- `id`: selbst generierte UUID (neu) oder ID der zu aktualisierenden Meldung
- `api_version`: muss `V2` sein (`V1` wird nicht mehr akzeptiert)

Weitere fachliche Feldregeln (Pflichtfelder, `null`-Verhalten, Betriebssituation und Plausibilitätskontext) finden Sie im Detaildokument:
[Client-seitige Qualitätssicherung für das DIVI-Intensivregister für Version V2 des Abfragebogens](<./Client-seitige Qualitätssicherung für das DIVI-Intensivregister für Version V2 des Abfragebogens.md>)

## 6. Meldungsfreigabe (fachliche Aktivierung)

Meldungen können technisch angenommen werden, ohne sofort fachlich freigegeben zu sein:

- Mit Client ID/Secret können Meldungen gesendet werden (bei zugeordnetem Meldebereich).
- Ohne Freigabe werden Meldungen gespeichert, aber nicht fachlich ausgewertet und nicht in der Oberfläche angezeigt (`aktiv = 0`).
- Nach Qualitätsprüfung durch das RKI wird der Client freigegeben. Nachfolgende Meldungen sind dann automatisch freigegeben.
- Bereits zuvor gespeicherte Meldungen bleiben im Zustand "nicht freigegeben".

Auf der TEST-Umgebung erfolgt die Freigabe in der Regel direkt; die obigen Einschränkungen betreffen primär die **PROD-Umgebung**.

## 7. Validierungen und Fehlerformat

Vor Speicherung oder Aktualisierung werden Meldungen plausibilisiert.

- Es gibt Feldvalidierungen (z. B. `0 <= faelleCovidAktuell <= 999`).
- Es gibt feldübergreifende Regeln (z. B. `faelleCovidAktuell <= intensivBettenBelegt`).

Bei Verstößen antwortet die API mit `400 Bad Request` und einer `errors`-Liste.
Jeder Eintrag enthält typischerweise:

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

Weiterführende Plausibilisierungsregeln:
[https://github.com/Intensivregister/intensivregister-meldungsvalidierung](https://github.com/Intensivregister/intensivregister-meldungsvalidierung)

## 8. Kompatibilität

Wichtig für Clients:

- Aktivieren Sie bei der Antwortverarbeitung ein "ignore unknowns"-Verhalten.
- Neue Felder können jederzeit in Responses auftauchen.

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

Nicht öffentlich kommunizierte Endpunkte können technisch nutzbar sein, werden aber nicht offiziell unterstützt.

## 9. Kontakt

- Hilfe bei der Einrichtung von Zugängen und bei technischen Fragen: `intensivregister-hilfe@rki.de`
- Allgemeiner Kontakt RKI: `intensivregister@rki.de`

## Annotationen

- (*keycloak): Wir nutzen Keycloak, eine OpenID-Connect-fähige Lösung.
- (*openapi-generator): Eine gute Option für die automatische Client-Generierung ist der [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator).
- (*meldung-403): Falls eine `meldungId` bereits von einem anderen Meldebereich verwendet wird, wird die Meldung mit `403` abgelehnt.


