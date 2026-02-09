# Abfragekategorien des DIVI – Intensivregisters
*Verfasser: RKI-Team des DIVI-Intensivregisters (intensivregister@rki.de)*

Das folgende Dokument führt alle aktuellen Text-Labels und Mouseovers auf, die auf dem DIVI-Intensivregister Front-End beim Erfassen einer täglichen Meldung zu sehen sind. In den Tabellen befinden sich die Textlabel der Datenfelder, die Mouseover-Texte für die jeweiligen Datenfelder sowie die zugehörigen, Schnittstellen-relevanten Variablennamen.

**Hinweise zum Dokument:**
* Die **Reihenfolge** der Abfragen in diesem Dokument spiegelt die Abfragemaske im Front-End wider. Die Nummerierung der einzelnen Abfragen (Q1 – Q52) ist historisch begründet und bei Änderungen der Abfragemaske beibehalten worden, sodass eine einmal zugeordnete Nummerierung bestehen bleibt.
* Solange kein Hinweis erfolgt, gelten die Abfragen für alle Meldebereiche (Erwachsenen-ITS, PICU, NICU).
* **Durchgestrichene Passagen** betreffen derzeit **pausierte**, d. h. nicht erfragte Variablen. Nach Ende der Pausierung sollen die Variablen wieder erfragt werden.

## ICU-STATUS
### 1. Erfassung: Betreibbare Intensivbetten
*i-Button: Alle betreibbaren Intensivbetten in Ihrem Meldebereich*
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q1:** Gesamt aktuell betreibbare Intensivbetten im Rahmen der Akutversorgung | Wie viele der regulär aufgestellten Intensivbetten (freie + belegte) sind aktuell im Meldebereich betreibbar im Rahmen der Akutversorgung? | Integer | kapazitaeten: intensivBetten |
| **Q2:** Anzahl aktuell belegter Intensivbetten oder verplant zur Belegung (bis 23:59 Uhr des jeweiligen Tages) | Teilmenge von „Gesamt aktuell betreibbare Intensivbetten“. Wie viele Intensivbetten sind im Rahmen der Akutversorgung aktuell im Meldebereich belegt (unabhängig von der Behandlungsursache) bzw. heute zur Belegung verplant? | Integer | kapazitaeten: intensivBettenBelegt |

### 2. Erfassung: Intensivpatient\*innen mit Beatmung/ECMO
*i-Button: Alle Patient\*innen mit Beatmung oder ECMO-Behandlung in Ihrem Meldebereich (unabhängig von der Behandlungsursache)*
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q3:** Alle Intensivpatient\*innen mit nicht-invasiver (NIV) Beatmung im Rahmen der Akutversorgung | Wie viele Patient*innen erhalten im Rahmen der Akutversorgung eine nicht-invasive Beatmung (NIV) als höchste Stufe der respiratorischen Unterstützung in den letzten 24 h (unabhängig von der Behandlungsursache) in Ihrem Meldebereich? | Integer | kapazitaeten: patientenNichtInvasivBeatmet |
| **Q4:** Alle Intensivpatient\*innen mit invasiver Beatmung im Rahmen der Akutversorgung | Wie viele Patient\*innen erhalten im Rahmen der Akutversorgung aktuell eine invasive Beatmung (unabhängig von der Behandlungsursache) in Ihrem Meldebereich)? \*Hier sind alle invasiv beatmeten Patient\*innen zu berücksichtigen, unabhängig davon, ob bei ihnen (auch) eine ECMO-Behandlung läuft. | Integer | kapazitaeten: patientenInvasivBeatmet |
| **Q5:** Alle Intensivpatient\*innen mit ECMO-Behandlungen im Rahmen der Akutversorgung | Wie viele Patient\*innen erhalten im Rahmen der Akutversorgung aktuell eine ECMO-Behandlung (unabhängig von der Behandlungsursache) in Ihrem Meldebereich; unabhängig davon, ob (auch) eine Beatmungsbehandlung vorliegt?| Integer | kapazitaeten: patientenEcmo |

