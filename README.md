# course-catalog-canonicity

The data and code in this repository is the basis for the paper "On reconstructions of academic canonicity", published in the context of DH24, see: [link].

The repository contains a dataset that describes which writers appear how frequently in course descriptions of (modern) German studies courses in a randomly 
selected but non-representative sample of six German, two Austrian universities and one Swiss university.

The idea behind this is that mentions of writers in course descriptions of literary studies programs are an indicator for what is taught in these courses and that again is an indicator of the academic canonicity of the respective writer in the time the course is held. 

The subject "German studies" is usually divided in three to four parts: Literary studies for the medieval period (ÄDL), Literary studies for the modern period (NDL), Linguistics and, if it is a program for teachers-to-be Didactics. For this project, the focus was on the modern period. 

In the following you find 1. a description of the data in this repository and 2. a more detailed documentation on how the data was obtained.

# Description
## Data
### data/lvs.csv
Each row represents a university course. The columns represent:
- an ID (assigned by me, composed of the city of the university and a number) (ID)
- the title of the course (Titel)
- the description of the course (Inhalt)
- the information whether an associated description has been extracted (1) or not (0). (inhalt_count)
- the semester in which the course was held (semester)
- the semester begin as a timestamp (09+year for winter term; 04+year for summer term) (Intervall-Start)
- the semester end as a timestamp (09+year for winter term; 04+year for summer term) (Intervall-End)
- a short name of the university where the course was held (university)
- the country of the university (Land)
- the identified Named Entities in the course title (NER_Pers_Titel)
- the identified Named Entities in the course description (NER_Pers_Inhalt)
- the identified Named Entities in the course title in a lemmatised form (NER_Pers_Titel_lemm)
- the identified Named Entities in the course description in a lemmatised form (NER_Pers_Inhalt_lemm)
- the IDs from the Gemeinsame Normdatei (GND) for the identified people in the titles (only for those Entities who could be identified. (For a more detailed description see below.) (NER_GNDs_Titel)
- the IDs from the Gemeinsame Normdatei (GND) for the identified people in the descriptions (only for those Entities who could be identified. (For a more detailed description see below.) (NER_GNDs_Inhalt)

### data/authors.csv
Each row represents a writer identified in the course titles or descriptions. Writers here are people, where "Schriftsteller", "Lyriker", "Dramatiker" or "Erzähler" is stored as an occupation in the GND. The columns reprsent metadata about these people from the GND as well as counts of there mentions in the courses:
- the GND ID of the identified person (GND-Nummer)
- the persons`s preferred name (Bevorzugter Name)
- the person`s country/countries of residence (Ländercode)
- a short form of the person`s country/countries of residence, only considering Germany, Austria and Switzerland (Land)
- the persons`s occupation(s) (Beruf oder Beschäftigung)
- the person`s date of birth (Geburtsdatum)
- the person`s sex (Geschlecht)

## data/people.csv
Each row represents a person identified in the course titles or descriptions. The columns reprsent metadata about these people from the GND:
- the GND ID of the identified person (GND-Nummer)
- the persons`s preferred name (Bevorzugter Name)
- the person`s country/countries of residence (Ländercode)
- a short form of the person`s country/countries of residence, only considering Germany, Austria and Switzerland (Land)
- the persons`s occupation(s) (Beruf oder Beschäftigung)
- the person`s date of birth (Geburtsdatum)
- the person`s sex (Geschlecht)

## data/hein_1990.csv
The table contains the data collected in a study by Jürgen Hein in 1990. Hein collected data on courses from several German universities and counted per writer the number of courses dealing with them for the time between the summer semester 1970 until winter semester 1986/87. In the table each row represents a writer. The column represent:
- the name of the writer as named by Hein (Hein_Benennung)
- the GND-ID of the writer as identified by me (GND-Nummer)
- the count of courses as listet by Hein (Gesamt_count) \[The numbers in square brackets stand for the number of courses after 1980.\]
- the literary period as listet by Hein (Epoche_Hein)

## data/universities.csv
The table contains short names and the real names of the universities sampled.

## Notebooks
There are three notebooks in which figures describing the data are produced and analyses on the data are carried out.
The Notebook notebooks/analyse+vis_1_summarize_data.ipynb produces figures to explore and visualize the lvs dataset.
The Notebook notebooks/analyse+vis_2_people.ipynb produces figures to explore and visualize the people dataset.
The Notebook notebooks/analyse+vis_2_people.ipynb brings the two datasets (lvs and people) together and analyses how often which writers appear in the courses.


## Selection Process

### Lists of German Studies Programs in Germany, Austria and Switzerland.
The population surveyed in this project are German studies courses in Germany, Austria and Switzerland. Therefore, the first step was to compile lists of German studies courses in these countries. 

#### Germany
For Germany, the website of the "Agentur für Arbeit" (https://web.arbeitsagentur.de/studiensuche/) was used and the keywords "Germanistik" and "Deutsch" were searched for, using the search function. The resulting lists were copied into a table. 
Degree programs with a focus on the Middle Ages, linguistics, interculturality, translation, German as a second or foreign language, degree programs in which German Studies was only a minor subject, as well as degree programs that clearly did not deal with literary studies but only with the German language were removed. For the remaining study programs, the following was recorded (partly automatically, partly manually):
the city, the federal state, the degree, whether it was a teachers' training program and, if so, for which type of school. Subsequently, for the sake of simplicity all teacher training programs except for the "Gymnasium" (high school) type were removed.
126 study programs were obtained that way.

#### Austria
The starting point was the website www.studienwahl.at. There, the keyword "Germanistik" was searched for, and, as with the example of Germany, some degree courses were removed and additional information recorded.
15 study programs were obtained that way.

#### Switzerland
The starting point for Switzerland was the website https://www.berufsberatung.ch/dyn/show/17500, which already offers degree programs in the subject "German Linguistics and Literature" are already listed. As with Germany and Austria, these degree programs were adjusted and enriched with additional data.
12 study programs were obtained that way.

### Selection
The initial idea was to select degree programs. The parameters used were the type of degree and whether or not it was a teacher training course. For Germany, the federal states were also taken into account.
In order to scrape a course catalog, one has to familiarize with its structure. Since this takes some time, it quickly became clear that it would make more sense to scrape all courses of a university in the field of NDL (modern German literature), instead of keeping the respective course of study as an additional limiting parameter.
Therefore, the lists of study programs were reduced to lists of universities. For Germany, three universities were selected from the former East German states and three from the former West German states (Berlin was included in both groups). If a university from a federal state was selected, the other universities from this federal state were removed from the list so that no federal state would be represented by two universities. Two universities were selected for Austria and one for Switzerland.
The random selection was repeated for individual cases if there was no online course catalog that met the criteria (see below: Scraping).

## Scraping
The randomly selected universities were then checked to see whether they had an online course catalog, whether this went back at least 10 semesters and whether it could be scraped with regard to the criteria relevant here (specification that it is an NDL course). Cases in which these criteria were not met were considered non-responses and a random selection was repeated. 
What does this mean: A course catalog cannot be scraped? In some cases, for example, the search functions in the course catalog were limited, the names of degree programs and module regulations changed too often, so that it was not possible for me as an outsider to understand how I could automatically access the necessary information without very extensive detailed analysis.
In any case, the scraping involved familiarizing myself with how the online course catalog is structured, where the NDL courses are located or how they are named. The code used for scraping was therefore very individual for the different universities and the data set is certainly not completely correct since in some cases relevant courses may not have been found. (Since I cleaned the dataset afterwards for example from courses of the medieval field or from Linguistics it is to be expected that the precision is high, but the recall may be not).

## Scraped courses data
A total of 7185 events were scraped in this way, 6127 of which also had course descriptions scraped. The course descriptions contain a total of 868125 tokens and have an average length of 141,7 tokens.
The Figures in the subfolder "datasummaries" show the quantitites of descriptions and titles obtained per semester and uni as well as the distributions of text lengths for the titles and descriptions. These figures are plotted using the notebook analyse+vis_1_summarize_data.ipynb.

## Manual annotation
Six course descriptions were randomly chosen from each university for manual annotation. References to Persons (NEs) and, as a subset, to writers were annotated. Of the 179 NEs identified, about 53%, belonged to writers. This led to the insight that Named Entitiy Recognition (NER) alone is not sufficient to identify writers and additional Entitiy Linking was necessary.

## NER
For NER, five models were evaluated based on the manually annotated data (see table 1 in the paper). Subsequently, the best model was used to annotate named entities in all content descriptions. After NER the NEs were lemmatised using spacy. Single word NEs were automatically matched with multiword NEs if possible. A total of 7689 NEs were identified that way. 

## Entity Linking
The program OpenRefine was used for Entity Linking. The entities were linked automatically to the GND data of the German national library. Manual linking was attempted for entities for which the automatic assignment did not work (e.g. due to misspellings, abbreviations or several entities with the same name). Due to limited resources, however, **no detailed research was carried out on the entities, but a heuristic approach was taken**: If there was a writer of the particular name, it was considered likely that this was meant and not, for example, a doctor of the same name. My knowledge of authors, literary scholars and other people who are likely to be mentioned in course descriptions was also incorporated into this work. Qualitative impressions of this work were 1. that the names were often misspelled overall (presumably because course descriptions are written more quickly and less carefully than publications, for example) and 2. that names of non-German origin in particular were often misspelled and therefore not assigned. This is really a subjective impression, but an additional Google search was sometimes carried out for such names in order to compensate for any bias.
A total of 4410 entities were identified in this way. (This does not mean, by implication, that 3279 entities could not be matched, because due to variable and incorrect spellings of the names, some entities previously appeared more than once).
For each of the matched identities, a standardized name, gender, occupation and country code were retrieved from the Gemeinsame Normdatei.

## Further Cleaning and Processing
The GND uses gendered forms for job titles (for example: "Schriftsteller" and "Schriftstellerin"). These terms were reduced to the masculine form to faciliate quantitative analysis, since the gender is stored in a separate column in each case anyway.
In one case of a trans man, the gender given in the GND was corrected from "Female; Male" to "Male". 
(In a previous version of this dataset, the gender of those people, where the GND recorded the sex as "Unbekannt" was guessed using the Python Gender Guesser. This step was reversed in the current version.)


# Resources

https://web.arbeitsagentur.de/studiensuche/
www.studienwahl.at
https://www.berufsberatung.ch/dyn/show/17500
https://openrefine.org
https://www.dnb.de/DE/Professionell/Standardisierung/GND/gnd_node.html
https://pypi.org/project/gender-guesser/

