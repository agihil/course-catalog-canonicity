# course-catalog-canonicity

The repository contains a dataset that describes which writers appear how frequently in course descriptions of (modern) German studies courses in a randomly 
selected but non-representative sample of six German, two Austrian universities and one Swiss university.

The idea behind this is that mentions of writers in course descriptions of literary studies programs are an indicator for what is taught in these courses and that again is an indicator of the academic canonicity of the respective writer in the time the course is held. 

The subject "German studies" is usually divided in three to four parts: Literary studies for the medieval period, Literary studies for the modern period, Linguistics and, if it is a program for teachers-to-be Didactics. For this project, the focus was on the modern period (since this is my field of study). 

In the following you find 1. a description of the data, code and results in this repository and 2. a more detailed documentation on how the data was obtained and which data was used.

# Description
## Data
### data/courses.csv
Die Tabelle enthält:
- eine Spalte mit der Kurs-ID (von mir vergeben, setzt sich zusammen aus stadt und Nummer)
- eine Spalte mit dem Titel des jeweiligen Kurses
- eine Spalte mit der Information ob eine zugehörige Inhaltsbeschreibung extrahiert wurde (1) oder nicht (0). Die Inhaltsbeschreibung selbst wird hier nicht veröffentlicht, damit keine Rechte verletzt werden.
- eine Spalte mit dem Semester
- eine Spalte mit dem Start des Semesters als Zeitdatum
- eine Spalte mit der Universität (welche Unis das jeweils sind findet sich in der Tabelle unis)
- eine Spalte mit dem Land
- eine Spalte mit den GND-IDs zu den Entities die mit dem unten beschriebenen Verfahren in der Kursbeschreibung identifiziert wurden.
- counts...

### data/writers.csv
Each row represents a writer. The columns represent:
- the GND-ID
- the occupation according to the GND
- the date of birth accoring to the GND
- the date of death according to the GND
- the gender according to the GND/as identified by the gender guesser (see section on further cleaning below)
- the associated countries according to the GND
- a normalised form of the countries, where only the association with Germany (D), Austria (Ö) or Switzerland (S) is stored. (In the case of Switzerland there seems to be a mix up in the GND so that "Europa" is the entry for "Switzerland", this was corrected in the normalised column.)
- counts... 

### data/universities.csv
The table contains short names and the real names of the universities sampled.

# Detailed Documentation

## Selection Process

### Lists of German Studies Programs in Germany, Austria and Switzerland.
The population surveyed in this project are German studies courses in Germany, Austria and Switzerland. Therefore, the first step was to compile lists of German studies courses in these countries. 
The initial aim was to select degree programs, not universities. Therefore, the information collected is more detailed than actually used in the later analysis.

