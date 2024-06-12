# course-catalog-canonicity

The data and code in this repository is the basis for the paper "On reconstructions of academic canonicity", published in the context of DH24, see: [link].

The repository contains a dataset that describes which writers appear how frequently in course descriptions of (modern) German studies courses in a randomly 
selected but non-representative sample of six German, two Austrian universities and one Swiss university.

The idea behind this is that mentions of writers in course descriptions of literary studies programs are an indicator for what is taught in these courses and that again is an indicator of the academic canonicity of the respective writer in the time the course is held. 

The subject "German studies" is usually divided in three to four parts: Literary studies for the medieval period (ÄDL), Literary studies for the modern period (NDL), Linguistics and, if it is a program for teachers-to-be Didactics. For this project, the focus was on the modern period. 

In the following you find 1. a description of the data, code and results in this repository and 2. a more detailed documentation on how the data was obtained.

# Description
## Data
### data/courses.csv
Each row represents a university course. The columns represent:
- an ID (assigned by me, composed of city and number) (ID)
- the title of the course (title)
- the information whether an associated description has been extracted (1) or not (0). The content description itself is not published here so that no rights are violated. (inhalt_count)
- the semester in which the course is held (semester)
- the semester as a timestamp (09+year for winter term; 04+year for summer term) (semester_start)
- the short name of the university (university)
- the respective country (country)
- the GND-IDs belonging to the named entitites identified in the course description. (For a more detailed description look below.) (NER_GNDs)

### data/writers.csv
Each row represents a writer. The columns represent:
- the GND-ID (GND-ID)
- the name according to the GND (Name_identifiziert)
- the occupation according to the GND (occupation_GND)
- the date of birth accoring to the GND (birth_GND)
- the date of death according to the GND (death_GND)
- the gender according to the GND/as identified by the gender guesser (see section on further cleaning below) (gender)
- the associated countries according to the GND (country_GND)
- a normalised form of the countries, where only the association with Germany (D), Austria (Ö) or Switzerland (S) is stored. (In the case of Switzerland there seems to be a mix up in the GND so that "Europa" is the entry for "Switzerland", this was corrected in the normalised column.) (country_norm)

### data/persons.csv
Each row represents a person. The columns represent:
- the GND-ID (GND-ID)
- the occupation according to the GND (occupation_GND)
- the date of birth accoring to the GND (birth_GND)
- the date of death according to the GND (death_GND)
- the gender according to the GND (gender_GND)
- the associated countries according to the GND (country_GND)

### data/writer_counts.csv
The table is obtained from the tables data/writers.csv and data/courses.csv using the notebook writers_get_counts.ipynb.
Each row represents a writer, indexed by the GND-ID.
The columns represent:
- the total number of mentions in all the course descriptions (total)
- for each university: the total number of mentions of this author in the courses held at this university (resp.uni+_total)
- for each university: the relative number of mentions of the resp. author in the courses held at this university. Relative number in this context means: the total number of mentions of the resp. author divided by the number of course descriptions scraped for the resp. university (resp.uni+_rel)
- for each country: the total number of mentions in all course descriptions of the universities in that country. (G is short for Germany, A for Austria, S for Switzerland) (total_+resp.country)
- for each country: the relative number of mentions of the resp. author. Relative number in this context means: The sum of relative numbers of mentions of the resp. author per university divided by the number of universities assigned to the resp. country. (rel_+resp.country)

### data/time_counts.csv
The table is obtained from the tables data/writers.csv and data/courses.csv using the notebook time_get_counts.ipynb.
Each row represents a semester, Which semester can be seen from the column (semester-start) that contains timestamps (see above: data/courses.csv).
The columns represent:
- for each university: the total number of writers mentioned in the course descriptions of the resp. semester (resp.uni+_total)
- for each university: the rel number of writers mentioned in the resp. semester, meaning that the total number of writers in the resp. semester is divided by the number of course descriptions available in this semester. (resp.uni+_rel)
- for each university: the total number of female writers mentioned in the course descriptions of the resp. semester (resp.uni+_total_F)
- for each university: the total number of male writers mentioned in the course descriptions of the resp. semester (resp.uni+_total_M)
- for each university: the relative number of female writers mentioned in the course descriptions of the resp. semester divided by the total number of writers mentioned in the course descriptions of the resp. semester (resp.uni+_rel_F)
- for each university: the relative number of male writers mentioned in the course descriptions of the resp. semester divided by the total number of writers mentioned in the course descriptions of the resp. semester (resp.uni+_rel_M)
- for each university: the relative number of male writers mentioned in the course descriptions of the resp. semester divided by the total number of writers mentioned in the course descriptions of the resp. semester (resp.uni+_relrel_F)
- for each university: the double-relative number of male writers mentioned in the course descriptions of the resp. semester divided by the total number of writers mentioned in the course descriptions of the resp. semester and again divided by the number of course descriptions available for the resp. semester (resp.uni+_relrel_M)

