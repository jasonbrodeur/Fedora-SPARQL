<header>
Digital Archive / Fedora SPARQL cookbook
============
</header>
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

SELECT ?pid ?label ?parent
FROM <#ri>
WHERE {

?pid <info:fedora/fedora-system:def/model#hasModel> <info:fedora/islandora:sp_large_image_cmodel> ;
    <fedora-model:label> ?label ;
<info:fedora/fedora-system:def/model#state> ?state ;
<info:fedora/fedora-system:def/relations-external#isMemberOfCollection> ?parent ;
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
