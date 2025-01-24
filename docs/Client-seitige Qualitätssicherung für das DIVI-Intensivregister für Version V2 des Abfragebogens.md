# Client-seitige Qualitätssicherung für das DIVI-Intensivregister für Version V2 des Abfragebogens
*Verfasser: RKI-Team des DIVI-Intensivregisters (intensivregister@rki.de)*

Dieses Dokument fasst alle Plausibilitäts-Checks & Systemanforderungen zusammen, welche der Software-Hersteller beim Erfassen von Meldungen für das DIVI-IntensivRegister implementiert haben soll.

**Hinweise**:
* **Durchgestrichene Passagen** betreffen derzeit **pausierte**, d. h. nicht erfragte Variablen. Nach Ende der Pausierung treten die betreffenden Prüfregeln wieder in Kraft.
* Die Nummerierung der Prüfregeln ist historisch bedingt. Durch nachträgliche Änderungen, Hinzufügen oder Streichen von Plausibilitätsprüfungen kann daher ein Sprung in der Nummerierung auftreten.

## 1) Zusammenfassung nach der Meldung aller Daten
Nach der Eingabe aller Datenfelder sieht der User in einem letzten Schritt jene eingegebenen Daten zusammengefasst, die sich seit der letzten Meldung geändert haben. Diese Übersicht nennen wir im weiteren Verlauf des Dokuments "Meldungszusammenfassungsseite". Hier sollen die eingegeben Werte noch einmal überprüft werden, bevor sie final gespeichert werden. Dies dient der Sicherstellung einer hohen Datenqualität. An diesem Schritt sollen auch Warnmeldungen erscheinen, um den User auf potenzielle Fehler hinzuweisen (siehe Kapitel 2, 6 und 7).

## 2) Pflichtfelder zum Speichern der Meldung
Eine Meldung kann von technischer Seite gespeichert werden, wenn sie mindestens Angaben zu den folgenden Datenfeldern enthält:
* betreibbare Betten *(intensiv_betten)*
* belegte Betten *(intensiv_betten_belegt)* 