### 3. Erfassung: Freie Behandlungskapazitäten
*i-Button: Alle freien betreibbaren Behandlungskapazitäten (unabhängig von der Behandlungsursache)*
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q6:** Freie betreibbare invasive Beatmungsmöglichkeiten im Rahmen der Akutversorgung | Wie viele weitere invasive Beatmungen im Rahmen der Akutversorgung (zusätzlich zu den bereits laufenden) sind aktuell möglich (unabhängig von der Behandlungsursache)? \*Hier ist die mögliche Gesamtzahl der zusätzlich invasiven Beatmungen gemeint: Berücksichtigen Sie dafür mögliche Neuaufnahmen von Patient\*innen zur invasiven Beatmung in freien Betten sowie Patient\*innen, die schon in Behandlung sind und ggf. verlegt werden können, um weitere Kapazität zu schaffen. Die hier angegebene Zahl kann damit die Zahl Ihrer aktuell freien Betten auch übersteigen. | Integer | kapazitaeten: freieIvKapazitaet |
| **Q7:** Freie betreibbare ECMO-Behandlungsmöglichkeiten | Wie viele weitere ECMO-Behandlungen (zusätzlich zu den bereits laufenden) sind aktuell möglich (unabhängig von der Behandlungsursache)? \*Berücksichtigen Sie dazu mögliche Neuaufnahmen in freie Betten von Patient\*innen, die ECMO benötigen, sowie Patient\*innen, die schon in Behandlung sind, ohne bisher nötige ECMO-Behandlung. | Integer | kapazitaeten: freieEcmoKapazitaet |

### 4. Erfassung: Verfügbarkeit
*i-Button: Berücksichtigt werden neben Bettenauslastung auch Personalsituation, Arbeitsbelastung etc.*
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| Wie schätzen Sie persönlich die aktuelle Situation der Behandlungsplätze in Ihrem Meldebereich ein? | - | *{Dies ist ein erklärender Text zu Folgefragen Q8.1 – Q8.3 im Frontend. Der erklärende Text selbst hat keine Antwortoption.}* | - |
| **Q8.2:** ICU High-Care (optional) | - | **Frontend:** „Verfügbar“, „Begrenzt“, „Ausgelastet“, „Keine Angabe“ **API-Meldung:** „VERFUEGBAR“, „BEGRENZT“, „NICHT_VERFUEGBAR“, "KEINE_ANGABE"| kapazitaeten: statusEinschaetzungHighcare |
| **Q8.3:** ICU ECMO (optional) | - | **Frontend:**„Verfügbar“, „Begrenzt“, „Ausgelastet“, „Keine Angabe“ **API-Meldung:** „VERFUEGBAR“, „BEGRENZT“, „NICHT_VERFUEGBAR“, "KEINE_ANGABE" | kapazitaeten: statusEinschaetzungEcmo |

**Q8.1** und **Q9** wurden gestrichen.

## ICU-RESERVE
### 5. Erfassung: Anzahl zusätzlich aktivierbarer Intensivbehandlungsplätze im Notfall-Szenario
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q10:** Wie viele Intensivbehandlungsplätze, außerhalb der aktuell tatsächlich aufgestellten Intensivbehandlungsplätze, ließen sich im Rahmen eines intensivmedizinischen Notfall-Szenarios innerhalb von 7 Tagen an Ihrem Standort betreiben? | Gehen Sie von einem Notfall-Szenario aus, in welchem sämtliche intensivmedizinischen Reserven in Ihrem Standort innerhalb von 7 Tagen aktiviert werden müssen. Es sollen hier Intensivbetten angegeben werden, die aktuell inaktiv gehalten werden und nicht Gegenstand der täglichen Bettenplanung sind (z .B. zusätzlich geschaffene aber inaktive Kapazitäten, aufgestellte aber inaktive Räume/Stationen, weitere eingelagerte Bestände etc.), die aber innerhalb von 7 Tagen personell und strukturell betreibbar wären. Machen Sie diese Angabe unter der Annahme eines bei Ihnen möglichen, an die Situation angepassten Personalschlüssels. | Integer | kapazitaeten: intensivBettenNotfall7d |

