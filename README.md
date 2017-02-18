# Chado-PostGraphQL

Repo for connecting postgraphql to chado. Some custom SQL is needed to expose two search functions that work well:

```
function find_features(argOrgName varchar, argRefseq text, argSoType text, argFmin int, argFmax int) returns setof feature
function find_sequence(argOrgName varchar, argRefseq text, argFmin int, argFlen int) returns text
```

These two functions allow for 1) fetching top-level features (and then recursing into them using GraphQL), 2) fetching sequence data

The functions need to be loaded from the sql file INTO the database, before launching PostGraphQL

## Example Queries

Fetch a region of sequence

```graphql
{
  sequence: findSequence(argorgname:"yeast",argrefseq:"chrI", argfmin: 0, argflen: 100) 
}
```

Fetch top level gene features (their names, dbxrefId, and uniquename), as well as the names of their children

```graphql
{
  findFeatures(organismName:"yeast",refseq:"chrI", sotype:"gene", fmin:1, fmax:1000) {
    edges {
      node {
        name
        dbxrefId
        uniquename
        featureRelationshipsByObjectId {
          edges {
            node {
              featureBySubjectId {
                name
              }
            }
          }
        }
      }
    }
  }
}
```

List all Organisms

```graphql
query{
  allOrganisms {
    edges {
      node {
        __id
        organismId
        abbreviation
        genus
        species
        commonName
        infraspecificName
        typeId
        comment
      }
    }
  }
}
```


Creating a new Organism

```graphql
mutation CreateOrganism($input: CreateOrganismInput!) {
	createOrganism(input: $input) {
		clientMutationId
		organism {
			organismId
		}
	}
}
```

data

```json
{
	"input": {
		"clientMutationId": "549b5e7c-0516-4fc9-8944-125401211590",
		"organism": {
			"abbreviation": "abcd",
			"genus": "Abb",
			"species": "cddd",
			"commonName": "asdf",
			"typeId": null,
			"comment": ""
		}
	}
}
```