### data/universities.csv
The table contains short names and the real names of the universities sampled.

## Notebooks
The notebooks are found in the main folder. A description of what the notebooks do is found at the top of each file.
Overview: The notebook total_numbers.ipynb visualizes statistics about the identified persons and writers. The notebooks writers_get_counts.ipynb and time_get_counts.ipynb are used to extract the tables data/writer_counts.csv and data/time_counts.csv from the tables data/writers.csv and data/courses.csv. The extracted that way is than visualized, and further statistics computed for it in the notebooks writers_statistics_and_visualize.ipynb and time_statistics_and_visualize.ipynb.

# Detailed Documentation

## Selection Process

### Lists of German Studies Programs in Germany, Austria and Switzerland.
The population surveyed in this project are German studies courses in Germany, Austria and Switzerland. Therefore, the first step was to compile lists of German studies courses in these countries. 
The initial aim was to select degree programs, not universities. Therefore, the information collected is more detailed than actually used in the later analysis.

#### Germany
For Germany, the website of the "Agentur für Arbeit" (https://web.arbeitsagentur.de/studiensuche/) was used and the keywords "Germanistik" and "Deutsch" were searched for using the search function. The resulting lists were copied into a table. 
Degree programs with a focus on the Middle Ages, linguistics, interculturality, translation, German as a second or foreign language, degree programs in which German Studies was only a minor subject, as well as degree programs that clearly did not deal with literary studies but only with the German language were removed. For the remaining study programs, the following was recorded (partly automatically, partly manually):
the city, the federal state, the degree, whether it was a teachers' training program and, if so, for which type of school. Subsequently, for the sake of simplicity all teacher training programs except for the "Gymnasium" school type were removed.
126 study programs were obtained that way.

#### Austria
The starting point was the website www.studienwahl.at. There, the keyword "Germanistik" was searched for and, as with the example of Germany, some degree courses were removed and additional information recorded.
15 study programs were obtained that way.

#### Switzerland
The starting point for Switzerland was the website https://www.berufsberatung.ch/dyn/show/17500, which already offers degree programs in the subject "German Linguistics and Literature" are already listed. As with Germany and Austria, these degree programs were adjusted and enriched with additional data.
12 study programs were obtained that way.

### Selection
The initial idea was to select degree programs. The parameters used were the type of degree and whether or not it was a teacher training course. For Germany, it was also taken into account whether it was a former East German or West Germany.
In order to scrape a course catalog, you have to familiarize yourself with its structure. Since this takes some time, it quickly became clear that it would make more sense to scrape all courses of a university in the field of NDL (modern German literature) if you have already familiarized yourself with the structure, instead of keeping the respective course of study as an additional limiting parameter.
Therefore, the lists of study programs were reduced to lists of universities. For Germany, three universities were selected from the former East German states and three from the former West German states (Berlin was included in both groups). If a university from a federal state was selected, the other universities from this federal state were removed from the list so that no federal state would be represented by two universities. Two universities were selected for Austria and one for Switzerland.
The random selection was repeated for individual cases if there was no online course catalog that met the criteria (see below: Scraping).

## Scraping
The randomly selected universities were then checked to see whether they had an online course catalog, whether this went back at least 10 semesters and whether it could be scraped with regard to the criteria relevant here (specification that it is an NDL course). Cases in which these criteria were not met were considered non-responses and a random selection was repeated. 
What does this mean: A course catalog cannot be scraped? In some cases, for example, the search functions in the course catalog were limited, the names of degree programs and module regulations changed too often, so that it was not possible for me as an outsider to understand how I could automatically access the necessary information without very extensive detailed analysis.
In any case, the scraping involved familiarizing myself with how the online course catalog is structured, where the NDL courses are located or how they are named. The code used for scraping was therefore very individual for the different universities and the data set is certainly not completely correct since in some cases relevant courses may not have been found. (Since I cleaned the dataset afterwards for example from courses of the medieval field or from Linguistics it is to be expected that the precision is high, but the recall may be not).

## Scraped courses data
A total of 7185 events were scraped in this way, 6127 of which also had course descriptions scraped. The course descriptions contain a total of 868125 tokens and have an average length of 141,7 tokens.
Figures figures/total_numbers1.png and figures/total_numbers2.png show the quantitites of descriptions and titles obtained per semester and uni. The figures are plotted using the notebook total_numbers.ipynb.

## Manual annotation
Six course descriptions were randomly chosen from each university for manual annotation. References to Persons (NEs) and, as a subset, to writers were annotated. Of the 179 NEs identified, about 53%, belong to writers. This led to the insight that Named Entitiy Recognition (NER) alone is not sufficient to identify writers.

## NER
For NER, five models were evaluated based on the manually annotated data (see table 1 in results folder). Subsequently, the best model was used to annotate named entities in all content descriptions. Nach der NER wurden die NEs mit Spacy lemmatisiert. Single word NEs were automatically matched with multiword NEs if possible. A total of 7689 NEs were identified that way. 

## Entity Linking
The program OpenRefine was used for Entity Linking. The entities were linked automatically to the GND data of the German national library. Manual linking was attempted for entities for which the automatic assignment did not work (e.g. due to misspellings, abbreviations or several entities with the same name). Due to limited resources, however, **no detailed research was carried out on the entities, but a heuristic approach was taken**: If there was a writer of the particular name, it was considered likely that this was meant and not, for example, a doctor of the same name. My knowledge of authors, literary scholars and other people who are likely to be mentioned in course descriptions was also incorporated into this work. Qualitative impressions of this work were 1. that the names were often misspelled overall (presumably because course descriptions are written more quickly and less carefully than publications, for example) and 2. that names of non-German origin in particular were often misspelled and therefore not assigned. This is really a subjective impression, but an additional Google search was sometimes carried out for such names in order to compensate for any bias.
A total of 4410 entities were identified in this way. ( This does not mean, by implication, that 3279 entities could not be matched, because due to variable and incorrect spellings of the names, some entities previously appeared more than once).
For each of the matched identities, a standardized name, gender, occupation and country code were retrieved from the Gemeinsame Normdatei.

## Further Cleaning and Processing
No gender was recorded in the GND for 704 of the identified entities. For these, a gender was guessed based on the first names using the Python Gender Guesser. This left 203 entities for which no gender is recorded in the metadata. In one case of a trans man, the gender given in the GND was corrected from "Female; Male" to "Male". 
The GND uses gendered forms for job titles (for example: "Schriftsteller" and "Schriftstellerin"). These terms were reduced to the masculine form to faciliate quantitative analysis, since the gender is stored in a separate column in each case anyway.
Subsequently, all entities that belong to writers were extracted (data/writers.csv). The GND lists various names for this (writer, playwright, lyricist, narrator), all of these occupations was counted as "writer". The number of writers extracted is in total 1814.

# Counting
Finally, the GND IDs were matched with the seminar data and counted: 1. (per author) how often which author is mentioned where and how often and 2. (per semester): How many authors are mentioned in which semester? How many women are mentioned? How many men are mentioned?
In both cases, absolute and relative values were collected. Relative here means in part a double normalization. Firstly, normalization must be carried out in relation to the universities, as there are major differences in how many content descriptions are available for which university. In a second step, the values for the countries were divided by the number of universities per country in order to compare them with each other.





# Resources

https://web.arbeitsagentur.de/studiensuche/
www.studienwahl.at
https://www.berufsberatung.ch/dyn/show/17500
https://openrefine.org
https://www.dnb.de/DE/Professionell/Standardisierung/GND/gnd_node.html
https://pypi.org/project/gender-guesser/