Für die anderen Datenfelder sind auch NULL-Werte zugelassen. Das Speichern der Meldung ist trotzdem möglich. Wenn eines dieser Felder leer gelassen wurde (NULL), sollte es einen Warnhinweis geben. Im Intensivregister Frontend ist der Warnhinweis so implementiert, dass er auf der Meldungszusammenfassungsseite angezeigt wird, wenn ein Datenfeld mit Zahlenwert aus einer vorherigen Meldung auf eine NULLmeldung wechselt. Danach werden keine weiteren Warnhinweise bei späteren Meldungen angezeigt, bis wieder erstmalig ein Datenfeld leer gelassen wird.
**Bitte beachten Sie: Grundsätzlich besteht eine Meldepflicht, die über die technischen Pflichtfelder hinausgeht. Die gesetzliche Grundlage bildet die Verordnung zur Krankenhauskapazitätssurveillance (https://www.gesetze-im-internet.de/khkapsurv/BJNR626200022.html). Alle Datenfelder sind demnach rechtlich verpflichtend für die Meldung, ausgenommen Datenfelder, welche explizit mit dem Label „(optional)“ versehen sind.**

#### Fall: Datenfeld Betriebseinschränkung
NULL-Werte sind auch hier zugelassen und bedeuten, dass der Anwender nicht um den Zustand des Feldes weiß.
* Bei Auswahl der Betriebssituation mit ‚KEINE_ANGABE‘ oder ‚REGULAERER_BETRIEB‘ sollen die vier Felder für Betriebseinschränkungen (betriebseinschraenkungPersonal, betriebseinschraenkungRaum, betriebseinschraenkungBeatmungsgeraet, betriebseinschraenkungVerbrauchsmaterial) NULL sein. 
* Bei Auswahl der Betriebssituation mit ‚TEILWEISE_EINGESCHRAENKT‘ oder ‚EINGESCHRAENKT‘ sollen die vier Felder ‚Gründe der Betriebseinschränkung‘ angezeigt werden. Es können beliebig viele dieser Betriebseinschränkungsgründe gesetzt werden (Mehrfachauswahl), kein Ausfüllen (NULL-Wert) der vier Felder ist auch möglich.

## 3) Notwendige Datentypen für Datenfelder
Die Datentypen der gemeldeten Werte müssen mit den Datentypen übereinstimmen, die in der Dokumentation der Schnittstelle (https://www.intensivregister.de/api/public/api-docs-ui) als Schema hinterlegt sind.

## 4) Notwendige Zahlenintervalle für numerische Eingabewerte
Die gemeldeten Werte aller numerischer Datenfelder müssen ganze Zahlen sein in dem Intervall [0,999] (0 und 999 eingeschlossen).

## 5) Notwendige Prüfregeln auf Eingabeseite
Die Prüfregeln sind **immer** aktiv, selbst wenn kein Wert in einem für die Regel relevanten Feld eingegeben wurde. Die Prüfregel **muss** eingehalten werden, um eine Meldung speichern zu können.

Wenn ein für die Regel relevantes Datenfeld **nicht** ausgefüllt wurde, werden für die Prüfregel leere Felder als die Zahl „0“ interpretiert. *Beispiel:* Für intensiv_betten wurde kein Wert eingetragen, intensiv_betten_belegt = 2. Nach Regel 5A wird nun getestet auf 0 >= 2. Da dies nicht erfüllt ist, wird ein Speichern verhindert.

Sollte gegen eine Regel verstoßen werden, soll der User auf diesen Regelbruch hingewiesen werden (z. B. in Form einer Fehlermeldung oder durch ein Pop-up während der Eingabe). 

* **Regel 5A:** intensiv_betten >= intensiv_betten_belegt 

* **Regel 5B (Beziehung freie Behandlungskapazitäten <-> Bettenzahl):**  
    **5B4** intensiv_betten >= freie_iv_kapazitaet  
    **5B5** intensiv_betten >= freie_ecmo_kapazitaet 

* **Regel 5C (Beziehung COVID-19-(bzw. RSV-, Influenza-)Patient\*innen_Gesamt <-> Behandlungsgruppen COVID-19-/RSV-/Influenza-Patient\*innen):**

    **Regeln für COVID-19 (alle Meldebereiche)**  
    **5C4A** faelle_covid_aktuell_mit_manifestation >= faelle_covid_aktuell_mit_manifestation_ecmo  
    **5C5A** faelle_covid_aktuell_mit_manifestation >= faelle_covid_aktuell_mit_manifestation_beatmet + faelle_covid_aktuell_mit_manifestation_nicht_invasiv_beatmet  
    
    **5C5B** faelle_covid_aktuell_ohne_manifestation >= faelle_covid_aktuell_ohne_manifestation_beatmet

    **5C6** faelle_covid_aktuell ==
    faelle_covid_aktuell_mit_manifestation + faelle_covid_aktuell_ohne_manifestation
     
    <del>**Regeln für RSV (nur für Kinder-Meldebereiche)**  
    **5C41** faelle_rsv_aktuell >= faelle_rsv_aktuell_ecmo  
    **5C51** faelle_rsv_aktuell >= faelle_rsv_aktuell_beatmet + faelle_rsv_aktuell_high_flow_oxygen + faelle_rsv_aktuell_nicht_invasiv_beatmet</del>

    <del>**Regeln für Influenza (nur für Kinder-Meldebereiche)**  
    **5C42** faelle_influenza_aktuell >= faelle_influenza_aktuell_ecmo  
    **5C52** faelle_influenza_aktuell >= faelle_influenza_aktuell_beatmet + faelle_influenza_aktuell_high_flow_oxygen + faelle_influenza_aktuell_nicht_invasiv_beatmet</del>

* **Regel 5D (Beziehung Patient\*innen_Alle <-> COVID-19-/RSV-/Influenza-Patient\*innen):**  
    Hinweis: Für Erwachsenen-Meldebereiche sind RSV/Influenza-Felder alle NULL, im Sinne der Validierung 0.  
    **5D1** patienten_invasiv_beatmet >= faelle_covid_aktuell_mit_manifestation_beatmet + faelle_covid_aktuell_ohne_manifestation_beatmet <del>+ faelle_rsv_aktuell_beatmet + faelle_influenza_aktuell_beatmet</del>.
    
    **5D2** patienten_nicht_invasiv_beatmet >= faelle_covid_aktuell_mit_manifestation_nicht_invasiv_beatmet <del>+ faelle_rsv_aktuell_nicht_invasiv_beatmet + faelle_influenza_aktuell_nicht_invasiv_beatmet</del>

    **5D3** patienten_ecmo >= faelle_covid_aktuell_mit_manifestation_ecmo <del>+ faelle_rsv_aktuell_ecmo + faelle_influenza_aktuell_ecmo</del>
    
    **5D4** intensiv_betten_belegt >= faelle_covid_aktuell + faelle_rsv_aktuell + faelle_influenza_aktuell

* **Regel 5E (Beziehung Belegte Betten_Patient\*innen_Alle <-> Behandlungsgruppen Patient\*innen_Alle):**  
    **5E1** intensiv_betten_belegt >= patienten_ecmo  
    **5E4** intensiv_betten_belegt >= patienten_invasiv_beatmet + patienten_nicht_invasiv_beatmet 

* **Regel 5G (Beziehung COVID-19-Patient\*innen <-> COVID-19-Alterstrata):**  
    **5G1** Folgende Altersstrata werden *nicht* in Kinder-Meldebereichen erfasst, sondern **nur** für Erwachsenen-Meldebereiche bzw. Meldebereiche ohne definierten Behandlungsschwerpunkt:
    * stratum17minus
    * stratum18bis29
    * stratum30bis39
    * stratum40bis49
    * stratum50bis59
    * stratum60bis69
    * stratum70bis79
    * stratum80plus

    **5G2** sum(COVID-19-Alterstrata) <= faelle_covid_aktuell  
    *Diese Regel prüft „<=“, damit keine Meldungen verloren werden, wenn Meldebereiche einige Altersgruppen nicht angeben können oder möchten. Siehe hierzu auch Regel 6B: nicht speicherverhindernde Warnung bei sum (Alterstrata COVID-19-Fälle) < faelle_covid_aktuell*
    
* **Regel 5J (Beziehung COVID-19-Patient\*innen <-> SARS-CoV-2-Virusvarianten):**  
    **5J1** sum(SARS-CoV-2-Virusvarianten-Gruppen) <= faelle_covid_aktuell  
    *Diese Regel prüft „<=“, damit keine Meldungen verloren werden, wenn Meldebereiche keine Angaben zu VOCs machen.*
    
* **Regel 5K (Neuaufnahmen und Verlegungen/Versterben von COVID-19-Patient\*innen):**  
    **5K1** Wenn die betreffende Meldung die erste Meldung des Kalendertages (0:00:00 - 23:59:59 Uhr) ist, erfolgt keine automatische Vorausfüll-Funktionalität bei den Datenfeldern Neuaufnahmen und Verlegungen/Versterben. Falls am gleichen Kalendertag schon eine Meldung für den Meldebereich abgegeben wurde, wird der Wert der letzten vorherigen Meldung des Tages vorausgefüllt (siehe auch Abschnitt 9, Ausnahmen).

    **5K2** (faelle_covid_vortag_erstaufnahmen <del>+ faelle_covid_vortag_verlegungen</del>) < (intensiv_betten * 2) + 10

    **5K3** (faelle_covid_vortag_verstorben + faelle_covid_vortag_verlegt) <  (intensiv_betten * 2) +10 
    
* **Regel 5L (Schwangere COVID-19-Patient\*innen):**  
    **5L1** Die Abfrage zu Schwangeren soll nur für Meldebereiche angezeigt werden, die als  Behandlungsschwerpunkt „Erwachsene“ angegeben haben oder den Behandlungsschwerpunkt nicht angegeben haben („NA“) 

    **5L2** anzahl_schwangere <= faelle_covid_aktuell

* **Regel 5M (Impf-Status der COVID-19-Patient\*innen):**  
    **5M3A** faelle_covid_vortag_erstaufnahmen == impfstatus_unbekannt + impfstatus_0_impfungen + impfstatus_1_impfung + impfstatus_2_impfungen + impfstatus_3_impfungen + impfstatus_4+_impfungen 

    **5M7** Wenn die betreffende Meldung die erste Meldung des Kalendertages (0:00:00 - 23:59:59 Uhr) ist, erfolgt keine automatische Vorausfüll-Funktionalität bei den Datenfeldern für Impfstatus. Falls am gleichen Kalendertag schon eine Meldung für den Meldebereich abgegeben wurde, werden die gemeldeten Werte für Impfstatus der letzten vorherigen Meldung des Tages vorausgefüllt (siehe auch Abschnitt 9, Ausnahmen).
    
* **Regel 5P (technische Regel):**  
    **5P1** Wenn meldung.covid19StatusV3 übermittelt wird, müssen folgende veraltete Felder NULL sein (bzw. nicht definiert):
    * faelle_covid_aktuell_beatmet
    * faelle_covid_aktuell_high_flow_oxygen
    * faelle_covid_aktuell_nicht_invasiv_beatmet
    * faelle_covid_aktuell_ecmo
    * faelle_covid_ohne_symptomatik

## 6) Warnmeldung bei bestimmten Prüfregeln auf Eingabeseite

Die folgenden Prüfregeln werden nur mit einer Warnung versehen. Eine Meldung kann trotzdem gespeichert werden, auch wenn diese Prüfregeln nicht erfüllt sind. 
Bei Regelbruch sollte eine Warnung mit der Aufforderung, die Angaben zu überprüfen, erscheinen (z. B. auf der Meldungszusammenfassungsseite oder durch ein Pop-up bei der Eingabe). Wenn ein für die Regel relevantes Datenfeld nicht ausgefüllt wurde, soll die Warnmeldung folglich nicht angezeigt werden.

* **Regel 6A** patienten_invasiv_beatmet >= patienten_ecmo

    **Regel 6A2A** faelle_covid_aktuell_mit_manifestation_beatmet >= faelle_covid_aktuell_mit_manifestation_ecmo

* **Regel 6B** Eine Warnung auf der Zusammenfassungsseite ist anzuzeigen, wenn die Summe(COVID-19-Altersstrata) < faelle_covid_aktuell.

## 7) Warnmeldung bei stark veränderten Werten
Bei zu starker Abweichung von der letzten Meldung sollte die folgende Warnmeldung für den User erscheinen: "Sind sie sicher, dass die Eingabe bei [Label] [Wert] korrekt ist, da die Zahl sehr stark abweicht?". Diese Warnmeldungen sollten nur bei numerischen Datenfeldern angezeigt werden. Wie groß die Abweichung sein muss, damit eine Warnmeldung angezeigt wird, hängt von der Größenordnung der vorherigen Meldung ab:
* bei Größenordnung 0-9 vorheriger Meldungen: Warnmeldung nur bei Sprung von +- >= 7
* bei Größenordnung 10-19 vorheriger Meldungen: Warnmeldung bei Verdoppelung oder Reduktion auf ein Viertel
* bei Größenordnung ab 20 vorheriger Meldungen: Warnmeldung bei Verdoppelung oder Halbierung

Eine Warnung muss für jedes Datenfeld, das eine starke Abweichung aufzeigt, einzeln angezeigt werden. Diese Warnungen erscheinen mit der Aufforderung, die Angaben auf der Zusammenfassungs-Seite der Meldung zu überprüfen. 

Die Integration der Warnmeldung setzt voraus, dass das Client-System die letzte Meldung des Meldebereichs gespeichert hat und abrufen kann.

## 8) Anzeige der abgefragten Datenfelder je nach Meldebereich
Alle Datenfelder werden in „Meldung erfassen“ per Default allen Meldebereichen angezeigt.

**Ausnahmen sind:**
1.	Die Abfrage des Alters erfolgt **nur** bei Meldebereichen für **Erwachsene** bzw. ohne Behandlungsschwerpunkt (siehe auch 5G1) über folgende Variablen:
    * stratum17minus
    * stratum18bis29
    * stratum30bis39
    * stratum40bis49
    * stratum50bis59
    * stratum60bis69
    * stratum70bis79
    * stratum80plus
2.	Felder zu RSV- und Influenza-Patient\*innen werden **nur** für **Kinder** erfasst:
    * faelle_rsv_aktuell
    * <del>faelle_rsv_aktuell_high_flow_oxygen</del>
    * <del>faelle_rsv_aktuell_nicht_invasiv_beatmet</del>
    * <del>faelle_rsv_aktuell_beatmet</del>
    * <del>faelle_rsv_aktuell_ecmo</del>
    * faelle_influenza_aktuell
    * <del>faelle_influenza_aktuell_high_flow_oxygen</del>
    * <del>faelle_influenza_aktuell_nicht_invasiv_beatmet</del>
    * <del>faelle_influenza_aktuell_beatmet</del>
    * <del>faelle_influenza_aktuell_ecmo</del>
3.	Die Abfrage zu schwangeren COVID-19-Patient\*innen erfolgt **nur** bei Meldebereichen für **Erwachsene** bzw. ohne Behandlungsschwerpunkt erfasst (siehe Regel 5L1):
    * anzahl_schwangere 
4.	ECMO-bezogene Eingabefelder (in *ICU-Status*, *COVID-19-Status*, <del>*RSV- und Influenza-Status*</del>) werden **nicht** in Meldebereichen abgefragt, die unter „Mein Krankenhausstandort/ Meldebereichsdaten“ die Check-Box des dortigen Datenfeldes „ICU ECMO vorhanden“ **abgewählt** haben und damit über keine ECMO-Kapazitäten im Meldebereich verfügen.

## 9) Funktionalität: Automatisches Werte-Vorausfüllen
Beim „Meldung erfassen“ kann eine automatische Werte-Vorausfüll-Funktionalität genutzt werden, die alle Datenfelder automatisch mit den Daten der letzten Erfassung befüllt.
Dies soll den Meldenden das Melden erleichtern, da nur Werte, die sich seit der letzten Erfassung verändert haben, angepasst werden müssen. Die Funktionalität setzt voraus, dass das Client-System die letzte Meldung des Meldebereichs gespeichert hat und abrufen kann.

**Ausnahmen sind:**

Für die Abfragen "Verlegte COVID-19-Intensivpatient\*innen", "Verstorbene COVID-19-Intensivpatient\*innen, "ITS-Erstaufnahme" <del>und "ITS-zu-ITS-Verlegung"</del> sowie zum Impfstatus im Reiter „COVID-19-Status: Neuaufnahmen und Abgänge“ erfolgt keine automatische Vorausfüll-Funktionalität, wenn die betreffende Meldung die erste Meldung des Kalendertages (0:00:00 - 23:59:59 Uhr) ist. Falls jedoch am gleichen Kalendertag schon eine Meldung für den Meldebereich abgegeben wurde, werden die Datenfelder mit der letzten vorherigen Meldung des Tages vorausgefüllt.

Auch im Korrektur-Modus einer schon erfolgten Meldung ist ein Vorausfüllen unabhängig vom Zeitpunkt der vorhergehenden Meldung möglich, auch für die hier unter *Ausnahmen* aufgeführten Datenfelder.

## 10) Funktionalität: Nachträgliche Werte-Korrektur von erfassten Meldungen
Eine Meldung kann rückwirkend bis zu 14 Tage nach der initialen Erfassung der Meldung von allen Meldenden eines Meldebereichs korrigiert werden.
Im Frontend des Intensivregisters ist die Korrekturfunktionalität zu finden unter dem Menüpunkt "Mein Krankenhaus-Standort/ Mein Meldebereich:/ Meldungshistorie des Meldebereichs (korrigierbar)".

