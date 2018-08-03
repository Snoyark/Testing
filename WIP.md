
# Open Knowledge Graphs

#### What they are and how to make them


Do you ever have a bunch of digit-based-codes or random abbreviations that are endemic to your life, either personal or enterprise? They could be drug IDs for a pharmaceutical company, numerical codes for different international entities, or simply LEGO set codes for an avid collector. Of course, there is some datasheet somewhere detailing all of the connections, but it takes time to both find and get the information one needs. This is where knowledge graphs in data.world can come in. Knowledge graphs can allow one to bring in new pieces of data and have them linked with the basic information relating to the given subject. Instead of looking up the country for ISO Code "CHE", you could see in a neighboring column that that is Switzerland. They can also show related information. If you are cataloguing the inventory of LEGO parts, and match on Colors and Parts, you could also bring in Part Categories, to see if perhaps an entire category is in shortage. 

To effectively create an knowledge graph, one needs metadata about the actual "entities" one is working with. It should ideally have more information than just its name, in order to fully utilize knowledge graphs. For example, a knowledge graph that allows the matching of state abbreviations to the original state is useful, but it would be better if it could also link in census regions, or population, among many other distinct attributes per entity.

For clarification, knowledge graphs are split into two types of structures: the ontologies and the entities. The ontologies hold the class information and the logical connections between different concepts and ideas. The entities represent the real-world manifestations of these ideas and have entity-specific attributes. An example of this would be the ontology containing information on the concept of a US Administrative Division, while the entities would represent every state/territory of the US.

The ontology and entities are written in [RDF](https://en.wikipedia.org/wiki/Resource_Description_Framework). The query language for RDF is SPARQL, which is used for building out the entities themselves. If you are familiar with RDF and SPARQL, you likely know the information found in the next few paragraphs. If you don’t, there is a fantastic tutorial on the data.world docs [here](https://docs.data.world/tutorials/sparql/) for SPARQL. Learning about SPARQL will help you understand you understand RDF.

The main concept behind RDF is that data is represented in triples, which are composed of three parts: the subject, the predicate, and the object. The subject is the thing in reference, the predicate is an attribute or type (more like a pointer), and the object is the actual value for this type/attribute for this subject. RDF is not a whitespace sensitive language, nor is the SPARQL language that creates Turtle files (the filetype of RDF).

Here are a few examples involving Texas:
`<usstate_texas> a <UsState>.` This states that the entity usstate_texas is a  UsState.

`<usstate_texas> <fipsAbbreviation> “TX”.` This states that the entity usstate_texas has a fipsAbbreviation with the value TX.

`<usstate_texas> <fipsAbbreviation> “TX”. ` is the same as

`<usstate_texas> <fipsAbbreviation> “TX”.`

At its most base level, triples consist of three [Internationalized Resource Identifiers](https://en.wikipedia.org/wiki/Internationalized_Resource_Identifier) (IRIs) in series, followed by a period. One can think of IRIs as URLs (the latter is a subtype of the former). Just like in URLs, one has to include all the information for explicit IRIs, otherwise the resource will not be retrieved. The object itself could be an IRI OR a literal (a string, integer, double, etc. Look [here](https://www.w3.org/TR/turtle/#literals) for all the types).

This document pertains to building "open" knowledge graphs. By this, it is meant that these knowledge graphs are easily shared and are more flexible in their use. They are also more easily built so the actual process happens more quickly. In contrast, "restricted" knowledge graphs are not meant for easy sharing and live in one place. The main difference in form is that the subject, predicate, or object do not have to be explicit IRIs for open knowledge graphs, which would otherwise restrict them to that specific location. Data.World uses the local notation for IRIs and builds the knowledge graph they define in whichever data.world dataset these files are placed into. The method used to do this uses the local notation for IRIs, using angle brackets. This will be shown below.

To fully understand and follow this ontology-building guide, you should be familiar with SPARQL, and to a lesser extent, data.world. This is definitely doable if the tutorial above is followed to the letter. 
# Ontology

  

## The Class Definitions:
The basic information about entities will live in the class itself, such as the overall title and basic description.

The general structure of a codeset is outlined below, going line by line. The specific *Case*: will be pulling from the US States ontology. 

### `<__name__>`

The __name__ is the name of the type of thing one is trying to create.

##### Case: `<USState>`

### `a owl:Class;`

Tells the type of the object (‘a’ is equivalent to ‘rdf:type’). In this case, this is a Class.

##### Case: `a owl:Class;`

### `rdfs:label “__name__”;`

Tells the singular name of the class.

##### Case: `rdfs:label “US State”;`

### `label:plural ”__names__”;`

Tells the plural name of the class.

##### Case: `label:plural “US States”;`

### `dc:description “description goes here”;`

A short description of what exactly the class is and what attributes it may contain.

##### Case: `dc:description “A constituent political entity of the United States. (excluding territories)”;`

### `foaf:depiction <https://ontology-assets.data.world/icons/__file__>;`

An image that depicts the type of entity this is (left is used as default).

##### Case: `foaf:depiction <https://ontology-assets.data.world/icons/icon-geo.svg>;`

  

### `rdfs:isDefinedBy <>;`
The predicate that defines where the ontology lives. The empty set of angle brackets indicates that this ontology does not have single URL to link back to.

##### Case: `rdfs:isDefinedBy <>;`

### `dwo:contextQuery “__query__”.`

*OPTIONAL* 

A multiline SPARQL query would go here to customize what one would want as the popup (i.e., to include more information or style it based on the information available in every entity)

  

More documentation would be made available later in regards to this

  
 

### `rdfs:subClassOf <__parentClass__>`

*OPTIONAL* 

This creates a class hierarchy in your ontology, with some classes being subclasses of others

##### Case: `rdfs:subClassOf <UsAdministrativeDivision>`

  

## FOAF:Depiction List

All items in this list take the form <https://ontology-assets.data.world/icons/__file__>, where __file__ can be:


### `icon-currency.svg`

A coin with a dollar sign in it

### `icon-fish.svg`

A fish

### `icon-ind.svg`

A little factory (think industry for ind)

### `icon-medical.svg`

A medical plus

### `icon-sex.svg`

A faceless person

### `icon-geo.svg`

Image used as default; a globe

  

## CodeSet:
Given a list of unique attributes (such as state abbreviations), you should be able to get information based on the entity the attributes come from (the states themselves). The way to do this is through the use of a CodeSet.

The general structure of a codeset is outlined below, going line by line. The *Case*: used here will be the US County Short Name codeset (which maps Travis to Travis County, Texas [when given the right context]).

### `<Code__name__>`

The __name__ should match the name of the class that this is a code for

Code should be prepended for ease of reading.

##### Case: `<Code_UsCountyShortName>;`

### `a dwo:CodeSet;`

Tells the type of the object (‘a’ is equivalent to ‘rdf:type’). In this case, this is a Codeset.

##### Case: `a dwo:CodeSet;`

### `dwo:denotation <__name__>;`

Tells what type of class this codeset is for, and thus must have __name__ equivalent to the name of the class this denotes

##### Case: `dwo:denotation <County>;`

### `rdfs:label “__name__ code”`

_Whatever the name of this specific attribute is, plural]

##### Case: `rdfs:label “US County Short Name”;`

### `Label:plural “__name__ codes”;`

Whatever the name of this specific attribute is, plural]