#### Germany
For Germany, the website of the "Agentur für Arbeit" (https://web.arbeitsagentur.de/studiensuche/) was used and the keywords "Germanistik" and "Deutsch" were searched for using the search function. The resulting lists were copied into a table. 
Degree programs with a focus on the Middle Ages, linguistics, interculturality, translation, German as a second or foreign language, degree programs in which German Studies was only a minor subject, as well as degree programs that clearly did not deal with literary studies but only with the German language were removed. For the remaining study programs, the following was recorded (partly automatically, partly manually):
the city, the federal state, the degree, whether it was a teachers' training program and, if so, for which type of school. Subsequently, for the sake of simplicity
all teacher training programs except for the "Gymnasium" school type were removed.
126 study programs were obtained that way.

#### Austria
The starting point was the website www.studienwahl.at. There, the keyword "Germanistik" was searched for and, as with the example of Germany, some degree courses were removed and additional information recorded.
15 study programs were obtained that way.

#### Switzerland
The starting point for Switzerland was the website https://www.berufsberatung.ch/dyn/show/17500, which already offers degree programs in the subject "German Linguistics and Literature". 
are already listed. As with Germany and Austria, these degree programs were adjusted and enriched with additional data.
12 study programs were obtained that way.

### Selection
Zunächst war die Idee, Studiengänge auszuwählen. Als Parameter sollten die Art des Abschlusses dienen und ob es sich um einen Lehramts-Studiengang handelte oder nicht. Für Deutschland wurde außerdem berücksichtigt, ob es sich um ein ehemals ostdeutsches oder westdeutsches Bundesland handelte. 
Um ein Vorlesungsverzeichnis zu scrapen muss man sich mit dessen Struktur vertraut machen. Da das etwas Zeit kostet wurde schnell klar, dass es sinnvoller wäre, alle Veranstaltungen einer Uni im Bereich NDL zu scrapen, wenn man sich schon mit der Struktur vertraut gemacht hat, anstatt den jeweiligen Studiengang als zusätzlich einschränkenden Parameter zu behalten.
Deshalb wurden die Listen von Studiengängen reduziert auf Listen von Universitäten. Für Deutschland wurden drei Universitäten aus den ehemals ostdeutschen Bundesländern, drei aus den ehemals westdeutschen Bundesländern ausgewählt (Berlin wurde beiden Gruppen zugerechnet). Wurde eine Uni eines Bundeslandes ausgewählt, wurden die anderen Unis dieses Bundeslands aus der Liste entfernt, damit kein Bundesland mit zwei Unis vertreten wäre. Für Österreich wurden zwei, für die Schweiz ein Bundesland ausgewählt.
Die Zufallsauswahl wurde für einzelne Fälle wiederholt, wenn es kein Online-Vorlesungsverzeichnis gab, das den Kriterien entsprach (s. u.: Scraping).

## Scraping
Die per Zufallsauswahl gewählten Universitäten wurden anschließend daraufhin überprüft: ob sie ein online zugängliches Vorlesungsverzeichnis haben, ob dieses mindestens 10 Semester zurücking und ob es sich hinsichtlich der hier relevanten Kriterien (Einschränkung auf Modern German Literature) scrapen lässt. Fälle, in denen diese Kriterien nicht erfüllt waren, wurden als Nonresponse betrachtet und eine Zufallsauswahl (unter Rücknahme aller Unis des jew. Bundeslands) wiederholt. Was bedeutet: Ein Vorlesungsverzeichnis lässt sich nicht scrapen? In manchen Fällen waren etwa die Suchfunktionen im Vorlesungsverzeichnis eingeschränkt, Namen von Studiengängen und Modulordnungen haben zu oft gewechselt, sodass sich für mich als Außenstehende ohne sehr aufwendige Detailanalyse nicht nachvollziehen ließ, wie ich die nötigen Informationen automatisiert abgreifen kann.
In jedem Fall involvierte das Scrapen, sich damit vertraut zu machen, wie das Online-Vorlesungsverzeichnis strukturiert ist, wo die Veranstaltungen aus dem Bereich NDL liegen oder wie sie benannt sind. Der für das Scraping verwendete Code war damit für die unterschiedlichen Unis sehr individuell und sicherlich ist das Datenset nicht vollständig korrekt.
Figure 1 im Unterordner results zeigt die Menge der erzielten Daten.

## Manual annotation
Six course descriptions were randomly chosen from each university for manual annotation. References to Persons (NEs) and, as a subset, to writers were annotated. Of the 179 NEs identified, about 53%, belong to writers. This led to the insight that Named Entitiy Recognition (NER) alone is not sufficient to identify writers.

## NER
For NER, five models were evaluated based on the manually annotated data (see table 1 in results folder). Subsequently, the best model was used to annotate named entities in all content descriptions. Nach der NER wurden die NEs mit Spacy lemmatisiert. Single word NEs were automatically matched with multiword NEs if possible. A total of 7689 NEs were identified that way. 

## Entity Linking
The program OpenRefine was used for Entity Linking. The entities were linked automatically to the GND data of the German national library. Für die Entitäten, bei denen die automatische Zuordnung nicht funktioniert hat (etwa wegen Fehlschreibungen, Abkürzungen oder mehreren Entitäten des gleichen Namens) wurde manuelles Linking versucht. Wegen beschränkter Ressourcen wurde aber **keine detaillierte Recherche zu den Entitäten durchgeführt, sondern heuristisch vorgegangen**: Wenn es einen Schriftsteller des jeweiligen Namens gab, wurde es als wahrscheinlich angesehen, dass dieser gemeint ist und nicht beispielsweise ein Arzt gleichen Namens. Auch meine Kenntnis über Autoren, Literaturwissenschaftler und andere Personen, die wahrscheinlich in Kursbeschreibungen genannt werden, floss in diese Arbeit ein. Qualitative Eindrücke bei dieser Arbeit waren 1. dass die Namen insgesamt häufig falsch geschrieben waren (vermutlich weil Kursbeschreibungen schneller und weniger sorgfältig geschrieben werden als beispielsweise Publikationen) und 2. das v.a. Namen nicht-deutschen Ursprungs häufig falsch geschrieben wurden und deshalb nicht zugeordnet wurden. Das ist wirklich ein subjektiver Eindruck, trotzdem wurde bei solchen Namen zum Teil eine zusätzliche Google-Suche hinzugenommen, um einen eventuellen Bias auszugleichen.
Insgesamt wurden auf diese Weise 4410 Entitäten ausgemacht. (Das bedeutet im Umkehrschluss nicht, dass 3279 Entitäten nicht zugeordnet werden konnte, denn aufgrund variablen und falschen Schreibungen der Namen kamen einige Entitäten vorher mehrfach vor).
Für die gematchten Identitäten wurden aus der Gemeinsamen Normdatei jeweils ein standardisierter Name, das Geschlecht, der Beruf und der Ländercode abgerufen.

## Further Cleaning and Processing
Für 704 der identifizierten Entitäten ist in der GND kein Geschlecht hinterlegt. Für diese wurde mithilfe des Python Gender Guessers ein Geschlecht anhand der Vornamen geraten. Auf diese Weise blieben 203 Entitäten übrig, für die kein Geschlecht in den Metadaten vermerkt ist. Im Fall eines Transmannes wurde das in der GND angegebene Geschlecht von "Weiblich; Männlich" zu "Männlich" korrigiert. 
Die GND verwendet bei den Berufsbezeichnungen gegenderte Formen (zum Beispiel: "Schriftsteller" und "Schriftstellerin"). Diese Bezeichnungen wurden für die quantitative Auswertung auf die männliche Form reduziert, das Geschlecht ist ja jewils in einer separaten Spalte gespeichert.
Anschließend wurden alle Entitäten extrahiert, bei denen es sich um Schriftsteller handelt. Die GND führt hierfür verschiedene Namen (Schrifsteller, Dramatiker, Lyriker, Erzähler).

# Counting
Schließlich wurden die GND-IDs mit den Seminardaten gematcht und folgende Zählungen erhoben:




# Resources

https://web.arbeitsagentur.de/studiensuche/
www.studienwahl.at
https://www.berufsberatung.ch/dyn/show/17500
https://openrefine.org
https://www.dnb.de/DE/Professionell/Standardisierung/GND/gnd_node.html
https://pypi.org/project/gender-guesser/

