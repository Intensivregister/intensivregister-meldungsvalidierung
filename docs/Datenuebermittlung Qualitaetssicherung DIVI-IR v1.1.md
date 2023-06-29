# Beschreibung des Prozesses zur Qualitätssicherung

*Automatische Datenübermittlung aus einem Fremdsystem für das
DIVI-Intensivregister*

*Schriftführung: RKI/BMG*

**§1 Verantwortung**

Die Prüfung wird durch das Intensivregister-Team am Robert Koch-Institut
durchgeführt.

**§2 Gegenstand der Prüfung**

1. Ein Prüfvorgang umfasst ausschließlich eine durch einen
Software-Hersteller bereitgestellte Erfassungs- und
Übermittlungstechnologie.

2. Ein Prüfvorgang umfasst nur eine Instanz/ein Deployment; z. B. ein
KIS in einem einzelnen Krankenhausstandort; z. B. ein von mehreren
Krankenhausstandorten genutztes, übergeordnetes System, welches in einer
(physischen) Server-Konfiguration produktiv installiert und angepasst
wurde.

3. Die Prüfung verwandter Systeme (z. B. weiteres Deployment eines
KIS in einem anderen Krankenhausstandort oder Bundesland) kann im
Ermessen der prüfenden Einrichtung in Vollform durchgeführt oder
abgekürzt werden.

**§3 Auskunfts- und Erreichbarkeitspflicht des Software-Herstellers**

\(1\) Der Software-Hersteller ist verpflichtet, vor Beginn der Prüfung
folgende Angaben der prüfenden Einrichtung bereitzustellen:

a\) Kurze System-/Softwarebeschreibung (wofür wird sie eingesetzt, wer
nutzt sie, wie können die Datenfelder des DIVI-Intensivregisters über
die Software eingebunden werden)

b\) Ansprechpartner\*innen für die technische Entwicklung

c\) Beschreibung der intern verwendeten Datenelemente (nur der Bereich,
der auch übermittelt werden soll)

\(2\) Der Software-Hersteller verpflichtet sich, Änderungen der
Erreichbarkeit unverzüglich der prüfenden Einrichtung zu melden

**§4 Zugang und Nutzen einer Testumgebung**

Der Software-Hersteller verpflichtet sich, der prüfenden Einrichtung
Zugang zu der Software (z. B. Testsystem) zu ermöglichen. Dabei muss die
Möglichkeit der Datenübertragung an das DIVI-Intensivregister zu
Testzwecken gewährleistet sein. Bevor eine Doppeleingabe begonnen werden
kann, führt das Intensivregister-Team die Testdurchläufe durch um die
folgenden Funktionen des Testsystems zu gewährleisten:

\(1\) Die Textangaben des Testsystems stimmen mit den aktuellen Vorgaben
des Intensivregister-Teams überein.

\(2\) Das Testsystem gewährt die aktuelle Client-seitige
Qualitätssicherung.

\(3\) Die vom Testsystem erfasste Meldung wird in korrekter Vollform an
das Intensivregister-Backend gesendet.

**§5 Doppeleingabe**

\(1\) Jede Neuprüfung umfasst eine zweiwöchige prospektive Doppeleingabe
über das System des Software-Herstellers und über das
DIVI-Intensivregister-Frontend. Diese Doppeleingabe muss vom
Klinikpersonal (resp. Meldeverantwortliche) durchgeführt werden.

\(2\) Jede Neuprüfung umfasst die manuelle Dateneingabe und
Systemprüfung durch die prüfende Einrichtung.

\(3\) Die prüfende Einrichtung kann bei noch ungeklärten Fragen zur
Datenqualität die Punkte (1) und (2) wiederholen lassen (auch mehrfach).
Es obliegt der prüfenden Einrichtung auch die Punkte (1) und (2)
gegebenenfalls umzugestalten oder zu kürzen.

**§6 Änderung der Meldeinhalte**

\(1\) Das BMG in Zusammenarbeit mit dem RKI behält sich eine mögliche
Änderung/Anpassung der zu meldenden Inhalte und deren
Operationalisierung vor.