##### Case: `label:plural “US County Short Names”;`

### `rdfs:isDefinedBy <>;`
The predicate that defines where the ontology lives. The empty set of angle brackets indicates that this ontology does not have single URL to link back to.
  

##### Case: `rdfs:isDefinedBy <>;`

### `dwo:denotedByPredicate <__attr__>`

This identifies __object__ as the specific entity attribute that this codeset looks for

##### Case: `dwo:denotedByPredicate <countyShortName>;`

### `dwo:requiresContext <__context__>`

*OPTIONAL*

Used when one has multiple entries with the same name or similar attributes, and needs something to distinguish between them

  

The __something__ would be another class/ontology that has an entity that is used somewhere as an object

  

E.g. Counties have a partOfUsState clause, and have dwo:contextByPredicate dwo:UsAdministrativeDivision clause on their codesets, due to the fact that counties have a lot of repeat names

  

##### Case: `dwo:requiresContext <UsAdministrativeDivision>;`

### `dwo:contextByPredicate <__contextPred__>`

*Required if one uses the predicate above, otherwise optional*

  

__pred__ refers specifically to the attribute that contains the entity whose class is denoted by __something__, shown above

In the above example, __pred__ would be dwo:partOfUsState

  
  

##### Case: `dwo:contextByPredicate <partOfUsState>;`

### `dwo:reshapeRule [dwo:regex “regexPattern”; dwo:regex_replace “replacement”]`

*OPTIONAL*

Checks data (in other datasets) for the specific “regexPattern” and replaces them with the “replacement” pattern before attempting to match them

##### Case: `dwo:reshapeRule [dwo:regex "^(.*) (county|parish|borough).*$"; dwo:regex_replace "$1"].`

  

  
## Notes about Ontology Creation
-   You should create this file by hand in some file editor and then save this as __name__.ttl, where __name__ is up to the creator
    
-   At the top of the ontology, one must include these lines of code:
```
<> a owl:Ontology ;
		a dwo:Ontology ;
		dc:title "__name1__ Ontology"@en ;
		rdfs:label "__name1__ Ontology"@en ;
		owl:versionIRI "https://__username__.linked.data.world/d/ddw-ontologies/v0" ;
		owl:versionInfo 0.0 .
```