## COVID-19 STATUS: BELEGUNG
### 6. Erfassung: Aktuell COVID-19-Patient\*innen in Behandlung
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q11:** Anzahl aller aktuell intensivmedizinisch behandelten COVID-19-Patient\*innen | Anzahl aller aktuell in intensivmedizinischer Behandlung befindlicher SARS-CoV-2-positiver Patient\*innen (nur bei labordiagnostischem Nukleinsäure- oder Antigennachweis; keine klinischen Verdachtsfälle). Inklusive Zählung von COVID-19-Patient\*innen mit zurückliegendem SARS-CoV-2-Nachweis, die weiterhin mit ihrer COVID-19-Erkrankung intensivmedizinisch behandelt werden. | Integer | faelleCovidAktuell |
| **Q11.1:** Anzahl der COVID-19-Patient\*innen mit intensivmedizinisch relevanter COVID-19-Manifestation | Dazu gehören alle intensivmedizinisch behandelten Patient\*innen, bei denen eine primäre Lungen- und/oder Systembeteiligung einer COVID-19-Erkrankung vorliegt oder deren klinischer Zustand sich durch COVID-19 verschlechtert hat. Hierzu zählen auch Patient\*innen, deren Hauptdiagnose und/oder Grunderkrankungen durch die COVID-19-Erkrankung beeinflusst werden oder vice versa. *Limitation:* Die Zuordnung der Patient\*innen in diese Gruppe obliegt der klinischen medizinischen Einschätzung. | Integer | covid19StatusV3: mitManifestation: faelleAktuell |
| **davon** Anzahl mit folgender Behandlung in den letzten 24 h: | - | *{Dies ist ein erklärender Text zu Folgefragen Q11.1.1 – Q11.1.4 im Frontend. Der erklärende Text selbst hat keine Antwortoption.}* | - |
| **Q11.1.3:** NIV (optional) | als höchste Stufe der respiratorischen Unterstützung in den letzten 24 h | Integer | covid19StatusV3: mitManifestation: faelleAktuellNichtInvasivBeatmet |
| **Q11.1.1:** invasive Beatmung | invasive Beatmung in den letzten 24 h; unabhängig davon, ob (auch) eine ECMO-Behandlung vorliegt | Integer | covid19StatusV3: mitManifestation: faelleAktuellBeatmet |
| **Q11.1.4:** ECMO (optional) | ECMO-Behandlung in den letzten 24 h; unabhängig davon, ob (auch) eine Beatmungsbehandlung vorliegt | Integer | covid19StatusV3: mitManifestation: faelleAktuellEcmo |
| **Q11.2:** Anzahl der COVID-19-Patient\*innen ohne intensivmedizinisch relevante COVID-19-Manifestation | Dazu gehören alle intensivmedizinisch behandelten Patient\*innen ohne Hinweis auf eine intensivmedizinisch relevante Manifestation der Infektion. Die Hauptdiagnose und/oder Grunderkrankungen sind durch die SARS-CoV-2-Infektion nicht beeinflusst. *Limitation:* Die Zuordnung der Patient\*innen in diese Gruppe obliegt der klinischen medizinischen Einschätzung. | Integer | covid19StatusV3: ohneManifestation: faelleAktuell |
| **davon** Anzahl mit folgender Behandlung in den letzten 24 h: | - | *{Dies ist ein erklärender Text zu Folgefragen Q11.2.1 im Frontend. Der erklärende Text selbst hat keine Antwortoption.}* | - |
| **Q11.2.1:** invasive Beatmung | invasive Beatmung in den letzten 24 h; unabhängig davon, ob (auch) eine ECMO-Behandlung vorliegt | Integer | covid19StatusV3: ohneManifestation: faelleAktuellBeatmet |

**Q11.1.2**, **Q.11.2.2 – Q11.2.4** sowie **Q12 – Q15.1** und **Q20 – Q22** wurden gestrichen.

### 7. Erfassung: Betriebs-Einschränkung (optional)
*i-Button: Persönliche Einschätzung der meldenden Person, inwieweit der Betrieb des gesamten Intensivbereichs eingeschränkt ist; Beurteilung im Vergleich zum Regelbetrieb (keine Pandemie/Epidemie, kein besonders hohes Patient\*innenaufkommen)*
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q27:** Betriebs-Einschränkung | Isoliermöglichkeiten, Pflegepersonal, Schutzausrüstung etc. werden vorausgesetzt. | "Nicht eingeschränkt, regulärer Betrieb möglich", "Teilweise eingeschränkt, regulärer Betrieb gerade noch möglich", "Eingeschränkte Behandlungskapazität (ausgelastet oder überlastet)", "Keine Angabe" | betriebssituation |

