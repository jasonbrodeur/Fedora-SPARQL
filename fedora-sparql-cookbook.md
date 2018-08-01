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

## Example 2: Show all predicates and objects related to a subject (good for exploration) 

```sparql

PREFIX islandora-rels-ext: <http://islandora.ca/ontology/relsext#>

SELECT ?p ?o

FROM <#ri>

WHERE {

<info:fedora/macrepo:83967> ?p ?o ; 

}
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