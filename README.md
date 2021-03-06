# freya
FREyA is a Natural Language Interface for Querying Ontologies
FREyA is an interactive Natural Language Interface for querying ontologies which combines usability enhancement methods such as feedback and clari
cation dialogs in an attempt to: 1) improve recall by generating the dialog and enriching the domain lexicon from the user's vocabulary, whenever an "unknown" term appears in a question 2) improve precision by resolving ambiguities more effectively through the dialog. The suggestions shown to the user are
found through ontology reasoning and are initially ranked using the combination of string similarity and synonym detection. The system then learns from the user's selections, and improves its performance over time.

FREyA Web Site:
https://sites.google.com/site/naturallanguageinterfaces/freya


Preparation
--------------------------------------------------------------------------------
1. Prepare your repository: you can use sesame workbench to set up a Sesame SPARQL endpoint or you could use OWLIM:
[Setting up OWLIM repository using Sesame Workbench (http://researchsemantics.blogspot.co.uk/2012/03/set-up-your-own-sparql-endpoint-with.html)

2. Check out the FREyA code

cd to the dir where you want to check out the freya project e.g. 

cd ~/projects

git clone https://github.com/danicadamljanovic/freya freya

Quickstart Freya (using Mooney geography)
--------------------------------------------------------------------------------

1. Update freya.properties to point to your repositoryURL and repositoryId.
   - you can find mooney ontology in freya-annotate/src/main/resources/ontologies/mooney folder
   - it is recommended to use owlimlite with owlimhorst when setting up repository
2. mvn clean install -DskipTests will create war file in freya-annotate/target directory and skip running all tests
3. Copy war file into your tomcat webapps folder and start tomcat.
4. Open it: http://localhost:8080/freya


Using freya with other ontologies
--------------------------------------------------------------------------------

1. Configure freya.properties to point to the right repositoryURL and repositoryId
2. Open the homepage of freya at: http://localhost:8080/freya
and click 'Reindex'.


Setting up SOLR:
--------------------------------------------------------------------------------

Download Solr 4.6 Recommended version: http://lucene.apache.org/solr/downloads.html

unpack SOLR and go to example dir

Copy conf file into relevant dir -> example/solr/collection1/conf 


Reindexing SOLR:
----------------------------------------
If for any reason you want to wipe out the SORL index and build it again, the best way to do this is as follows:

1. Startup your solr
2. In the browser call this command:
http://localhost:8983/solr/update?stream.body=%3Cdelete%3E%3Cquery%3E*:*%3C/query%3E%3C/delete%3E
3. Then this command:
http://localhost:8983/solr/update?stream.body=%3Ccommit/%3E
4. Wait for X seconds for updates to take place
5. Start up freya
6. Call Reindex from the freya home page or directly:
http://localhost:8080/freya/service/solr/reindex


What types of natural language queries are supported by FREyA?
--------------------------------------------------------------------------------
Factual questions, e.g.:
-  List cities.
-  What is the capital of California?
-  What is the smallest city in California? (using minimum function on cityPopulation of City locatedIn California)
-  What is the largest city in California? (using maximum function on cityPopulation of City locatedIn California)
-  What is the total state area? (using sum function on stateArea)
-  what is the average population of the cities in california? (using avg function on cityPopulation of City locatedIn California)  


SPARQL example
----------------------------------------
NLP query: 

List cities.

SPARQL query:

prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

prefix xsd: <http://www.w3.org/2001/XMLSchema#>

select distinct ?c0 where {{{  ?c0  ?typeRelationc0 <http://www.mooney.net/geo#City> .  }}} 

LIMIT 10000


Uploading bulk ontologies using Freya:
----------------------------------------
use loadBulk service from FreyaService.

See an example in FreyaServiceTest.loadBulk;