Bei voriger Betriebs-Einschränkung Auswahl = **"Teilweise eingeschränkt"** oder **"Eingeschränkte Behandlungskapazität"** wird zudem folgende Spezifikation angezeigt: 
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q27.1:** Personal | Persönliche Einschätzung der meldenden Person, dass die Betriebs-Einschränkung vornehmlich am Mangel von Personal liegt. | kann per Klick ausgewählt oder freigelassen werden | betriebseinschraenkungPersonal |
| **Q27.2:** Räume | Persönliche Einschätzung der meldenden Person, dass die Betriebs-Einschränkung vornehmlich am Mangel von Räumen liegt. | kann per Klick ausgewählt oder freigelassen werden | betriebseinschraenkungRaum |
| **Q27.3:** Beatmungsgeräte | Persönliche Einschätzung der meldenden Person, dass die Betriebs-Einschränkung vornehmlich am Mangel von Beatmungsgeräten liegt. | kann per Klick ausgewählt oder freigelassen werden | betriebseinschraenkungBeatmungsgeraet |
| **Q27.4:** Verbrauchsmaterial | Persönliche Einschätzung der meldenden Person, dass die Betriebs-Einschränkung vornehmlich am Mangel von Verbrauchsmaterial liegt. | kann per Klick ausgewählt oder freigelassen werden | betriebseinschraenkungVerbrauchsmaterial |

## COVID-19 STATUS: SPEZIFIKATION
**Q25.1** wurde gestrichen.

### 8. Erfassung: Aktuelle COVID-19-Patient*innen nach Alter (optional) [Erwachsenen-Meldebereiche]
*i-Button: Anzahl der aktuell in intensivmedizinischer Behandlung befindlichen COVID-19-Patient\*innen pro Altersgruppe. Nur nachgewiesene Infektionen mit SARS-CoV-2, KEINE Verdachtsfälle. Die Summe der Einträge entspricht der Angabe im Feld „Aktuell COVID-19 in Behandlung“.*  
*Achtung*: Diese Erfassung wird nur für Meldebereiche abgefragt, die als Behandlungsschwerpunkt „Erwachsene“ angegeben haben oder den Behandlungsschwerpunkt nicht angegeben haben („NA“).
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| Anzahl der aktuell intensivmedizinisch behandelten COVID-19-Patient\*innen pro Altersgruppe (in Jahren) | - | *{Dies ist ein erklärender Text zu Folgefragen Q25.2 – Q25.9 im Frontend. Der erklärende Text selbst hat keine Antwortoption.}* | - |
| **Q25.2:** 0-17 | - | Integer | altersstrata: stratum17minus |
| **Q25.3:** 18-29 | - | Integer | altersstrata: stratum18bis29 |
| **Q25.4:** 30-39 | - | Integer | altersstrata: stratum30bis39 |
| **Q25.5:** 40-49 | - | Integer | altersstrata: stratum40bis49 |
| **Q25.6:** 50-59 | - | Integer | altersstrata: stratum50bis59 |
| **Q25.7:** 60-69 | - | Integer | altersstrata: stratum60bis69 |
| **Q25.8:** 70-79 | - | Integer | altersstrata: stratum70bis79 |
| **Q25.9:** ≥80 | - | Integer | altersstrata: stratum80plus |

### 9. Erfassung: Aktuelle COVID-19-Patient\*innen nach Virusvarianten
*i-Button: Anzahl der aktuell in intensivmedizinischer Behandlung befindlichen COVID-19-Patient\*innen mit einer nachgewiesenen VOC. Die Summe der Einträge sollte der Angabe im Feld “Aktuell COVID-19 in Behandlung” entsprechen.*
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| Anzahl der aktuell intensivmedizinisch behandelten COVID-19-Patient\*innen mit SARS-CoV-2-Virusvarianten (Variants of concern (VOC)) (falls bekannt) | - | *{Dies ist ein erklärender Text zu Folgefragen Q26.1 – Q26.4 im Frontend. Der erklärende Text selbst hat keine Antwortoption.}* | - |
| **Q26.1:** B.1.617.2 (Delta) | - | Integer | varianten: delta |
| **Q26.2:** B.1.1.529 (Omikron) | - | Integer | varianten: omikron |
| **Q26.3:** Alle anderen SARS-CoV-2-Varianten | - | Integer | varianten: sonstige |
| **Q26.4:** Nicht bestimmt oder nicht bekannt | - | Integer | varianten: unbekannt |