Where the __name1__ and __username__ should be replaced with the overall name for the ontology and your username (respectively). These changes are largely just for bookkeeping to show who created the ontology.

  

## Example ontology/codeset with entity for reference

  

```
<Colors>
	a owl:Class ;
	rdfs:isDefinedBy <>;
	rdfs:label "Lego Color" ;
	label:plural "Lego Colors" ;
	foaf:depiction <https://ontology-assets.data.world/icons/icon-geo.svg>;
	dc:description "A color of a lego brick/piece" .

<Code_Colors>
	a dwo:CodeSet ;
	rdfs:isDefinedBy <> ;
	dwo:denotation <Colors>;
	rdfs:label "Color ID" ;
	label:plural "Color IDs";
	dwo:denotedByPredicate <colorId> .
```

One color entity, for reference:

```
<color_10> <colorId> 10 ;
	<istrans> false ;
	<rgb> "4B9F4A" ;
	a <dataset:Colors> ;
	rdfs:label "Bright Green" .
```

  

# Entity

## Entity Guidelines

For the entity files (the actual things that will be matched against)

-   These will be (hopefully) programmatically generated using data.world
-   The best way to produce these is using tabular data and data.world
-   Predicates
    

-   Most of these should be in angle brackets unless specifically referring to an attribute of another ontology with a different IRI, or referring to attributes defined by another ontology (such as geo:lat/geo:long)
	- These should be the attributes of your entities, like: 
	- `dwo:usstate_texas dwo:partOfCountry dwo:country_united_states_of_america`
    

	-   In the example, this is stating that Texas is part of the country of the US.
    
	-   These should be taken from the tabular data.
    
    
-   The two required attributes are detailed below, using the country of Australia for the *Case*.
   

### `skos:prefLabel ?something;`

This is what data.world will actually match on natively, without codesets. This predicate and object are crucial.

Mind the camel cased spelling!
##### Case: 
```
skos:prefLabel “Commonwealth of Australia”@en;
#the @en gives the prefLabel a language 
```


### `rdfs:label ?something;`

This is an internal label; it should be descriptive enough to know what entity this is without looking at its attributes.

#### Case: `rdfs:label “Australia”;`
#### Important:
After finishing the SPARQL query that creates the entity file and then downloading the file, you will have to find all occurrences of `dataset:` and remove them. 
## Provenance

This section is devoted to ensuring that datasets, projects, the creator, and the data itself are documented within the ontology. In the sections below marked with <Prov …..Prov/>, the provenance parts are listed and do not require much to change from ontology to ontology.

  

The __queryURL__ refers to the data.world URL of the query you are working on, where this provenance is being included.

  

The __user__ refers to the data.world user creating the ontology.

  

The __resultingName__ refers to the name of the file that will be created (whatever it will be called in the final dataset).

## Example Construct Query
The generation of entities overall should have a similar, overall look to it. --type-- refers to the __name__ defined in the *ontology* for a given class. The series of blank spaces are left for the name of the source data and the columns in this data.

Below is a template for what every query that generates entities should look like.


