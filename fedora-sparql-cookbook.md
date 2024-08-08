# Digital Archive / Fedora SPARQL cookbook 
Used with the McMaster Islandora [Fedora Resource Index Query Service](http://dcs1.mcmaster.ca/fedora/risearch)	
<main>

# Examples 
## Example 1: Display list of all Active (non-deleted) items in a collection
Returned: item URI | item label/title | item's state (Active/Inactive) 

```sparql

PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#>

SELECT ?pid ?label ?id ?state
FROM <#ri>
WHERE {

?pid <fedora-rels-ext:isMemberOfCollection> <info:fedora/macrepo:66660> ;
	<fedora-model:label> ?label ;
<dc:identifier> ?id ;
<info:fedora/fedora-system:def/model#state> ?state ;
<info:fedora/fedora-system:def/model#state> <info:fedora/fedora-system:def/model#Active>;

}

ORDER BY ?pid
```

## Example 2: Display list of all Active (non-deleted) items in a collection, along with derivatives (good for checking)
```sparql

PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#>

SELECT ?pid ?label ?id ?state ?derivs
FROM <#ri>
WHERE {

?pid <fedora-rels-ext:isMemberOfCollection> <info:fedora/macrepo:85282> ;
	<fedora-model:label> ?label ;
	<dc:identifier> ?id ;
<info:fedora/fedora-system:def/model#state> ?state ;
<info:fedora/fedora-system:def/model#state> <info:fedora/fedora-system:def/model#Active>;
<info:fedora/fedora-system:def/view#disseminates> ?derivs

}

ORDER BY ?pid
```



## Example 3: Show all predicates and objects related to a subject (good for exploration) 

```sparql

PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#>

SELECT ?p ?o

FROM <#ri>

WHERE {

<info:fedora/macrepo:83967> ?p ?o ; 

}
```
## Example 4: List all images within the map collection of the Digital Archive
Assumes that all items from the Map Collection use the large image content model (and that other collections aren't using it)

```sparql

PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#>

SELECT ?pid ?label ?parent ?id
FROM <#ri>
WHERE {

?pid <info:fedora/fedora-system:def/model#hasModel> <info:fedora/islandora:sp_large_image_cmodel> ;
    <fedora-model:label> ?label ;
    <dc:identifier> ?id ;
<info:fedora/fedora-system:def/model#state> ?state ;
<info:fedora/fedora-system:def/relations-external#isMemberOfCollection> ?parent ;
<info:fedora/fedora-system:def/model#state> <info:fedora/fedora-system:def/model#Active> ;

}

ORDER BY ?pid
```
## Example 4b: List all images within the map collection of the Digital Archive - no title, no parent
Assumes that all items from the Map Collection use the large image content model (and that other collections aren't using it). Use this version of the script instead of Example 4 when you need to split into a list -- the title field may have commas in it.

```sparql

PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#>

SELECT ?pid ?id
FROM <#ri>
WHERE {

?pid <info:fedora/fedora-system:def/model#hasModel> <info:fedora/islandora:sp_large_image_cmodel> ;
    <dc:identifier> ?id ;
<info:fedora/fedora-system:def/model#state> ?state ;
<info:fedora/fedora-system:def/model#state> <info:fedora/fedora-system:def/model#Active> ;

}

ORDER BY ?pid
```


## Example 5: List all collections within the Digital Archive and their parents

```sparql

PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#>

SELECT ?pid ?label ?parent
FROM <#ri>
WHERE {

?pid <info:fedora/fedora-system:def/model#hasModel> <info:fedora/islandora:collectionCModel> ;
    <fedora-model:label> ?label ;
<info:fedora/fedora-system:def/model#state> ?state ;
<info:fedora/fedora-system:def/relations-external#isMemberOfCollection> ?parent ;
<info:fedora/fedora-system:def/model#state> <info:fedora/fedora-system:def/model#Active> ;

}

ORDER BY ?pid
```
## Example 6: List all images within the map collection of the Digital Archive and their created date
Assumes that all items from the Map Collection use the large image content model (and that other collections aren't using it)

```sparql

PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#>

SELECT ?pid ?label ?parent ?date
FROM <#ri>
WHERE {

?pid <info:fedora/fedora-system:def/model#hasModel> <info:fedora/islandora:sp_large_image_cmodel> ;
    <fedora-model:label> ?label ;
    <info:fedora/fedora-system:def/model#createdDate> ?date ;
<info:fedora/fedora-system:def/model#state> ?state ;
<info:fedora/fedora-system:def/relations-external#isMemberOfCollection> ?parent ;
<info:fedora/fedora-system:def/model#state> <info:fedora/fedora-system:def/model#Active> ;

}

ORDER BY ?pid
```

## Example 7: List all active items in digital archive, along with creation date:

```sparql
PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#>

SELECT ?pid ?label ?date
FROM <#ri>
WHERE {

?pid <info:fedora/fedora-system:def/model#hasModel> <info:fedora/fedora-system:FedoraObject-3.0> ;
    <fedora-model:label> ?label ;
    <info:fedora/fedora-system:def/model#createdDate> ?date ;
<info:fedora/fedora-system:def/model#state> ?state ;
	<info:fedora/fedora-system:def/model#state> <info:fedora/fedora-system:def/model#Active> ;


}

ORDER BY ?pid		
```

## Example 8: List all active items in digital archive, along with creation date and content type:

```sparql
PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#>

SELECT ?pid ?label ?date ?ctype
FROM <#ri>
WHERE {

?pid <info:fedora/fedora-system:def/model#hasModel> <info:fedora/fedora-system:FedoraObject-3.0> ;
    <fedora-model:label> ?label ;
    <info:fedora/fedora-system:def/model#createdDate> ?date ;
    <info:fedora/fedora-system:def/model#hasModel> ?ctype ;

<info:fedora/fedora-system:def/model#state> ?state ;
	<info:fedora/fedora-system:def/model#state> <info:fedora/fedora-system:def/model#Active> ;

}

ORDER BY ?pid		
```
## Example 9: Gather lists of all types of objects
- Run this iteratively, replacing `<info:fedora/islandora:bookCModel>` with the following items:
  -  `<info:fedora/islandora:sp_videoCModel>` # video
  -  `<info:fedora/islandora:sp_large_image_cmodel>` # image
  -  `<info:fedora/islandora:sp-audioCModel>` # audio

```sparql

PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#> 

SELECT ?pid ?label ?parent ?date 

FROM <#ri> 

WHERE { 

?pid <info:fedora/fedora-system:def/model#hasModel> <info:fedora/islandora:bookCModel> ; 

    <fedora-model:label> ?label ; 
    <info:fedora/fedora-system:def/model#createdDate> ?date ; 
    <info:fedora/fedora-system:def/model#state> ?state ; 
    <info:fedora/fedora-system:def/relations-external#isMemberOfCollection> ?parent ; 
    <info:fedora/fedora-system:def/model#state> <info:fedora/fedora-system:def/model#Active> ; 
    #FILTER ( ?date < "2022-04-30T14:45:13.815-05:00"^^xsd:dateTime ) 
} 

 
ORDER BY ?pid	 

```





# Notes
How to identify different types
* info:fedora/islandora:sp-audioCModel
* info:fedora/islandora:sp_videoCModel
* info:fedora/islandora:bookCModel, and 
* info:fedora/islandora:iaBookCModel
* info:fedora/islandora:sp_large_image_cmodel

# Resources
## Islandora-specific
https://islandora.ca/sites/default/files/Scripts%20and%20Tricks%20to%20Improve%20Your%20Repo.pdf
https://islandora.ca/sites/default/files/Don%E2%80%99t%20be%20Dreary,%20Let%E2%80%99s%20Query!.pdf
## Fedora-specific
https://wiki.duraspace.org/display/FEDORA47/SPARQL+Recipes
https://wiki.duraspace.org/display/FEDORA37/Triples+in+the+Resource+Index
https://wiki.duraspace.org/display/FEDORA37/Resource+Index+Search
## General SPARQL
https://www.w3.org/2009/Talks/0615-qbe/
https://www.w3.org/2001/sw/DataAccess/rq23/examples.html

</main>