### 10. Erfassung: Schwangere und frisch Entbundene mit COVID-19 [Erwachsenen-Meldebereiche]
*Achtung:* Diese Erfassung wird nur für Meldebereiche abgefragt, die als Behandlungsschwerpunkt „Erwachsene“ angegeben haben oder den Behandlungsschwerpunkt nicht angegeben haben („NA“).
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q17:** Anzahl aktuell schwangerer bzw. frisch entbundener COVID-19-Patient\*innen auf Ihrer ITS | Anzahl der aktuell schwangeren oder frisch entbundenen COVID-19-Patient\*innen (in allen Intensivbereichen: Low-Care, High-Care, ECMO) im Rahmen der Akutversorgung; nur nachgewiesene Infektionen mit SARS-CoV-2, KEINE Verdachtsfälle. | Integer | schwangereCovidStatus: anzahlSchwangere |

**Q16**, **Q18** und **Q19** wurden gestrichen.

## COVID-19-STATUS: NEUAUFNAHMEN UND ABGÄNGE

### 11. Erfassung: Neuaufnahmen von COVID-19-Patient\*innen
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q28:** ITS-Erstaufnahme: Anzahl der COVID-19-Patient\*innen, die innerhalb des gestrigen Kalendertages erstmals zur ITS-Behandlung aufgenommen wurden (keine Zählung von Neuaufnahmen aufgrund von ITS-zu-ITS Verlegungen). | Zählung von COVID-19-Patient\*innen, die erstmals auf die ITS verlegt werden von einer Nicht-Intensivstation (Notaufnahme, Normalstation, etc.) bzw. bei voriger ITS-Behandlung nicht im Intensivregister gemeldet wurden (keine Zählung wiederkehrender Patient\*innen). Nur nachgewiesene Infektionen mit SARS-CoV-2, KEINE Verdachtsfälle.	| Integer | neuaufnahmen: erstaufnahmen |
| <del>**Q29:** ITS-zu-ITS Verlegung: Anzahl der COVID-19-Patient\*innen, die innerhalb des gestrigen Kalendertages im Rahmen einer Verlegung von einem anderen ITS-Meldebereich aufgenommen wurden.</del> | <del>Zählung nur von COVID-19-Patient\*innen, die bereits zuvor in einem anderen ITS-Meldebereich behandelt wurden und im Rahmen einer Verlegung vom hier meldenden ITS-Meldebereich aufgenommen wurden. Nur nachgewiesene Infektionen mit SARS-CoV-2, KEINE Verdachtsfälle.</del>	| <del>Integer</del> | <del>neuaufnahmen: verlegungen</del> |

**Q29.0** wurde gestrichen.

### 12. Erfassung: SARS-CoV-2-Impfstatus der COVID-19-ITS-Erstaufnahmen
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| Anzahl der COVID-ITS-Erstaufnahmen (aufgenommen innerhalb des gestrigen Kalendertages) mit jeweiliger Anzahl Impfungen gegen SARS-CoV-2 | Erfassung einmalig bei ITS-Erstaufnahme! Es werden nur Impfungen mit einem in Deutschland zugelassenen Impfstoff gezählt, unabhängig vom Zeitpunkt der Impfung. Nur nachgewiesene Infektionen mit SARS-CoV-2, keine Verdachtsfälle. | *{Dies ist ein erklärender Text zu Folgefragen Q30.1 – Q30.6 im Frontend. Der erklärende Text selbst hat keine Antwortoption.}* | - |
| **Q30.1:** 0 | Anzahl COVID-19-Erstaufnahmen ohne Impfung | Integer | impfstatusV2: impfstatus0Impfungen |
| **Q30.2:** 1 | Anzahl COVID-19-Erstaufnahmen mit 1 Impfung | Integer | impfstatusV2: impfstatus1Impfungen |
| **Q30.3:** 2 | Anzahl COVID-19-Erstaufnahmen mit 2 Impfungen | Integer | impfstatusV2: impfstatus2Impfungen |
| **Q30.4:** 3 | Anzahl COVID-19-Erstaufnahmen mit 3 Impfungen | Integer | impfstatusV2: impfstatus3Impfungen |
| **Q30.5:** 4+ | Anzahl COVID-19-Erstaufnahmen mit 4 oder mehr Impfungen | Integer | impfstatusV2: impfstatus4PlusImpfungen |
| **Q30.6:** Unbekannt | Anzahl der COVID-19-Erstaufnahmen mit UNBEKANNTEM SARS-CoV-2-Impfstatus | Integer | impfstatusV2: impfstatusUnbekannt |