```
BASE <dataset:>
#The above statement sets up the use of local notation
PREFIX build: <> # this is, by default, blank, looking like this: ( : <>).
PREFIX data: <> # where the dataset that was created with this project lives
#the content in the <> should look something like this:
#https://__user__.linked.data.world/d/__dataset__
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dwo: <https://ontology.data.world/v0#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX wdrs: <http://www.w3.org/2007/05/powder-s#>
PREFIX dwf: <http://data.world/function/functions#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

#The construct block below is what actually builds the Entities
#part of the knowledge graph
#
#Everything in the Construct must be defined below in the FROM NAMED/GRAPH Sections
CONSTRUCT {
	?iri a ?classIri;
		rdfs:label ?label;
		skos:prefLabel ?preferredLabel;
		Every_other_attribute_goes_here ?otherObjects.
	#the above code block calls for an entity called ?iri, which
	#is a ?classIri. It has the rdfs:label ?label, the skos:prefLabel ?preferredLabel, and some other attributes.
	#

# For every last attribute, one has to use a period
# it is possible to do this for multiple types of entities at once
# e.g., if you have a table for animals (for example), you could do one set for fish and one for squids, etc.

  

#<prov
#gets the name of the activity (user defined) and records the time and agent
	build:__name__ a prov:Activity ;
	prov:wasAssociatedWith ?agent ; prov:startedAtTime ?timeStamp ;
	prov:used ?projectVersion, ?datasetVersion, ?queryVersion ;
	prov:generated ?resultGraph
.
#records the version for the project
	?projectVersion a prov:Entity ;
	prov:specializationOf ?project ; dwo:versionId ?projectVersionId
.
#records the version for the dataset
	?datasetVersion a prov:Entity ;
	prov:specializationOf ?dataset ; dwo:versionId ?datasetVersionId
.
#records the version for the query
	?queryVersion a prov:Entity ;
	prov:specializationOf ?query ; wdrs:text ?queryText ; wdrs:sha1sum ?querySha
.

#prov/>
}

#Tells data.world where to get the information from; should be 
#a standard 
FROM NAMED <datasetURL> {
	GRAPH<datasetURL> {
		?x a tbl-___________; #this would be the tabular data filename
			data:col-____________; #all the columns to pull from the table to give to every entity
			data:col-_________ ?name.

#Assumes some name of the entity can be obtained from the list, and that this name is the entity’s identifier

#This creates an IRI safe name (without spaces and all lowercase)
	BIND(
		LCASE(
			REPLACE(?name, “\\W”, “_”)
		)
	AS ?Lname)

#This creates the ?iri variable seen above
	BIND (
		IRI (
			CONCAT(
				STR(<>), “--type--_”, ?Lname)
			)
		)
	AS ?iri)

#This creates the ?classIri variable seen above
	BIND (
		IRI (
			CONCAT(
				STR(<>), “--type--”
			)
		)
	AS ?classIri)

#This retrieves the ?datasetVersionId for the provenance above
	data: foaf:homepage ?dataset ; dwo:versionId ?datasetVersionId .

}

  

#<prov
#This retrieves the ?projectVersionId for the provenance above
build: foaf:homepage ?dataset ; dwo:versionId ?projectVersionId .

#this should be the URL of the data.world page you are PRESENTLY on
BIND(<__queryURL__> AS ?query)
BIND(<https://data.world/__user__> AS ?agent)
BIND(NOW() AS ?timeStamp)
BIND(dwf:queryText() AS ?queryText)
BIND(SHA1(dwf:queryText()) AS ?querySha)
BIND(IRI(CONCAT(STR(build:project_), ?projectVersionId)) AS ?projectVersion)
BIND(IRI(CONCAT(STR(build:dataset_), ?datasetVersionId)) AS ?datasetVersion)
BIND(IRI(CONCAT(STR(build:query_), ?querySha)) AS ?queryVersion)
BIND(IRI(CONCAT(STR(?project), "/user/", "__resultingName__.ttl")) AS ?resultGraph)

#The above section captures the query, dataset, and project id as
#well as the query text, the creator of the query, and the time it was created

#prov/>
```
  

}

  

# Full Example

An example ontology has been built demonstrating the majority of the use cases for building knowledge graphs. This knowledge graph models the games in the Legend of Zelda video game series. This can be found in this [project](https://data.world/videogameontology/exampleontproject), and all related files are underneath this public organization, including an example match, a piece of example data to build the ontology from, an example of a Wikidata query from where to pull the data, and the actual ontological files themselves.

# Final Step
If, after running the query, one gets results (any results), that most likely means that your query is in the right form and it’s getting the proper values for the entities. You should download this as a Turtle file. You should then place this and the ontology .ttl file in a new dataset titled ddw-ontologies. After doing this, one should be able to see matches for these entities in any new uploaded file.

# Troubleshooting

 If you do not get any results above, there are a few probable errors in the ontology/entities files.

-   The ontology is not written correctly, which would seen by not being able to view it after uploading. This is true for the entities file as well, if the syntax is not proper.
    

-   The most common mistake here is a period or semicolon either missing or in the wrong place.
    

-   The entities natively only match on the ‘skos:prefLabel’ and ‘skos:altLabel’ objects, so you may want to change these or add a codeset that finds entities to match based on different attributes than these
    
-   Ensure everything in a string (that is formatted) is spelled/formatted correctly
    
-   Ensure that the PREFIXes and any IRI are spelled the correct way
    

-   This was a surprisingly large portion of the errors found when creating ontologies
    
-   This includes CAPITALIZATION
    

-   A general rule is a phrase is lower camel case (somethingLikeThisForEveryNewWord)
    

-   Ensure everything ends with either a comma, semicolon, or period.
    

-   The reason why whitespace doesn’t matter in SPARQL is due to it looking for these delimiting punctuation.
    

-   If you’re missing certain attributes or certain entities, ensure that all entities have the attributes you seek; if they do not, make that optional using a clause like this:
    
```
OPTIONAL {
	?x data:col-____attr____ ?value.
}
#where ____attr____ is the column name of the attribute that is needed for a given value
```

-   If it doesn’t work when you feel it should, delete the file that you’re trying to test with (likely your original piece of tabular data) and wait about 30 seconds, and then try it. Sometimes it takes a second to process.