\(2\) Über eine Änderung wird der Ansprechpartner des
Software-Herstellers mit einer Frist von sieben Tagen und der Bitte um
Anpassung informiert. Nach Ablauf der sieben Tage werden bis zum
Abschluss der erneut durchzuführenden Prüfung (hier: nur
Anpassungsprüfung) die automatisch übermittelten Daten nicht in das
DIVI-Intensivregister übernommen. Entsprechend muss in der
Übergangsphase mit noch nicht angepasster Software auf die manuelle
Eingabe über das DIVI-Intensivregister-Frontend zurückgegriffen werden.

**§7 Sicherstellung**

\(1\) Der Software-Hersteller stellt gegenüber der prüfenden Einrichtung
sicher, dass die für das Intensivregister maßgeblichen methodischen
Rahmenbedingungen auf der Seite des Krankenhausstandortes bekannt sind
und eingehalten werden. Dazu legt der Software-Hersteller der prüfenden
Einrichtung schriftliche Bestätigungen über die Unterrichtung aller
Meldebereiche eines Krankenhauses vor, die Daten an das
DIVI-Intensivregister automatisiert übermitteln wollen.

\(2\) Die Bestätigungen enthalten Angaben

a\) zur für den Meldebereich verantwortlichen Person, inkl. Angaben zur
Kontaktaufnahme (z. B. Funktionsmailadresse)

b\) zu einer Bestätigung, dass die Daten mindestens täglich aktualisiert
werden

c\) dass die für den Meldebereich verantwortliche Person die Inhalte der
Verordnung zur Krankenhauskapazitätssurveillance §2zur Kenntnis genommen
hat und sich selbstständig zu Änderungsverordnungen informiert

d\) dass die für den Meldebereich verantwortliche Person bei Änderungen
der Strukturdaten des Meldebereichs umgehend Anpassungen über das
Frontend des DIVI-Intensivregisters vornimmt.

**§8 Client-seitige Qualitätssicherung**

Das Frontend des DIVI-Intensivregisters überprüft Client-seitig
bestimmte Aspekte der Datenqualität. Die prüfende Einrichtung stellt dem
Software-Hersteller auf Anfrage die aktuelle Form der Client-seitigen
Qualitätssicherung bereit.

\(1\) Der Software-Hersteller sichert in einer eigenen Systemstruktur
die zuvor gemachten Angaben und zeigt diese in der Eingabeumgebung bei
erneuter Eingabe/Aktualisierung an. Entsprechend genauerer
Spezifizierungen, werden prozentuale Veränderung einer Variablen im
Vergleich zur vorherigen Übermittlung vor dem Speichern der Meldung
angezeigt.

\(2\) der Software-Hersteller implementiert Client-seitig die durch das
DIVI-Intensivregister vorgegebene Werteeinschränkung und statischen
Prüfregeln.

Es obliegt der prüfenden Einrichtung, konkrete Abweichungen von den
Client-seitigen Prüfregeln auf Seiten des Software-Herstellers
abzulehnen oder zu genehmigen.

**§9 Meldebereiche**

\(1\) Die übermittelten Daten werden im DIVI-Intensivregister nur
verwendet, wenn sie eindeutig einem aktuell aktiv meldenden und gültigen
Meldebereich zugeordnet werden können.

\(2\) Intensivbereiche mit Kinder- oder Neugeborenen-Intensivbetten
müssen diese als getrennte Meldebereiche übermitteln.

**§10 Freischaltung**

\(1\) Die Freischaltung für den Empfang von automatisiert übermittelten
Daten im DIVI-Intensivregister wird erstmals nach vollständiger Prüfung
durch die prüfende Einrichtung vorgenommen. Bis dahin müssen die
Meldebereiche über das Frontend des DIVI-Intensivregisters melden.

\(2\) Automatisiert übermittelte Daten erscheinen bis zur Freischaltung
nicht als Meldenachweis in den Krankenhausstandort-spezifischen
Meldeübersichten und den Meldeübersichten für die obersten
Landesbehörden.

\(3\) Der prüfenden Einrichtung ist es bei Verdacht auf Probleme mit der
Datenqualität vorbehalten jederzeit die Freischaltung innerhalb von 24h
zurückzuziehen. Dabei wird ausschließlich die Kontaktstelle des
Software-Herstellers kontaktiert.