*Hinweis an die Meldenden:* Nach dem Speichern dieser Meldung finden Sie einen Button, um bekannt gewordene Impfstatus früherer Meldungen nachzutragen.

### 13. Erfassung: Anzahl verstorbener und verlegter/entlassener COVID-19-Intensivpatient\*innen
**Q23** und **Q24** wurden durch **Q51** und **Q52** ersetzt.
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q51:** Anzahl der innerhalb des gestrigen Kalendertages verstorbenen COVID-19-Intensivpatient\*innen in Ihrem Meldebereich | Anzahl der verstorbenen COVID-19-Patient\*innen in Ihrem Meldebereich innerhalb des gestrigen Kalendertages. Hierzu zählen alle verstorbenen Intensivpatient*innen, bei denen im Verlauf der intensivmedizinischen Behandlung eine SARS-CoV-2-Infektion nachgewiesen wurde (keine Verdachtsfälle) oder die aufgrund einer COVID-19-Erkrankung intensivmedizinisch behandelt wurden. | Integer | faelleCovidVortagVerlegtVerstorben: faelleCovidVortagVerstorben |
| **Q52:** Anzahl der innerhalb des gestrigen Kalendertages von Ihrem Meldebereich wegverlegten oder entlassenen COVID-19-Intensivpatient\*innen | Anzahl der innerhalb des gestrigen Kalendertages (lebend) von Ihrem Meldebereich entlassenen oder verlegten COVID-19-Intensivpatient\*innen. Hierzu zählen alle entlassenen oder verlegten Intensivpatient*innen, bei denen im Verlauf der intensivmedizinischen Behandlun eine SARS-CoV-2-Infektion nachgewiesen wurde (keine Verdachtsfälle) oder die aufgrund einer COVID-19-Erkrankung intensivmedizinisch behandelt wurden. | Integer | faelleCovidVortagVerlegtVerstorben: faelleCovidVortagVerlegt |
	
## RSV- UND INFLUENZA-STATUS [Kinder-Meldebereiche]
### 14. Erfassung: RSV-Status [Kinder-Meldebereiche]
*Achtung:* Diese Erfassung wird nur für Meldebereiche abgefragt, die als Behandlungsschwerpunkt „Kinder“ angegeben haben.
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q41<sup>Kinder</sup>:** Anzahl aller aktuell intensivmedizinisch behandelten RSV-Patient\*innen | Anzahl aller aktuell in intensivmedizinischer Behandlung (beatmet und nicht beatmet) befindlicher RSV-Patient\*innen (in allen Intensivbereichen: Low-Care, High-Care, ECMO); nur nachgewiesene Infektionen mit RSV, KEINE Verdachtsfälle. | Integer | rsvStatus: faelleAktuell |

**Q42**, **Q43**, **Q44** und **Q45** wurden gestrichen.

### 15. Erfassung: Influenza-Status [Kinder-Meldebereiche]
*Achtung:* Diese Erfassung wird nur für Meldebereiche abgefragt, die als Behandlungsschwerpunkt „Kinder“ angegeben haben.
| Textlabel des Datenfeldes | Mouseover-Text | Antwort-Möglichkeiten | Variablen-Name |
| :- | :- | :- | :- |
| **Q46<sup>Kinder</sup>:** Anzahl aller aktuell intensivmedizinisch behandelten Influenza-Patient\*innen | Anzahl aller aktuell in intensivmedizinischer Behandlung (beatmet und nicht beatmet) befindlicher Influenza-Patient\*innen (in allen Intensivbereichen: Low-Care, High-Care, ECMO); nur nachgewiesene Infektionen mit Influenza, KEINE Verdachtsfälle. | Integer | influenzaStatus: faelleAktuell |

**Q47**, **Q48**, **Q49** und **Q50** wurden gestrichen.
